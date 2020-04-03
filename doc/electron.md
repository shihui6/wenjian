windows

需要有 Python2 和 本地编译环境（比如命令行的vs或者完整的vs环境）

方案一：安装命令行编译工具

```bash
### 会同时安装python2
npm install --global --production windows-build-tools
```

方案二：安装完整编译环境

* 安装python2环境
* 安装vs2017（或vs2015）

::: tip
方案一与方案二在smart项目中没有优劣之分，采用什么方案取决于你机器上已有的环境

专注于前端开发的人员一般只需要使用方案一即可。
:::