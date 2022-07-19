# npm更新报错问题

# npm更新遇到报错问题

npm提示更新，如下：

```shell
╭────────────────────────────────────────────────────────────────╮
   │                                                                │
   │      New major version of npm available! 6.14.16 → 8.11.0      │
   │   Changelog: https://github.com/npm/cli/releases/tag/v8.11.0   │
   │               Run npm install -g npm to update!                │
   │                                                                │
   ╰────────────────────────────────────────────────────────────────╯
```

运行npm install -g npm后报错如下：

```shell
npm ERR! code EACCES
npm ERR! syscall access
npm ERR! path /usr/local/lib/node_modules/npm/node_modules/abbrev
npm ERR! errno -13
npm ERR! Error: EACCES: permission denied, access '/usr/local/lib/node_modules/npm/node_modules/abbrev'
npm ERR!  [Error: EACCES: permission denied, access '/usr/local/lib/node_modules/npm/node_modules/abbrev'] {
npm ERR!   errno: -13,
npm ERR!   code: 'EACCES',
npm ERR!   syscall: 'access',
npm ERR!   path: '/usr/local/lib/node_modules/npm/node_modules/abbrev'
npm ERR! }
npm ERR! 
npm ERR! The operation was rejected by your operating system.
npm ERR! It is likely you do not have the permissions to access this file as the current user
npm ERR! 
npm ERR! If you believe this might be a permissions issue, please double-check the
npm ERR! permissions of the file and its containing directories, or try running
npm ERR! the command again as root/Administrator.
```

解决方案：

```shell
# 在命令前加上sudo就好了
sudo npm install -g npm
```
