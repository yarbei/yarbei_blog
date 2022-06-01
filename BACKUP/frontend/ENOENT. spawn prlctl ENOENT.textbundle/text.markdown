## ENOENT. spawn prlctl ENOENT

Mac打包electron项目Windows安装包报错如下：

**Error: Exit code: ENOENT. spawn prlctl ENOENT**

![1650882986721-ea136229-2617-4ff6-9da7-bcd609aaa2eb.png](assets/1650882986721-ea136229-2617-4ff6-9da7-bcd609aaa2eb.png)

出现这种原因只需将electron-builder升级到最新版本即可

执行yarn add electron-builder@latests -D

```javascript
yarn add electron-builder@latests -D
```

