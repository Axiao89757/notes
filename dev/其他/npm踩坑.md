# 1. npm install 

## 1.1 url找不到

```javascript
npm ERR! code E404
npm ERR! 404 Not Found - GET https://registry.npm.taobao.org/@babel/core/-/core-7.12.17.tgz - [not_found] document not found
npm ERR! 404
npm ERR! 404  '@babel/core@https://registry.npm.taobao.org/@babel/core/-/core-7.12.17.tgz' is not in the npm registry.
npm ERR! 404 You should bug the author to publish it (or use the name yourself!)
npm ERR! 404
npm ERR! 404 Note that you can also install from a
npm ERR! 404 tarball, folder, http url, or git url.
```

解决：

更换registry

```javascript
# 查看 registry 配置
npm config get registry
# 查看npm当前配置
npm config list 
# 临时使用
npm --registry https://registry.npm.taobao.org install 包名
# 设置registry配置
语法：npm config set registry url
# 设置淘宝镜像 通过npm使用
npm config set registry https://registry.npm.taobao.org
# 设置淘宝镜像，通过cnpm使用
npm install -g cnpm --registry=https://registry.npm.taobao.org
# 设置回官网
npm config set registry https://registry.npmjs.org
# 强制清理npm的缓存
npm cache clear --force
```

