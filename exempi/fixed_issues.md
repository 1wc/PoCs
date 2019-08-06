## 3 issues in exempi 2.5.0

### 0x00 Description

I found 3 issues of exempi 2.5.0 by fuzzing with AFL. The developer has already fixed them and cuz they were little faults, I decided to disclosure them here.

### 0x01  heap-based buffer over-read in WEBP_Support.cpp

```
processing file /home/liwc/crashes/case1
dump_xmp for file /home/liwc/crashes/case1
=================================================================
==58159==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x60200000ef77 at pc 0x0000005ce0b3 bp 0x7ffc7d9b8580 sp 0x7ffc7d9b8570
READ of size 1 at 0x60200000ef77 thread T0
    #0 0x5ce0b2 in WEBP::VP8XChunk::VP8XChunk(WEBP::Container*) /home/liwc/exempi-master/XMPFiles/source/FormatSupport/WEBP_Support.cpp:125
    #1 0x5ce81c in WEBP::Container::Container(WEBP_MetaHandler*) /home/liwc/exempi-master/XMPFiles/source/FormatSupport/WEBP_Support.cpp:203
    #2 0x5c9661 in WEBP_MetaHandler::CacheFileData() /home/liwc/exempi-master/XMPFiles/source/FileHandlers/WEBP_Handler.cpp:89
    #3 0x48874f in DoOpenFile /home/liwc/exempi-master/XMPFiles/source/XMPFiles.cpp:1076
    #4 0x488f8a in XMPFiles::OpenFile(char const*, unsigned int, unsigned int) /home/liwc/exempi-master/XMPFiles/source/XMPFiles.cpp:1179
    #5 0x47e415 in WXMPFiles_OpenFile_1 /home/liwc/exempi-master/XMPFiles/source/WXMPFiles.cpp:233
    #6 0x41dab4 in TXMPFiles<std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > >::OpenFile(char const*, unsigned int, unsigned int) (/home/liwc/exempi-master/exempi/exempi+0x41dab4)
    #7 0x40bd79 in xmp_files_open_new /home/liwc/exempi-master/exempi/exempi.cpp:294
    #8 0x408ccb in get_xmp_from_file /home/liwc/exempi-master/exempi/main.cpp:242
    #9 0x408ece in dump_xmp /home/liwc/exempi-master/exempi/main.cpp:257
    #10 0x409b54 in process_file /home/liwc/exempi-master/exempi/main.cpp:348
    #11 0x408728 in main /home/liwc/exempi-master/exempi/main.cpp:194
    #12 0x7fd3f681d82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #13 0x407a58 in _start (/home/liwc/exempi-master/exempi/exempi+0x407a58)

0x60200000ef77 is located 1 bytes to the right of 6-byte region [0x60200000ef70,0x60200000ef76)
allocated by thread T0 here:
    #0 0x7fd3f7b54592 in operator new(unsigned long) (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x99592)
    #1 0x4fedc6 in __gnu_cxx::new_allocator<unsigned char>::allocate(unsigned long, void const*) /usr/include/c++/5/ext/new_allocator.h:104
    #2 0x4fe8f0 in std::allocator_traits<std::allocator<unsigned char> >::allocate(std::allocator<unsigned char>&, unsigned long) /usr/include/c++/5/bits/alloc_traits.h:491
    #3 0x4fe50f in std::_Vector_base<unsigned char, std::allocator<unsigned char> >::_M_allocate(unsigned long) /usr/include/c++/5/bits/stl_vector.h:170
    #4 0x58ec6d in unsigned char* std::vector<unsigned char, std::allocator<unsigned char> >::_M_allocate_and_copy<std::move_iterator<unsigned char*> >(unsigned long, std::move_iterator<unsigned char*>, std::move_iterator<unsigned char*>) (/home/liwc/exempi-master/exempi/exempi+0x58ec6d)
    #5 0x58e6c9 in std::vector<unsigned char, std::allocator<unsigned char> >::reserve(unsigned long) /usr/include/c++/5/bits/vector.tcc:75
    #6 0x5cd019 in WEBP::Chunk::Chunk(WEBP::Container*, WEBP_MetaHandler*) /home/liwc/exempi-master/XMPFiles/source/FormatSupport/WEBP_Support.cpp:38
    #7 0x5ce72c in WEBP::Container::Container(WEBP_MetaHandler*) /home/liwc/exempi-master/XMPFiles/source/FormatSupport/WEBP_Support.cpp:190
    #8 0x5c9661 in WEBP_MetaHandler::CacheFileData() /home/liwc/exempi-master/XMPFiles/source/FileHandlers/WEBP_Handler.cpp:89
    #9 0x48874f in DoOpenFile /home/liwc/exempi-master/XMPFiles/source/XMPFiles.cpp:1076
    #10 0x488f8a in XMPFiles::OpenFile(char const*, unsigned int, unsigned int) /home/liwc/exempi-master/XMPFiles/source/XMPFiles.cpp:1179
    #11 0x47e415 in WXMPFiles_OpenFile_1 /home/liwc/exempi-master/XMPFiles/source/WXMPFiles.cpp:233
    #12 0x41dab4 in TXMPFiles<std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > >::OpenFile(char const*, unsigned int, unsigned int) (/home/liwc/exempi-master/exempi/exempi+0x41dab4)
    #13 0x40bd79 in xmp_files_open_new /home/liwc/exempi-master/exempi/exempi.cpp:294
    #14 0x408ccb in get_xmp_from_file /home/liwc/exempi-master/exempi/main.cpp:242
    #15 0x408ece in dump_xmp /home/liwc/exempi-master/exempi/main.cpp:257
    #16 0x409b54 in process_file /home/liwc/exempi-master/exempi/main.cpp:348
    #17 0x408728 in main /home/liwc/exempi-master/exempi/main.cpp:194
    #18 0x7fd3f681d82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

SUMMARY: AddressSanitizer: heap-buffer-overflow /home/liwc/exempi-master/XMPFiles/source/FormatSupport/WEBP_Support.cpp:125 WEBP::VP8XChunk::VP8XChunk(WEBP::Container*)
Shadow bytes around the buggy address:
  0x0c047fff9d90: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9da0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9db0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9dc0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9dd0: fa fa fa fa fa fa 00 02 fa fa 00 04 fa fa 00 06
=>0x0c047fff9de0: fa fa fd fd fa fa 06 fa fa fa 00 fa fa fa[06]fa
  0x0c047fff9df0: fa fa fd fa fa fa fd fd fa fa fd fa fa fa 00 00
  0x0c047fff9e00: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e10: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e20: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e30: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07 
  Heap left redzone:       fa
  Heap right redzone:      fb
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack partial redzone:   f4
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
==58159==ABORTING

```

https://gitlab.freedesktop.org/libopenraw/exempi/issues/12

[Patch](https://gitlab.freedesktop.org/libopenraw/exempi/commit/acee2894ceb91616543927c2a6e45050c60f98f7)

### 0x02  heap-based buffer over-read in ID3_Support.cpp

```
liwc@ubuntu:~/exempi-master_asan/exempi$ ./exempi -x poc
processing file poc
dump_xmp for file poc
=================================================================
==79549==ERROR: AddressSanitizer: heap-buffer-overflow on address 0x60200000eed1 at pc 0x0000004be2ae bp 0x7fffc062ec30 sp 0x7fffc062ec20
READ of size 4 at 0x60200000eed1 thread T0
    #0 0x4be2ad in GetUns32BE ../../../source/EndianUtils.hpp:152
    #1 0x4c2256 in ID3_Support::ID3v2Frame::getFrameValue(unsigned char, unsigned int, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >*) /home/liwc/exempi-master/XMPFiles/source/FormatSupport/ID3_Support.cpp:702
    #2 0x4de2a0 in MP3_MetaHandler::ProcessXMP() /home/liwc/exempi-master/XMPFiles/source/FileHandlers/MP3_Handler.cpp:334
    #3 0x48aafd in XMPFiles::GetXMP(TXMPMeta<std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > >*, char const**, unsigned int*, XMP_PacketInfo*) /home/liwc/exempi-master/XMPFiles/source/XMPFiles.cpp:1471
    #4 0x47fac8 in WXMPFiles_GetXMP_1 /home/liwc/exempi-master/XMPFiles/source/WXMPFiles.cpp:331
    #5 0x41e353 in TXMPFiles<std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > >::GetXMP(TXMPMeta<std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > >*, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >*, XMP_PacketInfo*) (/home/liwc/exempi-master_asan/exempi/exempi+0x41e353)
    #6 0x40c0f1 in xmp_files_get_new_xmp /home/liwc/exempi-master/exempi/exempi.cpp:346
    #7 0x408d07 in get_xmp_from_file /home/liwc/exempi-master/exempi/main.cpp:244
    #8 0x408ece in dump_xmp /home/liwc/exempi-master/exempi/main.cpp:257
    #9 0x409b54 in process_file /home/liwc/exempi-master/exempi/main.cpp:348
    #10 0x408728 in main /home/liwc/exempi-master/exempi/main.cpp:194
    #11 0x7f7b7b20482f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #12 0x407a58 in _start (/home/liwc/exempi-master_asan/exempi/exempi+0x407a58)

0x60200000eed3 is located 0 bytes to the right of 3-byte region [0x60200000eed0,0x60200000eed3)
allocated by thread T0 here:
    #0 0x7f7b7c53b712 in operator new[](unsigned long) (/usr/lib/x86_64-linux-gnu/libasan.so.2+0x99712)
    #1 0x4c10c6 in ID3_Support::ID3v2Frame::read(XMP_IO*, unsigned char) /home/liwc/exempi-master/XMPFiles/source/FormatSupport/ID3_Support.cpp:579
    #2 0x4dd4ae in MP3_MetaHandler::CacheFileData() /home/liwc/exempi-master/XMPFiles/source/FileHandlers/MP3_Handler.cpp:220
    #3 0x48874f in DoOpenFile /home/liwc/exempi-master/XMPFiles/source/XMPFiles.cpp:1076
    #4 0x488f8a in XMPFiles::OpenFile(char const*, unsigned int, unsigned int) /home/liwc/exempi-master/XMPFiles/source/XMPFiles.cpp:1179
    #5 0x47e415 in WXMPFiles_OpenFile_1 /home/liwc/exempi-master/XMPFiles/source/WXMPFiles.cpp:233
    #6 0x41dab4 in TXMPFiles<std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > >::OpenFile(char const*, unsigned int, unsigned int) (/home/liwc/exempi-master_asan/exempi/exempi+0x41dab4)
    #7 0x40bd79 in xmp_files_open_new /home/liwc/exempi-master/exempi/exempi.cpp:294
    #8 0x408ccb in get_xmp_from_file /home/liwc/exempi-master/exempi/main.cpp:242
    #9 0x408ece in dump_xmp /home/liwc/exempi-master/exempi/main.cpp:257
    #10 0x409b54 in process_file /home/liwc/exempi-master/exempi/main.cpp:348
    #11 0x408728 in main /home/liwc/exempi-master/exempi/main.cpp:194
    #12 0x7f7b7b20482f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)

SUMMARY: AddressSanitizer: heap-buffer-overflow ../../../source/EndianUtils.hpp:152 GetUns32BE
Shadow bytes around the buggy address:
  0x0c047fff9d80: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9d90: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9da0: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9db0: fa fa 00 fa fa fa 00 fa fa fa 00 fa fa fa 00 00
  0x0c047fff9dc0: fa fa fd fd fa fa fd fa fa fa fd fa fa fa 00 00
=>0x0c047fff9dd0: fa fa 01 fa fa fa 04 fa fa fa[03]fa fa fa 03 fa
  0x0c047fff9de0: fa fa 00 04 fa fa 05 fa fa fa 00 04 fa fa fd fd
  0x0c047fff9df0: fa fa 00 05 fa fa fd fa fa fa 05 fa fa fa 00 00
  0x0c047fff9e00: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e10: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
  0x0c047fff9e20: fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa fa
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07 
  Heap left redzone:       fa
  Heap right redzone:      fb
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack partial redzone:   f4
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
==79549==ABORTING

```

https://gitlab.freedesktop.org/libopenraw/exempi/issues/13

[Patch](https://gitlab.freedesktop.org/libopenraw/exempi/commit/0370edff0f91bd04a5600b15c5b3f1ed03d0d157)

### 0x03  global-buffer-overflow in MP3_Hanlder.cpp

```
liwc@ubuntu:~/exempi-master_asan/exempi$ ./exempi -x poc2
processing file poc2
dump_xmp for file poc2
=================================================================
==80231==ERROR: AddressSanitizer: global-buffer-overflow on address 0x0000006cdce0 at pc 0x0000004dbb24 bp 0x7ffc0ad43a50 sp 0x7ffc0ad43a40
READ of size 4 at 0x0000006cdce0 thread T0
    #0 0x4dbb23 in GetUns32BE ../../../source/EndianUtils.hpp:152
    #1 0x4de11f in MP3_MetaHandler::ProcessXMP() /home/liwc/exempi-master/XMPFiles/source/FileHandlers/MP3_Handler.cpp:320
    #2 0x48aafd in XMPFiles::GetXMP(TXMPMeta<std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > >*, char const**, unsigned int*, XMP_PacketInfo*) /home/liwc/exempi-master/XMPFiles/source/XMPFiles.cpp:1471
    #3 0x47fac8 in WXMPFiles_GetXMP_1 /home/liwc/exempi-master/XMPFiles/source/WXMPFiles.cpp:331
    #4 0x41e353 in TXMPFiles<std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > >::GetXMP(TXMPMeta<std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > >*, std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >*, XMP_PacketInfo*) (/home/liwc/exempi-master_asan/exempi/exempi+0x41e353)
    #5 0x40c0f1 in xmp_files_get_new_xmp /home/liwc/exempi-master/exempi/exempi.cpp:346
    #6 0x408d07 in get_xmp_from_file /home/liwc/exempi-master/exempi/main.cpp:244
    #7 0x408ece in dump_xmp /home/liwc/exempi-master/exempi/main.cpp:257
    #8 0x409b54 in process_file /home/liwc/exempi-master/exempi/main.cpp:348
    #9 0x408728 in main /home/liwc/exempi-master/exempi/main.cpp:194
    #10 0x7f2eeb8bc82f in __libc_start_main (/lib/x86_64-linux-gnu/libc.so.6+0x2082f)
    #11 0x407a58 in _start (/home/liwc/exempi-master_asan/exempi/exempi+0x407a58)

0x0000006cdce1 is located 0 bytes to the right of global variable '*.LC33' defined in 'MP3_Handler.cpp' (0x6cdce0) of size 1
  '*.LC33' is ascii string ''
SUMMARY: AddressSanitizer: global-buffer-overflow ../../../source/EndianUtils.hpp:152 GetUns32BE
Shadow bytes around the buggy address:
  0x0000800d1b40: f9 f9 f9 f9 04 f9 f9 f9 f9 f9 f9 f9 00 03 f9 f9
  0x0000800d1b50: f9 f9 f9 f9 05 f9 f9 f9 f9 f9 f9 f9 04 f9 f9 f9
  0x0000800d1b60: f9 f9 f9 f9 00 00 00 05 f9 f9 f9 f9 00 03 f9 f9
  0x0000800d1b70: f9 f9 f9 f9 05 f9 f9 f9 f9 f9 f9 f9 04 f9 f9 f9
  0x0000800d1b80: f9 f9 f9 f9 05 f9 f9 f9 f9 f9 f9 f9 04 f9 f9 f9
=>0x0000800d1b90: f9 f9 f9 f9 05 f9 f9 f9 f9 f9 f9 f9[01]f9 f9 f9
  0x0000800d1ba0: f9 f9 f9 f9 05 f9 f9 f9 f9 f9 f9 f9 04 f9 f9 f9
  0x0000800d1bb0: f9 f9 f9 f9 00 00 02 f9 f9 f9 f9 f9 05 f9 f9 f9
  0x0000800d1bc0: f9 f9 f9 f9 04 f9 f9 f9 f9 f9 f9 f9 07 f9 f9 f9
  0x0000800d1bd0: f9 f9 f9 f9 05 f9 f9 f9 f9 f9 f9 f9 04 f9 f9 f9
  0x0000800d1be0: f9 f9 f9 f9 00 01 f9 f9 f9 f9 f9 f9 05 f9 f9 f9
Shadow byte legend (one shadow byte represents 8 application bytes):
  Addressable:           00
  Partially addressable: 01 02 03 04 05 06 07 
  Heap left redzone:       fa
  Heap right redzone:      fb
  Freed heap region:       fd
  Stack left redzone:      f1
  Stack mid redzone:       f2
  Stack right redzone:     f3
  Stack partial redzone:   f4
  Stack after return:      f5
  Stack use after scope:   f8
  Global redzone:          f9
  Global init order:       f6
  Poisoned by user:        f7
  Container overflow:      fc
  Array cookie:            ac
  Intra object redzone:    bb
  ASan internal:           fe
==80231==ABORTING

```

https://gitlab.freedesktop.org/libopenraw/exempi/issues/14

[Patch](https://gitlab.freedesktop.org/libopenraw/exempi/commit/fdd4765a699f9700850098b43b9798b933acb32f)

