# node 

## npm 仓库加速

```bash

npm config set registry https://registry.npm.taobao.org
npm config get registry (验证registry是否配置成功)

npm config set disturl https://npm.taobao.org/dist
npm config get disturl (验证disturl是否配置成功)

npm config set electron_mirror https://npm.taobao.org/mirrors/electron/ 
npm config get electron_mirror (验证electron_mirror 是否配置成功)

npm config set sass_binary_site https://npm.taobao.org/mirrors/node-sass/ 
npm config get sass_binary_site (验证sass_binary_site 是否配置成功)

npm config set phantomjs_cdnurl https://npm.taobao.org/mirrors/phantomjs/
npm config get phantomjs_cdnurl (验证phantomjs_cdnurl 是否配置成功)

```