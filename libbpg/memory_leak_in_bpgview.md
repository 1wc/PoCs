## A memory leak vulnerability in libbpg's bpgview

### 0x00  How to find it

​	I build libbpg on Ubuntu 16.04 with gcc & ASAN, then I simply test the bpgview and the report is as below:

```
liwc@ubuntu:~/libbpg-0.9.8$ ./bpgview poc 
=================================================================
==10029==ERROR: LeakSanitizer: detected memory leaks

Direct leak of 11204 byte(s) in 1 object(s) allocated from:
    #0 0x7fe3e0991662 in malloc (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x98662)
    #1 0x4030ac in bpg_load /home/liwc/libbpg-0.9.8/bpgview.c:94
```

​	After reading the source code of bpg_load, I confirmed it a memory leak bug.

### 0x01  simple analyse

​	Just look at the unsigned integer pointer **buf** and we can see it should be freed unless the malloc operation failed.  

```
Frame *bpg_load(FILE *f, int *pframe_count, int *ploop_count)
{
    ...
    uint8_t *buf;
    ...
    buf = malloc(len);
    if (!buf)
        return NULL;
    if (fread(buf, 1, len, f) != len)
        return NULL;
    ...
    for(;;) {
        if (bpg_decoder_start(s, BPG_OUTPUT_FORMAT_RGBA32) < 0)
            break;
        bpg_decoder_get_frame_duration(s, &delay_num, &delay_den);
        frames = realloc(frames, sizeof(frames[0]) * (frame_count + 1));
        img = SDL_CreateRGBSurface(SDL_HWSURFACE, bi->width, bi->height, 32,
                                   rmask, gmask, bmask, amask);
        if (!img) 
            goto fail;
    
        SDL_LockSurface(img);
        for(y = 0; y < bi->height; y++) {
            bpg_decoder_get_line(s, (uint8_t *)img->pixels + y * img->pitch);
        }
        SDL_UnlockSurface(img);
        frames[frame_count].img = img;
        frames[frame_count].delay = (delay_num * 1000) / delay_den;
        frame_count++;
    }
    bpg_decoder_close(s);
    *pframe_count = frame_count;
    *ploop_count = bi->loop_count;
    return frames;
 fail:
    bpg_decoder_close(s);
    for(i = 0; i < frame_count; i++) {
        SDL_FreeSurface(frames[i].img);
    }
    free(frames);
    *pframe_count = 0;
    return NULL;
}
```

