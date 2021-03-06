# 初识脚本


## 什么是脚本

类似于话剧中的剧本，脚本是计算机的“剧本”，脚本即给计算机一行一行执行的文本。
用不同的语言写脚本，有不同的语法。bash 脚本有 bash 脚本语法，JS 脚本有其语法，只是 JS 脚本使用 JavaScript 语言写的。

## 用 bash 写脚本

### 创建脚本

1. 新建脚本
   - 找个地方新建目录`mkdir ～/local`
   - 进入目录`cd ~/local`
   - 创建脚本`touch demo.txt`（文件后缀无所谓的）
2. 编辑脚本内容如下:
   ```bash
    pwd # 确认一下当前路径是不是 ~/local 或者 /c/Users/你的名字/local
    mkdir demo
    cd demo
    mkdir css js
    touch index.html css/style.css js/main.js
    exit
   ```
3. 在任意位置执行`sh ~/local/demo.txt`即可运行此脚本。
   - `cd ~/Desktop`
   - `sh ~/local/demo.txt`
   - 在当前目录里会多出一个 demo 目录，demo 目录里会有一些文件

### 设置 PATH

1. `cd ~/local;pwd`得到`local`的绝对路径
2. 临时设置 PATH
   a. 运行`export PATH="loacal的绝对路径：$PATH"`,这句话是把 local 目录加到 PATH 里
   b. 这时只需要运行`demo.txt`就相当于运行`sh ~/local/demo.txt`
3. 永久设置 PATH，上面的 PATH 在你重启 Bash 之后就会失效，如果你希望 PATH 一直生效，看下面：
   a. 创建～/.bashrc:`touch ~/.bashrc`
   b. 编辑～/.bashrc:`start ~/.bashrc`
   c. 在编辑器里添加一行：`export PATH="loacal的绝对路径：$PATH"`
   d. `source ~/.bashrc`
   e. 现在只需要运行 demo.txt 即可

### 升级脚本

1. 升级脚本，让目录名可变：
   `$1`表示命令的第一个参数
   `-d $1`判断目录是否存在
   ```bash
    if [ -d $1 ]; then
    echo 'error: dir exists'
    exit
    else
    mkdir $1
    cd $1
    mkdir css js
    touch index.html css/style.css js/main.js
    echo 'success'
    exit
    fi
   ```
2. 创建文件的同时编写内容（注意换行与转义）：
   ```bash
    echo -e "<!DOCTYPE>\n<title>Hello</title>\n<h1>Hi</h1>" > index.html
    echo -e "h1{color: red;}" > css/style.css
    echo -e "var string = \"Hello World\"\nalert(string)" > js/main.js
   ```
3. 返回值
   - `exit 0`表示没有错误
   - `exit 1`表示错误代码为 1

## 用 node.js 写脚本

运行 JS 脚本使用命令`node demo.js`

### 用 JS 切换目录

`console.log(process.cwd())` 打印当前目录
`// process.chdir('~/Desktop');` 这句话不行的，因为 JS 不认识 ~ 目录
`process.chdir("/Users/frank/Desktop")`

### JS 脚本创建目录

- `let fs = require('fs')`请求文件系统
- `fs.mkdirSync('demo')`创建目录

### JS 脚本创建文件

- `let fs = require('fs')`
- `fs.writeFileSync("./index.html", "")`创建内容为空的文件

### 用 JS 脚本重写 demo.sh

1. 创建~/local/jsdemo.js 脚本

```js
#!/usr/bin/env node

var fs = require('fs')

var dirName = process.argv[2] //你传的参数是从第二个开始

if (fs.existsSync('./' + dirName)) {
	console.log('文件已经存在')
} else {
	fs.mkdirSync('./' + dirName)
	process.chdir('./' + dirName)
	fs.mkdirSync('css')
	fs.mkdirSync('js')

	fs.writeFileSync('./index.html', '<!DOCTYPE>\n<title>Hello</title>\n<h1>Hi</h1>')
	fs.writeFileSync('css/style.css', 'h1{color: red;}')
	fs.writeFileSync('js/main.js', 'var string = "Hello World"\nalert(string)')
}

process.exit(0)
```

2. 进入桌面，运行`node ～/localjsdemo.js zzz`可以看到 zzz 目录创建成功

## 什么是 PATH

PATH 的作用：在 Bash 中输入的命令（类似 ls，cd 等），实际上是一个都是一个脚本文件，Bash 都会去 PATH 中寻找对应的文件，找到则运行该文件脚本。

- 使用`type demo` 可以看到该命令的寻找过程
- 使用`which demo` 可以看到该命令的寻找结果

## 什么是 shebang

可以在脚本中配置执行环境，如在 node.js 脚本中第一行添加：
`#!/usr/bin/env node` 则脚本自动运行在 ndoe 环境中

## shell 命令中；、&&与||的使用

### ";"运算符

按先后顺序，一次执行多个命令
语法格式：
`command1；command2；command3`

### "&&"运算符

`&&`左边的命令（命令 1）返回真(即返回 0，成功被执行）后，`&&`右边的命令（命令 2）才能够被执行；
换句话说，“如果这个命令执行成功&&那么执行这个命令”。
语法格式如下：
`command1 && command2 [&& command3 ...]`

### "||"运算符

`||`则与`&&`相反。如果`||`左边的命令（命令 1）未执行成功，那么就执行`||`右边的命令（命令 2）；
换句话说，“如果这个命令执行失败了||那么就执行这个命令。
语法格式如下：
`command1 || command2`

