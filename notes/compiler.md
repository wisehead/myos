#1.作者编译程序顺序
asm->making-->run

* c文件：c语言文件。
* gas文件：c语言生成的汇编语言，只不过是gcc版的asm，不是作者用的nas汇编。（$(CC1) cc1.exe -o bootpack.gas bootpack.c）
* nas文件：汇编语言文件，类似于ASM。gas文件可以生成nas文件。（$(GAS2NASK) gas2nask.exe bootpack.gas bootpack.nas）
* obj文件：目标文件，nas文件生成。（$(NASK) nask.exe bootpack.nas bootpack.obj bootpack.lst）
bim文件：obj文件生成bim文件。经过link操作的二进制文件。（$(OBJ2BIM) obj2bim.exe @$(RULEFILE) out:bootpack.bim stack:3136k map:bootpack.map bootpack.obj naskfunc.obj）
//问题？？bim和obj，bin的关系？？？？？？？
hrb文件：bim生成hrb文件。在bim的基础上，还需要针对每一种操作系统的要求进行必要的加工，比如加上识别用的文件头，或者压缩等。($(BIM2HRB) bim2hrb.exe bootpack.bim bootpack.hrb 0)

* bin文件：二进制文件，nas文件生成。但是这个文件不能和目标文件连接，因为它不是目标文件。还要加上连接所需要的接口信息，将它变为目标文件。这项工作由bin2obj.exe完成。
* sys文件：hrb文件+ asmhead.bin文件生成，操作系统的全部代码（haribote.sys : asmhead.bin bootpack.hrb Makefile  ———— copy /B asmhead.bin+bootpack.hrb haribote.sys）
img文件：真正的镜像文件，可执行文件。sys文件+ipl.bin。（haribote.img : ipl10.bin haribote.sys Makefile
    $(EDIMG)   imgin:../z_tools/fdimg0at.tek \
        wbinimg src:ipl10.bin len:512 from:0 to:0 \
        copy from:haribote.sys to:@: \
        imgout:haribote.img）

* lst文件：文本文件，简单的确认每个指令是怎样翻译成机器语言的。（../z_tools/nask.exe ipl.nas ipl.bin ipl.lst）



#2.推导规则：
* nask.exe: *.nas-->*.bin
* edimg.exe: *.bin --> *.img

* run:

    copy helloos.img ..\z_tools\qemu\fdimage0.bin
    ../z_tools/make.exe -C ../z_tools/qemu

* install://暂时用不到，用于真正的windows机器。我们用虚拟机，所以make run就可以。


    ../z_tools/make.exe img
    ../z_tools/imgtol.com w a: helloos.img