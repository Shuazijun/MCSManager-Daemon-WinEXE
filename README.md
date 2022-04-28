# MCSManager-Daemon-WinEXE

用PKG打包Daemon守护进程为exe,一键运行不需要安装nodejs环境（但每次更新都需要再次打包）

------

### 测试打包环境需要安装 nodejs（推荐安装v14.19.1）

##### 请根据系统版本选择msi结尾的安装包（node-v14.19.1-x64.msi） [点击跳转至Nodejs下载 (nodejs.org)](https://nodejs.org/download/release/v14.19.1/)

------

系统：Windows 11 x64
系统Node版本：14.19.1
打包模块名称（当前版本5.6.0）：pkg
打包模块安装命令（全局）

```shell
npm install -g pkg
```

Powershell关闭安全限制命令(输入后按Y回车)

```shell
set-ExecutionPolicy RemoteSigned
```

------

打包前需要对Daemon守护进程进行依赖安装（命令）
##在Daemon所在目录鼠标右键-在终端中打开并输入以下指令
```shell
npm install
```

输出exe打包（命令）首次打包会下载一个node的预编译文件,具体时间依个人网络质量而定

```shell
pkg app.js -C GZip -t node14-win-x64
```

```shell
pkg app.js -C Brotli -t node14-win-x64
```

（不带 node14-**xxx**-x64 的xxx参数会生成所有系统的单个可执行文件）
（GZip、Brotli对应压缩，不添加即不压缩。已知Brotli压缩率最高）

------

以上命令目前仅适用于 Daemon 守护进程打包 [MCSManager/MCSManager-Daemon-Production](https://github.com/MCSManager/MCSManager-Daemon-Production)

关于打包后前端链接到守护进程无法正常获取版本号的问题，目前有以下两种解决方案（无版本号不影响使用）

##### 1.保留原有package.json文件到已打包好的exe位置

如题，复制package.json，放到你打包好的app.exe所在位置后再运行程序即可正确识别版本号

##### 2.打开app.js找到版本识别下的对应语句进行修改后再打包（应不知道名字的Suwings说：“你这是暴力解”）

举例版本：**1.5.3**

使用VSCode等软件打开下载好的 **app.js** 搜索以下字段

```js
if(c.default.set("version","Unknown")
```

```js
{return c.default.get("version","Unknown")}
```

将上面的字段中 **Unknown** 修改为 **1.5.3** 后保存更改（每次更新版本都需要重新修改app.js）

再次运行pkg指令打包即可
