为rm命令增加安全性，使得执行后不是直接删除而是放入系统回收站。
首先安装工具
```bash
sudo apt install trash-cli
```
然后修改rm命令
```bash
cd ~
vim .bashrc
# 进入后添加一行alias
alias rm='trash-put'
# 然后保存退出，并使得配置生效
source .bashrc
```
这样以后执行rm命令，就会把文件放入回收站。
清空回收站用 `trash-empty`
| 命令            | 含义                                    |
| --------------- | --------------------------------------- |
| `trash-put`     | trashes files and directories.          |
| `trash-empty`   | empty the trashcan(s).                  |
| `trash-list`    | list trashed file.                      |
| `trash-restore` | restore a trashed file.                 |
| `trash-rm`      | remove individual files from trash can. |