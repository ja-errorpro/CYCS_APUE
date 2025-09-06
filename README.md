# 中原資工 Linux 系統呼叫課程作業環境

參考：Advanced Programming in the UNIX Environment

原本給的教材是不知道幾百年前的東西了，有修改一些東西讓程式碼可以在現代系統上編譯

已測試所使用的 Linux 發行版：Kali Linux 6.12.20

需要額外安裝套件

```
sudo apt-get update && sudo apt-get install -y libbsd-dev
```

並且加了這份檔案以便後續編譯

```
$ cat ~/.local/bin/bcc
#!/bin/sh
gcc $@ -I /home/err0rpro/CYCU-APUE/apue.3e/include -L /home/err0rpro/CYCU-APUE/apue.3e/lib -lapue
```

修改了以下內容:

```
diff '--color=auto' -r ./db/Makefile "/tmp/apue.3e/db/Makefile"
12c12
<   LDCMD=$(CC) -shared -o libapue_db.so.1 -L$(ROOT)/lib -lapue -lc db.o
---
>   LDCMD=$(CC) -shared -Wl,-dylib -o libapue_db.so.1 -L$(ROOT)/lib -lapue -lc db.o
```

```
diff '--color=auto' -r ./filedir/devrdev.c "/tmp/apue.3e/filedir/devrdev.c"
5d4
< #include <sys/sysmacros.h>
```

```
diff '--color=auto' -r ./stdio/buf.c "/tmp/apue.3e/stdio/buf.c"
88c88
< /*
---
>
94c94
< */
---
>
98c98
<       return(fp->_flags & _IONBF);
---
>       return(fp->_flag & _IONBF);
104c104
<       return(fp->_flags & _IOLBF);
---
>       return(fp->_flag & _IOLBF);
111c111
<       return(fp->_IO_buf_end - fp->_IO_buf_base);
---
>       return(fp->_base - fp->_ptr);
```


