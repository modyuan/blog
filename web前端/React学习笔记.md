# React学习笔记

## 前置技能

- Html
- CSS
- JavaScript
- nodejs的使用

## npm配置

把npm的镜像源设为淘宝的：

```bash
npm config set registry https://registry.npm.taobao.org
```

还原：

```bash
npm config ``set` `registry https:``//registry.npmjs.org/
```



## npm命令参考

```bash
npm install moduleName # 安装模块到项目目录下 

npm install -g moduleName # -g 的意思是将模块安装到全局，具体安装到磁盘哪个位置，要看 npm config prefix 的位置。

npm install -save moduleName # -save 的意思是将模块安装到项目目录下，并在package文件的dependencies节点写入依赖。

npm install -save-dev moduleName # -save-dev 的意思是将模块安装到项目目录下，并在package文件的devDependencies节点写入依赖。

# —-save 可以简写成 -S
# --save-dev 可以简写成 -D
# install 可以简写成 i
```

## 依赖包

- react
- react-dom
- babel  (babel-standalone)