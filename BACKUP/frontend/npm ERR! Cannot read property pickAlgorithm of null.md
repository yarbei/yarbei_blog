# npm ERR! Cannot read property 'pickAlgorithm' of null

# npm ERR! Cannot read property 'pickAlgorithm' of null

#### 错误描述

> 安装某 package 时，提示报错：
>

> npm ERR! Invalid response body while trying to fetch [https://registry.npmjs.org/inherits:](https://links.jianshu.com/go?to=https%3A%2F%2Fregistry.npmjs.org%2Finherits%3A) Cannot read property 'pickAlgorithm' of null
>

#### 解决

清理缓存后再尝试安装

```javascript
npm cache clear --force
```
