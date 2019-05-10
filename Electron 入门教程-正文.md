**Electron 是一个使用 JavaScript、 HTML 和 CSS 等 Web 技术创建跨平台桌面应用程序的框架，它负责比较难搞的部分，你只需把精力放在你的核心业务开发上即可。说到把精力放到核心业务开发上，这听起来特别诱人，但是很多初学者在第一个 Hello World 上被各种拦路虎挡住了，我这篇文章的目标就是帮助大家跳过去，把精力放在核心业务上。**

**这是一篇 Electron 入门教程，让大家能快速搭建开发环境、会写 Hello World、会用 Git 做开发版本管理、会用 SemVer 做发布版本管理、会打包成 exe 文件发布，目标是让大家能把精力放到业务开发上去。**

**适宜人群**
Web 初学者
Electron 初学者
想用 Web 技术开发桌面应用的开发者

**看完本教程，您将收获如下知识点：**
1. 搭建开发环境
2. 使用国内高速镜像
3. 手工创建 Demo 项目
4. 安装 Electron-forge 并自动创建 Demo 项目
5. 处理 loadFile...... is not function报错
6. 编译打包成 exe 
7. 用 Git 做开发版本管理
8. 遵循 SemVer 做发布版本管理
>**tips:**
>本教程Electron 版本是 5.0.0，Node.js 版本是 12.1.0，操作系统是 Windows10，没有Mac OS 和 Linux相关的特定内容。
>Electron 支持 Windows 7 及以上版本，任何在低版本 Windows 上开发 Electron 的尝试都将是徒劳无功的。 如果你的 Windows 低于Windows 7，您可以使用微软向开发者免费提供的 Windows 10 虚拟机镜像，链接：[https://developer.microsoft.com/zh-cn/windows/downloads/virtual-machines](https://developer.microsoft.com/zh-cn/windows/downloads/virtual-machines)。
>
>**约定：**
>下文说在 VSCode 终端工具上执行下面的命令，是指在 Visual Studio Code 横排菜单的那个终端工具里执行命令，当然您完全可以在操作系统自带的 CMD 命令行工具或 Windows PowerShell 上执行命令，在一些场景它可能还更高效，如果您还不会灵活使用，不用纠结，下面按提示操作即可。
>

**由于本人能力有限，疏漏或错误在所难免，还望读者朋友若发现错误或者其他问题，能给予谅解及批评指正。**

---
# 一、简介
Electron 通俗点讲就是用 HTML、CSS、JavaScript 开发跨平台桌面应用程序的一个框架。
专业介绍请看官网：[https://electronjs.org/](https://electronjs.org/) ，但是不建议去看了，我想您搜到这篇文章，是**直接来收干货**的。
# 二、安装
1. 安装 Visual Studio Code

**下载并安装：**[https://code.visualstudio.com](https://code.visualstudio.com/)（不得不提，VSCode就是 Electron 开发的，如果您有其他IDE今天可以不下载它）
Windows 系统下载后直接下一步下一步，按默认选项安装即可，这里不再讲安装。
今天也不深入讲解VSCode的使用，也不建议你去看，不过教程传送门还是上一下：[https://www.jianshu.com/p/75652eb00680](https://www.jianshu.com/p/75652eb00680)
2. 安装 Node.js

Electron 依赖 Node.js，所以要先下载并安装 Node.js，不过您得先确定要装哪个版本的 Electron，然后下载对应的 Node.js 版本，比如我这里是用Electron 5.0.1，我下载的 Node.js 是12.1.0。
![图片](https://uploader.shimo.im/f/cEY0bQOK9SobYhUT.png!thumbnail)
各版本如图
Windows 系统下载后直接下一步下一步，按默认选项安装即可，这里不再讲安装。npm 是包管理器，它会一起安装，下面将用到它。
**下载并安装：**[http://nodejs.cn/download/](http://nodejs.cn/download/)
检查安装是否成功，在 VSCode 终端工具上执行命令：
```
node -v
npm -v
```
成功输出版本则安装成功
1. 安装 Electron

在 VSCode 终端工具上用  npm 包管理器安装 Electron (在国内怕一天一夜都没装起来的朋友，请自动忽略这一步，跳到淘宝镜像那一步)：
```
npm install -g electron
```
弃用上面的命令，执行下面的命令，用淘宝镜像提速：
```
npm install -g cnpm --registry=https://registry.npm.taobao.org
cnpm install -g electron
```
>淘宝镜像官网：[https://npm.taobao.org/](https://npm.taobao.org/)
>cnpm 跟 npm 用法完全一致
>删除 Electron：cnpm uninstall electron
>升级 Electron：cnpm update electron -g

如果执行上一步提示了 npm 版本过低，在 VSCode 终端工具上用下面的命令升级 npm。
```
cnpm install -g npm
```
>tips：如果想不同项目使用不同版本的 Electron，安装时就不能用 -g 参数，如生产环境用 LTS 稳定版本，新功能学习用 Current 版本。
>分别用到参数为 -g，--save，--save-dev，-D。
>关于参数的更多介绍请看教程：[http://www.cnblogs.com/limitcode/p/7906447.html](http://www.cnblogs.com/limitcode/p/7906447.html)
>--save：是将模块安装到项目目录下，并在 package.json 文件的 dependencies 节点写入依赖
>--save-dev 或 -D：意思是将模块安装到项目目录下，并在 package.json 文件的 devDependencies节点写入依赖
>那 package.json 文件里面的 devDependencies  和 dependencies 对象有什么区别呢？devDependencies  里面的插件只用于开发环境，不用于生产环境，而 dependencies  是需要发布到生产环境的
>为什么要保存在 package.json？因为节点插件包相对来说非常庞大，不加入版本管理，将配置信息写入 package.json，方便将其加入版本管理，其他开发者对应下载即可（命令提示符执行 npm install，则会根据 package.json 下载所有需要的包）。
>
>全局安装：-g
>全局安装代码：cnpm install -g electron
>全局将会安装在 C：\ Users \ Administrator \ AppData \ Roaming \ npm，并且写入系统环境变量；
>全局安装可以通过命令行任何地方调用它；
>
>非全局安装：--save-dev
>非全局代码：cnpm install --save-dev electron
>非全局安装：将会安装在当前定位目录（需要cd切换到具体项目目录，或者在项目目录按住 Shift，然后右键，启动 Powershell 执行命令）；
>非全局安装将安装在定位目录的 node_modules 文件夹下，通过要求（）调用；
1. 安装 Electron-forge
>如果想手工搭建项目可以不安装

Electron-forge 是 Electron 的一个脚手架，它可以自动生成项目文件，这篇文章不会深入讲它，也可以选择安装，但是暂时不学它。
在 VSCode 终端工具上执行命令：
```
cnpm install -g electron-forge
```
# 三、手工创建项目
1. **按下面的目录结构创建文件夹**
```
D:\ElectronProject   
   ├── ElectronDemo
```
手工在 D 盘创建文件夹，ElectronDemo 文件夹在 ElectronProject 文件夹里面（这次推荐手工快速建），或者在 VSCode 终端工具上执行下面的命令创建文件夹：
```
D: #本行写法是为了兼容 cmd 命令行工具切换盘符的写法
mkdir \ElectronProject\ElectronDemo
```
1. 创建文件

用 Visual Studio Code 软件来新建三个文件，这里不多说，跟微软其他软件新建文件一样，创建完以后，保存到 D:\ElectronProject\ElectronDemo，目录是这样的。
>习惯用其他 IDE，请直接有用其他 IDE，注意新建的文件编码用 UTF-8
```
D:\ElectronProject\ElectronDemo    
                   ├── package.json    
                   ├── index.js    
                   └── index.html
```
三个文件之间的关系：index.html 是我们想要显示的页面，index.js 为此应用的入口，package.json 为 npm 项目的配置文件。

1. 初始化 package.json

在 VSCode 终端工具上执行下面的命令：
```
cd D: #本行写法是为了兼容 cmd 命令行工具切换盘符
cd \ElectronProject\ElectronDemo
npm init
```
>如果你觉得cd命令慢，你可以在 ElectronDemo 文件夹下按住 Shift 键，然后点右键，启动 Powershell，然后输入 npm init 后回车

然后一路回车，可以全部用默认，也可以改内容，创建完，我这里显示如下，其中版本我改了一下
```
{
  "name": "ElectronDemo",
  "version": "0.1.0",
  "description": "test",
  "main": "index.js",
  "author": "qian",
  "license": "ISC",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  }
}
```

1. 在 index.js 里写代码
```
//引入 electron 模块，它必须引入
//该模块导出了一个 app 对象和一个 BrowserWindow 类，app 对象包含一些方法，如 on 方法用于将事件绑定到事件函数中。
const {app,BrowserWindow} =require（'electron')
// 保持一个对于 window 对象的全局引用，如果你不这样做，
// 当 JavaScript 对象被垃圾回收， window 会被自动地关闭
let mainWindow
function createWindow(){
  //创建浏览器窗口
  mainWindow = new BrowserWindow({width:1024,height:768})
  //载入index.html
 mainWindow.loadURL(`file://${__dirname}/index.html`)
}
//ready是当Electron完成初始化后触发，这里初始化后就会去执行 createWindow 函数，然后创建浏览器窗口并加载主页面。
app.on('ready',createWindow)
```
>关于加载 html 的代码（上面第9行），在很多地方看到的写法是 mainWindow.loadFile('./index.html') ，但是会报 mainWindow.loadFile is not a function 错误，改写成上面的 mainWindow.loadURL 写法，它加载一个 url，可以是本地文件或者是远程 url，则不会报错，注意不是单引号，是键盘ESC键下面的那个，并且注意是要英文输入法下输入。
1. 在 index.html 里写代码
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8">
        <title>Electron 项目</title>
    </head>
    <body>
        <h1>这是我们的第一个 ELECTRON 练习项目</h1>
    </body>
</html>
```
1. 运行项目

在 VSCode 终端工具上执行下面的命令：
```
cd D:\ElectronProject\ElectronDemo
```
>tips：不切换到项目目录，无法启动项目
>如果你觉得cd命令慢，你可以在 ElectronDemo 文件夹下按住 Shift 键，然后点右键，启动 Powershell，然后执行 electron . 命令

继续执行命令：
```
electron .
```
不出意外，将正常启动项目

# 四、使用electron-forge自动创建项目
在 VSCode 终端工具上执行下面的命令：
```
D: #本行写法为兼容 cmd 命令行工具切换盘符的写法
cd \ElectronProject
electron-forge init Electron-forgeDemo
```
>如果你觉得cd命令慢，你可以在 ElectronProject 文件夹下按住 Shift 键，然后点右键，启动 Powershell，然后输入 electron-forge init Electron-forgeDemo 后回车
>创建过程杀毒软件可能会对操作dll进行阻止，点允许。

创建成功后，它会自动生成一个文件夹，文件目录应该是这样：
```
D:\ElectronProject   
   ├── ElectronDemo
   ├── Electron-forgeDemo
```

继续执行下面的命令启动项目
```
cd \ElectronProject\Eelectron-forgeDemo
electron-forge start
```
>tips：不切换到项目目录，无法启动项目
>如果你觉得cd命令慢，你可以在 Eelectron-forgeDemo 文件夹下按住 Shift 键，然后点右键，启动 Powershell，然后输入 electron-forge start 后回车
# 五、打包
1. 安装 Electron-prebuilt

它是一个 Electron 的预编译版本
```
cnpm install -g electron-prebuilt
```
1. 安装 Electron-packager 

它也是一个用于打包 Electron 应用的工具
```
cnpm install -g electron-packager
```
1. 打包

3.1 在 VSCode 终端工具上执行下面的打包命令：
```
electron-packager . ElectronDemo --win --out ../myElectronDemo --arch=x64 --electron-version=5.0.0 --overwrite --ignore=node_module
```
>格式这样
>electron-packager <应用目录> <应用名称> <打包平台> --out <输出目录> <架构> <应用版本>
>对应关系
>electron-packager . ElectronDemo --win --out ../myElectronDemo --arch=x64 --electron-version=5.0.0 --overwrite --ignore=node_module
>. 点代表当前目录
>ElectronDemo 代表应用名称
>win 代表打包平台 Windows
>../myElectronDemo 在项目上级文件夹新建文件夹
>electron-version=5.0.0  代表 Electron 的版本号，我这里是5.0.0
>--overwrite 是覆盖原文件
>--ignore=node_module 是忽略node_module

3.2 也可以将打包命令写到 package.json 文件里
```
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
    "package":"electron-packager . ElectronDemo --win --out ../myElectronDemo --arch=x64 --electron-version=5.0.0 --overwrite --ignore=node_module"
  }
```
然后在 VSCode 终端工具上执行下面的打包命令：
```
cnpm run-script package
```
上面两步的任何一步，执行完毕后，看到父级目录下已经产生了我们希望看到的应用文件夹，双击里面的应用程序 electron01.exe 就可以直接打开桌面应用了，不过还不是安装包。
1. 图标

如果我们想要更改窗口左上角的图标和任务栏的图标，只需要在打包的命令上加个 icon参数就好了。
```
electron-packager . ElectronDemo --win --out ../myElectronDemo --arch=x64 --electron-version=5.0.0 --overwrite --ignore=node_module --icon=./app/img/icon.ico
```
1. 生成安装包

5.1 使用火凤（[www.hofosoft.cn](http://www.hofosoft.cn)），这个比较简单，但是可控项目少，今天推荐用它。
5.2 使用 NSIS（[https://www.cnblogs.com/modou/p/3573772.html](https://www.cnblogs.com/modou/p/3573772.html)）。

# 五、开发版本控制
>这里我们用 Git 进行版本控制
>它是 Linux 创始人 *Linus Benedict Torvalds* 的第二神作，其他版本控制软件可以放弃了。
>想深学更多 Git，请看 Git 教程：[https://www.liaoxuefeng.com/wiki/896043488029600](https://www.liaoxuefeng.com/wiki/896043488029600)，不过今天不建议你点进去，后面再说，今天还不用去学那么深，不过它是必学。
1. 下载并安装 Git 客户端：[https://www.git-scm.com/download/](https://www.git-scm.com/download/)
2. 设置用户和邮箱

安装后，在 Windows 开始菜单找到并启动 Git Bash，设置用户和邮箱，执行下面的命令。至于为什么要要设置这个，暂时可以不管，如果要看为什么，看上面的 Git 教程。
```
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```
1. 生成本地仓库

在项目目录下右键，启动 Git Bash Here，并执行命令。
```
git init
```
这时将生成一个 git 文件夹，这个目录是 Git 用来跟踪管理版本库的，里面的东西不要手动去修改。
PS：解释一个谜团，所有的版本控制系统，其实只能跟踪文本文件的改动，另外文本是有编码的，如果没有历史遗留问题，建议统一用 UTF-8（VSCode新建的文件默认了UTF-8，在右下角可以看到），另外不要用 Windows 的记事本，就算设置为 UTF-8 也不要用，具体原因可百度。而图片、视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，也就是只知道图片从 10KB 改成了 30KB，但到底改了啥，版本控制系统不知道。
1. 提交文件到本地仓库

初始化好 git 仓库，我们将文件夹下的文件添加到 git 仓库。
第一步：在项目文件夹下右键，启动 Git Bash Here（如果上一步你关了的话才要重新启动），用 git add 命令告诉 Git，把文件添加到仓库，如果是所有文件，用点（.）表示，下面有两种代码，选一种。
```
git add 文件名.后缀
git add .
```
执行后没有什么提示，那就正确了，没有提示就是最好的提示。
第二步：用 git commit 命令告诉 Git，把文件提交到仓库，提交一次一个版本，后续可以回滚。
```
git commit -m "提交备注"
```
>PS：如果你对提交为什么要先 add 后 commit 的两步设计有疑问，请看上面的 Git 教程。备注也是重中之中，千万不要写一些没有意义的字符，不然后面可能会哭，正确的做法是简要描述你本本次提交里面改了或者新增了什么。
1. 远程仓库
>今天可以允许您跳过本节，不过它很重要后面一定要学。
>这里推荐选择的远程仓库平台有：
>码云：[https://gitee.com/](https://gitee.com/)，私有仓库也免费，生态差一点。
>GitHub：[https://github.com](https://github.com/)，私有仓库收费，生态比较好一点。
>下面我们讲 GitHub，先用邮箱注册一个账号即可，后面设置到 username，这里取好听一点，不过后面可改，这个是公开的，比如PG大神德哥的 GitHub 地址：[https://github.com/digoal](https://github.com/digoal) 上的digoal。
### **5.1 创建 SSH Key**
在项目目录右键，打开 Git Bash Here，然后输入如下命令，注意替换双引号里面的内容，然后一路回车，遇到要输入 y/n，输入 y 同意
```
ssh-keygen -t rsa -C "youremail@example.com"
```
>如果您的 github 地址是：[https://github.com/lixiang](https://github.com/lixiang)，那么你上面的"youremail@example.com"改为"lixiang@github.com"
>
>本地 Git 仓库和 GitHub 仓库之间的传输是通过 SSH 加密的，远程推送需要验证身份，所以需要设置，打开Git Bash，创建SSH Key。
>邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人，这里就放GitHub。
>![图片](https://uploader.shimo.im/f/iPUgZB4my5MsIj5H.png!thumbnail)
>生成的文件类似这样

创建过程，命令行退显示这个文件创建后在那个目录，可以到那个目录找到 id_rsa.pub ，然后在 GitHub 官网
* 右上角头像有个倒三角小图，到里面，点开 “Settings”
* 左侧竖排导航，点开 “SSH and GPG keys”
* 点击“New SSH key” 按钮
* Title 标题随便写，不过因为这个是每台电脑不一样，标题最好写明是您哪台电脑的
* Key 应该填写的是在本地电脑执行命令生成的 id_rsa.pub 文件里的内容，用**记事本**打开，全选，复制，粘贴到Key这里
* 点 “Add SSH key”按钮，提交，操作完成
>您可能还会留意到有教程，还可以用 https，如：[https://github.com/lixiang/ElectronDemo](https://github.com/lixiang/ElectronDemo).git，来连接，这里不推荐，用它速度会慢一点，另外每次连接要输入密码，有点麻烦。
### **5.2 创建一个新的仓库**
在 Git Hub 右上角找到 “Create a new repo” 按钮，填写仓库名 ElectronDemo ，然后点击 “Create repository” 按钮创建一个仓库。
>这里不用勾选生成 README，如果勾选了，后面第一次本地与远程同步，需要先 git pull 拉远程的到本地，然后 git push 推上去，我们今天只是入门，所以不建议大家搞复杂了。

### **5.3 本地仓库与远程仓库关联**
在本地项目文件夹里空白处右键，打开 Git Bash Here，然后输入如下命令
```
git remote add origin git@github.com:lixiang/ElectronDemo.git
```
>把上面的 lixiang 替换成你自己的 GitHub 账户名，否则您无法把您本地的项目推送到 Git Hub 仓库的，因为你本地没有我的 id_rsa.pub 公钥。ElectronDemo 替换成 Github 官网上的仓库名称（repository)，注意 Github 上的仓库名称，**不支持中文**
>执行命令如果提示：fatal: remote origin already exists. 它的意思是远程起源已经存在。那么执行：git remote rm origin 然后再重新执行 git remote add origin ......
### 5.4 本地库的 master 分支推送到 远程库
在本地项目文件夹里空白处右键，打开 Git Bash Here，然后输入如下命令
```
git push -u origin master
```
>由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

至此，本地仓库的内容就与远程仓库的内容一致了。
关于远程仓库我们本教程就到这里，其他内容建议学习上面的 Git 教程，真心写的好。
### 5.5 接下来本地库有 git commit -m "提交备注" 后，如果要同步到远程 Github 操作如下：

# 六、生产版本管理
生产版本管理，要根据下面这样的语义规则来，比如2.0.0跟1.5.3之间，应该是有 API 突破性变更，或者 Node.js 重大版本跟新，或者 Chromium 版本更新了，才能改最前面的一位，不是随意编出来的，我们发布版本时版本编号应该如下这样来确定每位的增加。
![图片](https://uploader.shimo.im/f/owfMS0akhQINsMcj.png!thumbnail)这里不再赘述，直接上别人的教程，今天可以不用深读。
生产版本建议参考：[https://semver.org/lang/zh-CN/](https://semver.org/lang/zh-CN/)
Electron 遵循：[https://electronjs.org/docs/tutorial/electron-versioning#semver](https://electronjs.org/docs/tutorial/electron-versioning#semver)

# 这里是这篇教程的结尾，另一篇教程的预告。
下一篇教程我们将精力放在业务上面，我们试着做个小项目，这样对大家的学习更有帮助，我们要尽快从 Hello World 往后面走，怀旧不适合学习编程，对吧。
强烈建议加QQ群：1025951999，方便交流，也方便有文章时能及时通知大家。大家也请在 GitChat 上关注我，以便更容易找到我。
# 项目
# 设计一个文本编辑器


