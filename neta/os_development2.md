# 30日でできる！OS自作入門奮闘記
買ったのでやる気を出すために日記を書くことにしました。頑張って続けられるように頑張ります。（いつもいってるような^^;）
## day1
前説を読んでやる気を高めました。   
バイナリエディタをダウンロードしてきました。   
[ここ](http://www.zob.ne.jp/~c.mos/soft/bz.html)はリンク切れなので、[こっち](https://www.vcraft.jp/soft/bz.html)でダウンロードできました。   
フロッピーがなかったので[ここ](https://www.soonlabo.com/30%E6%97%A5%E3%81%A7%E3%81%A7%E3%81%8D%E3%82%8Bos%E8%87%AA%E4%BD%9C%E5%85%A5%E9%96%80%E3%83%91%E3%83%BC%E3%83%88%EF%BC%91%EF%BC%881%E6%97%A5%E7%9B%AE%EF%BC%89/)の記事をみながらviratual boxを使用しました。   
またcd-romが使えないので[ここ](https://github.com/jacoloves/30nichideosjisaku/tree/master/01_day/helloos0)で本書にか書かれているバッチファイルをコピーしてきます。   
quem等のツール関連は[サポートページ](http://hrb.osask.jp/)を読みましょう。  
後続されるアセンブラのソースコードもしかりです。   
hellos.nasのコードは本書にあるのでぜひ手打ちしてみることをお勧めします。（かなり大変ですが^^;）   
実際書いたアセンブラのソースで恐ろしく打つのがめんどくさかったです。   
```
DB 0xeb, 0x4e, 0x90, 0x48, 0x45, 0x4c, 0x4c, 0x4f
DB 0x49, 0x50, 0x4c, 0x00, 0x02, 0x01, 0x01, 0x00
DB 0x02, 0xe0, 0x00, 0x40, 0x0b, 0xf0, 0x09, 0x00
DB 0x12, 0x00, 0x02, 0x00, 0x00, 0x00, 0x00, 0x00
DB 0x40, 0x0b, 0x00, 0x00, 0x00, 0x00, 0x29, 0xff
DB 0xff, 0xff, 0xff, 0x48, 0x45, 0x4c, 0x4c, 0x4f
DB 0x2d, 0x4f, 0x53, 0x20, 0x20, 0x20, 0x46, 0x41
DB 0x54, 0x31, 0x32, 0x20, 0x20, 0x20, 0x00, 0x00
RESB   16
DB 0xb8, 0x00, 0x00, 0x8e, 0xd0, 0xbc, 0x00, 0x7c
DB 0x8e, 0xd8, 0x8e, 0xc0, 0xbe, 0x74, 0x7c, 0x8a
DB 0x04, 0x83, 0xc6, 0x01, 0x3c, 0x00, 0x74, 0x09
DB 0xb4, 0x0e, 0xbb, 0x0f, 0x00, 0xcd, 0x10, 0xeb
DB 0xee, 0xf4, 0xed, 0xfd, 0x0a, 0x0a, 0x68, 0x65 
DB 0x6c, 0x6c, 0x6f, 0x2c, 0x20, 0x77, 0x6f, 0x72
DB 0x6c, 0x64, 0x0a, 0x00, 0x00, 0x00, 0x00, 0x00
RESB   368
DB 0x00, 0x00, 0x00, 0x00, 0x00, 0x00, 0x55, 0xaa
DB 0xf0, 0xff, 0xff, 0x00, 0x00, 0x00, 0x00, 0x00
RESB   4600
DB 0xf0, 0xff, 0xff, 0x00, 0x00, 0x00, 0x00, 0x00
RESB   1469432
```

さらにアセンブラを書いていきます   
こっちはだいぶ見やすいですね！   
```
; hello-os
; TAB=4

; FAT12 foramt floppy

        DB      0xeb, 0x4e, 0x90
        DB      "HELLOTPL"
        DW      512
        DB      1
        DW      1
        DB      2
        DW      224
        DW      2880
        DB      0xf0
        DW      9
        DW      18
        DW      2
        DD      0
        DD      2880
        DB      0,0,0x29
        DD      0xffffffff
        DB      "HELLO-OS   "
        DB      "FAT12   "
        RESB    18

; program

        DB      0xb8, 0x00, 0x00, 0x8e, 0xd0, 0xbc, 0x00, 0x7c
        DB      0x8e, 0xd8, 0x8e, 0xc0, 0xbe, 0x74, 0x7c, 0x8a
        DB      0x04, 0x83, 0xc6, 0x01, 0x3c, 0x00, 0x74, 0x09
        DB      0xb4, 0x0e, 0xbb, 0x0f, 0x00, 0xcd, 0x10, 0xeb
        DB      0xee, 0xf4, 0xeb, 0xfd

; message

        DB      0x0a, 0x0a
        DB      "hello, world2"
        DB      0x0a
        DB      0

        RESB    0x1fe-$

        DB      0x55, 0xaa

; boot sector

        DB      0xf0, 0xff, 0xff, 0x00, 0x00, 0x00, 0x00, 0x00
        RESB    4600
        DB      0xf0, 0xff, 0xff, 0x00, 0x00, 0x00, 0x00, 0x00
        RESB    1469432
```
hello,world2が出力されました！  
1日目はこれで完了です。中々にハードですね汗
## day2
二日目もアセンブラを書くところからスタートです。   
MOV命令やHLT命令などを詳しく学習しました。   
以下はその際記載したソース   
```
; hello-os
; TAB=4

        ORG     0x7c00

; FAT12 foramt floppy

        JMP     entry
        DB      0x90
        DB      "HELLOTPL"
        DW      512
        DB      1
        DW      1
        DB      2
        DW      224
        DW      2880
        DB      0xf0
        DW      9
        DW      18
        DW      2
        DD      0
        DD      2880
        DB      0,0,0x29
        DD      0xffffffff
        DB      "HELLO-OS   "
        DB      "FAT12   "
        RESB    18

; program

entry:
        MOV     AX,0
        MOV     SS,AX
        MOV     SP,0x7c00
        MOV     DS,AX
        MOV     ES,AX

        MOV     SI,msg
putloop:
        MOV     AL,[SI]
        ADD     SI,1
        CMP     AL,0
        JE      fin
        MOV     AH,0x0e
        MOV     BX,15
        INT     0x10
        JMP     putloop
fin:
        HLT
        JMP     fin

; message
msg:
        DB      0x0a, 0x0a
        DB      "hello, world3"
        DB      0x0a
        DB      0

        RESB    0x7dfe-$

        DB      0x55, 0xaa

; boot sector

        DB      0xf0, 0xff, 0xff, 0x00, 0x00, 0x00, 0x00, 0x00
        RESB    4600
        DB      0xf0, 0xff, 0xff, 0x00, 0x00, 0x00, 0x00, 0x00
        RESB    1469432
```

**0x00007c00-oxoooo7dff**はブートセクタが読み込まれたアドレスらしいです。   
だから0x7c00を指定しているのか。   
helloos4では各種バッチファイルをいじってosを作りました。   
**ipl.nas**というアセンブラソースファイルを作りました。helloos.nasとの違いはboot sectorの部分がないことです。   
makefileを作成してさらに使いやすくしました。
```
# file making rule

default:
	..\..\z_tools\make.exe img

ipl.bin : ipl.nas Makefile
	..\..\z_tools\nask.exe ipl.nas ipl.bin ipl.lst

helloos.img : ipl.bin Makefile
	..\..\z_tools\edimg.exe imgin:..\..\z_tools\fdimg0at.tek \
		wbinimg src:ipl.bin len:512 from:0 to:0 imgout:helloos.img

img :
	..\..\z_tools\make.exe -r helloos.img

asm :
	..\..\z_tools\make.exe -r ipl.bin

run :
	..\..\z_tools\make.exe img
	cmd.exe /C copy helloos.img ..\..\z_tools\qemu\fdimage0.bin
	..\..\z_tools\make.exe -C ..\..\z_tools\qemu

install :
	..\..\z_tools\make.exe img
	..\..\z_tools\imgtol.com w a: helloos.img

clean :
	rm -f ipl.bin
	rm -f ipl.lst

src_only :
	make clean
	rm -f helloos.img
```

途中meke runコマンドで下記エラーが出てしまい苦労しました。   
```
copy helloos.img ..\..\z_tools\qemu\fdimage0.bin
process_begin: CreateProcess((null), copy helloos.img ..\..\z_tools\qemu\fdimage0.bin, ...) failed.
make (e=2): 指定されたファイルが見つかりません。
..\..\z_tools\make.exe: *** [run] Error 2
```

[この記事](http://ng-r.hateblo.jp/entry/2019/05/24/013306)をみてソースを修正しました。  
```
cmd.exe /C copy helloos.img ..\..\z_tools\qemu\fdimage0.bin
```

原因は恐らくgitbashとwindowsのcopyコマンドが干渉しているからかなと思います。   
要するにディレクトリから直接batファイルたたけば書籍に書かれているソースでも問題なく叩けたのかなと思います。

## day3
アセンブラソースコードをいじりました。
```
; hello-os
; TAB=4

        ORG     0x7c00

; FAT12 foramt floppy

        JMP     entry
        DB      0x90
        DB      "HELLOTPL"
        DW      512
        DB      1
        DW      1
        DB      2
        DW      224
        DW      2880
        DB      0xf0
        DW      9
        DW      18
        DW      2
        DD      0
        DD      2880
        DB      0,0,0x29
        DD      0xffffffff
        DB      "HELLO-OS   "
        DB      "FAT12   "
        RESB    18

; program

entry:
        MOV     AX,0
        MOV     SS,AX
        MOV     SP,0x7c00
        MOV     DS,AX

; read disk

        MOV     AX,0x0820
        MOV     ES,AX
        MOV     CH,0
        MOV     DH,0
        MOV     CL,2

        MOV     AH,0x02
        MOV     AL,1
        MOV     BX,0
        MOV     DL,0x00
        INT     0x13
        JC      error

; read finished but nothing task, it is slept

fin:
        HLT
        JMP     fin

error:
        MOV      SI,msg
putloop:
        MOV     AL,[SI]
        ADD     SI,1
        CMP     AL,0
        JE      fin
        MOV     AH,0x0e
        MOV     BX,15
        INT     0x10
        JMP     putloop
; message
msg:
        DB      0x0a, 0x0a
        DB      "load error"
        DB      0x0a
        DB      0

        RESB    0x7dfe-$

        DB      0x55, 0xaa
```

makeファイルも書き換えました。
```
TOOLPATH = ..\..\z_tools\\

MAKE     = $(TOOLPATH)make.exe -r
NASK     = $(TOOLPATH)nask.exe
EDIMG    = $(TOOLPATH)edimg.exe
IMGTOL   = $(TOOLPATH)imgtol.com
COPY     = cmd.exe /C copy
DEL      = rm -f


# default operation
default:
	$(MAKE) img

# file making rule

ipl.bin : ipl.nas Makefile
	$(NASK) ipl.nas ipl.bin ipl.lst

helloos.img : ipl.bin Makefile
	$(EDIMG) imgin:..\..\z_tools\fdimg0at.tek \
		wbinimg src:ipl.bin len:512 from:0 to:0 imgout:helloos.img

# command

img :
	$(MAKE) helloos.img

asm :
	$(MAKE) ipl.bin

run :
	$(MAKE) img
	$(COPY) helloos.img ..\..\z_tools\qemu\fdimage0.bin
	$(MAKE) -C ..\..\z_tools\qemu

install :
	$(MAKE) img
	$(IMGTOL) w a: helloos.img

clean :
	-$(DEL) ipl.bin
	-$(DEL) ipl.lst

src_only :
	$(MAKE) clean
	-$(DEL) helloos.img
```

下記のようにTOLLPATHとMAKE変数の間に改行が入っている理由は「\」はコードが続いていると思われてしまうので改行で**MAKEはTOOLPATH**の続きではありませんと教えています。
```
TOOLPATH = ..\..\z_tools\\

MAKE     = $(TOOLPATH)make.exe -r
```

エラーになったらやり直せるようにosを改良しました。
```
; hello-os
; TAB=4

        ORG     0x7c00

; FAT12 foramt floppy

        JMP     entry
        DB      0x90
        DB      "HELLOTPL"
        DW      512
        DB      1
        DW      1
        DB      2
        DW      224
        DW      2880
        DB      0xf0
        DW      9
        DW      18
        DW      2
        DD      0
        DD      2880
        DB      0,0,0x29
        DD      0xffffffff
        DB      "HELLO-OS   "
        DB      "FAT12   "
        RESB    18

; program

entry:
        MOV     AX,0
        MOV     SS,AX
        MOV     SP,0x7c00
        MOV     DS,AX

; read disk

        MOV     AX,0x0820
        MOV     ES,AX
        MOV     CH,0
        MOV     DH,0
        MOV     CL,2

        MOV     SI,0
retry:
        MOV     AH,0x02
        MOV     AL,1
        MOV     BX,0
        MOV     DL,0x00
        INT     0x13
        JNC     fin
        ADD     SI,1
        CMP     SI,5
        JAE     error
        MOV     AH,0x00
        MOV     DL,0x00
        INT     0x13
        JMP     retry

; read finished but nothing task, it is slept

fin:
        HLT
        JMP     fin

error:
        MOV      SI,msg
putloop:
        MOV     AL,[SI]
        ADD     SI,1
        CMP     AL,0
        JE      fin
        MOV     AH,0x0e
        MOV     BX,15
        INT     0x10
        JMP     putloop
; message
msg:
        DB      0x0a, 0x0a
        DB      "load error"
        DB      0x0a
        DB      0

        RESB    0x7dfe-$

        DB      0x55, 0xaa
```

ここはスラスラすすむこことができました。   
18セクタ先まで読み込むアセンブラのソースを書きました。
```
entry:
        MOV     AX,0
        MOV     SS,AX
        MOV     SP,0x7c00
        MOV     DS,AX

; read disk

        MOV     AX,0x0820
        MOV     ES,AX
        MOV     CH,0
        MOV     DH,0
        MOV     CL,2
readloop:
        MOV     SI,0
retry:
        MOV     AH,0x02
        MOV     AL,1
        MOV     BX,0
        MOV     DL,0x00
        INT     0x13
        JNC     next
        ADD     SI,1
        CMP     SI,5
        JAE     error
        MOV     AH,0x00
        MOV     DL,0x00
        INT     0x13
        JMP     retry
next:
        MOV     AX,ES
        ADD     AX,0x0020
        MOV     ES,AX
        ADD     CL,1
        CMP     CL,18
        JBE     readloop
```

さらにアセンブラを加筆しました。   
今回は10シリンダ分読み込ませています。   
```
; hello-os
; TAB=4

;cylinders
CYLS    EQU     10

        ORG     0x7c00

; FAT12 foramt floppy

        JMP     entry
        DB      0x90
        DB      "HARIBOTE"
        DW      512
        DB      1
        DW      1
        DB      2
        DW      224
        DW      2880
        DB      0xf0
        DW      9
        DW      18
        DW      2
        DD      0
        DD      2880
        DB      0,0,0x29
        DD      0xffffffff
        DB      "HELLO-OS   "
        DB      "FAT12   "
        RESB    18

; program

entry:
        MOV     AX,0
        MOV     SS,AX
        MOV     SP,0x7c00
        MOV     DS,AX

; read disk

        MOV     AX,0x0820
        MOV     ES,AX
        MOV     CH,0
        MOV     DH,0
        MOV     CL,2
readloop:
        MOV     SI,0
retry:
        MOV     AH,0x02
        MOV     AL,1
        MOV     BX,0
        MOV     DL,0x00
        INT     0x13
        JNC     next
        ADD     SI,1
        CMP     SI,5
        JAE     error
        MOV     AH,0x00
        MOV     DL,0x00
        INT     0x13
        JMP     retry
next:
        MOV     AX,ES
        ADD     AX,0x0020
        MOV     ES,AX
        ADD     CL,1
        CMP     CL,18
        JBE     readloop
        MOV     CL,1
        ADD     DH,1
        CMP     DH,2
        JB      readloop
        MOV     DH,0
        ADD     CH,1
        CMP     CH,CYLS
        JB      readloop
```

次はOSを書きました。 
まずはアセンブラのから  
```
fin:
        HLT
        JMP     fin
```

次にMakefile
```
TOOLPATH = ..\..\z_tools\\

MAKE     = $(TOOLPATH)make.exe -r
NASK     = $(TOOLPATH)nask.exe
EDIMG    = $(TOOLPATH)edimg.exe
IMGTOL   = $(TOOLPATH)imgtol.com
COPY     = cmd.exe /C copy
DEL      = rm -f


# default operation
default:
	$(MAKE) img

# file making rule

ipl.bin : ipl.nas Makefile
	$(NASK) ipl.nas ipl.bin ipl.lst

haribote.sys : haribote.nas Makefile
	$(NASK) haribote.nas haribote.sys haribote.lst

haribote.img : ipl.bin haribote.sys Makefile
	$(EDIMG)   imgin:..\..\z_tools/fdimg0at.tek \
		wbinimg src:ipl.bin len:512 from:0 to:0 \
		copy from:haribote.sys to:@: \
		imgout:haribote.img

# command

img :
	$(MAKE) haribote.img

run :
	$(MAKE) img
	$(COPY) haribote.img ..\..\z_tools\qemu\fdimage0.bin
	$(MAKE) -C ..\..\z_tools\qemu

install :
	$(MAKE) img
	$(IMGTOL) w a: haribote.img

clean :
	-$(DEL) ipl.bin
	-$(DEL) ipl.lst
	-$(DEL) haribote.sys
	-$(DEL) haribote.lst

src_only :
	$(MAKE) clean
	-$(DEL) haribote.img
```

なんというか一番苦労させられたのはMakefileの修正です。   
結局エラーの原因はharibote.nasに名前を**harinote.nas**にしてたからという誤字が原因でした。   

今回はharibote.nasとipl.nasに少し手を加えました。
haribote.nas
```
; haribote-os
; TAB=4

        ORG     0xc200
fin:
        HLT
        JMP     fin
```

ipl.nas
```
next:
        MOV     AX,ES
        ADD     AX,0x0020
        MOV     ES,AX
        ADD     CL,1
        CMP     CL,18
        JBE     readloop
        MOV     CL,1
        ADD     DH,1
        CMP     DH,2
        JB      readloop
        MOV     DH,0
        ADD     CH,1
        CMP     CH,CYLS
        JB      readloop

; read finished and execute haribote.sys!
        JMP 0xc200
```

次は画面モードを開発をしました。   
haribote.nasとipl10.nas(ipl.nasから名前を変更しています。)とMakefileを修正しました。   
まずharibote.nasが
```
; haribote-os
; TAB=4

        ORG     0xc200

; screen mode
        MOV     AL,0x13
        MOV     AH,0x00
        INT     0x10
```

AH=0x00がビデオBIOSとの切り替えに使われるらしいです。   
ipl10.nasは
```
next:
        MOV     AX,ES
        ADD     AX,0x0020
        MOV     ES,AX
        ADD     CL,1
        CMP     CL,18
        JBE     readloop
        MOV     CL,1
        ADD     DH,1
        CMP     DH,2
        JB      readloop
        MOV     DH,0
        ADD     CH,1
        CMP     CH,CYLS
        JB      readloop

; read finished and execute haribote.sys!
        MOV     [0x0ff0],CH
        JMP     0xc200
```
JMP     0xc200の前にMOV     [0x0ff0],CHを追加しました。   
Makefile
```
TOOLPATH = ..\..\z_tools\\

MAKE     = $(TOOLPATH)make.exe -r
NASK     = $(TOOLPATH)nask.exe
EDIMG    = $(TOOLPATH)edimg.exe
IMGTOL   = $(TOOLPATH)imgtol.com
COPY     = cmd.exe /C copy
DEL      = rm -f


# default operation
default:
	$(MAKE) img

# file making rule

ipl10.bin : ipl10.nas Makefile
	$(NASK) ipl10.nas ipl10.bin ipl10.lst

haribote.sys : haribote.nas Makefile
	$(NASK) haribote.nas haribote.sys haribote.lst

haribote.img : ipl10.bin haribote.sys Makefile
	$(EDIMG)   imgin:..\..\z_tools/fdimg0at.tek \
		wbinimg src:ipl10.bin len:512 from:0 to:0 \
		copy from:haribote.sys to:@: \
		imgout:haribote.img

# command

img :
	$(MAKE) haribote.img

run :
	$(MAKE) img
	$(COPY) haribote.img ..\..\z_tools\qemu\fdimage0.bin
	$(MAKE) -C ..\..\z_tools\qemu

install :
	$(MAKE) img
	$(IMGTOL) w a: haribote.img

clean :
	-$(DEL) ipl10.bin
	-$(DEL) ipl10.lst
	-$(DEL) haribote.sys
	-$(DEL) haribote.lst

src_only :
	$(MAKE) clean
	-$(DEL) haribote.img
```

ipl~と記載があった部分をipl10~に変更しています。   
続いてはキーボードの状態をBIOSに教えてもらうプログラムを作成しました。   
haribote.nasを修正しました。   
```
; haribote-os
; TAB=4

; BOOT_INFO
CYLS    EQU     0x0ff0
LEDS    EQU     0x0ff1
VMODE   EQU     0x0ff2
SCRNX   EQU     0x0ff4
SCRNY   EQU     0x0ff6
VRAM    EQU     0x0ff8

        ORG     0xc200

; screen mode
        MOV     AL,0x13
        MOV     AH,0x00
        INT     0x10
        MOV     BYTE [VMODE],8
        MOV     WORD [SCRNX],320
        MOV     WORD [SCRNY],200
        MOV     DWORD [VRAM],0x000a0000

; keybord's LED state is teached BIOS

        MOV     AH,0x02
        INT     0x16
        MOV     [LEDS],AL
fin:
        HLT
        JMP     fin
```

いよいよ最大の鬼門、32bit化です。   
ソースコードを写経しながら眺めるスタンスは変わらないのですが、一つだけツールの問題で環境を多少いじらないといけないエラーにぶつかることがあります。   
環境によってはその問題が起きないのですが私の環境では発生して苦労したので共有します。   
```
C:\Users\hariboteos\day_03\harib00i>..\..\z_tools\make.exe bootpack.hrb
..\..\z_tools\\cc1.exe -I..\..\z_tools\haribote\\ -Os -Wall -quiet -o bootpack.gas bootpack.c
bootpack.c:8:2: warning: no newline at end of file
..\..\z_tools\\gas2nask.exe -a bootpack.gas  bootpack.nas
..\..\z_tools\\nask.exe bootpack.nas bootpack.obj bootpack.lst
..\..\z_tools\\obj2bim.exe @..\..\z_tools\\haribote\haribote.rul out:bootpack.bim stack:3136k map:bootpack.map \
        bootpack.obj
Rule file error : can't open file : ../z_tools/haribote/harilibc.lib
..\..\z_tools\make.exe: *** [bootpack.bim] Error 6
```

上記のエラーは**make bootpack.hrb**と叩くと発生します。   
恐らくobj2bim.exeがディレクトリ探索する際、**../z_tools/haribote**配下を見に行く仕様になってるから発生するエラーだと思われます。   
私のように./〇〇/day_03/harib00iみたいなディレクトリ構造をしてるとエラーになってしまうので発生したら、**コンソール起動している一段上のディレクトリにz_toolsフォルダ格納する**ことでエラーが解消されます。

全体的に作り替えなくてはいけないところも多いですがぜひ頑張ってチャレンジしてみてください！   

asmhead.nas
```
; haribote-os boot asm
; TAB=4

BOTPAK  EQU     0x00280000
DSKCAC  EQU     0x00100000
DSKCAC0 EQU     0x00008000

; BOOT_INFO
CYLS    EQU     0x0ff0
LEDS    EQU     0x0ff1
VMODE   EQU     0x0ff2
SCRNX   EQU     0x0ff4
SCRNY   EQU     0x0ff6
VRAM    EQU     0x0ff8

        ORG     0xc200

; screen mode

        MOV     AL,0x13
        MOV     AH,0x00
        INT     0x10
        MOV     BYTE [VMODE],8
        MOV     WORD [SCRNX],320
        MOV     WORD [SCRNY],200
        MOV     DWORD [VRAM],0x000a0000

; keybord's LED state is teached BIOS

        MOV     AH,0x02
        INT     0x16
        MOV     [LEDS],AL

; PICが一切の割り込みを受け付けないようにする
;	AT互換機の仕様では、PICの初期化をするなら、
;	こいつをCLI前にやっておかないと、たまにハングアップする
;	PICの初期化はあとでやる

        MOV     AL,0xff
        OUT     0x21,AL
        NOP
        OUT     0xa1,AL

        CLI

; Set A20GATE so that CPU can access more than 1MB of memory

        CALL    waitkbdout
        MOV     AL,0xd1
        OUT     0x64,AL
        CALL    waitkbdout
        MOV     AL,0xdf
        OUT     0x60,AL
        CALL    waitkbdout

; move protect mode

[INSTRSET "i486p"]

        LGDT    [GDTR0]
        MOV     EAX,CR0
        AND     EAX,0x7fffffff
        OR      EAX,0x00000001
        MOV     CR0,EAX
        JMP     pipelineflush
pipelineflush:
        MOV     AX,1*8
        MOV     DS,AX
        MOV     ES,AX
        MOV     FS,AX
        MOV     GS,AX
        MOV     SS,AX

; bootpack trancfer

        MOV     ESI,bootpack
        MOV     EDI,BOTPAK
        MOV     ECX,512*1024/4
        CALL    memcpy

; ついでにディスクデータも本来の位置へ転送

; boot sector

        MOV     ESI,0x7c00
        MOV     EDI,DSKCAC
        MOV     ECX,512/4
        CALL    memcpy

; remaining

        MOV     ESI,DSKCAC0+512
        MOV     EDI,DSKCAC+512
        MOV     ECX,0
        MOV     CL,BYTE [CYLS]
        IMUL    ECX,512*18*2/4
        SUB     ECX,512/4
        CALL    memcpy

; bootpack
;

; bootpack run

        MOV     EBX,BOTPAK
        MOV     ECX,[EBX+16]
        ADD     ECX,3
        SHR     ECX,2
        JZ      skip
        MOV     ESI,[EBX+20]
        ADD     ESI,EBX
        MOV     EDI,[EBX+12]
        CALL    memcpy
skip:
        MOV     ESP,[EBX+12]
        JMP     DWORD 2*8:0x0000001b

waitkbdout:
        IN      AL,0x64
        AND     AL,0x02
        JNZ    waitkbdout
        RET

memcpy:
        MOV     EAX,[ESI]
        ADD     ESI,4
        MOV     [EDI],EAX
        ADD     EDI,4
        SUB     ECX,1
        JNZ     memcpy
        RET
; memcpy can also be written with string instructions, unless you forget the address size prefix

        ALIGNB  16
GDT0:
        RESB    8
        DW      0xffff,0x0000,0x9200,0x00cf
        DW      0xffff,0x0000,0x9a28,0x0047

        DW      0
GDTR0:
        DW      8*3-1
        DD      GDT0

        ALIGNB  16
bootpack:
```

Makefile
```
TOOLPATH = ..\..\z_tools\\

INCPATH  = ..\..\z_tools\haribote\\

MAKE     = $(TOOLPATH)make.exe -r
NASK     = $(TOOLPATH)nask.exe
CC1		 = $(TOOLPATH)cc1.exe -I$(INCPATH) -Os -Wall -quiet
GAS2NASK = $(TOOLPATH)gas2nask.exe -a
OBJ2BIM  = $(TOOLPATH)obj2bim.exe
BIM2HRB  = $(TOOLPATH)bim2hrb.exe
RULEFILE = $(TOOLPATH)haribote\haribote.rul
EDIMG    = $(TOOLPATH)edimg.exe
IMGTOL   = $(TOOLPATH)imgtol.com
COPY     = cmd.exe /C copy
DEL      = rm -f


# default operation
default:
	$(MAKE) img

# file making rule

ipl10.bin : ipl10.nas Makefile
	$(NASK) ipl10.nas ipl10.bin ipl10.lst

asmhead.bin : asmhead.nas Makefile
	$(NASK) asmhead.nas asmhead.bin asmhead.lst

bootpack.gas : bootpack.c Makefile
	$(CC1) -o bootpack.gas bootpack.c

bootpack.nas : bootpack.gas Makefile
	$(GAS2NASK) bootpack.gas  bootpack.nas 

bootpack.obj : bootpack.nas Makefile
	$(NASK) bootpack.nas bootpack.obj bootpack.lst

bootpack.bim : bootpack.obj Makefile
	$(OBJ2BIM) @$(RULEFILE) out:bootpack.bim stack:3136k map:bootpack.map \
		bootpack.obj
# 3MB+64KB=3136KB

bootpack.hrb : bootpack.bim Makefile
	$(BIM2HRB) bootpack.bim bootpack.hrb 0

haribote.sys : asmhead.bin bootpack.hrb Makefile
	cat asmhead.bin >  haribote.sys
	cat bootpack.hrb >> haribote.sys

haribote.img : ipl10.bin haribote.sys Makefile
	$(EDIMG)   imgin:..\..\z_tools\fdimg0at.tek \
		wbinimg src:ipl10.bin len:512 from:0 to:0 \
		copy from:haribote.sys to:@: \
		imgout:haribote.img

# command

img :
	$(MAKE) haribote.img

run :
	$(MAKE) img
	$(COPY) haribote.img ..\..\z_tools\qemu\fdimage0.bin
	$(MAKE) -C ..\..\z_tools\qemu

install :
	$(MAKE) img
	$(IMGTOL) w a: haribote.img

clean :
	-$(DEL) *.bin
	-$(DEL) *.lst
	-$(DEL) *.gas
	-$(DEL) *.obj
	-$(DEL) bootpack.nas
	-$(DEL) bootpack.map
	-$(DEL) bootpack.bim
	-$(DEL) bootpack.hrb
	-$(DEL) haribote.sys

src_only :
	$(MAKE) clean
	-$(DEL) haribote.img
```
 今日の最後にHLTを追加する処理を実装しました。   
新規で追加するモジュールもありますがここまできた皆さんならそこまで難しくないのではないでしょうか。   
naskfunc.nas
```
TOOLPATH = ..\..\z_tools\\

INCPATH  = ..\..\z_tools\haribote\\

MAKE     = $(TOOLPATH)make.exe -r
NASK     = $(TOOLPATH)nask.exe
CC1		 = $(TOOLPATH)cc1.exe -I$(INCPATH) -Os -Wall -quiet
GAS2NASK = $(TOOLPATH)gas2nask.exe -a
OBJ2BIM  = $(TOOLPATH)obj2bim.exe
BIM2HRB  = $(TOOLPATH)bim2hrb.exe
RULEFILE = $(TOOLPATH)haribote\haribote.rul
EDIMG    = $(TOOLPATH)edimg.exe
IMGTOL   = $(TOOLPATH)imgtol.com
COPY     = cmd.exe /C copy
DEL      = rm -f


# default operation
default:
	$(MAKE) img

# file making rule

ipl10.bin : ipl10.nas Makefile
	$(NASK) ipl10.nas ipl10.bin ipl10.lst

asmhead.bin : asmhead.nas Makefile
	$(NASK) asmhead.nas asmhead.bin asmhead.lst

bootpack.gas : bootpack.c Makefile
	$(CC1) -o bootpack.gas bootpack.c

bootpack.nas : bootpack.gas Makefile
	$(GAS2NASK) bootpack.gas  bootpack.nas 

bootpack.obj : bootpack.nas Makefile
	$(NASK) bootpack.nas bootpack.obj bootpack.lst

naskfunc.obj : naskfunc.nas Makefile
	$(NASK) naskfunc.nas naskfunc.obj naskfunc.lst

bootpack.bim : bootpack.obj naskfunc.obj Makefile
	$(OBJ2BIM) @$(RULEFILE) out:bootpack.bim stack:3136k map:bootpack.map \
		bootpack.obj naskfunc.obj
# 3MB+64KB=3136KB

bootpack.hrb : bootpack.bim Makefile
	$(BIM2HRB) bootpack.bim bootpack.hrb 0

haribote.sys : asmhead.bin bootpack.hrb Makefile
	cat asmhead.bin >  haribote.sys
	cat bootpack.hrb >> haribote.sys

haribote.img : ipl10.bin haribote.sys Makefile
	$(EDIMG)   imgin:..\..\z_tools\fdimg0at.tek \
		wbinimg src:ipl10.bin len:512 from:0 to:0 \
		copy from:haribote.sys to:@: \
		imgout:haribote.img

# command

img :
	$(MAKE) haribote.img

run :
	$(MAKE) img
	$(COPY) haribote.img ..\..\z_tools\qemu\fdimage0.bin
	$(MAKE) -C ..\..\z_tools\qemu

install :
	$(MAKE) img
	$(IMGTOL) w a: haribote.img

clean :
	-$(DEL) *.bin
	-$(DEL) *.lst
	-$(DEL) *.gas
	-$(DEL) *.obj
	-$(DEL) bootpack.nas
	-$(DEL) bootpack.map
	-$(DEL) bootpack.bim
	-$(DEL) bootpack.hrb
	-$(DEL) haribote.sys

src_only :
	$(MAKE) clean
	-$(DEL) haribote.img
```

bootpack.c
```
void io_hlt(void);

void HariMain(void)
{

fin:
    io_hlt();
    goto fin;
    
}
```

Makefile
```
# file making rule

ipl10.bin : ipl10.nas Makefile
	$(NASK) ipl10.nas ipl10.bin ipl10.lst

asmhead.bin : asmhead.nas Makefile
	$(NASK) asmhead.nas asmhead.bin asmhead.lst

bootpack.gas : bootpack.c Makefile
	$(CC1) -o bootpack.gas bootpack.c

bootpack.nas : bootpack.gas Makefile
	$(GAS2NASK) bootpack.gas  bootpack.nas 

bootpack.obj : bootpack.nas Makefile
	$(NASK) bootpack.nas bootpack.obj bootpack.lst

naskfunc.obj : naskfunc.nas Makefile
	$(NASK) naskfunc.nas naskfunc.obj naskfunc.lst

bootpack.bim : bootpack.obj naskfunc.obj Makefile
	$(OBJ2BIM) @$(RULEFILE) out:bootpack.bim stack:3136k map:bootpack.map \
		bootpack.obj naskfunc.obj
# 3MB+64KB=3136KB
```

これにて本日は終了です。   
凄くハードでしたがお疲れさまでした。 

**実装過程**   
・　[f19327e0cd072bdbc70833d803d423437a7c9823](f19327e0cd072bdbc70833d803d423437a7c9823)


VRAMに何か書き込みたいのでnaskfunc.nasをいじります。
naskfunc.nas
```
; naskfunc
; TAB=4

[FORMAT "WCOFF"]
[INSTRSET "i486p"]
[BITS 32]

; object file

[FILE "naskfunc.nas"]

        GLOBAL  _io_hlt,_write_mem8

; function

[SECTION .test]

_io_hlt:
        HLT
        RET

_write_mem8:    ; void write_mem8(int addr, int data);
        MOV     ECX,[ESP+4]     ; [ESP+4] in addr read ECX
        MOV     AL,[ESP+8]      ; [ESP+8] in data read AL
        MOV     [ECX],AL
        RET
```

_write_mem8を呼び出すためにbootpack.cも改造します。
bootpack.c
```
void io_hlt(void);
void write_mem8(int addr, int data);

void HariMain(void)
{
    int i;

    for (i = 0xa0000; i <= 0xaffff; i++) {
        write_mem8(i, 15);
    }

    for (;;) {
        io_hlt();
    }
    
}
```

次はしましまに改造するためにbootpack.cに少し手を加えます。   
bootpack.c
```
void HariMain(void)
{
    int i;

    for (i = 0xa0000; i <= 0xaffff; i++) {
        write_mem8(i, i & 0x0f);
    }

    for (;;) {
        io_hlt();
    }
    
}
```

ここからはポインタについて見ていく回なのでざっと書いていきます。   
青い画面が表示されます。   
bootpack.c
```
void HariMain(void)
{
    int i;
    char *p;

    for (i = 0xa0000; i <= 0xaffff; i++) {
        p =i;
        *p = 1 & 0x0f;
    }

    for (;;) {
        io_hlt();
    }
    
}
```
次はしましまの画面が表示されます。   
bootpack.c
```
void HariMain(void)
{
    int i;
    char *p;

    p = (char *) 0xa0000;

    for (i = 0; i <= 0xffff; i++) {
        *(p + i) = i & 0x0f;
    }

    for (;;) {
        io_hlt();
    }
    
}
```
次も企保的に瓦井ですが*(p + i)の部分がp[i]になっています。   
bootpack.c
```
void HariMain(void)
{
    int i;
    char *p;

    p = (char *) 0xa0000;

    for (i = 0; i <= 0xffff; i++) {
        p[i] = i & 0x0f;
    }

    for (;;) {
        io_hlt();
    }
    
}
```

**実装過程**   
・　[cb092449d4b3f7dee5bd09004a736b6905d3a889](cb092449d4b3f7dee5bd09004a736b6905d3a889)

## day4
day5と言っていますがこれは書籍の4日目ではなく筆者がOS作りを初めて4日目がということを指します。煩わしくて申し訳ありません。   
今回は色をカラフルにしてあげるところからスタートします。C言語をゴリゴリ実装していくのでとても楽しみです。   
bootpack.c
```
void io_hlt(void);
void io_cli(void);
void io_out8(int port, int data);
int io_load_eflags(void);
void io_store_eflags(int eflags);

void init_palette(void);
void set_palette(int start, int end, unsigned char *rgd);

void HariMain(void)
{
    int i;
    char *p;

    init_palette();

    p = (char *) 0xa0000;

    for (i = 0; i <= 0xffff; i++) {
        p[i] = i & 0x0f;
    }

    for (;;) {
        io_hlt();
    }
    
}

void init_palette(void)
{
    static unsigned char table_rgb[16 * 3] = {
        0x00, 0x00, 0x00,   /* 0:black */
        0xff, 0x00, 0x00,   /* 1:light red */
        0x00, 0xff, 0x00,   /* 2:light green */
        0xff, 0xff, 0x00,   /* 3:light yellow */
        0x00, 0x00, 0xff,   /* 4:light blue */
        0xff, 0x00, 0xff,   /* 5:light puple */
        0x00, 0xff, 0xff,   /* 6:light sky blue */
        0xff, 0xff, 0xff,   /* 7:white */
        0xc6, 0xc6, 0xc6,   /* 8:light grey */
        0x84, 0x00, 0x00,   /* 9:dark green */
        0x00, 0x84, 0x00,   /* 10:dark green */
        0x84, 0x84, 0x00,   /* 11:dark yellow */
        0x00, 0x00, 0x84,   /* 12:dark blue */
        0x84, 0x00, 0x84,   /* 13:dark puple */
        0x00, 0x84, 0x84,   /* 14:dark sky blue */
        0x84, 0x84, 0x84    /* 15:dark grey */
    };
    set_palette(0, 15, table_rgb);
    return;
}

void set_palette(int start, int end, unsigned char *rgb)
{
    int i, eflags;
    eflags = io_load_eflags();
    io_cli();
    io_out8(0x03c8, start);
    for (i = start; i <= end; i++) {
        io_out8(0x03c9, rgb[0] / 4);
        io_out8(0x03c9, rgb[1] / 4);
        io_out8(0x03c9, rgb[2] / 4);
        rgb += 3;
    }
    io_store_eflags(eflags);
    return;
}
```


続いては、    
void io_cli(void)    
void io_out8(int port, int data)    
int io_load_eflags(void)    
void io_store_eflags(int eflags)    
の4つを実装していくためにnaskfunc.nasを修正します。    
naskfunc.nas
```
; naskfunc
; TAB=4

[FORMAT "WCOFF"]
[INSTRSET "i486p"]
[BITS 32]
[FILE "naskfunc.nas"]

        GLOBAL  _io_hlt, _io_cli, _io_sci, _io_stihlt
        GLOBAL  _io_in8, _io_in16, _io_in32
        GLOBAL  _io_out8, _io_out16, _io_out32
        GLOBAL  _io_load_eflags, _io_store_eflags

; object file

[FILE "naskfunc.nas"]

; function

[SECTION .text]

_io_hlt:
        HLT
        RET

_io_cli:
        CLI
        RET

_io_sti:
        STI
        HLT
        RET

_io_in8:
        MOV     EDX,[ESP+4]
        MOV     EAX,0
        IN      AL,DX
        RET

_io_in16:
        MOV     EDX,[ESP+4]
        MOV     EAX,0
        IN      AX,DX
        RET

_io_in32:
        MOV     EDX,[ESP+4]
        IN      EAX,DX
        RET
        
_io_out8:
        MOV     EDX,[ESP+4]
        MOV     AL,[ESP+8]
        OUT     DX,AL
        RET

_io_out16:
        MOV     EDX,[ESP+4]
        MOV     EAX,[ESP+8]
        OUT     DX,AX
        RET

_io_out32:
        MOV     EDX,[ESP+4]
        MOV     EAX,[ESP+8]
        OUT     DX,EAX
        RET

_io_load_eflags:
        PUSHFD
        POP     EAX
        RET

_io_store_eflags:
        MOV     EAX,[ESP+4]
        PUSH    EAX
        POPFD
        RET
```

これで画面のしましまのいろが変わりました。    
続いて四角形を描画していきます。ここではdefineでvramの番地をあらかじめ決めています。    
bootpack.c
```
void io_hlt(void);
void io_cli(void);
void io_out8(int port, int data);
int io_load_eflags(void);
void io_store_eflags(int eflags);

void init_palette(void);
void set_palette(int start, int end, unsigned char *rgd);
void boxfill8(unsigned char *vram, int xsize, unsigned char c, int x0, int y0, int x1, int y1);

#define COL8_000000  0
#define COL8_FF0000  1
#define COL8_00FF00  2
#define COL8_FFFF00  3
#define COL8_0000FF  4
#define COL8_FF00FF  5
#define COL8_00FFFF  6
#define COL8_FFFFFF  7
#define COL8_C6C6C6  8
#define COL8_840000  9
#define COL8_008400  10
#define COL8_848400  11
#define COL8_000084  12
#define COL8_840084  13
#define COL8_008484  14
#define COL8_848484  15


void HariMain(void)
{
    int i;
    char *p;

    init_palette();

    p = (char *) 0xa0000;

    boxfill8(p, 320, COL8_FF0000, 20, 20, 120, 120);
    boxfill8(p, 320, COL8_00FF00, 70, 50, 170, 150);
    boxfill8(p, 320, COL8_0000FF, 120, 80, 220, 180);

    for (;;) {
        io_hlt();
    }
    
}

void init_palette(void)
{
    static unsigned char table_rgb[16 * 3] = {
        0x00, 0x00, 0x00,   /* 0:black */
        0xff, 0x00, 0x00,   /* 1:light red */
        0x00, 0xff, 0x00,   /* 2:light green */
        0xff, 0xff, 0x00,   /* 3:light yellow */
        0x00, 0x00, 0xff,   /* 4:light blue */
        0xff, 0x00, 0xff,   /* 5:light puple */
        0x00, 0xff, 0xff,   /* 6:light sky blue */
        0xff, 0xff, 0xff,   /* 7:white */
        0xc6, 0xc6, 0xc6,   /* 8:light grey */
        0x84, 0x00, 0x00,   /* 9:dark green */
        0x00, 0x84, 0x00,   /* 10:dark green */
        0x84, 0x84, 0x00,   /* 11:dark yellow */
        0x00, 0x00, 0x84,   /* 12:dark blue */
        0x84, 0x00, 0x84,   /* 13:dark puple */
        0x00, 0x84, 0x84,   /* 14:dark sky blue */
        0x84, 0x84, 0x84    /* 15:dark grey */
    };
    set_palette(0, 15, table_rgb);
    return;
}

void set_palette(int start, int end, unsigned char *rgb)
{
    int i, eflags;
    eflags = io_load_eflags();
    io_cli();
    io_out8(0x03c8, start);
    for (i = start; i <= end; i++) {
        io_out8(0x03c9, rgb[0] / 4);
        io_out8(0x03c9, rgb[1] / 4);
        io_out8(0x03c9, rgb[2] / 4);
        rgb += 3;
    }
    io_store_eflags(eflags);
    return;
}

void boxfill8(unsigned char *vram, int xsize, unsigned char c, int x0, int y0, int x1, int y1)
{
    int x, y;
    for (y= y0; y <= y1; y++) {
        for (x = x0; x <= x1; x++)
            vram[y * xsize + x] = c;
    }
    return;
}
```

これで四角形が出てきました。   
つぎはタスクバーを表示させるところです。   
四角形を出すところで定義したbpxfill8関数を使っていきます。   

bootpack.c
```
void HariMain(void)
{
    char *vram;
    int xsize, ysize;

    init_palette();
    vram = (char *) 0xa0000;
    xsize = 320;
    ysize = 200;

    boxfill8(vram, xsize, COL8_008484,  0,          0,           xsize - 1,   ysize - 29);
    boxfill8(vram, xsize, COL8_C6C6C6,  0,          ysize - 28,  xsize - 1,   ysize - 28);
    boxfill8(vram, xsize, COL8_FFFFFF,  0,          ysize - 27,  xsize - 1,   ysize - 27);
    boxfill8(vram, xsize, COL8_C6C6C6,  0,          ysize - 26,  xsize - 1,   ysize -  1);

    boxfill8(vram, xsize, COL8_FFFFFF,  3,          ysize - 24,  59,          ysize - 24);
    boxfill8(vram, xsize, COL8_FFFFFF,  2,          ysize - 24,   2,          ysize -  4);
    boxfill8(vram, xsize, COL8_848484,  3,          ysize -  4,  59,          ysize -  4);
    boxfill8(vram, xsize, COL8_848484, 59,          ysize - 23,  59,          ysize -  5);
    boxfill8(vram, xsize, COL8_000000,  2,          ysize -  3,  59,          ysize -  3);
    boxfill8(vram, xsize, COL8_000000, 60,          ysize - 24,  60,          ysize -  3);

    boxfill8(vram, xsize, COL8_848484, xsize - 47,  ysize - 24,  xsize -  4,   ysize - 24);
    boxfill8(vram, xsize, COL8_848484, xsize - 47,  ysize - 23,  xsize - 47,   ysize -  4);
    boxfill8(vram, xsize, COL8_FFFFFF, xsize - 47,  ysize -  3,  xsize -  4,   ysize -  3);
    boxfill8(vram, xsize, COL8_FFFFFF, xsize -  3,  ysize - 24,  xsize -  3,   ysize -  3);

    for (;;) {
        io_hlt();
    }
    
}
```

これでOSっぽくなりました。   
達成感半端ないです！    

本では五日目に突入しました。   
asmhead.nasの値を取得するために少しbootpack.cを改造します。   
bootpack.c
```
void HariMain(void)
{
    char *vram;
    int xsize, ysize;
    short *binfo_scrnx, *binfo_scrny;
    int *binfo_vram;

    init_palette();
    binfo_scrnx = (short *) 0x0ff4;
    binfo_scrny = (short *) 0x0ff6;
    binfo_vram = (int *) 0x0ff8;
    xsize = *binfo_scrnx;
    ysize = *binfo_scrny;
    vram = (char *) *binfo_vram;
```

次は構造体を使用してHariMain関数の行数を削りました。   

```
struct BOOTINFO {
    char cyls, leds, vmode, reserve;
    short scrnx, scrny;
    char *vram;
};

void HariMain(void)
{
    char *vram;
    int xsize, ysize;
    struct BOOTINFO *binfo;

    init_palette();
    binfo = (struct BOOTINFO *) 0x0ff0;
    xsize = (*binfo).scrnx;
    ysize = (*binfo).scrny;
    vram = (*binfo).vram;
```

あまり削れてないような。
さら削るためにアロー演算子を使ってみます   

```
void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) 0x0ff0;
 
    init_palette();
    init_screen(binfo->vram, binfo->scrnx, binfo->scrny);

    for (;;) {
        io_hlt();
    }
    
}
```

またinit_screen関数を定義してboxfill8(vram, x, COL8_008484,  0,          0,       x - 1,    y - 29);の文を小分けにしました。

```
void init_screen(char *vram, int x, int y)
{
    boxfill8(vram, x, COL8_008484,  0,          0,       x - 1,    y - 29);
    boxfill8(vram, x, COL8_C6C6C6,  0,          y - 28,  x - 1,    y - 28);
    boxfill8(vram, x, COL8_FFFFFF,  0,          y - 27,  x - 1,    y - 27);
    boxfill8(vram, x, COL8_C6C6C6,  0,          y - 26,  x - 1,    y -  1);

    boxfill8(vram, x, COL8_FFFFFF,  3,          y - 24,  59,       y - 24);
    boxfill8(vram, x, COL8_FFFFFF,  2,          y - 24,   2,       y -  4);
    boxfill8(vram, x, COL8_848484,  3,          y -  4,  59,       y -  4);
    boxfill8(vram, x, COL8_848484, 59,          y - 23,  59,       y -  5);
    boxfill8(vram, x, COL8_000000,  2,          y -  3,  59,       y -  3);
    boxfill8(vram, x, COL8_000000, 60,          y - 24,  60,       y -  3);

    boxfill8(vram, x, COL8_848484, x - 47,      y - 24,  x -  4,   y - 24);
    boxfill8(vram, x, COL8_848484, x - 47,      y - 23,  x - 47,   y -  4);
    boxfill8(vram, x, COL8_FFFFFF, x - 47,      y -  3,  x -  4,   y -  3);
    boxfill8(vram, x, COL8_FFFFFF, x -  3,      y - 24,  x -  3,   y -  3);
    return;
}
```

プロト関数も忘れずにつけましょう。   
```
void init_palette(void);
void set_palette(int start, int end, unsigned char *rgd);
void boxfill8(unsigned char *vram, int xsize, unsigned char c, int x0, int y0, int x1, int y1);
void init_screen(char *vram, int x, int y);
```

**実装過程**   
・　[57a54599e9dedb9733ffbfdcb8a75f72732b31c7](57a54599e9dedb9733ffbfdcb8a75f72732b31c7)

## day5
文字を出力させるためにbootpack.cを修正ます。   
今回はAを出します。   

```
void io_hlt(void);
void io_cli(void);
void io_out8(int port, int data);
int io_load_eflags(void);
void io_store_eflags(int eflags);

void init_palette(void);
void set_palette(int start, int end, unsigned char *rgd);
void boxfill8(unsigned char *vram, int xsize, unsigned char c, int x0, int y0, int x1, int y1);
void init_screen(char *vram, int x, int y);
void putfont8(char *vram, int xsize, int x, int y, char c, char *font);

#define COL8_000000  0
#define COL8_FF0000  1
#define COL8_00FF00  2
#define COL8_FFFF00  3
#define COL8_0000FF  4
#define COL8_FF00FF  5
#define COL8_00FFFF  6
#define COL8_FFFFFF  7
#define COL8_C6C6C6  8
#define COL8_840000  9
#define COL8_008400  10
#define COL8_848400  11
#define COL8_000084  12
#define COL8_840084  13
#define COL8_008484  14
#define COL8_848484  15

struct BOOTINFO {
    char cyls, leds, vmode, reserve;
    short scrnx, scrny;
    char *vram;
};

void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) 0x0ff0;
    static char font_A[16] = {
        0x00, 0x18, 0x18, 0x18, 0x24, 0x24, 0x24,
        0x24, 0x7e, 0x42, 0x42, 0xe7, 0x00, 0x00    
    };
 
    init_palette();
    init_screen(binfo->vram, binfo->scrnx, binfo->scrny);
    putfont8(binfo->vram, binfo->scrnx, 10, 10, COL8_FFFFFF, font_A);

    for (;;) {
        io_hlt();
    }
    
}

void init_palette(void)
{
    static unsigned char table_rgb[16 * 3] = {
        0x00, 0x00, 0x00,   /* 0:black */
        0xff, 0x00, 0x00,   /* 1:light red */
        0x00, 0xff, 0x00,   /* 2:light green */
        0xff, 0xff, 0x00,   /* 3:light yellow */
        0x00, 0x00, 0xff,   /* 4:light blue */
        0xff, 0x00, 0xff,   /* 5:light puple */
        0x00, 0xff, 0xff,   /* 6:light sky blue */
        0xff, 0xff, 0xff,   /* 7:white */
        0xc6, 0xc6, 0xc6,   /* 8:light grey */
        0x84, 0x00, 0x00,   /* 9:dark green */
        0x00, 0x84, 0x00,   /* 10:dark green */
        0x84, 0x84, 0x00,   /* 11:dark yellow */
        0x00, 0x00, 0x84,   /* 12:dark blue */
        0x84, 0x00, 0x84,   /* 13:dark puple */
        0x00, 0x84, 0x84,   /* 14:dark sky blue */
        0x84, 0x84, 0x84    /* 15:dark grey */
    };
    set_palette(0, 15, table_rgb);
    return;
}

void set_palette(int start, int end, unsigned char *rgb)
{
    int i, eflags;
    eflags = io_load_eflags();
    io_cli();
    io_out8(0x03c8, start);
    for (i = start; i <= end; i++) {
        io_out8(0x03c9, rgb[0] / 4);
        io_out8(0x03c9, rgb[1] / 4);
        io_out8(0x03c9, rgb[2] / 4);
        rgb += 3;
    }
    io_store_eflags(eflags);
    return;
}

void boxfill8(unsigned char *vram, int xsize, unsigned char c, int x0, int y0, int x1, int y1)
{
    int x, y;
    for (y= y0; y <= y1; y++) {
        for (x = x0; x <= x1; x++)
            vram[y * xsize + x] = c;
    }
    return;
}

void init_screen(char *vram, int x, int y)
{
    boxfill8(vram, x, COL8_008484,  0,          0,       x - 1,    y - 29);
    boxfill8(vram, x, COL8_C6C6C6,  0,          y - 28,  x - 1,    y - 28);
    boxfill8(vram, x, COL8_FFFFFF,  0,          y - 27,  x - 1,    y - 27);
    boxfill8(vram, x, COL8_C6C6C6,  0,          y - 26,  x - 1,    y -  1);

    boxfill8(vram, x, COL8_FFFFFF,  3,          y - 24,  59,       y - 24);
    boxfill8(vram, x, COL8_FFFFFF,  2,          y - 24,   2,       y -  4);
    boxfill8(vram, x, COL8_848484,  3,          y -  4,  59,       y -  4);
    boxfill8(vram, x, COL8_848484, 59,          y - 23,  59,       y -  5);
    boxfill8(vram, x, COL8_000000,  2,          y -  3,  59,       y -  3);
    boxfill8(vram, x, COL8_000000, 60,          y - 24,  60,       y -  3);

    boxfill8(vram, x, COL8_848484, x - 47,      y - 24,  x -  4,   y - 24);
    boxfill8(vram, x, COL8_848484, x - 47,      y - 23,  x - 47,   y -  4);
    boxfill8(vram, x, COL8_FFFFFF, x - 47,      y -  3,  x -  4,   y -  3);
    boxfill8(vram, x, COL8_FFFFFF, x -  3,      y - 24,  x -  3,   y -  3);
    return;
}

void putfont8(char *vram, int xsize, int x, int y, char c, char *font)
{
    int i;
    char *p, d;
    for (i = 0; i < 16; i++) {
        p = vram + (y + i) * xsize + x;
        d = font[i];
        if ((d & 0x80) != 0) { p[0] = c; }
        if ((d & 0x40) != 0) { p[1] = c; }
        if ((d & 0x20) != 0) { p[2] = c; }
        if ((d & 0x10) != 0) { p[3] = c; }
        if ((d & 0x08) != 0) { p[4] = c; }
        if ((d & 0x04) != 0) { p[5] = c; }
        if ((d & 0x02) != 0) { p[6] = c; }
        if ((d & 0x01) != 0) { p[7] = c; }
    }
    return ;
}
```

make runしてAが出ました！   
やりました！   

つぎは文字を大量に羅列したいのでhankaku.txtというファイルをします。これは大きなファイルのでここに記載しません。githubからダウンロードしてきてください。   
また、hankaku.txtをhankaku.objに変更するためにMakefileを書き換えます。   

```Makefile
TOOLPATH = ..\..\z_tools\\

INCPATH  = ..\..\z_tools\haribote\\

MAKE     = $(TOOLPATH)make.exe -r
NASK     = $(TOOLPATH)nask.exe
CC1		 = $(TOOLPATH)cc1.exe -I$(INCPATH) -Os -Wall -quiet
GAS2NASK = $(TOOLPATH)gas2nask.exe -a
OBJ2BIM  = $(TOOLPATH)obj2bim.exe
MAKEFONT = $(TOOLPATH)makefont.exe
BIN2OBJ  = $(TOOLPATH)bin2obj
BIM2HRB  = $(TOOLPATH)bim2hrb.exe
RULEFILE = $(TOOLPATH)haribote\haribote.rul
EDIMG    = $(TOOLPATH)edimg.exe
IMGTOL   = $(TOOLPATH)imgtol.com
COPY     = cmd.exe /C copy
DEL      = rm -f


# default operation
default:
	$(MAKE) img

# file making rule

ipl10.bin : ipl10.nas Makefile
	$(NASK) ipl10.nas ipl10.bin ipl10.lst

asmhead.bin : asmhead.nas Makefile
	$(NASK) asmhead.nas asmhead.bin asmhead.lst

bootpack.gas : bootpack.c Makefile
	$(CC1) -o bootpack.gas bootpack.c

bootpack.nas : bootpack.gas Makefile
	$(GAS2NASK) bootpack.gas  bootpack.nas 

bootpack.obj : bootpack.nas Makefile
	$(NASK) bootpack.nas bootpack.obj bootpack.lst

naskfunc.obj : naskfunc.nas Makefile
	$(NASK) naskfunc.nas naskfunc.obj naskfunc.lst

hankaku.bin : hankaku.txt Makefile
	$(MAKEFONT) hankaku.txt hankaku.bin

hankaku.obj : hankaku.bin Makefile
	$(BIN2OBJ) hankaku.bin hankaku.obj _hankaku

bootpack.bim : bootpack.obj naskfunc.obj hankaku.obj Makefile
	$(OBJ2BIM) @$(RULEFILE) out:bootpack.bim stack:3136k map:bootpack.map \
		bootpack.obj naskfunc.obj hankaku.obj
# 3MB+64KB=3136KB

bootpack.hrb : bootpack.bim Makefile
	$(BIM2HRB) bootpack.bim bootpack.hrb 0

haribote.sys : asmhead.bin bootpack.hrb Makefile
	cat asmhead.bin >  haribote.sys
	cat bootpack.hrb >> haribote.sys

haribote.img : ipl10.bin haribote.sys Makefile
	$(EDIMG)   imgin:..\..\z_tools\fdimg0at.tek \
		wbinimg src:ipl10.bin len:512 from:0 to:0 \
		copy from:haribote.sys to:@: \
		imgout:haribote.img

# command

img :
	$(MAKE) haribote.img

run :
	$(MAKE) img
	$(COPY) haribote.img ..\..\z_tools\qemu\fdimage0.bin
	$(MAKE) -C ..\..\z_tools\qemu

install :
	$(MAKE) img
	$(IMGTOL) w a: haribote.img

clean :
	-$(DEL) *.bin
	-$(DEL) *.lst
	-$(DEL) *.gas
	-$(DEL) *.obj
	-$(DEL) bootpack.nas
	-$(DEL) bootpack.map
	-$(DEL) bootpack.bim
	-$(DEL) bootpack.hrb
	-$(DEL) haribote.sys

src_only :
	$(MAKE) clean
	-$(DEL) haribote.img
```

ABC123と出力させるためにHariMainを書き換えます。
```c
void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) 0x0ff0;
    extern char hankaku[4096];
 
    init_palette();
    init_screen(binfo->vram, binfo->scrnx, binfo->scrny);
    putfont8(binfo->vram, binfo->scrnx,  8, 8, COL8_FFFFFF, hankaku + 'A' * 16);
    putfont8(binfo->vram, binfo->scrnx, 16, 8, COL8_FFFFFF, hankaku + 'B' * 16);
    putfont8(binfo->vram, binfo->scrnx, 24, 8, COL8_FFFFFF, hankaku + 'C' * 16);
    putfont8(binfo->vram, binfo->scrnx, 40, 8, COL8_FFFFFF, hankaku + '1' * 16);
    putfont8(binfo->vram, binfo->scrnx, 48, 8, COL8_FFFFFF, hankaku + '2' * 16);
    putfont8(binfo->vram, binfo->scrnx, 56, 8, COL8_FFFFFF, hankaku + '3' * 16);

    for (;;) {
        io_hlt();
    }
    
}
```

これでABC123が画面に出力されました！   
すごいすごい！   

いちいち一文字ずつ関数を呼び出すのはめんどくさいので新しく関数を定義してみます。   
```c
void io_hlt(void);
void io_cli(void);
void io_out8(int port, int data);
int io_load_eflags(void);
void io_store_eflags(int eflags);

void init_palette(void);
void set_palette(int start, int end, unsigned char *rgd);
void boxfill8(unsigned char *vram, int xsize, unsigned char c, int x0, int y0, int x1, int y1);
void init_screen(char *vram, int x, int y);
void putfont8(char *vram, int xsize, int x, int y, char c, char *font);
void putfonts8_asc(char *vram, int xsize, int x, int y, char c, unsigned char *s);

#define COL8_000000  0
#define COL8_FF0000  1
#define COL8_00FF00  2
#define COL8_FFFF00  3
#define COL8_0000FF  4
#define COL8_FF00FF  5
#define COL8_00FFFF  6
#define COL8_FFFFFF  7
#define COL8_C6C6C6  8
#define COL8_840000  9
#define COL8_008400  10
#define COL8_848400  11
#define COL8_000084  12
#define COL8_840084  13
#define COL8_008484  14
#define COL8_848484  15

struct BOOTINFO {
    char cyls, leds, vmode, reserve;
    short scrnx, scrny;
    char *vram;
};

void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) 0x0ff0;
    extern char hankaku[4096];
 
    init_palette();
    init_screen(binfo->vram, binfo->scrnx, binfo->scrny);
    putfonts8_asc(binfo->vram, binfo->scrnx,   8,  8, COL8_FFFFFF, "ABC 123");
    putfonts8_asc(binfo->vram, binfo->scrnx,  31, 31, COL8_000000, "Haribote OS.");
    putfonts8_asc(binfo->vram, binfo->scrnx,  30, 30, COL8_FFFFFF, "Haribote OS.");

    for (;;) {
        io_hlt();
    }
    
}

void init_palette(void)
{
    static unsigned char table_rgb[16 * 3] = {
        0x00, 0x00, 0x00,   /* 0:black */
        0xff, 0x00, 0x00,   /* 1:light red */
        0x00, 0xff, 0x00,   /* 2:light green */
        0xff, 0xff, 0x00,   /* 3:light yellow */
        0x00, 0x00, 0xff,   /* 4:light blue */
        0xff, 0x00, 0xff,   /* 5:light puple */
        0x00, 0xff, 0xff,   /* 6:light sky blue */
        0xff, 0xff, 0xff,   /* 7:white */
        0xc6, 0xc6, 0xc6,   /* 8:light grey */
        0x84, 0x00, 0x00,   /* 9:dark green */
        0x00, 0x84, 0x00,   /* 10:dark green */
        0x84, 0x84, 0x00,   /* 11:dark yellow */
        0x00, 0x00, 0x84,   /* 12:dark blue */
        0x84, 0x00, 0x84,   /* 13:dark puple */
        0x00, 0x84, 0x84,   /* 14:dark sky blue */
        0x84, 0x84, 0x84    /* 15:dark grey */
    };
    set_palette(0, 15, table_rgb);
    return;
}

void set_palette(int start, int end, unsigned char *rgb)
{
    int i, eflags;
    eflags = io_load_eflags();
    io_cli();
    io_out8(0x03c8, start);
    for (i = start; i <= end; i++) {
        io_out8(0x03c9, rgb[0] / 4);
        io_out8(0x03c9, rgb[1] / 4);
        io_out8(0x03c9, rgb[2] / 4);
        rgb += 3;
    }
    io_store_eflags(eflags);
    return;
}

void boxfill8(unsigned char *vram, int xsize, unsigned char c, int x0, int y0, int x1, int y1)
{
    int x, y;
    for (y= y0; y <= y1; y++) {
        for (x = x0; x <= x1; x++)
            vram[y * xsize + x] = c;
    }
    return;
}

void init_screen(char *vram, int x, int y)
{
    boxfill8(vram, x, COL8_008484,  0,          0,       x - 1,    y - 29);
    boxfill8(vram, x, COL8_C6C6C6,  0,          y - 28,  x - 1,    y - 28);
    boxfill8(vram, x, COL8_FFFFFF,  0,          y - 27,  x - 1,    y - 27);
    boxfill8(vram, x, COL8_C6C6C6,  0,          y - 26,  x - 1,    y -  1);

    boxfill8(vram, x, COL8_FFFFFF,  3,          y - 24,  59,       y - 24);
    boxfill8(vram, x, COL8_FFFFFF,  2,          y - 24,   2,       y -  4);
    boxfill8(vram, x, COL8_848484,  3,          y -  4,  59,       y -  4);
    boxfill8(vram, x, COL8_848484, 59,          y - 23,  59,       y -  5);
    boxfill8(vram, x, COL8_000000,  2,          y -  3,  59,       y -  3);
    boxfill8(vram, x, COL8_000000, 60,          y - 24,  60,       y -  3);

    boxfill8(vram, x, COL8_848484, x - 47,      y - 24,  x -  4,   y - 24);
    boxfill8(vram, x, COL8_848484, x - 47,      y - 23,  x - 47,   y -  4);
    boxfill8(vram, x, COL8_FFFFFF, x - 47,      y -  3,  x -  4,   y -  3);
    boxfill8(vram, x, COL8_FFFFFF, x -  3,      y - 24,  x -  3,   y -  3);
    return;
}

void putfont8(char *vram, int xsize, int x, int y, char c, char *font)
{
    int i;
    char *p, d;
    for (i = 0; i < 16; i++) {
        p = vram + (y + i) * xsize + x;
        d = font[i];
        if ((d & 0x80) != 0) { p[0] = c; }
        if ((d & 0x40) != 0) { p[1] = c; }
        if ((d & 0x20) != 0) { p[2] = c; }
        if ((d & 0x10) != 0) { p[3] = c; }
        if ((d & 0x08) != 0) { p[4] = c; }
        if ((d & 0x04) != 0) { p[5] = c; }
        if ((d & 0x02) != 0) { p[6] = c; }
        if ((d & 0x01) != 0) { p[7] = c; }
    }
    return ;
}

void putfonts8_asc(char *vram, int xsize, int x, int y, char c, unsigned char *s)
{
    extern char hankaku[4096];
    for (; *s != 0x00; s++) {
        putfont8(vram, xsize, x, y, c, hankaku + *s * 16);
        x += 8;
    }
    return;
}
```

これで文字の出力が楽になりました。   

次は変数をみれるように改造します。   
sprintfを使います。   
```c
#include <stdio.h>

void io_hlt(void);
void io_cli(void);
void io_out8(int port, int data);
int io_load_eflags(void);
void io_store_eflags(int eflags);

void init_palette(void);
void set_palette(int start, int end, unsigned char *rgd);
void boxfill8(unsigned char *vram, int xsize, unsigned char c, int x0, int y0, int x1, int y1);
void init_screen(char *vram, int x, int y);
void putfont8(char *vram, int xsize, int x, int y, char c, char *font);
void putfonts8_asc(char *vram, int xsize, int x, int y, char c, unsigned char *s);

#define COL8_000000  0
#define COL8_FF0000  1
#define COL8_00FF00  2
#define COL8_FFFF00  3
#define COL8_0000FF  4
#define COL8_FF00FF  5
#define COL8_00FFFF  6
#define COL8_FFFFFF  7
#define COL8_C6C6C6  8
#define COL8_840000  9
#define COL8_008400  10
#define COL8_848400  11
#define COL8_000084  12
#define COL8_840084  13
#define COL8_008484  14
#define COL8_848484  15

struct BOOTINFO {
    char cyls, leds, vmode, reserve;
    short scrnx, scrny;
    char *vram;
};

void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) 0x0ff0;
    char s[40];
 
    init_palette();
    init_screen(binfo->vram, binfo->scrnx, binfo->scrny);
    putfonts8_asc(binfo->vram, binfo->scrnx,   8,  8, COL8_FFFFFF, "ABC 123");
    putfonts8_asc(binfo->vram, binfo->scrnx,  31, 31, COL8_000000, "Haribote OS.");
    putfonts8_asc(binfo->vram, binfo->scrnx,  30, 30, COL8_FFFFFF, "Haribote OS.");
    sprintf(s, "scrnx = %d", binfo->scrnx);
    putfonts8_asc(binfo->vram, binfo->scrnx, 16, 64, COL8_FFFFFF, s);

    for (;;) {
        io_hlt();
    }
    
}
```

次はマウスカーソルを表示させたいのでさらにソースコードに手を加えていきます。   
```c
#include <stdio.h>

void io_hlt(void);
void io_cli(void);
void io_out8(int port, int data);
int io_load_eflags(void);
void io_store_eflags(int eflags);

void init_palette(void);
void set_palette(int start, int end, unsigned char *rgd);
void boxfill8(unsigned char *vram, int xsize, unsigned char c, int x0, int y0, int x1, int y1);
void init_screen(char *vram, int x, int y);
void putfont8(char *vram, int xsize, int x, int y, char c, char *font);
void putfonts8_asc(char *vram, int xsize, int x, int y, char c, unsigned char *s);
void init_mouse_cursor8(char *mouse, char bc);
void putblock8_8(char *vram, int vxsize, int pxsize,
    int pysize, int px0, int py0, char *buf, int bxsize);

#define COL8_000000  0
#define COL8_FF0000  1
#define COL8_00FF00  2
#define COL8_FFFF00  3
#define COL8_0000FF  4
#define COL8_FF00FF  5
#define COL8_00FFFF  6
#define COL8_FFFFFF  7
#define COL8_C6C6C6  8
#define COL8_840000  9
#define COL8_008400  10
#define COL8_848400  11
#define COL8_000084  12
#define COL8_840084  13
#define COL8_008484  14
#define COL8_848484  15

struct BOOTINFO {
    char cyls, leds, vmode, reserve;
    short scrnx, scrny;
    char *vram;
};

void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) 0x0ff0;
    char s[40], mcursor[256];
    int mx, my;
 
    init_palette();
    init_screen(binfo->vram, binfo->scrnx, binfo->scrny);
    mx = (binfo->scrnx - 16) / 2;
    my = (binfo->scrny - 28 - 16) / 2;
    init_mouse_cursor8(mcursor, COL8_008484);
    putblock8_8(binfo->vram, binfo->scrnx, 16, 16, mx, my, mcursor, 16);
    sprintf(s, "(%d, %d)", mx, my);
    putfonts8_asc(binfo->vram, binfo->scrnx, 0, 0, COL8_FFFFFF, s);

    for (;;) {
        io_hlt();
    }
    
}

void init_palette(void)
{
    static unsigned char table_rgb[16 * 3] = {
        0x00, 0x00, 0x00,   /* 0:black */
        0xff, 0x00, 0x00,   /* 1:light red */
        0x00, 0xff, 0x00,   /* 2:light green */
        0xff, 0xff, 0x00,   /* 3:light yellow */
        0x00, 0x00, 0xff,   /* 4:light blue */
        0xff, 0x00, 0xff,   /* 5:light puple */
        0x00, 0xff, 0xff,   /* 6:light sky blue */
        0xff, 0xff, 0xff,   /* 7:white */
        0xc6, 0xc6, 0xc6,   /* 8:light grey */
        0x84, 0x00, 0x00,   /* 9:dark green */
        0x00, 0x84, 0x00,   /* 10:dark green */
        0x84, 0x84, 0x00,   /* 11:dark yellow */
        0x00, 0x00, 0x84,   /* 12:dark blue */
        0x84, 0x00, 0x84,   /* 13:dark puple */
        0x00, 0x84, 0x84,   /* 14:dark sky blue */
        0x84, 0x84, 0x84    /* 15:dark grey */
    };
    set_palette(0, 15, table_rgb);
    return;
}

void set_palette(int start, int end, unsigned char *rgb)
{
    int i, eflags;
    eflags = io_load_eflags();
    io_cli();
    io_out8(0x03c8, start);
    for (i = start; i <= end; i++) {
        io_out8(0x03c9, rgb[0] / 4);
        io_out8(0x03c9, rgb[1] / 4);
        io_out8(0x03c9, rgb[2] / 4);
        rgb += 3;
    }
    io_store_eflags(eflags);
    return;
}

void boxfill8(unsigned char *vram, int xsize, unsigned char c, int x0, int y0, int x1, int y1)
{
    int x, y;
    for (y= y0; y <= y1; y++) {
        for (x = x0; x <= x1; x++)
            vram[y * xsize + x] = c;
    }
    return;
}

void init_screen(char *vram, int x, int y)
{
    boxfill8(vram, x, COL8_008484,  0,          0,       x - 1,    y - 29);
    boxfill8(vram, x, COL8_C6C6C6,  0,          y - 28,  x - 1,    y - 28);
    boxfill8(vram, x, COL8_FFFFFF,  0,          y - 27,  x - 1,    y - 27);
    boxfill8(vram, x, COL8_C6C6C6,  0,          y - 26,  x - 1,    y -  1);

    boxfill8(vram, x, COL8_FFFFFF,  3,          y - 24,  59,       y - 24);
    boxfill8(vram, x, COL8_FFFFFF,  2,          y - 24,   2,       y -  4);
    boxfill8(vram, x, COL8_848484,  3,          y -  4,  59,       y -  4);
    boxfill8(vram, x, COL8_848484, 59,          y - 23,  59,       y -  5);
    boxfill8(vram, x, COL8_000000,  2,          y -  3,  59,       y -  3);
    boxfill8(vram, x, COL8_000000, 60,          y - 24,  60,       y -  3);

    boxfill8(vram, x, COL8_848484, x - 47,      y - 24,  x -  4,   y - 24);
    boxfill8(vram, x, COL8_848484, x - 47,      y - 23,  x - 47,   y -  4);
    boxfill8(vram, x, COL8_FFFFFF, x - 47,      y -  3,  x -  4,   y -  3);
    boxfill8(vram, x, COL8_FFFFFF, x -  3,      y - 24,  x -  3,   y -  3);
    return;
}

void putfont8(char *vram, int xsize, int x, int y, char c, char *font)
{
    int i;
    char *p, d;
    for (i = 0; i < 16; i++) {
        p = vram + (y + i) * xsize + x;
        d = font[i];
        if ((d & 0x80) != 0) { p[0] = c; }
        if ((d & 0x40) != 0) { p[1] = c; }
        if ((d & 0x20) != 0) { p[2] = c; }
        if ((d & 0x10) != 0) { p[3] = c; }
        if ((d & 0x08) != 0) { p[4] = c; }
        if ((d & 0x04) != 0) { p[5] = c; }
        if ((d & 0x02) != 0) { p[6] = c; }
        if ((d & 0x01) != 0) { p[7] = c; }
    }
    return ;
}

void putfonts8_asc(char *vram, int xsize, int x, int y, char c, unsigned char *s)
{
    extern char hankaku[4096];
    for (; *s != 0x00; s++) {
        putfont8(vram, xsize, x, y, c, hankaku + *s * 16);
        x += 8;
    }
    return;
}

void init_mouse_cursor8(char *mouse, char bc)
{
    static char cursor[16][16] = {
        "**************..",
        "*OOOOOOOOOOO*...",
        "*OOOOOOOOOO*....",
        "*OOOOOOOOO*.....",
        "*OOOOOOOO*......",
        "*OOOOOO*........",
        "*OOOOOO*........",
        "*OOOOOOO*.......",
        "*OOOO**OOO*.....",
        "*OOO*..*OOO*....",
        "*OO*....*OOO*...",
        "*O*......*OOO*..",
        "**........*OOO*.",
        "*..........*OOO*",
        "............*OO*",
        ".............***"
    };
    int x, y;

    for (y = 0; y < 16; y++) {
        for (x = 0; x < 16; x++) {
            if (cursor[y][x] == '*') {
                mouse[y * 16 + x] = COL8_000000;
            }
            if (cursor[y][x] == 'O') {
                mouse[y + 16 + x] = COL8_FFFFFF;
            }
            if (cursor[y][x] == '.') {
                mouse[y * 16 + x] = bc;
            }
        }
    }
    return;
}

void putblock8_8(char *vram, int vxsize, int pxsize,
    int pysize, int px0, int py0, char *buf, int bxsize)
{
    int x, y;
    for (y = 0; y < pysize; y++) {
        for (x = 0; x < pxsize; x++) {
            vram[(py0 + y) * vxsize + (px0 + x)] = buf[y * bxsize + x];
        }
    }
    return; 
}
```

これでマウスカーソルが出力されました。

**実装過程**   
・　[917d43b0ce87e6d93f346fbf9b6b742aa09c6ecd](917d43b0ce87e6d93f346fbf9b6b742aa09c6ecd)

## day6
割り込み命令を作るために、GDT/IDTから作成していきます。   
bootpack.c
```c
struct SEGMENT_DESCRIPTOR {
    short limit_low, base_low;
    char base_mid, access_right;
    char limit_high, base_high;
};

struct GATE_DESCRIPTOR {
    short offset_low, selector;
    char dw_count, access_right;
    short offset_high;
};

void init_gdtidt(void);
void set_segmdesc(struct SEGMENT_DESCRIPTOR *sd, unsigned int limit, int base, int ar);
void set_gatedesc(struct GATE_DESCRIPTOR *gd, int offset, int selector, int ar);
void load_gdtr(int limit, int addr);
void load_idtr(int limit, int addr);
```

```c
void init_gdtidt(void) {
    struct SEGMENT_DESCRIPTOR *gdt = (struct SEGMENT_DESCRIPTOR *) 0x00270000;
    struct GATE_DESCRIPTOR *idt = (struct GATE_DESCRIPTOR *) 0x0026f800;
    int i;

    for (i = 0; i < 8192; i++) {
        set_segmdesc(gdt + i, 0, 0, 0);
    }

    set_segmdesc(gdt + 1, 0xffffffff, 0x00000000, 0x4092);
    set_segmdesc(gdt + 2, 0x0007ffff, 0x00280000, 0x409a);
    load_gdtr(0xffff, 0x00270000);

    for (i = 0; i < 256; i++) {
        set_gatedesc(idt + i, 0, 0, 0);
    }

    load_idtr(0x7ff, 0x0026f800);

    return;
}

void set_segmdesc(struct SEGMENT_DESCRIPTOR *sd, unsigned int limit, int base, int ar)
{
    if (limit > 0xffff) {
        ar |= 0x8000;
        limit /= 0x1000;
    }

    sd->limit_low = limit & 0xffff;
    sd->base_low = base & 0xffff;
    sd->base_mid = (base >> 16) & 0xff;
    sd->access_right = ar & 0xff;
    sd->limit_high = ((limit >> 16) & 0x0f) |  ((ar >> 8) & 0xf0);
    sd->base_high = (base >> 24) & 0xff;
    return;
}

void set_gatedesc(struct GATE_DESCRIPTOR *gd, int offset, int selector, int ar)
{
    gd->offset_low = offset & 0xffff;
    gd->selector = selector;
    gd->dw_count = (ar >> 8) & 0xff;
    gd->access_right = ar & 0xff;
    gd->offset_high = (offset >> 16) & 0xffff;
    return;
}
```

nasfunc.nas
```
GLOBAL  _load_gdtr, _load_idtr

_load_gdtr:
        MOV     AX,[ESP+4]
        MOV     [ESP+6],AX
        LGDT    [ESP+6]
        RET

_load_idtr:
        MOV     AX,[ESP+4]
        MOV     [ESP+6],AX
        LIDT    [ESP+6]
        RET
```

make runをしても前回と何も変わらないですが、これが6日目の布石になるみたいです。

ここからは6日目の内容になります。    
まずはbootpack.cが長すぎるのでファイルを分割しました。   
ついでにMakefileも改造しています。

bootpack.c
```c
TOOLPATH = ..\..\z_tools\\

INCPATH  = ..\..\z_tools\haribote\\

MAKE     = $(TOOLPATH)make.exe -r
NASK     = $(TOOLPATH)nask.exe
CC1		 = $(TOOLPATH)cc1.exe -I$(INCPATH) -Os -Wall -quiet
GAS2NASK = $(TOOLPATH)gas2nask.exe -a
OBJ2BIM  = $(TOOLPATH)obj2bim.exe
MAKEFONT = $(TOOLPATH)makefont.exe
BIN2OBJ  = $(TOOLPATH)bin2obj
BIM2HRB  = $(TOOLPATH)bim2hrb.exe
RULEFILE = $(TOOLPATH)haribote\haribote.rul
EDIMG    = $(TOOLPATH)edimg.exe
IMGTOL   = $(TOOLPATH)imgtol.com
COPY     = cmd.exe /C copy
DEL      = rm -f


# default operation
default:
	$(MAKE) img

# file making rule

ipl10.bin : ipl10.nas Makefile
	$(NASK) ipl10.nas ipl10.bin ipl10.lst

asmhead.bin : asmhead.nas Makefile
	$(NASK) asmhead.nas asmhead.bin asmhead.lst

bootpack.gas : bootpack.c Makefile
	$(CC1) -o bootpack.gas bootpack.c

bootpack.nas : bootpack.gas Makefile
	$(GAS2NASK) bootpack.gas  bootpack.nas 

bootpack.obj : bootpack.nas Makefile
	$(NASK) bootpack.nas bootpack.obj bootpack.lst

naskfunc.obj : naskfunc.nas Makefile
	$(NASK) naskfunc.nas naskfunc.obj naskfunc.lst

hankaku.bin : hankaku.txt Makefile
	$(MAKEFONT) hankaku.txt hankaku.bin

hankaku.obj : hankaku.bin Makefile
	$(BIN2OBJ) hankaku.bin hankaku.obj _hankaku

graphic.gas : graphic.c Makefile
	$(CC1) -o graphic.gas graphic.c

graphic.nas : graphic.gas Makefile
	$(GAS2NASK) graphic.gas graphic.nas

graphic.obj : graphic.nas Makefile
	$(NASK) graphic.nas graphic.obj graphic.lst

dsctbl.gas : dsctbl.c Makefile
	$(CC1) -o dsctbl.gas dsctbl.c

dsctbl.nas : dsctbl.gas Makefile
	$(GAS2NASK) dsctbl.gas dsctbl.nas

dsctbl.obj : dsctbl.nas Makefile
	$(NASK) dsctbl.nas dsctbl.obj dsctbl.lst

bootpack.bim : bootpack.obj naskfunc.obj hankaku.obj graphic.obj dsctbl.obj Makefile
	$(OBJ2BIM) @$(RULEFILE) out:bootpack.bim stack:3136k map:bootpack.map \
		bootpack.obj naskfunc.obj hankaku.obj graphic.obj dsctbl.obj
# 3MB+64KB=3136KB

bootpack.hrb : bootpack.bim Makefile
	$(BIM2HRB) bootpack.bim bootpack.hrb 0

haribote.sys : asmhead.bin bootpack.hrb Makefile
	cat asmhead.bin >  haribote.sys
	cat bootpack.hrb >> haribote.sys

haribote.img : ipl10.bin haribote.sys Makefile
	$(EDIMG)   imgin:..\..\z_tools\fdimg0at.tek \
		wbinimg src:ipl10.bin len:512 from:0 to:0 \
		copy from:haribote.sys to:@: \
		imgout:haribote.img

# command

img :
	$(MAKE) haribote.img

run :
	$(MAKE) img
	$(COPY) haribote.img ..\..\z_tools\qemu\fdimage0.bin
	$(MAKE) -C ..\..\z_tools\qemu

install :
	$(MAKE) img
	$(IMGTOL) w a: haribote.img

clean :
	-$(DEL) *.bin
	-$(DEL) *.lst
	-$(DEL) *.gas
	-$(DEL) *.obj
	-$(DEL) bootpack.nas
	-$(DEL) bootpack.map
	-$(DEL) bootpack.bim
	-$(DEL) bootpack.hrb
	-$(DEL) haribote.sys

src_only :
	$(MAKE) clean
	-$(DEL) haribote.img
```

graphic.c
```c
/* graphic process */

void io_hlt(void);
void io_cli(void);
void io_out8(int port, int data);
int io_load_eflags(void);
void io_store_eflags(int eflags);

void init_palette(void);
void set_palette(int start, int end, unsigned char *rgd);
void boxfill8(unsigned char *vram, int xsize, unsigned char c, int x0, int y0, int x1, int y1);
void init_screen(char *vram, int x, int y);
void putfont8(char *vram, int xsize, int x, int y, char c, char *font);
void putfonts8_asc(char *vram, int xsize, int x, int y, char c, unsigned char *s);
void init_mouse_cursor8(char *mouse, char bc);
void putblock8_8(char *vram, int vxsize, int pxsize,
    int pysize, int px0, int py0, char *buf, int bxsize);

#define COL8_000000  0
#define COL8_FF0000  1
#define COL8_00FF00  2
#define COL8_FFFF00  3
#define COL8_0000FF  4
#define COL8_FF00FF  5
#define COL8_00FFFF  6
#define COL8_FFFFFF  7
#define COL8_C6C6C6  8
#define COL8_840000  9
#define COL8_008400  10
#define COL8_848400  11
#define COL8_000084  12
#define COL8_840084  13
#define COL8_008484  14
#define COL8_848484  15

void init_palette(void)
{
    static unsigned char table_rgb[16 * 3] = {
        0x00, 0x00, 0x00,   /* 0:black */
        0xff, 0x00, 0x00,   /* 1:light red */
        0x00, 0xff, 0x00,   /* 2:light green */
        0xff, 0xff, 0x00,   /* 3:light yellow */
        0x00, 0x00, 0xff,   /* 4:light blue */
        0xff, 0x00, 0xff,   /* 5:light puple */
        0x00, 0xff, 0xff,   /* 6:light sky blue */
        0xff, 0xff, 0xff,   /* 7:white */
        0xc6, 0xc6, 0xc6,   /* 8:light grey */
        0x84, 0x00, 0x00,   /* 9:dark green */
        0x00, 0x84, 0x00,   /* 10:dark green */
        0x84, 0x84, 0x00,   /* 11:dark yellow */
        0x00, 0x00, 0x84,   /* 12:dark blue */
        0x84, 0x00, 0x84,   /* 13:dark puple */
        0x00, 0x84, 0x84,   /* 14:dark sky blue */
        0x84, 0x84, 0x84    /* 15:dark grey */
    };
    set_palette(0, 15, table_rgb);
    return;
}

void set_palette(int start, int end, unsigned char *rgb)
{
    int i, eflags;
    eflags = io_load_eflags();
    io_cli();
    io_out8(0x03c8, start);
    for (i = start; i <= end; i++) {
        io_out8(0x03c9, rgb[0] / 4);
        io_out8(0x03c9, rgb[1] / 4);
        io_out8(0x03c9, rgb[2] / 4);
        rgb += 3;
    }
    io_store_eflags(eflags);
    return;
} 

void boxfill8(unsigned char *vram, int xsize, unsigned char c, int x0, int y0, int x1, int y1)
{
    int x, y;
    for (y= y0; y <= y1; y++) {
        for (x = x0; x <= x1; x++)
            vram[y * xsize + x] = c;
    }
    return;
}

void init_screen(char *vram, int x, int y)
{
    boxfill8(vram, x, COL8_008484,  0,          0,       x - 1,    y - 29);
    boxfill8(vram, x, COL8_C6C6C6,  0,          y - 28,  x - 1,    y - 28);
    boxfill8(vram, x, COL8_FFFFFF,  0,          y - 27,  x - 1,    y - 27);
    boxfill8(vram, x, COL8_C6C6C6,  0,          y - 26,  x - 1,    y -  1);

    boxfill8(vram, x, COL8_FFFFFF,  3,          y - 24,  59,       y - 24);
    boxfill8(vram, x, COL8_FFFFFF,  2,          y - 24,   2,       y -  4);
    boxfill8(vram, x, COL8_848484,  3,          y -  4,  59,       y -  4);
    boxfill8(vram, x, COL8_848484, 59,          y - 23,  59,       y -  5);
    boxfill8(vram, x, COL8_000000,  2,          y -  3,  59,       y -  3);
    boxfill8(vram, x, COL8_000000, 60,          y - 24,  60,       y -  3);

    boxfill8(vram, x, COL8_848484, x - 47,      y - 24,  x -  4,   y - 24);
    boxfill8(vram, x, COL8_848484, x - 47,      y - 23,  x - 47,   y -  4);
    boxfill8(vram, x, COL8_FFFFFF, x - 47,      y -  3,  x -  4,   y -  3);
    boxfill8(vram, x, COL8_FFFFFF, x -  3,      y - 24,  x -  3,   y -  3);
    return;
}

void putfont8(char *vram, int xsize, int x, int y, char c, char *font)
{
    int i;
    char *p, d;
    for (i = 0; i < 16; i++) {
        p = vram + (y + i) * xsize + x;
        d = font[i];
        if ((d & 0x80) != 0) { p[0] = c; }
        if ((d & 0x40) != 0) { p[1] = c; }
        if ((d & 0x20) != 0) { p[2] = c; }
        if ((d & 0x10) != 0) { p[3] = c; }
        if ((d & 0x08) != 0) { p[4] = c; }
        if ((d & 0x04) != 0) { p[5] = c; }
        if ((d & 0x02) != 0) { p[6] = c; }
        if ((d & 0x01) != 0) { p[7] = c; }
    }
    return ;
}

void putfonts8_asc(char *vram, int xsize, int x, int y, char c, unsigned char *s)
{
    extern char hankaku[4096];
    for (; *s != 0x00; s++) {
        putfont8(vram, xsize, x, y, c, hankaku + *s * 16);
        x += 8;
    }
    return;
}

void init_mouse_cursor8(char *mouse, char bc)
{
    static char cursor[16][16] = {
        "**************..",
		"*OOOOOOOOOOO*...",
		"*OOOOOOOOOO*....",
		"*OOOOOOOOO*.....",
		"*OOOOOOOO*......",
		"*OOOOOOO*.......",
		"*OOOOOOO*.......",
		"*OOOOOOOO*......",
		"*OOOO**OOO*.....",
		"*OOO*..*OOO*....",
		"*OO*....*OOO*...",
		"*O*......*OOO*..",
		"**........*OOO*.",
		"*..........*OOO*",
		"............*OO*",
		".............***"
    };
    int x, y;

    for (y = 0; y < 16; y++) {
        for (x = 0; x < 16; x++) {
            if (cursor[y][x] == '*') {
                mouse[y * 16 + x] = COL8_000000;
            }
            if (cursor[y][x] == 'O') {
                mouse[y + 16 + x] = COL8_FFFFFF;
            }
            if (cursor[y][x] == '.') {
                mouse[y * 16 + x] = bc;
            }
        }
    }
    return;
}

void putblock8_8(char *vram, int vxsize, int pxsize,
    int pysize, int px0, int py0, char *buf, int bxsize)
{
    int x, y;
    for (y = 0; y < pysize; y++) {
        for (x = 0; x < pxsize; x++) {
            vram[(py0 + y) * vxsize + (px0 + x)] = buf[y * bxsize + x];
        }
    }
    return; 
}
```

dsctbl.c
```c
struct SEGMENT_DESCRIPTOR {
    short limit_low, base_low;
    char base_mid, access_right;
    char limit_high, base_high;
};

struct GATE_DESCRIPTOR {
    short offset_low, selector;
    char dw_count, access_right;
    short offset_high;
};

void init_gdtidt(void);
void set_segmdesc(struct SEGMENT_DESCRIPTOR *sd, unsigned int limit, int base, int ar);
void set_gatedesc(struct GATE_DESCRIPTOR *gd, int offset, int selector, int ar);
void load_gdtr(int limit, int addr);
void load_idtr(int limit, int addr);

void init_gdtidt(void) {
    struct SEGMENT_DESCRIPTOR *gdt = (struct SEGMENT_DESCRIPTOR *) 0x00270000;
    struct GATE_DESCRIPTOR *idt = (struct GATE_DESCRIPTOR *) 0x0026f800;
    int i;

    for (i = 0; i < 8192; i++) {
        set_segmdesc(gdt + i, 0, 0, 0);
    }

    set_segmdesc(gdt + 1, 0xffffffff, 0x00000000, 0x4092);
    set_segmdesc(gdt + 2, 0x0007ffff, 0x00280000, 0x409a);
    load_gdtr(0xffff, 0x00270000);

    for (i = 0; i < 256; i++) {
        set_gatedesc(idt + i, 0, 0, 0);
    }

    load_idtr(0x7ff, 0x0026f800);

    return;
}

void set_segmdesc(struct SEGMENT_DESCRIPTOR *sd, unsigned int limit, int base, int ar)
{
    if (limit > 0xffff) {
        ar |= 0x8000;
        limit /= 0x1000;
    }

    sd->limit_low = limit & 0xffff;
    sd->base_low = base & 0xffff;
    sd->base_mid = (base >> 16) & 0xff;
    sd->access_right = ar & 0xff;
    sd->limit_high = ((limit >> 16) & 0x0f) |  ((ar >> 8) & 0xf0);
    sd->base_high = (base >> 24) & 0xff;
    return;
}

void set_gatedesc(struct GATE_DESCRIPTOR *gd, int offset, int selector, int ar)
{
    gd->offset_low = offset & 0xffff;
    gd->selector = selector;
    gd->dw_count = (ar >> 8) & 0xff;
    gd->access_right = ar & 0xff;
    gd->offset_high = (offset >> 16) & 0xffff;
    return;
}
```

Makefile
```Makefile
TOOLPATH = ..\..\z_tools\\

INCPATH  = ..\..\z_tools\haribote\\

MAKE     = $(TOOLPATH)make.exe -r
NASK     = $(TOOLPATH)nask.exe
CC1		 = $(TOOLPATH)cc1.exe -I$(INCPATH) -Os -Wall -quiet
GAS2NASK = $(TOOLPATH)gas2nask.exe -a
OBJ2BIM  = $(TOOLPATH)obj2bim.exe
MAKEFONT = $(TOOLPATH)makefont.exe
BIN2OBJ  = $(TOOLPATH)bin2obj
BIM2HRB  = $(TOOLPATH)bim2hrb.exe
RULEFILE = $(TOOLPATH)haribote\haribote.rul
EDIMG    = $(TOOLPATH)edimg.exe
IMGTOL   = $(TOOLPATH)imgtol.com
COPY     = cmd.exe /C copy
DEL      = rm -f


# default operation
default:
	$(MAKE) img

# file making rule

ipl10.bin : ipl10.nas Makefile
	$(NASK) ipl10.nas ipl10.bin ipl10.lst

asmhead.bin : asmhead.nas Makefile
	$(NASK) asmhead.nas asmhead.bin asmhead.lst

bootpack.gas : bootpack.c Makefile
	$(CC1) -o bootpack.gas bootpack.c

bootpack.nas : bootpack.gas Makefile
	$(GAS2NASK) bootpack.gas  bootpack.nas 

bootpack.obj : bootpack.nas Makefile
	$(NASK) bootpack.nas bootpack.obj bootpack.lst

naskfunc.obj : naskfunc.nas Makefile
	$(NASK) naskfunc.nas naskfunc.obj naskfunc.lst

hankaku.bin : hankaku.txt Makefile
	$(MAKEFONT) hankaku.txt hankaku.bin

hankaku.obj : hankaku.bin Makefile
	$(BIN2OBJ) hankaku.bin hankaku.obj _hankaku

graphic.gas : graphic.c Makefile
	$(CC1) -o graphic.gas graphic.c

graphic.nas : graphic.gas Makefile
	$(GAS2NASK) graphic.gas graphic.nas

graphic.obj : graphic.nas Makefile
	$(NASK) graphic.nas graphic.obj graphic.lst

dsctbl.gas : dsctbl.c Makefile
	$(CC1) -o dsctbl.gas dsctbl.c

dsctbl.nas : dsctbl.gas Makefile
	$(GAS2NASK) dsctbl.gas dsctbl.nas

dsctbl.obj : dsctbl.nas Makefile
	$(NASK) dsctbl.nas dsctbl.obj dsctbl.lst

bootpack.bim : bootpack.obj naskfunc.obj hankaku.obj graphic.obj dsctbl.obj Makefile
	$(OBJ2BIM) @$(RULEFILE) out:bootpack.bim stack:3136k map:bootpack.map \
		bootpack.obj naskfunc.obj hankaku.obj graphic.obj dsctbl.obj
# 3MB+64KB=3136KB

bootpack.hrb : bootpack.bim Makefile
	$(BIM2HRB) bootpack.bim bootpack.hrb 0

haribote.sys : asmhead.bin bootpack.hrb Makefile
	cat asmhead.bin >  haribote.sys
	cat bootpack.hrb >> haribote.sys

haribote.img : ipl10.bin haribote.sys Makefile
	$(EDIMG)   imgin:..\..\z_tools\fdimg0at.tek \
		wbinimg src:ipl10.bin len:512 from:0 to:0 \
		copy from:haribote.sys to:@: \
		imgout:haribote.img

# command

img :
	$(MAKE) haribote.img

run :
	$(MAKE) img
	$(COPY) haribote.img ..\..\z_tools\qemu\fdimage0.bin
	$(MAKE) -C ..\..\z_tools\qemu

install :
	$(MAKE) img
	$(IMGTOL) w a: haribote.img

clean :
	-$(DEL) *.bin
	-$(DEL) *.lst
	-$(DEL) *.gas
	-$(DEL) *.obj
	-$(DEL) bootpack.nas
	-$(DEL) bootpack.map
	-$(DEL) bootpack.bim
	-$(DEL) bootpack.hrb
	-$(DEL) haribote.sys

src_only :
	$(MAKE) clean
	-$(DEL) haribote.img
```
Makefileが長くなりすぎたので短くします。

Makefile
```Makefile
OBJS_BOOTPACK = bootpack.obj naskfunc.obj hankaku.obj graphic.obj dsctbl.obj

TOOLPATH = ..\..\z_tools\\

INCPATH  = ..\..\z_tools\haribote\\

MAKE     = $(TOOLPATH)make.exe -r
NASK     = $(TOOLPATH)nask.exe
CC1		 = $(TOOLPATH)cc1.exe -I$(INCPATH) -Os -Wall -quiet
GAS2NASK = $(TOOLPATH)gas2nask.exe -a
OBJ2BIM  = $(TOOLPATH)obj2bim.exe
MAKEFONT = $(TOOLPATH)makefont.exe
BIN2OBJ  = $(TOOLPATH)bin2obj
BIM2HRB  = $(TOOLPATH)bim2hrb.exe
RULEFILE = $(TOOLPATH)haribote\haribote.rul
EDIMG    = $(TOOLPATH)edimg.exe
IMGTOL   = $(TOOLPATH)imgtol.com
COPY     = cmd.exe /C copy
DEL      = rm -f


# default operation
default:
	$(MAKE) img

# file making rule

ipl10.bin : ipl10.nas Makefile
	$(NASK) ipl10.nas ipl10.bin ipl10.lst

asmhead.bin : asmhead.nas Makefile
	$(NASK) asmhead.nas asmhead.bin asmhead.lst

hankaku.bin : hankaku.txt Makefile
	$(MAKEFONT) hankaku.txt hankaku.bin

hankaku.obj : hankaku.bin Makefile
	$(BIN2OBJ) hankaku.bin hankaku.obj _hankaku

bootpack.bim : $(OBJS_BOOTPACK) Makefile
	$(OBJ2BIM) @$(RULEFILE) out:bootpack.bim stack:3136k map:bootpack.map \
		$(OBJS_BOOTPACK)
# 3MB+64KB=3136KB

bootpack.hrb : bootpack.bim Makefile
	$(BIM2HRB) bootpack.bim bootpack.hrb 0

haribote.sys : asmhead.bin bootpack.hrb Makefile
	cat asmhead.bin >  haribote.sys
	cat bootpack.hrb >> haribote.sys

haribote.img : ipl10.bin haribote.sys Makefile
	$(EDIMG)   imgin:..\..\z_tools\fdimg0at.tek \
		wbinimg src:ipl10.bin len:512 from:0 to:0 \
		copy from:haribote.sys to:@: \
		imgout:haribote.img

# rules
%.gas : %.c Makefile
	$(CC1) -o $*.gas $*.c

%.nas : %.gas Makefile
	$(GAS2NASK) $*.gas $*.nas

%.obj :%.nas Makefile
	$(NASK) $*.nas $*.obj $*.lst

# command

img :
	$(MAKE) haribote.img

run :
	$(MAKE) img
	$(COPY) haribote.img ..\..\z_tools\qemu\fdimage0.bin
	$(MAKE) -C ..\..\z_tools\qemu

install :
	$(MAKE) img
	$(IMGTOL) w a: haribote.img

clean :
	-$(DEL) *.bin
	-$(DEL) *.lst
	-$(DEL) *.gas
	-$(DEL) *.obj
	-$(DEL) bootpack.nas
	-$(DEL) bootpack.map
	-$(DEL) bootpack.bim
	-$(DEL) bootpack.hrb
	-$(DEL) haribote.sys

src_only :
	$(MAKE) clean
	-$(DEL) haribote.img
```

これでだいぶ短くなりました。

さらにヘッダーファイルを記載していきます。   
bootpack.h
```h
/* asmhead.nad */
struct BOOTINFO {
    char cyls, leds, vmode, reserve;
    short scrnx, scrny;
    char *vram;
};
#define ADR_BOOTINFO

/* naskfunc.nas */
void io_hlt(void);
void io_cli(void);
void io_out8(int port, int data);
int io_load_eflags(void);
void io_store_eflags(int eflags);
void load_gdtr(int limit, int addr);
void load_idtr(int limit, int addr);

/* graphic.c */
void init_palette(void);
void set_palette(int start, int end, unsigned char *rgd);
void boxfill8(unsigned char *vram, int xsize, unsigned char c, int x0, int y0, int x1, int y1);
void init_screen(char *vram, int x, int y);
void putfont8(char *vram, int xsize, int x, int y, char c, char *font);
void putfonts8_asc(char *vram, int xsize, int x, int y, char c, unsigned char *s);
void init_mouse_cursor8(char *mouse, char bc);
void putblock8_8(char *vram, int vxsize, int pxsize,
    int pysize, int px0, int py0, char *buf, int bxsize);

#define COL8_000000  0
#define COL8_FF0000  1
#define COL8_00FF00  2
#define COL8_FFFF00  3
#define COL8_0000FF  4
#define COL8_FF00FF  5
#define COL8_00FFFF  6
#define COL8_FFFFFF  7
#define COL8_C6C6C6  8
#define COL8_840000  9
#define COL8_008400  10
#define COL8_848400  11
#define COL8_000084  12
#define COL8_840084  13
#define COL8_008484  14
#define COL8_848484  15

/* dsctbl.c */
struct SEGMENT_DESCRIPTOR {
    short limit_low, base_low;
    char base_mid, access_right;
    char limit_high, base_high;
};

struct GATE_DESCRIPTOR {
    short offset_low, selector;
    char dw_count, access_right;
    short offset_high;
};

void init_gdtidt(void);
void set_segmdesc(struct SEGMENT_DESCRIPTOR *sd, unsigned int limit, int base, int ar);
void set_gatedesc(struct GATE_DESCRIPTOR *gd, int offset, int selector, int ar);
#define ADR_IDT         0x0026f800
#define LIMIT_IDT       0x000007ff
#define ADR_GDT         0x00270000
#define LIMIT_GDT       0x0000ffff
#define ADR_BOTPAK      0x00280000
#define LIMIT_BOTPAK    0x0007ffff
#define AR_DATA32_RW    0x4092
#define AR_CODE32_ER    0x409a
```

ヘッダーファイルを作ったことでCのソースファイルも短縮することができるようになりました。   
bootpack.c
```c
#include "bootpack.h"
#include <stdio.h>

void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) 0x0ff0;
    char s[40], mcursor[256];
    int mx, my;
 
    init_palette();
    init_screen(binfo->vram, binfo->scrnx, binfo->scrny);
    mx = (binfo->scrnx - 16) / 2;
    my = (binfo->scrny - 28 - 16) / 2;
    init_mouse_cursor8(mcursor, COL8_008484);
    putblock8_8(binfo->vram, binfo->scrnx, 16, 16, mx, my, mcursor, 16);
    sprintf(s, "(%d, %d)", mx, my);
    putfonts8_asc(binfo->vram, binfo->scrnx, 0, 0, COL8_FFFFFF, s);

    for (;;) {
        io_hlt();
    }
    
}
```

graphic.c
```c
/* graphic process */
#include "bootpack.h"

void init_palette(void)
{
    static unsigned char table_rgb[16 * 3] = {
        0x00, 0x00, 0x00,   /* 0:black */
        0xff, 0x00, 0x00,   /* 1:light red */
        0x00, 0xff, 0x00,   /* 2:light green */
        0xff, 0xff, 0x00,   /* 3:light yellow */
        0x00, 0x00, 0xff,   /* 4:light blue */
        0xff, 0x00, 0xff,   /* 5:light puple */
        0x00, 0xff, 0xff,   /* 6:light sky blue */
        0xff, 0xff, 0xff,   /* 7:white */
        0xc6, 0xc6, 0xc6,   /* 8:light grey */
        0x84, 0x00, 0x00,   /* 9:dark green */
        0x00, 0x84, 0x00,   /* 10:dark green */
        0x84, 0x84, 0x00,   /* 11:dark yellow */
        0x00, 0x00, 0x84,   /* 12:dark blue */
        0x84, 0x00, 0x84,   /* 13:dark puple */
        0x00, 0x84, 0x84,   /* 14:dark sky blue */
        0x84, 0x84, 0x84    /* 15:dark grey */
    };
    set_palette(0, 15, table_rgb);
    return;
}

void set_palette(int start, int end, unsigned char *rgb)
{
    int i, eflags;
    eflags = io_load_eflags();
    io_cli();
    io_out8(0x03c8, start);
    for (i = start; i <= end; i++) {
        io_out8(0x03c9, rgb[0] / 4);
        io_out8(0x03c9, rgb[1] / 4);
        io_out8(0x03c9, rgb[2] / 4);
        rgb += 3;
    }
    io_store_eflags(eflags);
    return;
} 

void boxfill8(unsigned char *vram, int xsize, unsigned char c, int x0, int y0, int x1, int y1)
{
    int x, y;
    for (y= y0; y <= y1; y++) {
        for (x = x0; x <= x1; x++)
            vram[y * xsize + x] = c;
    }
    return;
}

void init_screen(char *vram, int x, int y)
{
    boxfill8(vram, x, COL8_008484,  0,          0,       x - 1,    y - 29);
    boxfill8(vram, x, COL8_C6C6C6,  0,          y - 28,  x - 1,    y - 28);
    boxfill8(vram, x, COL8_FFFFFF,  0,          y - 27,  x - 1,    y - 27);
    boxfill8(vram, x, COL8_C6C6C6,  0,          y - 26,  x - 1,    y -  1);

    boxfill8(vram, x, COL8_FFFFFF,  3,          y - 24,  59,       y - 24);
    boxfill8(vram, x, COL8_FFFFFF,  2,          y - 24,   2,       y -  4);
    boxfill8(vram, x, COL8_848484,  3,          y -  4,  59,       y -  4);
    boxfill8(vram, x, COL8_848484, 59,          y - 23,  59,       y -  5);
    boxfill8(vram, x, COL8_000000,  2,          y -  3,  59,       y -  3);
    boxfill8(vram, x, COL8_000000, 60,          y - 24,  60,       y -  3);

    boxfill8(vram, x, COL8_848484, x - 47,      y - 24,  x -  4,   y - 24);
    boxfill8(vram, x, COL8_848484, x - 47,      y - 23,  x - 47,   y -  4);
    boxfill8(vram, x, COL8_FFFFFF, x - 47,      y -  3,  x -  4,   y -  3);
    boxfill8(vram, x, COL8_FFFFFF, x -  3,      y - 24,  x -  3,   y -  3);
    return;
}

void putfont8(char *vram, int xsize, int x, int y, char c, char *font)
{
    int i;
    char *p, d;
    for (i = 0; i < 16; i++) {
        p = vram + (y + i) * xsize + x;
        d = font[i];
        if ((d & 0x80) != 0) { p[0] = c; }
        if ((d & 0x40) != 0) { p[1] = c; }
        if ((d & 0x20) != 0) { p[2] = c; }
        if ((d & 0x10) != 0) { p[3] = c; }
        if ((d & 0x08) != 0) { p[4] = c; }
        if ((d & 0x04) != 0) { p[5] = c; }
        if ((d & 0x02) != 0) { p[6] = c; }
        if ((d & 0x01) != 0) { p[7] = c; }
    }
    return ;
}

void putfonts8_asc(char *vram, int xsize, int x, int y, char c, unsigned char *s)
{
    extern char hankaku[4096];
    for (; *s != 0x00; s++) {
        putfont8(vram, xsize, x, y, c, hankaku + *s * 16);
        x += 8;
    }
    return;
}

void init_mouse_cursor8(char *mouse, char bc)
{
    static char cursor[16][16] = {
        "**************..",
		"*OOOOOOOOOOO*...",
		"*OOOOOOOOOO*....",
		"*OOOOOOOOO*.....",
		"*OOOOOOOO*......",
		"*OOOOOOO*.......",
		"*OOOOOOO*.......",
		"*OOOOOOOO*......",
		"*OOOO**OOO*.....",
		"*OOO*..*OOO*....",
		"*OO*....*OOO*...",
		"*O*......*OOO*..",
		"**........*OOO*.",
		"*..........*OOO*",
		"............*OO*",
		".............***"
    };
    int x, y;

    for (y = 0; y < 16; y++) {
        for (x = 0; x < 16; x++) {
            if (cursor[y][x] == '*') {
                mouse[y * 16 + x] = COL8_000000;
            }
            if (cursor[y][x] == 'O') {
                mouse[y + 16 + x] = COL8_FFFFFF;
            }
            if (cursor[y][x] == '.') {
                mouse[y * 16 + x] = bc;
            }
        }
    }
    return;
}

void putblock8_8(char *vram, int vxsize, int pxsize,
    int pysize, int px0, int py0, char *buf, int bxsize)
{
    int x, y;
    for (y = 0; y < pysize; y++) {
        for (x = 0; x < pxsize; x++) {
            vram[(py0 + y) * vxsize + (px0 + x)] = buf[y * bxsize + x];
        }
    }
    return; 
}
```

dsctbl.c
```c
#include "bootpack.h"

void init_gdtidt(void) {
    struct SEGMENT_DESCRIPTOR *gdt = (struct SEGMENT_DESCRIPTOR *) 0x00270000;
    struct GATE_DESCRIPTOR *idt = (struct GATE_DESCRIPTOR *) 0x0026f800;
    int i;

    for (i = 0; i < 8192; i++) {
        set_segmdesc(gdt + i, 0, 0, 0);
    }

    set_segmdesc(gdt + 1, 0xffffffff, 0x00000000, 0x4092);
    set_segmdesc(gdt + 2, 0x0007ffff, 0x00280000, 0x409a);
    load_gdtr(0xffff, 0x00270000);

    for (i = 0; i < 256; i++) {
        set_gatedesc(idt + i, 0, 0, 0);
    }

    load_idtr(0x7ff, 0x0026f800);

    return;
}

void set_segmdesc(struct SEGMENT_DESCRIPTOR *sd, unsigned int limit, int base, int ar)
{
    if (limit > 0xffff) {
        ar |= 0x8000;
        limit /= 0x1000;
    }

    sd->limit_low = limit & 0xffff;
    sd->base_low = base & 0xffff;
    sd->base_mid = (base >> 16) & 0xff;
    sd->access_right = ar & 0xff;
    sd->limit_high = ((limit >> 16) & 0x0f) |  ((ar >> 8) & 0xf0);
    sd->base_high = (base >> 24) & 0xff;
    return;
}

void set_gatedesc(struct GATE_DESCRIPTOR *gd, int offset, int selector, int ar)
{
    gd->offset_low = offset & 0xffff;
    gd->selector = selector;
    gd->dw_count = (ar >> 8) & 0xff;
    gd->access_right = ar & 0xff;
    gd->offset_high = (offset >> 16) & 0xffff;
    return;
}
```

ヘッダーファイルを分割したことでさらに見やすくなりました。   

**実装過程**   
・　[6bf8c9f4729c76f7ab788aef9405847f1c7eb1cd](6bf8c9f4729c76f7ab788aef9405847f1c7eb1cd)

## day7
さらにマウスカーソルを動かす準備をしていきます。   
PICの初期化を実施します。   
int.c
```c
#include "bootpack.h"

void init_pic(void)
/* Initialize pic */
{
    io_out8(PIC0_IMR, 0xff );
    io_out8(PIC1_IMR, 0xff );

    io_out8(PIC0_ICW1, 0x11 );
    io_out8(PIC0_ICW2, 0x20 );
    io_out8(PIC0_ICW3, 1 << 2);
    io_out8(PIC0_ICW4, 0x01 );

    io_out8(PIC1_ICW1, 0x11 );
    io_out8(PIC1_ICW2, 0x28 );
    io_out8(PIC1_ICW3, 2 );
    io_out8(PIC1_ICW4, 0x01 );

    io_out8(PIC0_IMR, 0x0fb );
    io_out8(PIC1_IMR, 0xff );

    return;
}
```

bootpack.h
```h
/* int.c */
void ont_pic(void);
#define PIC0_ICW1       0x0020
#define PIC0_OCW2       0x0020
#define PIC0_IMR        0x0021
#define PIC0_ICW2       0x0021
#define PIC0_ICW3       0x0021
#define PIC0_ICW4       0x0021
#define PIC1_ICW1       0x00a0
#define PIC1_OCW2       0x00a0
#define PIC1_IMR        0x00a1
#define PIC1_ICW2       0x00a1
#define PIC1_ICW3       0x00a1
#define PIC1_ICW4       0x00a1
```

Makefile
```Makefile
OBJS_BOOTPACK = bootpack.obj naskfunc.obj hankaku.obj graphic.obj dsctbl.obj \
				int.obj
```

動作させてみても何も変わりませんがこれでまた一つ下地ができました。   

次は割り込みハンドラを作成しました。   
正直一番苦労したかもしれません^^;   
色々と変更したので載せていきます。

int.c
```c
void inthandler21(int *esp)
/* PS/2 keybord interrupt */
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) ADR_BOOTINFO;
    boxfill8(binfo->vram, binfo->scrnx, COL8_000000, 0, 0, 32 * 8 - 1, 15);
    putfonts8_asc(binfo->vram, binfo->scrnx, 0, 0, COL8_FFFFFF, "INT 21 (IRQ-1) : PS/2 keyboard");
    for (;;) {
        io_hlt();
    }
}

void inthandler2c(int *esp)
/* PS/2 mouse interrupt */
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) ADR_BOOTINFO;
    boxfill8(binfo->vram, binfo->scrnx, COL8_000000, 0, 0, 32 * 8 - 1, 15);
    putfonts8_asc(binfo->vram, binfo->scrnx, 0, 0, COL8_FFFFFF, "INT 2C (IRQ-12) : PS/2 mouse");
    for (;;) {
        io_hlt();
    }
}

void inthandler27(int *esp)
/* PIC0からの不完全割り込み対策 */
/* Athlon64X2機などではチップセットの都合によりPICの初期化時にこの割り込みが1度だけおこる */
/* この割り込み処理関数は、その割り込みに対して何もしないでやり過ごす */
/* なぜ何もしなくていいの？
	→  この割り込みはPIC初期化時の電気的なノイズによって発生したものなので、
		まじめに何か処理してやる必要がない。									*/
{
    io_out8(PIC0_OCW2, 0x67);
    return;
}
```

dsctbl.c
```c
void init_gdtidt(void) {
    struct SEGMENT_DESCRIPTOR *gdt = (struct SEGMENT_DESCRIPTOR *) ADR_GDT;
    struct GATE_DESCRIPTOR *idt = (struct GATE_DESCRIPTOR *) ADR_IDT;
    int i;

    for (i = 0; i <= LIMIT_GDT / 8; i++) {
        set_segmdesc(gdt + i, 0, 0, 0);
    }

    set_segmdesc(gdt + 1, 0xffffffff, 0x00000000, AR_DATA32_RW);
    set_segmdesc(gdt + 2, LIMIT_BOTPAK, ADR_BOTPAK, AR_CODE32_ER);
    load_gdtr(LIMIT_GDT, ADR_GDT);

    for (i = 0; i <= LIMIT_IDT / 8; i++) {
        set_gatedesc(idt + i, 0, 0, 0);
    }
    load_idtr(LIMIT_IDT, ADR_IDT);

    /* setting IDT */
    set_gatedesc(idt + 0x21, (int) asm_inthandler21, 2 * 8, AR_INTGATE32);
    set_gatedesc(idt + 0x27, (int) asm_inthandler27, 2 * 8, AR_INTGATE32);
    set_gatedesc(idt + 0x2c, (int) asm_inthandler2c, 2 * 8, AR_INTGATE32);

    return;
}
```

naskfunc.nas
```asm
GLOBAL	_asm_inthandler21, _asm_inthandler27, _asm_inthandler2c
EXTERN	_inthandler21, _inthandler27, _inthandler2c
```

```asm
_asm_inthandler21:
		PUSH	ES
		PUSH	DS
		PUSHAD
		MOV		EAX,ESP
		PUSH	EAX
		MOV		AX,SS
		MOV		DS,AX
		MOV		ES,AX
		CALL	_inthandler21
		POP		EAX
		POPAD
		POP		DS
		POP		ES
		IRETD

_asm_inthandler27:
		PUSH	ES
		PUSH	DS
		PUSHAD
		MOV		EAX,ESP
		PUSH	EAX
		MOV		AX,SS
		MOV		DS,AX
		MOV		ES,AX
		CALL	_inthandler27
		POP		EAX
		POPAD
		POP		DS
		POP		ES
		IRETD   

_asm_inthandler2c:
		PUSH	ES
		PUSH	DS
		PUSHAD
		MOV		EAX,ESP
		PUSH	EAX
		MOV		AX,SS
		MOV		DS,AX
		MOV		ES,AX
		CALL	_inthandler2c
		POP		EAX
		POPAD
		POP		DS
		POP		ES
		IRETD
```

bootpack.h
```h
#define ADR_BOOTINFO    0x00000ff0
```

```h
/* naskfunc.nas */
void asm_inthandler21(void);
void asm_inthandler27(void);
void asm_inthandler2c(void);
```

```h
/* graphic.c */
void init_mouse_cursor8(char *mouse, char bc);
```

```h
/* int.c */
void inthandler21(int *esp);
void inthandler27(int *esp);
void inthandler2c(int *esp);
```

bootpack.c

```c
void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) ADR_BOOTINFO;
    char s[40], mcursor[256];
    int mx, my;

    init_gdtidt();
    init_pic();
    io_sti();
 
    init_palette();
    init_screen8(binfo->vram, binfo->scrnx, binfo->scrny);
    mx = (binfo->scrnx - 16) / 2;
    my = (binfo->scrny - 28 - 16) / 2;
    init_mouse_cursor8(mcursor, COL8_008484);
    putblock8_8(binfo->vram, binfo->scrnx, 16, 16, mx, my, mcursor, 16);
    sprintf(s, "(%d, %d)", mx, my);
    putfonts8_asc(binfo->vram, binfo->scrnx, 0, 0, COL8_FFFFFF, s);

    io_out8(PIC0_IMR, 0xf9);
    io_out8(PIC1_IMR, 0xef);

    for (;;) {
        io_hlt();
    }
    
}
```

これでカーソルからの読み込みに対してメッセージを出すことができました。   

本では7日目に突入しました。   
キーコードを取得してみます。   
int.c
```c
#define PORT_KEYDAT  0x0060
```

```c
void inthandler21(int *esp)
/* PS/2 keybord interrupt */
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) ADR_BOOTINFO;
    unsigned char data, s[4];
    io_out8(PIC0_OCW2, 0x61);
    data = io_in8(PORT_KEYDAT);
    
    sprintf(s, "%02x", data);
    boxfill8(binfo->vram, binfo->scrnx, COL8_008484, 0, 16, 15, 31);
    putfonts8_asc(binfo->vram, binfo->scrnx, 0, 16, COL8_FFFFFF, s);
    
    return;
}
```

これで押したり話したりすると16進数でキーコードが返ってくるようになりました。   

次は割り込み処理を手早くするためにさらにそーそコードに改良を加えていきます。

bootpack.c
```c
extern struct KEYBUF keybuf;

void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) ADR_BOOTINFO;
    char s[40], mcursor[256];
    int mx, my, i;

    init_gdtidt();
    init_pic();
    io_sti();

    io_out8(PIC0_IMR, 0xf9);
    io_out8(PIC1_IMR, 0xef);
 
    init_palette();
    init_screen8(binfo->vram, binfo->scrnx, binfo->scrny);
    mx = (binfo->scrnx - 16) / 2;
    my = (binfo->scrny - 28 - 16) / 2;
    init_mouse_cursor8(mcursor, COL8_008484);
    putblock8_8(binfo->vram, binfo->scrnx, 16, 16, mx, my, mcursor, 16);
    sprintf(s, "(%d, %d)", mx, my);
    putfonts8_asc(binfo->vram, binfo->scrnx, 0, 0, COL8_FFFFFF, s);

    for (;;) {
        io_cli();
        if (keybuf.flag == 0) {
            io_stihlt();
        } else {
            i = keybuf.data;
            keybuf.flag = 0;
            io_sti();
            sprintf(s, "%02X", i);
            boxfill8(binfo->vram, binfo->scrnx, COL8_008484, 0, 16, 15, 31);
            putfonts8_asc(binfo->vram, binfo->scrnx, 0, 16, COL8_FFFFFF, s);
        }
    }
    
}
```

int.c
```c
struct KEYBUF keybuf;
```

```c
void inthandler21(int *esp)
/* PS/2 keybord interrupt */
{
    unsigned char data;
    io_out8(PIC0_OCW2, 0x61);
    data = io_in8(PORT_KEYDAT);
    if (keybuf.flag == 0) {
        keybuf.data = data;
        keybuf.flag = 1;
    }
    return;
}
```

bootpack.h
```h
/* int.c */
struct KEYBUF {
    unsigned char data, flag;
};
```

これで少しは早くなりました。（最近のpcは性能がいいためほとんど体感的にはかわりませんが笑）

FIFOバッファを作成します。   
int.c
```c
void inthandler21(int *esp)
/* PS/2 keybord interrupt */
{
    unsigned char data;
    io_out8(PIC0_OCW2, 0x61);
    data = io_in8(PORT_KEYDAT);
    if (keybuf.next < 32) {
        keybuf.data[keybuf.next] = data;
        keybuf.next++;
    }
    return;
}
```

bootpack.h
```h
/* int.c */
struct KEYBUF {
    unsigned char data[32];
    int next;
};
```

bootpack.c
```c
void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) ADR_BOOTINFO;
    char s[40], mcursor[256];
    int mx, my, i, j;

    init_gdtidt();
    init_pic();
    io_sti();

    io_out8(PIC0_IMR, 0xf9);
    io_out8(PIC1_IMR, 0xef);
 
    init_palette();
    init_screen8(binfo->vram, binfo->scrnx, binfo->scrny);
    mx = (binfo->scrnx - 16) / 2;
    my = (binfo->scrny - 28 - 16) / 2;
    init_mouse_cursor8(mcursor, COL8_008484);
    putblock8_8(binfo->vram, binfo->scrnx, 16, 16, mx, my, mcursor, 16);
    sprintf(s, "(%d, %d)", mx, my);
    putfonts8_asc(binfo->vram, binfo->scrnx, 0, 0, COL8_FFFFFF, s);

    for (;;) {
        io_cli();
        if (keybuf.next == 0) {
            io_stihlt();
        } else {
            i = keybuf.data[0];
            keybuf.next--;
            for (j = 0; j < keybuf.next; j++) {
                keybuf.data[j] = keybuf.data[j + 1];
            }
            io_sti();
            sprintf(s, "%02X", i);
            boxfill8(binfo->vram, binfo->scrnx, COL8_008484, 0, 16, 15, 31);
            putfonts8_asc(binfo->vram, binfo->scrnx, 0, 16, COL8_FFFFFF, s);
        }
    }
    
}
```

挙動は変わりませんがこれでバッファが完成しました。  
バッファを改良していきたいと思います。   
これをやればさらに文字を早く入力できます。   
bootpack.h
```h
/* int.c */
struct KEYBUF {
    unsigned char data[32];
    int next_r, next_w, len;
};
```

int.c
```c
void inthandler21(int *esp)
/* PS/2 keybord interrupt */
{
    unsigned char data;
    io_out8(PIC0_OCW2, 0x61);
    data = io_in8(PORT_KEYDAT);
    if (keybuf.len < 32) {
        keybuf.data[keybuf.next_w] = data;
        keybuf.len++;
        keybuf.next_w++;
        if (keybuf.next_w == 32) {
            keybuf.next_w = 0;
        }
    }
    return;
}
```

bootpack.c
```c
void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) ADR_BOOTINFO;
    char s[40], mcursor[256];
    int mx, my, i, j;

    init_gdtidt();
    init_pic();
    io_sti();

    io_out8(PIC0_IMR, 0xf9);
    io_out8(PIC1_IMR, 0xef);
 
    init_palette();
    init_screen8(binfo->vram, binfo->scrnx, binfo->scrny);
    mx = (binfo->scrnx - 16) / 2;
    my = (binfo->scrny - 28 - 16) / 2;
    init_mouse_cursor8(mcursor, COL8_008484);
    putblock8_8(binfo->vram, binfo->scrnx, 16, 16, mx, my, mcursor, 16);
    sprintf(s, "(%d, %d)", mx, my);
    putfonts8_asc(binfo->vram, binfo->scrnx, 0, 0, COL8_FFFFFF, s);

    for (;;) {
        io_cli();
        if (keybuf.len == 0) {
            io_stihlt();
        } else {
            i = keybuf.data[keybuf.next_r];
            keybuf.len--;
            keybuf.next_r++;
            if (keybuf.next_r == 32) {
                keybuf.next_r = 0;
            }
            io_sti();
            sprintf(s, "%02X", i);
            boxfill8(binfo->vram, binfo->scrnx, COL8_008484, 0, 16, 15, 31);
            putfonts8_asc(binfo->vram, binfo->scrnx, 0, 16, COL8_FFFFFF, s);
        }
    }
    
}
```

さらに改良してfifoを別のソースコードに分けてあげます。   
fifo.c
```c
/* FIFOライブラリ */

#include "bootpack.h"

#define FLAGS_OVERRUN		0x0001

void fifo8_init(struct FIFO8 *fifo, int size, unsigned char *buf)
/* FIFOバッファの初期化 */
{
	fifo->size = size;
	fifo->buf = buf;
	fifo->free = size; /* 空き */
	fifo->flags = 0;
	fifo->p = 0; /* 書き込み位置 */
	fifo->q = 0; /* 読み込み位置 */
	return;
}

int fifo8_put(struct FIFO8 *fifo, unsigned char data)
/* FIFOへデータを送り込んで蓄える */
{
	if (fifo->free == 0) {
		/* 空きがなくてあふれた */
		fifo->flags |= FLAGS_OVERRUN;
		return -1;
	}
	fifo->buf[fifo->p] = data;
	fifo->p++;
	if (fifo->p == fifo->size) {
		fifo->p = 0;
	}
	fifo->free--;
	return 0;
}

int fifo8_get(struct FIFO8 *fifo)
/* FIFOからデータを一つとってくる */
{
	int data;
	if (fifo->free == fifo->size) {
		/* バッファが空っぽのときは、とりあえず-1が返される */
		return -1;
	}
	data = fifo->buf[fifo->q];
	fifo->q++;
	if (fifo->q == fifo->size) {
		fifo->q = 0;
	}
	fifo->free++;
	return data;
}

int fifo8_status(struct FIFO8 *fifo)
/* どのくらいデータが溜まっているかを報告する */
{
	return fifo->size - fifo->free;
}
```

bootpack.c
```h
/* fifo.c */
struct FIFO8 {
	unsigned char *buf;
	int p, q, size, free, flags;
};
void fifo8_init(struct FIFO8 *fifo, int size, unsigned char *buf);
int fifo8_put(struct FIFO8 *fifo, unsigned char data);
int fifo8_get(struct FIFO8 *fifo);
int fifo8_status(struct FIFO8 *fifo);
```

int.c
```c
struct FIFO8 keyfifo;
```

```c
void inthandler21(int *esp)
{
	unsigned char data;
	io_out8(PIC0_OCW2, 0x61);	/* IRQ-01受付完了をPICに通知 */
	data = io_in8(PORT_KEYDAT);
	fifo8_put(&keyfifo, data);
	return;
}
```

bootpack.c
```C
extern struct FIFO8 keyfifo;

void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) ADR_BOOTINFO;
	char s[40], mcursor[256], keybuf[32];
	int mx, my, i;

	init_gdtidt();
	init_pic();
	io_sti(); /* IDT/PICの初期化が終わったのでCPUの割り込み禁止を解除 */

	fifo8_init(&keyfifo, 32, keybuf);
	io_out8(PIC0_IMR, 0xf9); /* PIC1とキーボードを許可(11111001) */
	io_out8(PIC1_IMR, 0xef); /* マウスを許可(11101111) */

	init_palette();
	init_screen8(binfo->vram, binfo->scrnx, binfo->scrny);
	mx = (binfo->scrnx - 16) / 2; /* 画面中央になるように座標計算 */
	my = (binfo->scrny - 28 - 16) / 2;
	init_mouse_cursor8(mcursor, COL8_008484);
	putblock8_8(binfo->vram, binfo->scrnx, 16, 16, mx, my, mcursor, 16);
	sprintf(s, "(%d, %d)", mx, my);
	putfonts8_asc(binfo->vram, binfo->scrnx, 0, 0, COL8_FFFFFF, s);

	for (;;) {
		io_cli();
		if (fifo8_status(&keyfifo) == 0) {
			io_stihlt();
		} else {
			i = fifo8_get(&keyfifo);
			io_sti();
			sprintf(s, "%02X", i);
			boxfill8(binfo->vram, binfo->scrnx, COL8_008484, 0, 16, 15, 31);
			putfonts8_asc(binfo->vram, binfo->scrnx, 0, 16, COL8_FFFFFF, s);
		}
	}
}

```

Makefile
```Makefile
OBJS_BOOTPACK = bootpack.obj naskfunc.obj hankaku.obj graphic.obj dsctbl.obj \
				int.obj fifo.obj
```

次にマウスの割り込みを感知させるようにしました。。   
bootpack.c
```c
#include "bootpack.h"
#include <stdio.h>

extern struct FIFO8 keyfifo;
void enable_mouse(void);
void init_keyboard(void);

void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) ADR_BOOTINFO;
	char s[40], mcursor[256], keybuf[32];
	int mx, my, i;

	init_gdtidt();
	init_pic();
	io_sti(); /* IDT/PICの初期化が終わったのでCPUの割り込み禁止を解除 */
	fifo8_init(&keyfifo, 32, keybuf);
	io_out8(PIC0_IMR, 0xf9); /* PIC1とキーボードを許可(11111001) */
	io_out8(PIC1_IMR, 0xef); /* マウスを許可(11101111) */

	init_keyboard();

	init_palette();
	init_screen8(binfo->vram, binfo->scrnx, binfo->scrny);
	mx = (binfo->scrnx - 16) / 2; /* 画面中央になるように座標計算 */
	my = (binfo->scrny - 28 - 16) / 2;
	init_mouse_cursor8(mcursor, COL8_008484);
	putblock8_8(binfo->vram, binfo->scrnx, 16, 16, mx, my, mcursor, 16);
	sprintf(s, "(%d, %d)", mx, my);
	putfonts8_asc(binfo->vram, binfo->scrnx, 0, 0, COL8_FFFFFF, s);

	enable_mouse();

	for (;;) {
		io_cli();
		if (fifo8_status(&keyfifo) == 0) {
			io_stihlt();
		} else {
			i = fifo8_get(&keyfifo);
			io_sti();
			sprintf(s, "%02X", i);
			boxfill8(binfo->vram, binfo->scrnx, COL8_008484, 0, 16, 15, 31);
			putfonts8_asc(binfo->vram, binfo->scrnx, 0, 16, COL8_FFFFFF, s);
		}
	}
}

#define PORT_KEYDAT			0x0060
#define PORT_KEYSTA			0x0064
#define PORAT_KEYCMD		0x0064
#define PORT_KEYCMD			0x0064
#define	KEYSTA_SEND_NOTREDY	0x02
#define KEYCMD_WRITE_MODE	0x60
#define KBC_MODE			0x47

void wait_KBC_sendready(void)
{
	/* キーボードコントローラーがデータ送信可能になるのを待つ */
	for (;;) {
		if ((io_in8(PORT_KEYSTA) & KEYSTA_SEND_NOTREDY) == 0) {
			break;
		}
	}
	return;
}


void init_keyboard(void)
{
	/* キーボードコントローラの初期化 */
	wait_KBC_sendready();
	io_out8(PORT_KEYCMD, KEYCMD_WRITE_MODE);
	wait_KBC_sendready();
	io_out8(PORT_KEYDAT, KBC_MODE);
	return;
}

#define KEYCMD_SENDTO_MOUSE	0xd4
#define MOUSECMD_ENABLE		0xf4

void enable_mouse(void)
{
	/* マウス有効 */
	wait_KBC_sendready();
	io_out8(PORT_KEYCMD, KEYCMD_SENDTO_MOUSE);
	wait_KBC_sendready();
	io_out8(PORT_KEYDAT, MOUSECMD_ENABLE);
	return; /* うまくいくとACK(0xfa)が送信されてくる */
}
```

マウスを感知したら次はデータを受信させてみます。   
int.c
```c
struct FIFO8 mousefifo;
```

```c
void inthandler2c(int *esp)
/* PS/2 mouse interrupt */
{
    unsigned char data;
    io_out8(PIC1_OCW2, 0x64);
    io_out8(PIC0_OCW2, 0x62);
    data = io_in8(PORT_KEYDAT);
    fifo8_put(&mousefifo, data);
    return;
}
```

bootpack.c
```c
extern struct FIFO8 keyfifo, mousefifo;
void enable_mouse(void);
void init_keyboard(void);

void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) ADR_BOOTINFO;
	char s[40], mcursor[256], keybuf[32], mousebuf[128];
	int mx, my, i;

	init_gdtidt();
	init_pic();
	io_sti(); /* IDT/PICの初期化が終わったのでCPUの割り込み禁止を解除 */
	fifo8_init(&keyfifo, 32, keybuf);
	fifo8_init(&mousefifo, 128, mousebuf);
	io_out8(PIC0_IMR, 0xf9); /* PIC1とキーボードを許可(11111001) */
	io_out8(PIC1_IMR, 0xef); /* マウスを許可(11101111) */

	init_keyboard();

	init_palette();
	init_screen8(binfo->vram, binfo->scrnx, binfo->scrny);
	mx = (binfo->scrnx - 16) / 2; /* 画面中央になるように座標計算 */
	my = (binfo->scrny - 28 - 16) / 2;
	init_mouse_cursor8(mcursor, COL8_008484);
	putblock8_8(binfo->vram, binfo->scrnx, 16, 16, mx, my, mcursor, 16);
	sprintf(s, "(%d, %d)", mx, my);
	putfonts8_asc(binfo->vram, binfo->scrnx, 0, 0, COL8_FFFFFF, s);

	enable_mouse();

	for (;;) {
		io_cli();
		if (fifo8_status(&keyfifo) + fifo8_status(&mousefifo) == 0) {
			io_stihlt();
		} else {
			if (fifo8_status(&keyfifo) != 0) {
				i = fifo8_get(&keyfifo);
				io_sti();
				sprintf(s, "%02X", i);
				boxfill8(binfo->vram, binfo->scrnx, COL8_008484, 0, 16, 15, 31);
				putfonts8_asc(binfo->vram, binfo->scrnx, 0, 16, COL8_FFFFFF, s);
			} else if (fifo8_status(&mousefifo) != 0) {
				i = fifo8_get(&mousefifo);
				io_sti();
				sprintf(s, "%02X", i);
				boxfill8(binfo->vram, binfo->scrnx, COL8_008484, 32, 16, 47, 31);
				putfonts8_asc(binfo->vram, binfo->scrnx, 32, 16, COL8_FFFFFF, s);
			}
		}
	}
}
```

これでデータを受信しました。   

ここからは本の8日目に突入です。   
初めはマウスを解読するために入力された3バイトを表示されます。    
bootpack.c
```c
void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) ADR_BOOTINFO;
	char s[40], mcursor[256], keybuf[32], mousebuf[128];
	int mx, my, i;
	unsigned char mouse_dbuf[3], mouse_phase;

	init_gdtidt();
	init_pic();
	io_sti(); /* IDT/PICの初期化が終わったのでCPUの割り込み禁止を解除 */
	fifo8_init(&keyfifo, 32, keybuf);
	fifo8_init(&mousefifo, 128, mousebuf);
	io_out8(PIC0_IMR, 0xf9); /* PIC1とキーボードを許可(11111001) */
	io_out8(PIC1_IMR, 0xef); /* マウスを許可(11101111) */

	init_keyboard();

	init_palette();
	init_screen8(binfo->vram, binfo->scrnx, binfo->scrny);
	mx = (binfo->scrnx - 16) / 2; /* 画面中央になるように座標計算 */
	my = (binfo->scrny - 28 - 16) / 2;
	init_mouse_cursor8(mcursor, COL8_008484);
	putblock8_8(binfo->vram, binfo->scrnx, 16, 16, mx, my, mcursor, 16);
	sprintf(s, "(%d, %d)", mx, my);
	putfonts8_asc(binfo->vram, binfo->scrnx, 0, 0, COL8_FFFFFF, s);

	enable_mouse();
	mouse_phase = 0; 

	for (;;) {
		io_cli();
		if (fifo8_status(&keyfifo) + fifo8_status(&mousefifo) == 0) {
			io_stihlt();
		} else {
			if (fifo8_status(&keyfifo) != 0) {
				i = fifo8_get(&keyfifo);
				io_sti();
				sprintf(s, "%02X", i);
				boxfill8(binfo->vram, binfo->scrnx, COL8_008484, 0, 16, 15, 31);
				putfonts8_asc(binfo->vram, binfo->scrnx, 0, 16, COL8_FFFFFF, s);
			} else if (fifo8_status(&mousefifo) != 0) {
				i = fifo8_get(&mousefifo);
				io_sti();
				if (mouse_phase == 0) {
					/* マウスの0xfaを待っている段階 */
					if (i == 0xfa) {
						mouse_phase = 1;
					}
				} else if (mouse_phase == 1) {
					/* マウスの1バイト目を待っている段階 */
					mouse_dbuf[0] = i;
					mouse_phase = 2;
				} else if (mouse_phase == 2) {
					mouse_dbuf[1] = i;
					mouse_phase = 3;
				} else if (mouse_phase == 3) {
					/* マウスの3バイト目を待っている段階 */
					mouse_dbuf[2] = i;
					mouse_phase = 1;
					/* データが3バイト揃ったので表示 */
					sprintf(s, "%02X %02X %02X", mouse_dbuf[0], mouse_dbuf[1], mouse_dbuf[2]);
					boxfill8(binfo->vram, binfo->scrnx, COL8_008484, 32, 16, 32 + 8 * 8 - 1, 31);
					putfonts8_asc(binfo->vram, binfo->scrnx, 32, 16, COL8_FFFFFF, s);
				}
			}
		}
	}
}
```

bootpack.cの中身のソースコードが多くなってみにくくなってきたので少し整理をしました。   
bootpack.c
```c
#include "bootpack.h"
#include <stdio.h>

struct MOUSE_DEC {
	unsigned char buf[3], phase;
};

extern struct FIFO8 keyfifo, mousefifo;
void enable_mouse(struct MOUSE_DEC *mdec);
void init_keyboard(void);
int mouse_decode(struct MOUSE_DEC *mdec, unsigned char dat);

void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) ADR_BOOTINFO;
	char s[40], mcursor[256], keybuf[32], mousebuf[128];
	int mx, my, i;
	struct MOUSE_DEC mdec;

	init_gdtidt();
	init_pic();
	io_sti(); /* IDT/PICの初期化が終わったのでCPUの割り込み禁止を解除 */
	fifo8_init(&keyfifo, 32, keybuf);
	fifo8_init(&mousefifo, 128, mousebuf);
	io_out8(PIC0_IMR, 0xf9); /* PIC1とキーボードを許可(11111001) */
	io_out8(PIC1_IMR, 0xef); /* マウスを許可(11101111) */

	init_keyboard();

	init_palette();
	init_screen8(binfo->vram, binfo->scrnx, binfo->scrny);
	mx = (binfo->scrnx - 16) / 2; /* 画面中央になるように座標計算 */
	my = (binfo->scrny - 28 - 16) / 2;
	init_mouse_cursor8(mcursor, COL8_008484);
	putblock8_8(binfo->vram, binfo->scrnx, 16, 16, mx, my, mcursor, 16);
	sprintf(s, "(%d, %d)", mx, my);
	putfonts8_asc(binfo->vram, binfo->scrnx, 0, 0, COL8_FFFFFF, s);

	enable_mouse(&mdec);

	for (;;) {
		io_cli();
		if (fifo8_status(&keyfifo) + fifo8_status(&mousefifo) == 0) {
			io_stihlt();
		} else {
			if (fifo8_status(&keyfifo) != 0) {
				i = fifo8_get(&keyfifo);
				io_sti();
				sprintf(s, "%02X", i);
				boxfill8(binfo->vram, binfo->scrnx, COL8_008484, 0, 16, 15, 31);
				putfonts8_asc(binfo->vram, binfo->scrnx, 0, 16, COL8_FFFFFF, s);
			} else if (fifo8_status(&mousefifo) != 0) {
				i = fifo8_get(&mousefifo);
				io_sti();
				if (mouse_decode(&mdec, i) != 0) {
					/* データが3バイト揃ったので表示 */
					sprintf(s, "%02X %02X %02X", mdec.buf[0], mdec.buf[1], mdec.buf[2]);
					boxfill8(binfo->vram, binfo->scrnx, COL8_008484, 32, 16, 32 + 8 * 8 - 1, 31);
					putfonts8_asc(binfo->vram, binfo->scrnx, 32, 16, COL8_FFFFFF, s);
				}
			}
		}
	}
}

#define PORT_KEYDAT			0x0060
#define PORT_KEYSTA			0x0064
#define PORAT_KEYCMD		0x0064
#define PORT_KEYCMD			0x0064
#define	KEYSTA_SEND_NOTREDY	0x02
#define KEYCMD_WRITE_MODE	0x60
#define KBC_MODE			0x47

void wait_KBC_sendready(void)
{
	/* キーボードコントローラーがデータ送信可能になるのを待つ */
	for (;;) {
		if ((io_in8(PORT_KEYSTA) & KEYSTA_SEND_NOTREDY) == 0) {
			break;
		}
	}
	return;
}


void init_keyboard(void)
{
	/* キーボードコントローラの初期化 */
	wait_KBC_sendready();
	io_out8(PORT_KEYCMD, KEYCMD_WRITE_MODE);
	wait_KBC_sendready();
	io_out8(PORT_KEYDAT, KBC_MODE);
	return;
}

#define KEYCMD_SENDTO_MOUSE	0xd4
#define MOUSECMD_ENABLE		0xf4

void enable_mouse(struct MOUSE_DEC *mdec)
{
	/* マウス有効 */
	wait_KBC_sendready();
	io_out8(PORT_KEYCMD, KEYCMD_SENDTO_MOUSE);
	wait_KBC_sendready();
	io_out8(PORT_KEYDAT, MOUSECMD_ENABLE);
	/* うまくいくとACK(0xfa)が送信されてくる */
	mdec->phase = 0; /* マウスの0xfaを待っている段階 */
	return; 
}

int mouse_decode(struct MOUSE_DEC *mdec, unsigned char dat)
{
	 if (mdec->phase == 0) {
		 /* マウスの0xfaを待っている段階 */
		 if (dat == 0xfa) {
			 mdec->phase = 1;
		 }
		 return 0;
	 }
	 if (mdec->phase == 1) {
		 /* マウスの1バイト目を待っている段階 */
		 mdec->buf[0] = dat;
		 mdec->phase = 2;
		 return 0;
	 }
	 if (mdec->phase == 2) {
		 /* マウスの2バイト目を待っている段階 */
		 mdec->buf[1] = dat;
		 mdec->phase = 3;
		 return 0;
	 }
	 if (mdec->phase == 3) {
		 /* マウスの3バイト目を待っている段階 */
		 mdec->buf[2] = dat;
		 mdec->phase = 1;
		 return 1;
	 }
	 return -1; /* ここに来ることはないはず */
}
```

これで少しはコードが見やすくなりました。   
さらにコードを解析するためにbootpack.cのソースコードをいじりました。   
移動情報とボタン状態を表示するようにしています。   
bootpack.c
```c
#include "bootpack.h"
#include <stdio.h>

struct MOUSE_DEC {
	unsigned char buf[3], phase;
	int x, y, btn;
};

extern struct FIFO8 keyfifo, mousefifo;
void enable_mouse(struct MOUSE_DEC *mdec);
void init_keyboard(void);
int mouse_decode(struct MOUSE_DEC *mdec, unsigned char dat);

void HariMain(void)
{
    struct BOOTINFO *binfo = (struct BOOTINFO *) ADR_BOOTINFO;
	char s[40], mcursor[256], keybuf[32], mousebuf[128];
	int mx, my, i;
	struct MOUSE_DEC mdec;

	init_gdtidt();
	init_pic();
	io_sti(); /* IDT/PICの初期化が終わったのでCPUの割り込み禁止を解除 */
	fifo8_init(&keyfifo, 32, keybuf);
	fifo8_init(&mousefifo, 128, mousebuf);
	io_out8(PIC0_IMR, 0xf9); /* PIC1とキーボードを許可(11111001) */
	io_out8(PIC1_IMR, 0xef); /* マウスを許可(11101111) */

	init_keyboard();

	init_palette();
	init_screen8(binfo->vram, binfo->scrnx, binfo->scrny);
	mx = (binfo->scrnx - 16) / 2; /* 画面中央になるように座標計算 */
	my = (binfo->scrny - 28 - 16) / 2;
	init_mouse_cursor8(mcursor, COL8_008484);
	putblock8_8(binfo->vram, binfo->scrnx, 16, 16, mx, my, mcursor, 16);
	sprintf(s, "(%d, %d)", mx, my);
	putfonts8_asc(binfo->vram, binfo->scrnx, 0, 0, COL8_FFFFFF, s);

	enable_mouse(&mdec);

	for (;;) {
		io_cli();
		if (fifo8_status(&keyfifo) + fifo8_status(&mousefifo) == 0) {
			io_stihlt();
		} else {
			if (fifo8_status(&keyfifo) != 0) {
				i = fifo8_get(&keyfifo);
				io_sti();
				sprintf(s, "%02X", i);
				boxfill8(binfo->vram, binfo->scrnx, COL8_008484, 0, 16, 15, 31);
				putfonts8_asc(binfo->vram, binfo->scrnx, 0, 16, COL8_FFFFFF, s);
			} else if (fifo8_status(&mousefifo) != 0) {
				i = fifo8_get(&mousefifo);
				io_sti();
				if (mouse_decode(&mdec, i) != 0) {
					/* データが3バイト揃ったので表示 */
					sprintf(s, "[lcr %4d%4d]", mdec.x, mdec.y);
					if ((mdec.btn & 0x01) != 0) {
						s[1] = 'L';
					}
					if ((mdec.btn & 0x02) != 0) {
						s[3] = 'R';
					}
					if ((mdec.btn & 0x04) != 0) {
						s[2]= 'c';
					}
					boxfill8(binfo->vram, binfo->scrnx, COL8_008484, 32, 16, 32 + 15 * 8 - 1, 31);
					putfonts8_asc(binfo->vram, binfo->scrnx, 32, 16, COL8_FFFFFF, s);
				}
			}
		}
	}
}

#define PORT_KEYDAT			0x0060
#define PORT_KEYSTA			0x0064
#define PORAT_KEYCMD		0x0064
#define PORT_KEYCMD			0x0064
#define	KEYSTA_SEND_NOTREDY	0x02
#define KEYCMD_WRITE_MODE	0x60
#define KBC_MODE			0x47

void wait_KBC_sendready(void)
{
	/* キーボードコントローラーがデータ送信可能になるのを待つ */
	for (;;) {
		if ((io_in8(PORT_KEYSTA) & KEYSTA_SEND_NOTREDY) == 0) {
			break;
		}
	}
	return;
}


void init_keyboard(void)
{
	/* キーボードコントローラの初期化 */
	wait_KBC_sendready();
	io_out8(PORT_KEYCMD, KEYCMD_WRITE_MODE);
	wait_KBC_sendready();
	io_out8(PORT_KEYDAT, KBC_MODE);
	return;
}

#define KEYCMD_SENDTO_MOUSE	0xd4
#define MOUSECMD_ENABLE		0xf4

void enable_mouse(struct MOUSE_DEC *mdec)
{
	/* マウス有効 */
	wait_KBC_sendready();
	io_out8(PORT_KEYCMD, KEYCMD_SENDTO_MOUSE);
	wait_KBC_sendready();
	io_out8(PORT_KEYDAT, MOUSECMD_ENABLE);
	/* うまくいくとACK(0xfa)が送信されてくる */
	mdec->phase = 0; /* マウスの0xfaを待っている段階 */
	return; 
}

int mouse_decode(struct MOUSE_DEC *mdec, unsigned char dat)
{
	 if (mdec->phase == 0) {
		 /* マウスの0xfaを待っている段階 */
		 if (dat == 0xfa) {
			 mdec->phase = 1;
		 }
		 return 0;
	 }
	 if (mdec->phase == 1) {
		 /* マウスの1バイト目を待っている段階 */
		 if ((dat & 0xc8) == 0x08) {
			/* 正しい1バイト目だった */
			mdec->buf[0] = dat;
		 	mdec->phase = 2;
		 }
		 return 0;
	 }
	 if (mdec->phase == 2) {
		 /* マウスの2バイト目を待っている段階 */
		 mdec->buf[1] = dat;
		 mdec->phase = 3;
		 return 0;
	 }
	 if (mdec->phase == 3) {
		 /* マウスの3バイト目を待っている段階 */
		 mdec->buf[2] = dat;
		 mdec->phase = 1;
		 mdec->btn = mdec->buf[0] & 0x07;
		 mdec->x = mdec->buf[1];
		 mdec->y = mdec->buf[2];
		 if ((mdec->buf[0] & 0x10) != 0) {
			 mdec->x |= 0xffffff00;
		 }
		 if ((mdec->buf[0] & 0x20) != 0) {
			 mdec->y |= 0xffffff00;
		 }
		 mdec->y = - mdec->y; /* マウスではy方向の符号が画面と反対 */
		 return 1;
	 }
	 return -1; /* ここに来ることはないはず */
}
```

**実装過程**   
・　[2dc2eed40057daaf4aa84bc00f02a949565165fc](2dc2eed40057daaf4aa84bc00f02a949565165fc)