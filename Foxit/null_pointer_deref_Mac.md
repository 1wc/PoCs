I found two vulnerabilities in Foxit Reader for Mac and Foxit PhantomPDF *Mac*, which both triggered null pointer dereferences and led the program to crash(Xinru Chi of Pangu Lab also found it independently).

## Basic Information

Product：Foxit Reader *Mac*/Foxit PhantomPDF *Mac*

Version：3.2.0.0404/ 3.2.0.0412

Platform：MacOS Mojave 10.14.4

Type：CWE-476: NULL Pointer Dereference

## Security Bulletin

https://www.foxitsoftware.com/support/security-bulletins.php

## How to find it

By Fuzzing with [riufuzz](https://github.com/riusksk/riufuzz).

## How to reproduce it

### GUI

1. open Foxit Reader *Mac*/Foxit PhantomPDF *Mac*
2. load poc.pdf
3. exit and report error

## Debug



```
1wcsdeMacBook-Pro:~ 1wc$ lldb /Applications/Foxit\ Reader.app/Contents/MacOS/Foxit\ Reader 
(lldb) target create "/Applications/Foxit Reader.app/Contents/MacOS/Foxit Reader"
Current executable set to '/Applications/Foxit Reader.app/Contents/MacOS/Foxit Reader' (x86_64).
(lldb) r
Process 78117 launched: '/Applications/Foxit Reader.app/Contents/MacOS/Foxit Reader' (x86_64)
QLocale::system().name():  "zh_CN"
2019-06-29 11:27:19.644960+0800 Foxit Reader[78117:2731014] [default] Unable to load Info.plist exceptions (eGPUOverrides)
2019-06-29 11:27:19.906285+0800 Foxit Reader[78117:2731026] MessageTracer: Falling back to default whitelist
[TimeStamp]-----main-----begin
efficiency_[TimeStamp]-----Launch-----begin
[FXCrypto load fail]:  "/Applications/Foxit Reader.app/Contents/MacOS/../fxplugins/FXCrypto"
test: ""
CLM_LicenseManager::CheckRegisterSuc():bRegister: 1
test: ""
CLM_LicenseManager::IsTrialExpired(): m_pCheckLicense m_isExpired 1
CLM_LicenseManager::IsTrialExpired(): m_isExpired 1
locale name: from AppleLanguages: "zh_CN"
cmd: ""
bMultInstance: false
svn: b1b9955
time:  "周六 6月 29 11:27:19 2019"


##################################################
##############Foxit Reader Setup Log##############
################################################## 


[TimeStamp]-----CReader_AppEx::InitInstance()-----begin
efficiency_[TimeStamp]-----CReader_AppEx::InitInstance()-----begin
Could not parse application stylesheet
[TimeStamp]-----CMainWindow::Init()-----begin
QObject::connect: Use the SLOT or SIGNAL macro to connect CMainWindow::
defName=  "com.foxitsoftware.foxitreader"
QLayout: Attempting to add QLayout "" to CMainToolbar "", which already has a layout
QLayout: Attempting to add QLayout "" to CMainToolbar "", which already has a layout
QLayout: Attempting to add QLayout "" to CMainToolbar "", which already has a layout
configpath:  "/Users/1wc/Library/Application Support/Foxit Software/Foxit Reader/Configuration/configtoolbar.xml"
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
this->indexOf(m_pSeparate); 0 "Color"
this->indexOf(m_pSeparate); 0 "SinglePage"
this->indexOf(m_pSeparate); 0 "Continuous"
this->indexOf(m_pSeparate); 0 "Facing"
this->indexOf(m_pSeparate); 0 "ContinuousFacing"
this->indexOf(m_pSeparate); 0 "SeparateCoverPage"
this->indexOf(m_pSeparate); 0 "RotateRight"
this->indexOf(m_pSeparate); 0 "RotateLeft"
this->indexOf(m_pSeparate); 0 "PrevView"
this->indexOf(m_pSeparate); 0 "NextView"
CMainToolbar::AddSecondaryToolBar:  "View"
CMainToolbar::AddSecondaryToolBar:  "Comment"
CMainToolbar::AddSecondaryToolBar:  "Protect"
CMainToolbar::AddSecondaryToolBar:  "Convert"
CMainToolbar::AddSecondaryToolBar:  "Form"
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
[TimeStamp]-----CReader_AppEx::LoadLibrarys()-----begin
efficiency_[TimeStamp]-----CReader_AppEx::LoadLibrarys()-----begin
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
QMetaObject::connectSlotsByName: No matching signal for on_symbol1_bullet_clicked()
QMetaObject::connectSlotsByName: No matching signal for on_symbol2_bullet_clicked()
QMetaObject::connectSlotsByName: No matching signal for on_symbol3_bullet_clicked()
QMetaObject::connectSlotsByName: No matching signal for on_symbol4_bullet_clicked()
QMetaObject::connectSlotsByName: No matching signal for on_symbol5_bullet_clicked()
QMetaObject::connectSlotsByName: No matching signal for on_symbol6_bullet_clicked()
QMetaObject::connectSlotsByName: No matching signal for on_symbol7_bullet_clicked()
QMetaObject::connectSlotsByName: No matching signal for on_symbol8_bullet_clicked()
QMetaObject::connectSlotsByName: No matching signal for on_DescripEdit_editingFinished()
QObject::connect: No such slot CExportModule::ToImage()
QObject::connect: (sender name:   'To PNG')
QObject::connect: No such slot CExportModule::ToImage()
QObject::connect: (sender name:   'TO JPEG')
QObject::connect: No such slot CExportModule::ToImage()
QObject::connect: (sender name:   'TO JPEG2000')
QObject::connect: No such slot CExportModule::ToImage()
QObject::connect: (sender name:   'TO TIFF')
QObject::connect: No such slot CExportModule::ToImage()
QObject::connect: (sender name:   'TO BMP')
QObject::connect: No such slot CExportModule::ToOffice()
QObject::connect: (sender name:   'To Rich Text')
[TimeStamp]-----CReader_AppEx::LoadLibrarys()-----end-----UsedTime: 65ms 
efficiency_[TimeStamp]-----CReader_AppEx::LoadLibrarys()-----end-----UsedTime: 65ms 
QMetaObject::connectSlotsByName: No matching signal for on_TabToolButton_click()
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
this->indexOf(m_pSeparate); 0 "Thickness"
this->indexOf(m_pSeparate); 0 "Color"
libpng warning: iCCP: known incorrect sRGB profile
this->indexOf(m_pSeparate); 0 "Highlight"
this->indexOf(m_pSeparate); 0 "Squiggly"
this->indexOf(m_pSeparate); 0 "Underline"
this->indexOf(m_pSeparate); 0 "StrikeOut"
this->indexOf(m_pSeparate); 0 "Replace"
this->indexOf(m_pSeparate); 0 "Caret"
this->indexOf(m_pSeparate); 0 "AreaHighlight"
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
this->indexOf(m_pSeparate); 0 "Typewriter"
this->indexOf(m_pSeparate); 0 "Textbox"
this->indexOf(m_pSeparate); 0 "Callout"
this->indexOf(m_pSeparate); 0 "Font"
QObject::connect: No such slot CMSA_Module::Update_DrawToolMenu()
QObject::connect: (sender name:   'mainwindow')
QObject::connect: No such slot CMSA_Module::Update_DrawToolMenu()
QObject::connect: (sender name:   'mainwindow')
QObject::connect: No such slot CMSA_Module::Update_DrawToolMenu()
QObject::connect: (sender name:   'mainwindow')
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
this->indexOf(m_pSeparate); 0 "Measure"
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
this->indexOf(m_pSeparate); 1 "Note"
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
this->indexOf(m_pSeparate); 0 "Shape"
this->indexOf(m_pSeparate); 0 "Pencil"
this->indexOf(m_pSeparate); 0 "Rubber"
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
this->indexOf(m_pSeparate); 1 "文件"
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
this->indexOf(m_pSeparate); 0 "Stamp"
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
this->indexOf(m_pSeparate); 0 "PDFSign"
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
this->indexOf(m_pSeparate); 0 "ResetForm"
this->indexOf(m_pSeparate); 0 "ImportForm"
this->indexOf(m_pSeparate); 0 "ExportForm"
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
this->indexOf(m_pSeparate); 0 "Sign_And_Certify"
this->indexOf(m_pSeparate); 0 "Validate_Sign"
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
this->indexOf(m_pSeparate); 0 "TNAME_FROMFILES"
this->indexOf(m_pSeparate); 0 "TNAME_CREATEBLANK"
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
this->indexOf(m_pSeparate); 0 "To Other"
[TimeStamp]-----CReader_AppEx::LoadPlugs()-----begin
efficiency_[TimeStamp]-----CReader_AppEx::LoadPlugs()-----begin
"“/Applications/Foxit Reader.app/Contents/fxplugins/libconverttopdf_plugin.dylib”不是Qt插件" 

"“/Applications/Foxit Reader.app/Contents/fxplugins/libfpcsdk.dylib”不是Qt插件" 

2019-06-29 11:27:21.080775+0800 Foxit Reader[78117:2730984] ADAL version 2.2.8-dev
"未知错误" 

"“/Applications/Foxit Reader.app/Contents/fxplugins/librpc.dylib”不是Qt插件" 

"未知错误" 

CFRMSPlg::PIInit
___________________CWaitingDlg::Hide
""
test: ""
test: ""
this->indexOf(m_pSeparate); 0 "RestrictAccess"
this->indexOf(m_pSeparate); 0 "Settings"
UpdateAPp PIInit() 

date: 2458664  -  2458663  =  1 

plugin date: 2458664  -  2458663  =  1 

[TimeStamp]-----CReader_AppEx::LoadPlugs()-----end-----UsedTime: 67ms 
efficiency_[TimeStamp]-----CReader_AppEx::LoadPlugs()-----end-----UsedTime: 67ms 
[TimeStamp]-----CMainWindow::Init()-----end-----UsedTime: 67ms 
[TimeStamp]-----CReader_AppEx::InitInstance()-----end-----UsedTime: 124ms 
efficiency_[TimeStamp]-----CReader_AppEx::InitInstance()-----end-----UsedTime: 1182ms 
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
ActivateTab--index= 0  m_nlastIndex= 0
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
[TimeStamp]-----main-----end-----UsedTime: 280ms 
efficiency_[TimeStamp]-----Launch-----end-----UsedTime: 1357ms 
g_GlobalDataMgr.m_PermanentData.m_bCheckRegister= 1
defName=  "com.foxitsoftware.foxitreader"
ca.QueryDefaultReader= 1
defName=  "com.foxitsoftware.foxitreader"
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
# press Ctrl + C
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
  frame #0: 0x00007fff5d56917a libsystem_kernel.dylib`mach_msg_trap + 10
libsystem_kernel.dylib`mach_msg_trap:
-> 0x7fff5d56917a <+10>: retq   
  0x7fff5d56917b <+11>: nop    

libsystem_kernel.dylib`mach_msg_overwrite_trap:
  0x7fff5d56917c <+0>: movq   %rcx, %r10
  0x7fff5d56917f <+3>: movl   $0x1000020, %eax         ; imm = 0x1000020 
Target 0: (Foxit Reader) stopped.
# break in function CReader_DocumentEx::IsPageLabelOnlyHaveNumD()
(lldb) b *0x10009A0B0
Breakpoint 1: where = Foxit Reader`CReader_DocumentEx::IsPageLabelOnlyHaveNumD() + 832, address = 0x000000010009a0b0
(lldb) c
Process 78117 resuming
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
libpng warning: iCCP: known incorrect sRGB profile
2019-06-29 11:27:46.470771+0800 Foxit Reader[78117:2731034] [default] Unable to load Info.plist exceptions (eGPUOverrides)
objc[78117]: Class FIFinderSyncExtensionHost is implemented in both /System/Library/PrivateFrameworks/FinderKit.framework/Versions/A/FinderKit (0x7fff8c4b5210) and /System/Library/PrivateFrameworks/FileProvider.framework/OverrideBundles/FinderSyncCollaborationFileProviderOverride.bundle/Contents/MacOS/FinderSyncCollaborationFileProviderOverride (0x12251cdc8). One of the two will be used. Which one is undefined.
CoreGraphics PDF has logged an error. Set environment variabe "CG_PDF_VERBOSE" to learn more.
[TimeStamp]-----CMainWindow::OpenFile-----begin
efficiency_[TimeStamp]-----OpenFile: /Users/1wc/Desktop/xxx/poc.pdf-----begin
m_pParser 0x10daa4a10
# stop at break
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = breakpoint 1.1
  frame #0: 0x000000010009a0b0 Foxit Reader`CReader_DocumentEx::IsPageLabelOnlyHaveNumD() + 832
Foxit Reader`CReader_DocumentEx::IsPageLabelOnlyHaveNumD:
-> 0x10009a0b0 <+832>: movq   -0x1a8(%rbp), %rdi
  0x10009a0b7 <+839>: movq   -0x1b0(%rbp), %rsi
  0x10009a0be <+846>: callq 0x100555dd0               ; CPDF_Dictionary::KeyExist(CFX_ByteStringC const&) const
  0x10009a0c3 <+851>: movl   %eax, -0x1b4(%rbp)
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010009a0b7 Foxit Reader`CReader_DocumentEx::IsPageLabelOnlyHaveNumD() + 839
Foxit Reader`CReader_DocumentEx::IsPageLabelOnlyHaveNumD:
-> 0x10009a0b7 <+839>: movq   -0x1b0(%rbp), %rsi
  0x10009a0be <+846>: callq 0x100555dd0               ; CPDF_Dictionary::KeyExist(CFX_ByteStringC const&) const
  0x10009a0c3 <+851>: movl   %eax, -0x1b4(%rbp)
  0x10009a0c9 <+857>: jmp   0x10009a0ce               ; <+862>
Target 0: (Foxit Reader) stopped.
# after executing "movq   movq   -0x1a8(%rbp), %rdi"，the value of rdi is 0, so the -0x1a8(%rbp) was a null pointer.
(lldb) register read/x
General Purpose Registers:
      rax = 0x0000000000000001
      rbx = 0x000000010024bd00 Foxit Reader`CGroupcontainer::qt_metacall(QMetaObject::Call, int, void**) + 48
      rcx = 0x00007ffeefbfc570
      rdx = 0x0000000101f021e4  "'S'"
      rdi = 0x0000000000000000
      rsi = 0x0000000000000020
      rbp = 0x00007ffeefbfc620
      rsp = 0x00007ffeefbfc3d0
      r8 = 0x00000000000130a8
      r9 = 0x0000000000000000
      r10 = 0x000000012307e530
      r11 = 0x000000012307e528
      r12 = 0x0000000000000034
      r13 = 0x0000000109989388  
      r14 = 0x0000000000000001
      r15 = 0x000000010b180b10
      rip = 0x000000010009a0b7 Foxit Reader`CReader_DocumentEx::IsPageLabelOnlyHaveNumD() + 839
  rflags = 0x0000000000000206
      cs = 0x000000000000002b
      fs = 0x0000000000000000
      gs = 0x0000000000000000
       
# step over to function CPDF_Dictionary::KeyExist
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010009a0be Foxit Reader`CReader_DocumentEx::IsPageLabelOnlyHaveNumD() + 846
Foxit Reader`CReader_DocumentEx::IsPageLabelOnlyHaveNumD:
-> 0x10009a0be <+846>: callq 0x100555dd0               ; CPDF_Dictionary::KeyExist(CFX_ByteStringC const&) const
  0x10009a0c3 <+851>: movl   %eax, -0x1b4(%rbp)
  0x10009a0c9 <+857>: jmp   0x10009a0ce               ; <+862>
  0x10009a0ce <+862>: leaq   -0xb0(%rbp), %rdi
Target 0: (Foxit Reader) stopped.
# step into function CPDF_Dictionary::KeyExist，then step over again
(lldb) s
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step into
  frame #0: 0x0000000100555dd0 Foxit Reader`CPDF_Dictionary::KeyExist(CFX_ByteStringC const&) const
Foxit Reader`CPDF_Dictionary::KeyExist:
-> 0x100555dd0 <+0>: pushq %rbp
  0x100555dd1 <+1>: movq   %rsp, %rbp
  0x100555dd4 <+4>: subq   $0x10, %rsp
  0x100555dd8 <+8>: addq   $0x20, %rdi
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x0000000100555dd1 Foxit Reader`CPDF_Dictionary::KeyExist(CFX_ByteStringC const&) const + 1
Foxit Reader`CPDF_Dictionary::KeyExist:
-> 0x100555dd1 <+1>: movq   %rsp, %rbp
  0x100555dd4 <+4>: subq   $0x10, %rsp
  0x100555dd8 <+8>: addq   $0x20, %rdi
  0x100555ddc <+12>: leaq   -0x8(%rbp), %rdx
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x0000000100555dd4 Foxit Reader`CPDF_Dictionary::KeyExist(CFX_ByteStringC const&) const + 4
Foxit Reader`CPDF_Dictionary::KeyExist:
-> 0x100555dd4 <+4>: subq   $0x10, %rsp
  0x100555dd8 <+8>: addq   $0x20, %rdi
  0x100555ddc <+12>: leaq   -0x8(%rbp), %rdx
  0x100555de0 <+16>: callq 0x10099d5a0               ; CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const
Target 0: (Foxit Reader) stopped.
# after executing "addq $0x20, %rdi", the value of rdi will be 0x20.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x0000000100555dd8 Foxit Reader`CPDF_Dictionary::KeyExist(CFX_ByteStringC const&) const + 8
Foxit Reader`CPDF_Dictionary::KeyExist:
-> 0x100555dd8 <+8>: addq   $0x20, %rdi
  0x100555ddc <+12>: leaq   -0x8(%rbp), %rdx
  0x100555de0 <+16>: callq 0x10099d5a0               ; CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const
  0x100555de5 <+21>: addq   $0x10, %rsp
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x0000000100555ddc Foxit Reader`CPDF_Dictionary::KeyExist(CFX_ByteStringC const&) const + 12
Foxit Reader`CPDF_Dictionary::KeyExist:
-> 0x100555ddc <+12>: leaq   -0x8(%rbp), %rdx
  0x100555de0 <+16>: callq 0x10099d5a0               ; CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const
  0x100555de5 <+21>: addq   $0x10, %rsp
  0x100555de9 <+25>: popq   %rbp
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x0000000100555de0 Foxit Reader`CPDF_Dictionary::KeyExist(CFX_ByteStringC const&) const + 16
Foxit Reader`CPDF_Dictionary::KeyExist:
-> 0x100555de0 <+16>: callq 0x10099d5a0               ; CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const
  0x100555de5 <+21>: addq   $0x10, %rsp
  0x100555de9 <+25>: popq   %rbp
  0x100555dea <+26>: retq   
Target 0: (Foxit Reader) stopped.
(lldb) s
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step into
  frame #0: 0x000000010099d5a0 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5a0 <+0>: pushq %rbp
  0x10099d5a1 <+1>: movq   %rsp, %rbp
  0x10099d5a4 <+4>: pushq %r15
  0x10099d5a6 <+6>: pushq %r14
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5a1 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 1
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5a1 <+1>: movq   %rsp, %rbp
  0x10099d5a4 <+4>: pushq %r15
  0x10099d5a6 <+6>: pushq %r14
  0x10099d5a8 <+8>: pushq %r12
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5a4 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 4
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5a4 <+4>: pushq %r15
  0x10099d5a6 <+6>: pushq %r14
  0x10099d5a8 <+8>: pushq %r12
  0x10099d5aa <+10>: pushq %rbx
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5a6 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 6
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5a6 <+6>: pushq %r14
  0x10099d5a8 <+8>: pushq %r12
  0x10099d5aa <+10>: pushq %rbx
  0x10099d5ab <+11>: movq   %rdx, %r14
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5a8 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 8
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5a8 <+8>: pushq %r12
  0x10099d5aa <+10>: pushq %rbx
  0x10099d5ab <+11>: movq   %rdx, %r14
  0x10099d5ae <+14>: movq   %rsi, %r12
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5aa Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 10
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5aa <+10>: pushq %rbx
  0x10099d5ab <+11>: movq   %rdx, %r14
  0x10099d5ae <+14>: movq   %rsi, %r12
  0x10099d5b1 <+17>: movl   0x8(%r12), %ecx
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5ab Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 11
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5ab <+11>: movq   %rdx, %r14
  0x10099d5ae <+14>: movq   %rsi, %r12
  0x10099d5b1 <+17>: movl   0x8(%r12), %ecx
  0x10099d5b6 <+22>: xorl   %r15d, %r15d
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5ae Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 14
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5ae <+14>: movq   %rsi, %r12
  0x10099d5b1 <+17>: movl   0x8(%r12), %ecx
  0x10099d5b6 <+22>: xorl   %r15d, %r15d
  0x10099d5b9 <+25>: testl %ecx, %ecx
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5b1 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 17
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5b1 <+17>: movl   0x8(%r12), %ecx
  0x10099d5b6 <+22>: xorl   %r15d, %r15d
  0x10099d5b9 <+25>: testl %ecx, %ecx
  0x10099d5bb <+27>: movl   $0x0, %eax
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5b6 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 22
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5b6 <+22>: xorl   %r15d, %r15d
  0x10099d5b9 <+25>: testl %ecx, %ecx
  0x10099d5bb <+27>: movl   $0x0, %eax
  0x10099d5c0 <+32>: jle   0x10099d5df               ; <+63>
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5b9 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 25
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5b9 <+25>: testl %ecx, %ecx
  0x10099d5bb <+27>: movl   $0x0, %eax
  0x10099d5c0 <+32>: jle   0x10099d5df               ; <+63>
  0x10099d5c2 <+34>: movq   (%r12), %rdx
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5bb Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 27
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5bb <+27>: movl   $0x0, %eax
  0x10099d5c0 <+32>: jle   0x10099d5df               ; <+63>
  0x10099d5c2 <+34>: movq   (%r12), %rdx
  0x10099d5c6 <+38>: xorl   %eax, %eax
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5c0 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 32
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5c0 <+32>: jle   0x10099d5df               ; <+63>
  0x10099d5c2 <+34>: movq   (%r12), %rdx
  0x10099d5c6 <+38>: xorl   %eax, %eax
  0x10099d5c8 <+40>: nopl   (%rax,%rax)
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5c2 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 34
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5c2 <+34>: movq   (%r12), %rdx
  0x10099d5c6 <+38>: xorl   %eax, %eax
  0x10099d5c8 <+40>: nopl   (%rax,%rax)
  0x10099d5d0 <+48>: imull  $0x1f, %eax, %esi
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5c6 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 38
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5c6 <+38>: xorl   %eax, %eax
  0x10099d5c8 <+40>: nopl   (%rax,%rax)
  0x10099d5d0 <+48>: imull  $0x1f, %eax, %esi
  0x10099d5d3 <+51>: movzbl (%rdx), %eax
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5c8 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 40
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5c8 <+40>: nopl   (%rax,%rax)
  0x10099d5d0 <+48>: imull  $0x1f, %eax, %esi
  0x10099d5d3 <+51>: movzbl (%rdx), %eax
  0x10099d5d6 <+54>: addl   %esi, %eax
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5d0 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 48
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5d0 <+48>: imull  $0x1f, %eax, %esi
  0x10099d5d3 <+51>: movzbl (%rdx), %eax
  0x10099d5d6 <+54>: addl   %esi, %eax
  0x10099d5d8 <+56>: incq   %rdx
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5d3 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 51
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5d3 <+51>: movzbl (%rdx), %eax
  0x10099d5d6 <+54>: addl   %esi, %eax
  0x10099d5d8 <+56>: incq   %rdx
  0x10099d5db <+59>: decl   %ecx
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5d6 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 54
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5d6 <+54>: addl   %esi, %eax
  0x10099d5d8 <+56>: incq   %rdx
  0x10099d5db <+59>: decl   %ecx
  0x10099d5dd <+61>: jne   0x10099d5d0               ; <+48>
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5d8 Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 56
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5d8 <+56>: incq   %rdx
  0x10099d5db <+59>: decl   %ecx
  0x10099d5dd <+61>: jne   0x10099d5d0               ; <+48>
  0x10099d5df <+63>: movq   0x8(%rdi), %rcx
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5db Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 59
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5db <+59>: decl   %ecx
  0x10099d5dd <+61>: jne   0x10099d5d0               ; <+48>
  0x10099d5df <+63>: movq   0x8(%rdi), %rcx
  0x10099d5e3 <+67>: testq %rcx, %rcx
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5dd Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 61
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5dd <+61>: jne   0x10099d5d0               ; <+48>
  0x10099d5df <+63>: movq   0x8(%rdi), %rcx
  0x10099d5e3 <+67>: testq %rcx, %rcx
  0x10099d5e6 <+70>: je     0x10099d625               ; <+133>
Target 0: (Foxit Reader) stopped.
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = instruction step over
  frame #0: 0x000000010099d5df Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 63
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5df <+63>: movq   0x8(%rdi), %rcx
  0x10099d5e3 <+67>: testq %rcx, %rcx
  0x10099d5e6 <+70>: je     0x10099d625               ; <+133>
  0x10099d5e8 <+72>: xorl   %r15d, %r15d
Target 0: (Foxit Reader) stopped.
# crash!!!!!!!!!!!!!!!!!!!!!!!!!!!!
(lldb) n
Process 78117 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = EXC_BAD_ACCESS (code=1, address=0x28)
  frame #0: 0x000000010099d5df Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 63
Foxit Reader`CFX_MapByteStringToPtr::Lookup:
-> 0x10099d5df <+63>: movq   0x8(%rdi), %rcx
  0x10099d5e3 <+67>: testq %rcx, %rcx
  0x10099d5e6 <+70>: je     0x10099d625               ; <+133>
  0x10099d5e8 <+72>: xorl   %r15d, %r15d
Target 0: (Foxit Reader) stopped.
(lldb) register read/x
General Purpose Registers:
      rax = 0x0000000000000053
      rbx = 0x000000010024bd00 Foxit Reader`CGroupcontainer::qt_metacall(QMetaObject::Call, int, void**) + 48
      rcx = 0x0000000000000000
      rdx = 0x0000000101f021e5  ""
      rdi = 0x0000000000000020 # invalid address
      rsi = 0x0000000000000000
      rbp = 0x00007ffeefbfc3a0
      rsp = 0x00007ffeefbfc380
      r8 = 0x00000000000130a8
      r9 = 0x0000000000000000
      r10 = 0x000000012307e530
      r11 = 0x000000012307e528
      r12 = 0x00007ffeefbfc570
      r13 = 0x0000000109989388  
      r14 = 0x00007ffeefbfc3b8
      r15 = 0x0000000000000000
      rip = 0x000000010099d5df Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 63
  rflags = 0x0000000000010246
      cs = 0x000000000000002b
      fs = 0x0000000000000000
      gs = 0x0000000000000000
(lldb) bt
* thread #1, queue = 'com.apple.main-thread', stop reason = EXC_BAD_ACCESS (code=1, address=0x28)
* frame #0: 0x000000010099d5df Foxit Reader`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 63
  frame #1: 0x0000000100555de5 Foxit Reader`CPDF_Dictionary::KeyExist(CFX_ByteStringC const&) const + 21
  frame #2: 0x000000010009a0c3 Foxit Reader`CReader_DocumentEx::IsPageLabelOnlyHaveNumD() + 851
  frame #3: 0x000000010007b99c Foxit Reader`CReader_DocumentEx::InitDocument(QString, int) + 4684
  frame #4: 0x00000001000f02cb Foxit Reader`CPDF_OwnerFileTypeHandler::OpenContinueNormal(void*, QString) + 235
  frame #5: 0x00000001000f1b40 Foxit Reader`CPDF_OwnerFileTypeHandler::DoOpen(CFX_ByteString, CFX_WideString) + 1712
  frame #6: 0x0000000100011417 Foxit Reader`CMainWindow::OpenFoxitDocumentFile(QString, bool, bool, bool, QString, bool, bool) + 439
  frame #7: 0x00000001000139b3 Foxit Reader`CMainWindow::OpenFile(QString const&, int, int, int, int, QString const&) + 3187
  frame #8: 0x0000000100010dab Foxit Reader`CMainWindow::on_actOpen() + 427
  frame #9: 0x000000010024cdcd Foxit Reader`___lldb_unnamed_symbol1722$$Foxit Reader + 4173
  frame #10: 0x000000010969c1da QtCore`QMetaObject::activate(QObject*, int, int, void**) + 2954
  frame #11: 0x000000010a017d35 QtWidgets`QAction::activate(QAction::ActionEvent) + 309
  frame #12: 0x0000000109694454 QtCore`QObject::event(QEvent*) + 788
  frame #13: 0x000000010a017bea QtWidgets`QAction::event(QEvent*) + 234
  frame #14: 0x000000010a021752 QtWidgets`QApplicationPrivate::notify_helper(QObject*, QEvent*) + 306
  frame #15: 0x000000010a022a6f QtWidgets`QApplication::notify(QObject*, QEvent*) + 383
  frame #16: 0x00000001002596b4 Foxit Reader`___lldb_unnamed_symbol1885$$Foxit Reader + 180
  frame #17: 0x000000010966af7f QtCore`QCoreApplication::notifyInternal2(QObject*, QEvent*) + 159
  frame #18: 0x000000010966c152 QtCore`QCoreApplicationPrivate::sendPostedEvents(QObject*, int, QThreadData*) + 850
  frame #19: 0x000000010c5ace0e libqcocoa.dylib`___lldb_unnamed_symbol552$$libqcocoa.dylib + 190
  frame #20: 0x000000010c5ad6d1 libqcocoa.dylib`___lldb_unnamed_symbol564$$libqcocoa.dylib + 33
  frame #21: 0x00007fff301fa395 CoreFoundation`__CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__ + 17
  frame #22: 0x00007fff301fa33b CoreFoundation`__CFRunLoopDoSource0 + 108
  frame #23: 0x00007fff301dddd1 CoreFoundation`__CFRunLoopDoSources0 + 195
  frame #24: 0x00007fff301dd37a CoreFoundation`__CFRunLoopRun + 1219
  frame #25: 0x00007fff301dcc64 CoreFoundation`CFRunLoopRunSpecific + 463
  frame #26: 0x00007fff2f473ab5 HIToolbox`RunCurrentEventLoopInMode + 293
  frame #27: 0x00007fff2f4737eb HIToolbox`ReceiveNextEventCommon + 618
  frame #28: 0x00007fff2f473568 HIToolbox`_BlockUntilNextEventMatchingListInModeWithFilter + 64
  frame #29: 0x00007fff2d72e363 AppKit`_DPSNextEvent + 997
  frame #30: 0x00007fff2d72d102 AppKit`-[NSApplication(NSEvent) _nextEventMatchingEventMask:untilDate:inMode:dequeue:] + 1362
  frame #31: 0x00007fff2d727165 AppKit`-[NSApplication run] + 699
  frame #32: 0x000000010c5ac58a libqcocoa.dylib`___lldb_unnamed_symbol546$$libqcocoa.dylib + 2186
  frame #33: 0x0000000109666a72 QtCore`QEventLoop::exec(QFlags<QEventLoop::ProcessEventsFlag>) + 418
  frame #34: 0x000000010966b692 QtCore`QCoreApplication::exec() + 402
  frame #35: 0x000000010000af94 Foxit Reader`main + 5940
  frame #36: 0x00007fff5d42fed9 libdyld.dylib`start + 1
```

Foxit PhantomPDF *Mac*'s crash stack backtrace.

```
(lldb) bt
* thread #1, queue = 'com.apple.main-thread', stop reason = EXC_BAD_ACCESS (code=1, address=0x28)
* frame #0: 0x000000010099fcaf Foxit PhantomPDF`CFX_MapByteStringToPtr::Lookup(CFX_ByteStringC const&, void*&) const + 63
  frame #1: 0x0000000100557f05 Foxit PhantomPDF`CPDF_Dictionary::KeyExist(CFX_ByteStringC const&) const + 21
  frame #2: 0x000000010009c6f3 Foxit PhantomPDF`CReader_DocumentEx::IsPageLabelOnlyHaveNumD() + 851
  frame #3: 0x000000010007dd2c Foxit PhantomPDF`CReader_DocumentEx::InitDocument(QString, int) + 4684
  frame #4: 0x00000001000f20cb Foxit PhantomPDF`CPDF_OwnerFileTypeHandler::OpenContinueNormal(void*, QString) + 235
  frame #5: 0x00000001000f3940 Foxit PhantomPDF`CPDF_OwnerFileTypeHandler::DoOpen(CFX_ByteString, CFX_WideString) + 1712
  frame #6: 0x0000000100011937 Foxit PhantomPDF`CMainWindow::OpenFoxitDocumentFile(QString, bool, bool, bool, QString, bool, bool) + 439
  frame #7: 0x0000000100013ed3 Foxit PhantomPDF`CMainWindow::OpenFile(QString const&, int, int, int, int, QString const&) + 3187
  frame #8: 0x00000001000112cb Foxit PhantomPDF`CMainWindow::on_actOpen() + 427
  frame #9: 0x000000010024efad Foxit PhantomPDF`___lldb_unnamed_symbol1734$$Foxit PhantomPDF + 4173
  frame #10: 0x000000010c02c1da QtCore`QMetaObject::activate(QObject*, int, int, void**) + 2954
  frame #11: 0x000000010c9a7d35 QtWidgets`QAction::activate(QAction::ActionEvent) + 309
  frame #12: 0x000000010c024454 QtCore`QObject::event(QEvent*) + 788
  frame #13: 0x000000010c9a7bea QtWidgets`QAction::event(QEvent*) + 234
  frame #14: 0x000000010c9b1752 QtWidgets`QApplicationPrivate::notify_helper(QObject*, QEvent*) + 306
  frame #15: 0x000000010c9b2a6f QtWidgets`QApplication::notify(QObject*, QEvent*) + 383
  frame #16: 0x000000010025b894 Foxit PhantomPDF`___lldb_unnamed_symbol1897$$Foxit PhantomPDF + 180
  frame #17: 0x000000010bffaf7f QtCore`QCoreApplication::notifyInternal2(QObject*, QEvent*) + 159
  frame #18: 0x000000010bffc152 QtCore`QCoreApplicationPrivate::sendPostedEvents(QObject*, int, QThreadData*) + 850
  frame #19: 0x0000000113c0fe0e libqcocoa.dylib`___lldb_unnamed_symbol552$$libqcocoa.dylib + 190
  frame #20: 0x0000000113c106d1 libqcocoa.dylib`___lldb_unnamed_symbol564$$libqcocoa.dylib + 33
  frame #21: 0x00007fff301fa395 CoreFoundation`__CFRUNLOOP_IS_CALLING_OUT_TO_A_SOURCE0_PERFORM_FUNCTION__ + 17
  frame #22: 0x00007fff301fa33b CoreFoundation`__CFRunLoopDoSource0 + 108
  frame #23: 0x00007fff301dde29 CoreFoundation`__CFRunLoopDoSources0 + 283
  frame #24: 0x00007fff301dd37a CoreFoundation`__CFRunLoopRun + 1219
  frame #25: 0x00007fff301dcc64 CoreFoundation`CFRunLoopRunSpecific + 463
  frame #26: 0x00007fff2f473ab5 HIToolbox`RunCurrentEventLoopInMode + 293
  frame #27: 0x00007fff2f4736f4 HIToolbox`ReceiveNextEventCommon + 371
  frame #28: 0x00007fff2f473568 HIToolbox`_BlockUntilNextEventMatchingListInModeWithFilter + 64
  frame #29: 0x00007fff2d72e363 AppKit`_DPSNextEvent + 997
  frame #30: 0x00007fff2d72d102 AppKit`-[NSApplication(NSEvent) _nextEventMatchingEventMask:untilDate:inMode:dequeue:] + 1362
  frame #31: 0x00007fff2d727165 AppKit`-[NSApplication run] + 699
  frame #32: 0x0000000113c0f58a libqcocoa.dylib`___lldb_unnamed_symbol546$$libqcocoa.dylib + 2186
  frame #33: 0x000000010bff6a72 QtCore`QEventLoop::exec(QFlags<QEventLoop::ProcessEventsFlag>) + 418
  frame #34: 0x000000010bffb692 QtCore`QCoreApplication::exec() + 402
  frame #35: 0x000000010000b284 Foxit PhantomPDF`main + 5940
  frame #36: 0x00007fff5d42fed9 libdyld.dylib`start + 1
  frame #37: 0x00007fff5d42fed9 libdyld.dylib`start + 1
```