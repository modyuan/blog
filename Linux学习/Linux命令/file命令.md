file命令可以判断一个文件是二进制文件还是一个脚本文件。
```bash
$ file `which apt`
/usr/bin/apt: ELF 64-bit LSB shared object, x86-64, ...
$ file `which pip3`
/usr/bin/pip3: Python script, ASCII text executable
```