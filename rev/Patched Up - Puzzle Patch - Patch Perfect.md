so the reason i put all these 3 into a same write up is because their solution is kind of the same
to check the key for the flag, memcmp is called to compare 2 strings

memcmp in c will translate into:
```
[thingsnobodycareabout]
test eax,eax
jne [someadress]
[thingsnobodycareabout]
```
and jne structure in asm is
```
75 [jumpaddress]
```
in comparison, je is
```
74 [jumpaddress]
```
so by changing the first byte from 75 to 74, the problems are basically solved

patch perfect actually differ from the others a little bit because it also check the intergrity of the file, but its actually just another memcmp so we can just do the same thing

example:
```
hacker@reverse-engineering~patch-perfect-easy:/challenge$ gdb *
GNU gdb (GDB) 16.3
Copyright (C) 2024 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-unknown-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<https://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
/home/hacker/.gdbinit:1: Error in sourced command file:
No symbol table is loaded.  Use the "file" command.
Reading symbols from patch-perfect-easy...
(No debugging symbols found in patch-perfect-easy)
(gdb) disas main
Dump of assembler code for function main:
   0x00000000000016e1 <+0>:     endbr64
   0x00000000000016e5 <+4>:     push   %rbp
   0x00000000000016e6 <+5>:     mov    %rsp,%rbp
   0x00000000000016e9 <+8>:     sub    $0x120,%rsp
   0x00000000000016f0 <+15>:    mov    %edi,-0x104(%rbp)
   0x00000000000016f6 <+21>:    mov    %rsi,-0x110(%rbp)
   0x00000000000016fd <+28>:    mov    %rdx,-0x118(%rbp)
   0x0000000000001704 <+35>:    mov    %fs:0x28,%rax
   0x000000000000170d <+44>:    mov    %rax,-0x8(%rbp)
   0x0000000000001711 <+48>:    xor    %eax,%eax
   0x0000000000001713 <+50>:    mov    0x2926(%rip),%rax        # 0x4040 <stdin@@GLIBC_2.2.5>
   0x000000000000171a <+57>:    mov    $0x0,%ecx
   0x000000000000171f <+62>:    mov    $0x2,%edx
   0x0000000000001724 <+67>:    mov    $0x0,%esi
   0x0000000000001729 <+72>:    mov    %rax,%rdi
   0x000000000000172c <+75>:    call   0x11b0 <setvbuf@plt>
   0x0000000000001731 <+80>:    mov    0x2910(%rip),%rax        # 0x4048 <stdout@@GLIBC_2.2.5>
   0x0000000000001738 <+87>:    mov    $0x0,%ecx
   0x000000000000173d <+92>:    mov    $0x2,%edx
   0x0000000000001742 <+97>:    mov    $0x0,%esi
   0x0000000000001747 <+102>:   mov    %rax,%rdi
   0x000000000000174a <+105>:   call   0x11b0 <setvbuf@plt>
   0x000000000000174f <+110>:   lea    0x9b6(%rip),%rdi        # 0x210c
   0x0000000000001756 <+117>:   call   0x1190 <puts@plt>
   0x000000000000175b <+122>:   mov    -0x110(%rbp),%rax
   0x0000000000001762 <+129>:   mov    (%rax),%rax
   0x0000000000001765 <+132>:   mov    %rax,%rsi
   0x0000000000001768 <+135>:   lea    0x9a1(%rip),%rdi        # 0x2110
   0x000000000000176f <+142>:   mov    $0x0,%eax
   0x0000000000001774 <+147>:   call   0x1170 <printf@plt>
   0x0000000000001779 <+152>:   lea    0x98c(%rip),%rdi        # 0x210c
   0x0000000000001780 <+159>:   call   0x1190 <puts@plt>
   0x0000000000001785 <+164>:   mov    $0xa,%edi
   0x000000000000178a <+169>:   call   0x11c0 <putchar@plt>
   0x000000000000178f <+174>:   lea    0x992(%rip),%rdi        # 0x2128
--Type <RET> for more, q to quit, c to continue without paging--c
   0x0000000000001796 <+181>:   call   0x1190 <puts@plt>
   0x000000000000179b <+186>:   lea    0x9fe(%rip),%rdi        # 0x21a0
   0x00000000000017a2 <+193>:   call   0x1190 <puts@plt>
   0x00000000000017a7 <+198>:   lea    0xa6a(%rip),%rdi        # 0x2218
   0x00000000000017ae <+205>:   call   0x1190 <puts@plt>
   0x00000000000017b3 <+210>:   lea    0xad6(%rip),%rdi        # 0x2290
   0x00000000000017ba <+217>:   call   0x1190 <puts@plt>
   0x00000000000017bf <+222>:   lea    0xb0a(%rip),%rdi        # 0x22d0
   0x00000000000017c6 <+229>:   call   0x1190 <puts@plt>
   0x00000000000017cb <+234>:   movl   $0x0,-0xf0(%rbp)
   0x00000000000017d5 <+244>:   lea    -0x473(%rip),%rax        # 0x1369 <bin_padding>
   0x00000000000017dc <+251>:   and    $0xfffffffffffff000,%rax
   0x00000000000017e2 <+257>:   sub    $0x1000,%rax
   0x00000000000017e8 <+263>:   mov    %rax,-0xc8(%rbp)
   0x00000000000017ef <+270>:   nop
   0x00000000000017f0 <+271>:   mov    -0xf0(%rbp),%eax
   0x00000000000017f6 <+277>:   lea    0x1(%rax),%edx
   0x00000000000017f9 <+280>:   mov    %edx,-0xf0(%rbp)
   0x00000000000017ff <+286>:   shl    $0xc,%eax
   0x0000000000001802 <+289>:   movslq %eax,%rdx
   0x0000000000001805 <+292>:   mov    -0xc8(%rbp),%rax
   0x000000000000180c <+299>:   add    %rdx,%rax
   0x000000000000180f <+302>:   mov    $0x7,%edx
   0x0000000000001814 <+307>:   mov    $0x1000,%esi
   0x0000000000001819 <+312>:   mov    %rax,%rdi
   0x000000000000181c <+315>:   call   0x1160 <mprotect@plt>
   0x0000000000001821 <+320>:   test   %eax,%eax
   0x0000000000001823 <+322>:   je     0x17f0 <main+271>
   0x0000000000001825 <+324>:   lea    0xb04(%rip),%rdi        # 0x2330
   0x000000000000182c <+331>:   call   0x1190 <puts@plt>
   0x0000000000001831 <+336>:   lea    -0xc0(%rbp),%rax
   0x0000000000001838 <+343>:   mov    %rax,%rdi
   0x000000000000183b <+346>:   call   0x1230 <MD5_Init@plt>
   0x0000000000001840 <+351>:   movl   $0x0,-0xec(%rbp)
   0x000000000000184a <+361>:   jmp    0x1881 <main+416>
   0x000000000000184c <+363>:   mov    -0xec(%rbp),%eax
   0x0000000000001852 <+369>:   shl    $0xc,%eax
   0x0000000000001855 <+372>:   movslq %eax,%rdx
   0x0000000000001858 <+375>:   mov    -0xc8(%rbp),%rax
   0x000000000000185f <+382>:   lea    (%rdx,%rax,1),%rcx
   0x0000000000001863 <+386>:   lea    -0xc0(%rbp),%rax
   0x000000000000186a <+393>:   mov    $0x1000,%edx
   0x000000000000186f <+398>:   mov    %rcx,%rsi
   0x0000000000001872 <+401>:   mov    %rax,%rdi
   0x0000000000001875 <+404>:   call   0x1200 <MD5_Update@plt>
   0x000000000000187a <+409>:   addl   $0x1,-0xec(%rbp)
   0x0000000000001881 <+416>:   mov    -0xf0(%rbp),%eax
   0x0000000000001887 <+422>:   sub    $0x1,%eax
   0x000000000000188a <+425>:   cmp    %eax,-0xec(%rbp)
   0x0000000000001890 <+431>:   jl     0x184c <main+363>
   0x0000000000001892 <+433>:   lea    -0xc0(%rbp),%rdx
   0x0000000000001899 <+440>:   lea    -0x60(%rbp),%rax
   0x000000000000189d <+444>:   mov    %rdx,%rsi
   0x00000000000018a0 <+447>:   mov    %rax,%rdi
   0x00000000000018a3 <+450>:   call   0x11f0 <MD5_Final@plt>
   0x00000000000018a8 <+455>:   lea    0xad1(%rip),%rdi        # 0x2380
   0x00000000000018af <+462>:   call   0x1190 <puts@plt>
   0x00000000000018b4 <+467>:   mov    $0x9,%edi
   0x00000000000018b9 <+472>:   call   0x11c0 <putchar@plt>
   0x00000000000018be <+477>:   movl   $0x0,-0xe8(%rbp)
   0x00000000000018c8 <+487>:   jmp    0x18f4 <main+531>
   0x00000000000018ca <+489>:   mov    -0xe8(%rbp),%eax
   0x00000000000018d0 <+495>:   cltq
   0x00000000000018d2 <+497>:   movzbl -0x60(%rbp,%rax,1),%eax
   0x00000000000018d7 <+502>:   movzbl %al,%eax
   0x00000000000018da <+505>:   mov    %eax,%esi
   0x00000000000018dc <+507>:   lea    0xac4(%rip),%rdi        # 0x23a7
   0x00000000000018e3 <+514>:   mov    $0x0,%eax
   0x00000000000018e8 <+519>:   call   0x1170 <printf@plt>
   0x00000000000018ed <+524>:   addl   $0x1,-0xe8(%rbp)
   0x00000000000018f4 <+531>:   cmpl   $0x1c,-0xe8(%rbp)
   0x00000000000018fb <+538>:   jle    0x18ca <main+489>
   0x00000000000018fd <+540>:   lea    0x806(%rip),%rdi        # 0x210a
   0x0000000000001904 <+547>:   call   0x1190 <puts@plt>
   0x0000000000001909 <+552>:   movl   $0x0,-0xe4(%rbp)
   0x0000000000001913 <+562>:   jmp    0x19e6 <main+773>
   0x0000000000001918 <+567>:   mov    -0xe4(%rbp),%eax
   0x000000000000191e <+573>:   add    $0x1,%eax
   0x0000000000001921 <+576>:   mov    %eax,%esi
   0x0000000000001923 <+578>:   lea    0xa83(%rip),%rdi        # 0x23ad
   0x000000000000192a <+585>:   mov    $0x0,%eax
   0x000000000000192f <+590>:   call   0x1170 <printf@plt>
   0x0000000000001934 <+595>:   lea    0xa87(%rip),%rdi        # 0x23c2
   0x000000000000193b <+602>:   mov    $0x0,%eax
   0x0000000000001940 <+607>:   call   0x1170 <printf@plt>
   0x0000000000001945 <+612>:   lea    -0xf2(%rbp),%rax
   0x000000000000194c <+619>:   mov    %rax,%rsi
   0x000000000000194f <+622>:   lea    0xa85(%rip),%rdi        # 0x23db
   0x0000000000001956 <+629>:   mov    $0x0,%eax
   0x000000000000195b <+634>:   call   0x1240 <__isoc99_scanf@plt>
   0x0000000000001960 <+639>:   lea    0xa78(%rip),%rdi        # 0x23df
   0x0000000000001967 <+646>:   mov    $0x0,%eax
   0x000000000000196c <+651>:   call   0x1170 <printf@plt>
   0x0000000000001971 <+656>:   lea    -0xf3(%rbp),%rax
   0x0000000000001978 <+663>:   mov    %rax,%rsi
   0x000000000000197b <+666>:   lea    0xa6f(%rip),%rdi        # 0x23f1
   0x0000000000001982 <+673>:   mov    $0x0,%eax
   0x0000000000001987 <+678>:   call   0x1240 <__isoc99_scanf@plt>
   0x000000000000198c <+683>:   movzbl -0xf3(%rbp),%ecx
   0x0000000000001993 <+690>:   movzwl -0xf2(%rbp),%eax
   0x000000000000199a <+697>:   movzwl %ax,%edx
   0x000000000000199d <+700>:   mov    -0xc8(%rbp),%rax
   0x00000000000019a4 <+707>:   add    %rdx,%rax
   0x00000000000019a7 <+710>:   mov    %ecx,%edx
   0x00000000000019a9 <+712>:   mov    %dl,(%rax)
   0x00000000000019ab <+714>:   movzbl -0xf3(%rbp),%eax
   0x00000000000019b2 <+721>:   movzbl %al,%eax
   0x00000000000019b5 <+724>:   movzwl -0xf2(%rbp),%edx
   0x00000000000019bc <+731>:   movzwl %dx,%ecx
   0x00000000000019bf <+734>:   mov    -0xc8(%rbp),%rdx
   0x00000000000019c6 <+741>:   add    %rdx,%rcx
   0x00000000000019c9 <+744>:   mov    %eax,%edx
   0x00000000000019cb <+746>:   mov    %rcx,%rsi
   0x00000000000019ce <+749>:   lea    0xa23(%rip),%rdi        # 0x23f8
   0x00000000000019d5 <+756>:   mov    $0x0,%eax
   0x00000000000019da <+761>:   call   0x1170 <printf@plt>
   0x00000000000019df <+766>:   addl   $0x1,-0xe4(%rbp)
   0x00000000000019e6 <+773>:   cmpl   $0x1,-0xe4(%rbp)
   0x00000000000019ed <+780>:   jle    0x1918 <main+567>
   0x00000000000019f3 <+786>:   lea    -0xc0(%rbp),%rax
   0x00000000000019fa <+793>:   mov    %rax,%rdi
   0x00000000000019fd <+796>:   call   0x1230 <MD5_Init@plt>
   0x0000000000001a02 <+801>:   movl   $0x0,-0xe0(%rbp)
   0x0000000000001a0c <+811>:   jmp    0x1a43 <main+866>
   0x0000000000001a0e <+813>:   mov    -0xe0(%rbp),%eax
   0x0000000000001a14 <+819>:   shl    $0xc,%eax
   0x0000000000001a17 <+822>:   movslq %eax,%rdx
   0x0000000000001a1a <+825>:   mov    -0xc8(%rbp),%rax
   0x0000000000001a21 <+832>:   lea    (%rdx,%rax,1),%rcx
   0x0000000000001a25 <+836>:   lea    -0xc0(%rbp),%rax
   0x0000000000001a2c <+843>:   mov    $0x1000,%edx
   0x0000000000001a31 <+848>:   mov    %rcx,%rsi
   0x0000000000001a34 <+851>:   mov    %rax,%rdi
   0x0000000000001a37 <+854>:   call   0x1200 <MD5_Update@plt>
   0x0000000000001a3c <+859>:   addl   $0x1,-0xe0(%rbp)
   0x0000000000001a43 <+866>:   mov    -0xf0(%rbp),%eax
   0x0000000000001a49 <+872>:   sub    $0x1,%eax
   0x0000000000001a4c <+875>:   cmp    %eax,-0xe0(%rbp)
   0x0000000000001a52 <+881>:   jl     0x1a0e <main+813>
   0x0000000000001a54 <+883>:   lea    -0xc0(%rbp),%rdx
   0x0000000000001a5b <+890>:   lea    -0x50(%rbp),%rax
   0x0000000000001a5f <+894>:   mov    %rdx,%rsi
   0x0000000000001a62 <+897>:   mov    %rax,%rdi
   0x0000000000001a65 <+900>:   call   0x11f0 <MD5_Final@plt>
   0x0000000000001a6a <+905>:   lea    0x9af(%rip),%rdi        # 0x2420
   0x0000000000001a71 <+912>:   call   0x1190 <puts@plt>
   0x0000000000001a76 <+917>:   mov    $0x9,%edi
   0x0000000000001a7b <+922>:   call   0x11c0 <putchar@plt>
   0x0000000000001a80 <+927>:   movl   $0x0,-0xdc(%rbp)
   0x0000000000001a8a <+937>:   jmp    0x1ab6 <main+981>
   0x0000000000001a8c <+939>:   mov    -0xdc(%rbp),%eax
   0x0000000000001a92 <+945>:   cltq
   0x0000000000001a94 <+947>:   movzbl -0x50(%rbp,%rax,1),%eax
   0x0000000000001a99 <+952>:   movzbl %al,%eax
   0x0000000000001a9c <+955>:   mov    %eax,%esi
   0x0000000000001a9e <+957>:   lea    0x902(%rip),%rdi        # 0x23a7
   0x0000000000001aa5 <+964>:   mov    $0x0,%eax
   0x0000000000001aaa <+969>:   call   0x1170 <printf@plt>
   0x0000000000001aaf <+974>:   addl   $0x1,-0xdc(%rbp)
   0x0000000000001ab6 <+981>:   cmpl   $0x1c,-0xdc(%rbp)
   0x0000000000001abd <+988>:   jle    0x1a8c <main+939>
   0x0000000000001abf <+990>:   lea    0x644(%rip),%rdi        # 0x210a
   0x0000000000001ac6 <+997>:   call   0x1190 <puts@plt>
   0x0000000000001acb <+1002>:  lea    -0x50(%rbp),%rcx
   0x0000000000001acf <+1006>:  lea    -0x60(%rbp),%rax
   0x0000000000001ad3 <+1010>:  mov    $0x10,%edx
   0x0000000000001ad8 <+1015>:  mov    %rcx,%rsi
   0x0000000000001adb <+1018>:  mov    %rax,%rdi
   0x0000000000001ade <+1021>:  call   0x1250 <memcmp@plt>
   0x0000000000001ae3 <+1026>:  test   %eax,%eax
   0x0000000000001ae5 <+1028>:  jne    0x1b5c <main+1147>
   0x0000000000001ae7 <+1030>:  lea    0x95a(%rip),%rdi        # 0x2448
   0x0000000000001aee <+1037>:  call   0x1190 <puts@plt>
   0x0000000000001af3 <+1042>:  movq   $0x0,-0x30(%rbp)
   0x0000000000001afb <+1050>:  movq   $0x0,-0x28(%rbp)
   0x0000000000001b03 <+1058>:  movq   $0x0,-0x20(%rbp)
   0x0000000000001b0b <+1066>:  movl   $0x0,-0x18(%rbp)
   0x0000000000001b12 <+1073>:  movw   $0x0,-0x14(%rbp)
   0x0000000000001b18 <+1079>:  lea    0x989(%rip),%rdi        # 0x24a8
   0x0000000000001b1f <+1086>:  call   0x1190 <puts@plt>
   0x0000000000001b24 <+1091>:  lea    -0x30(%rbp),%rax
   0x0000000000001b28 <+1095>:  mov    $0x1d,%edx
   0x0000000000001b2d <+1100>:  mov    %rax,%rsi
   0x0000000000001b30 <+1103>:  mov    $0x0,%edi
   0x0000000000001b35 <+1108>:  call   0x11d0 <read@plt>
   0x0000000000001b3a <+1113>:  lea    0x98b(%rip),%rdi        # 0x24cc
   0x0000000000001b41 <+1120>:  call   0x1190 <puts@plt>
   0x0000000000001b46 <+1125>:  mov    $0x9,%edi
   0x0000000000001b4b <+1130>:  call   0x11c0 <putchar@plt>
   0x0000000000001b50 <+1135>:  movl   $0x0,-0xd8(%rbp)
   0x0000000000001b5a <+1145>:  jmp    0x1b9c <main+1211>
   0x0000000000001b5c <+1147>:  lea    0x90d(%rip),%rdi        # 0x2470
   0x0000000000001b63 <+1154>:  call   0x1190 <puts@plt>
   0x0000000000001b68 <+1159>:  mov    $0x1,%edi
   0x0000000000001b6d <+1164>:  call   0x11a0 <exit@plt>
   0x0000000000001b72 <+1169>:  mov    -0xd8(%rbp),%eax
   0x0000000000001b78 <+1175>:  cltq
   0x0000000000001b7a <+1177>:  movzbl -0x30(%rbp,%rax,1),%eax
   0x0000000000001b7f <+1182>:  movzbl %al,%eax
   0x0000000000001b82 <+1185>:  mov    %eax,%esi
   0x0000000000001b84 <+1187>:  lea    0x81c(%rip),%rdi        # 0x23a7
   0x0000000000001b8b <+1194>:  mov    $0x0,%eax
   0x0000000000001b90 <+1199>:  call   0x1170 <printf@plt>
   0x0000000000001b95 <+1204>:  addl   $0x1,-0xd8(%rbp)
   0x0000000000001b9c <+1211>:  cmpl   $0x1c,-0xd8(%rbp)
   0x0000000000001ba3 <+1218>:  jle    0x1b72 <main+1169>
   0x0000000000001ba5 <+1220>:  lea    0x55e(%rip),%rdi        # 0x210a
   0x0000000000001bac <+1227>:  call   0x1190 <puts@plt>
   0x0000000000001bb1 <+1232>:  lea    0x928(%rip),%rdi        # 0x24e0
   0x0000000000001bb8 <+1239>:  call   0x1190 <puts@plt>
   0x0000000000001bbd <+1244>:  lea    -0xc0(%rbp),%rax
   0x0000000000001bc4 <+1251>:  mov    %rax,%rdi
   0x0000000000001bc7 <+1254>:  call   0x1230 <MD5_Init@plt>
   0x0000000000001bcc <+1259>:  lea    -0x30(%rbp),%rcx
   0x0000000000001bd0 <+1263>:  lea    -0xc0(%rbp),%rax
   0x0000000000001bd7 <+1270>:  mov    $0x1d,%edx
   0x0000000000001bdc <+1275>:  mov    %rcx,%rsi
   0x0000000000001bdf <+1278>:  mov    %rax,%rdi
   0x0000000000001be2 <+1281>:  call   0x1200 <MD5_Update@plt>
   0x0000000000001be7 <+1286>:  lea    -0xc0(%rbp),%rdx
   0x0000000000001bee <+1293>:  lea    -0x40(%rbp),%rax
   0x0000000000001bf2 <+1297>:  mov    %rdx,%rsi
   0x0000000000001bf5 <+1300>:  mov    %rax,%rdi
   0x0000000000001bf8 <+1303>:  call   0x11f0 <MD5_Final@plt>
   0x0000000000001bfd <+1308>:  lea    -0x30(%rbp),%rax
   0x0000000000001c01 <+1312>:  mov    $0x1d,%edx
   0x0000000000001c06 <+1317>:  mov    $0x0,%esi
   0x0000000000001c0b <+1322>:  mov    %rax,%rdi
   0x0000000000001c0e <+1325>:  call   0x1180 <memset@plt>
   0x0000000000001c13 <+1330>:  mov    -0x40(%rbp),%rax
   0x0000000000001c17 <+1334>:  mov    -0x38(%rbp),%rdx
   0x0000000000001c1b <+1338>:  mov    %rax,-0x30(%rbp)
   0x0000000000001c1f <+1342>:  mov    %rdx,-0x28(%rbp)
   0x0000000000001c23 <+1346>:  lea    0x91e(%rip),%rdi        # 0x2548
   0x0000000000001c2a <+1353>:  call   0x1190 <puts@plt>
   0x0000000000001c2f <+1358>:  mov    $0x9,%edi
   0x0000000000001c34 <+1363>:  call   0x11c0 <putchar@plt>
   0x0000000000001c39 <+1368>:  movl   $0x0,-0xd4(%rbp)
   0x0000000000001c43 <+1378>:  jmp    0x1c6f <main+1422>
   0x0000000000001c45 <+1380>:  mov    -0xd4(%rbp),%eax
   0x0000000000001c4b <+1386>:  cltq
   0x0000000000001c4d <+1388>:  movzbl -0x30(%rbp,%rax,1),%eax
   0x0000000000001c52 <+1393>:  movzbl %al,%eax
   0x0000000000001c55 <+1396>:  mov    %eax,%esi
   0x0000000000001c57 <+1398>:  lea    0x749(%rip),%rdi        # 0x23a7
   0x0000000000001c5e <+1405>:  mov    $0x0,%eax
   0x0000000000001c63 <+1410>:  call   0x1170 <printf@plt>
   0x0000000000001c68 <+1415>:  addl   $0x1,-0xd4(%rbp)
   0x0000000000001c6f <+1422>:  cmpl   $0x1c,-0xd4(%rbp)
   0x0000000000001c76 <+1429>:  jle    0x1c45 <main+1380>
   0x0000000000001c78 <+1431>:  lea    0x48b(%rip),%rdi        # 0x210a
   0x0000000000001c7f <+1438>:  call   0x1190 <puts@plt>
   0x0000000000001c84 <+1443>:  lea    0x8e5(%rip),%rdi        # 0x2570
   0x0000000000001c8b <+1450>:  call   0x1190 <puts@plt>
   0x0000000000001c90 <+1455>:  lea    0x931(%rip),%rdi        # 0x25c8
   0x0000000000001c97 <+1462>:  call   0x1190 <puts@plt>
   0x0000000000001c9c <+1467>:  mov    $0x9,%edi
   0x0000000000001ca1 <+1472>:  call   0x11c0 <putchar@plt>
   0x0000000000001ca6 <+1477>:  movl   $0x0,-0xd0(%rbp)
   0x0000000000001cb0 <+1487>:  jmp    0x1cdc <main+1531>
   0x0000000000001cb2 <+1489>:  mov    -0xd0(%rbp),%eax
   0x0000000000001cb8 <+1495>:  cltq
   0x0000000000001cba <+1497>:  movzbl -0x30(%rbp,%rax,1),%eax
   0x0000000000001cbf <+1502>:  movzbl %al,%eax
   0x0000000000001cc2 <+1505>:  mov    %eax,%esi
   0x0000000000001cc4 <+1507>:  lea    0x6dc(%rip),%rdi        # 0x23a7
   0x0000000000001ccb <+1514>:  mov    $0x0,%eax
   0x0000000000001cd0 <+1519>:  call   0x1170 <printf@plt>
   0x0000000000001cd5 <+1524>:  addl   $0x1,-0xd0(%rbp)
   0x0000000000001cdc <+1531>:  cmpl   $0x1c,-0xd0(%rbp)
   0x0000000000001ce3 <+1538>:  jle    0x1cb2 <main+1489>
   0x0000000000001ce5 <+1540>:  lea    0x41e(%rip),%rdi        # 0x210a
   0x0000000000001cec <+1547>:  call   0x1190 <puts@plt>
   0x0000000000001cf1 <+1552>:  lea    0x8f1(%rip),%rdi        # 0x25e9
   0x0000000000001cf8 <+1559>:  call   0x1190 <puts@plt>
   0x0000000000001cfd <+1564>:  mov    $0x9,%edi
   0x0000000000001d02 <+1569>:  call   0x11c0 <putchar@plt>
   0x0000000000001d07 <+1574>:  movl   $0x0,-0xcc(%rbp)
   0x0000000000001d11 <+1584>:  jmp    0x1d43 <main+1634>
   0x0000000000001d13 <+1586>:  mov    -0xcc(%rbp),%eax
   0x0000000000001d19 <+1592>:  cltq
   0x0000000000001d1b <+1594>:  lea    0x22ee(%rip),%rdx        # 0x4010 <EXPECTED_RESULT>
   0x0000000000001d22 <+1601>:  movzbl (%rax,%rdx,1),%eax
   0x0000000000001d26 <+1605>:  movzbl %al,%eax
   0x0000000000001d29 <+1608>:  mov    %eax,%esi
   0x0000000000001d2b <+1610>:  lea    0x675(%rip),%rdi        # 0x23a7
   0x0000000000001d32 <+1617>:  mov    $0x0,%eax
   0x0000000000001d37 <+1622>:  call   0x1170 <printf@plt>
   0x0000000000001d3c <+1627>:  addl   $0x1,-0xcc(%rbp)
   0x0000000000001d43 <+1634>:  cmpl   $0x1c,-0xcc(%rbp)
   0x0000000000001d4a <+1641>:  jle    0x1d13 <main+1586>
   0x0000000000001d4c <+1643>:  lea    0x3b7(%rip),%rdi        # 0x210a
   0x0000000000001d53 <+1650>:  call   0x1190 <puts@plt>
   0x0000000000001d58 <+1655>:  lea    0x8a1(%rip),%rdi        # 0x2600
   0x0000000000001d5f <+1662>:  call   0x1190 <puts@plt>
   0x0000000000001d64 <+1667>:  lea    -0x30(%rbp),%rax
   0x0000000000001d68 <+1671>:  mov    $0x1d,%edx
   0x0000000000001d6d <+1676>:  lea    0x229c(%rip),%rsi        # 0x4010 <EXPECTED_RESULT>
   0x0000000000001d74 <+1683>:  mov    %rax,%rdi
   0x0000000000001d77 <+1686>:  call   0x1250 <memcmp@plt>
   0x0000000000001d7c <+1691>:  test   %eax,%eax
   0x0000000000001d7e <+1693>:  jne    0x1d94 <main+1715>
   0x0000000000001d80 <+1695>:  mov    $0x0,%eax
   0x0000000000001d85 <+1700>:  call   0x15da <win>
   0x0000000000001d8a <+1705>:  mov    $0x0,%edi
   0x0000000000001d8f <+1710>:  call   0x11a0 <exit@plt>
   0x0000000000001d94 <+1715>:  lea    0x889(%rip),%rdi        # 0x2624
   0x0000000000001d9b <+1722>:  call   0x1190 <puts@plt>
   0x0000000000001da0 <+1727>:  mov    $0x1,%edi
   0x0000000000001da5 <+1732>:  call   0x11a0 <exit@plt>
End of assembler dump.
(gdb) q
hacker@reverse-engineering~patch-perfect-easy:/challenge$ ./*
###
### Welcome to ./patch-perfect-easy!
###

This license verifier software will allow you to read the flag. However, before you can do so, you must verify that you
are licensed to read flag files! This program consumes a license key over stdin. Each program may perform entirely
different operations on that input! You must figure out (by reverse engineering this program) what that license key is.
Providing the correct license key will net you the flag!

Unfortunately for you, the license key cannot be reversed. You'll have to crack this program.

In order to ensure code integrity, the code will be hashed and verified.

The pre-crack code integrity hash is:

        ec 83 b8 06 f0 24 e1 4e 8e 8f ac 85 ad c5 9e 83 c2 00 00 00 00 00 00 00 f7 36 13 39 fc 

Changing byte 1/2.
Offset (hex) to change: 1d7e
New value (hex): 74
The byte has been changed: *0x589764f8fd7e = 74.
Changing byte 2/2.
Offset (hex) to change: 1ae5
New value (hex): 74
The byte has been changed: *0x589764f8fae5 = 74.
The post-crack code integrity hash is:

        a9 93 b9 85 27 b1 36 b0 50 8f df 40 41 29 71 e5 f6 36 13 39 fc 7f 00 00 fd fd f8 64 97 

The code's integrity is secure!

Ready to receive your license key!

abc
Initial input:

        61 62 63 0a 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 

This challenge is now mangling your input using the `md5` mangler. This mangler cannot be reversed.

This mangled your input, resulting in:

        3b 58 18 5d d7 37 e9 7c 01 62 8c 9a fe 2c 1c 09 00 00 00 00 00 00 00 00 00 00 00 00 00 

The mangling is done! The resulting bytes will be used for the final comparison.

Final result of mangling input:

        3b 58 18 5d d7 37 e9 7c 01 62 8c 9a fe 2c 1c 09 00 00 00 00 00 00 00 00 00 00 00 00 00 

Expected result:

        7f 4d da 99 6b e7 7e 72 ce bf 3c 3d 52 af de 80 00 00 00 00 00 00 00 00 00 00 00 00 00 

Checking the received license key!

You win! Here is your flag:
pwn.college{fake_flag}
```