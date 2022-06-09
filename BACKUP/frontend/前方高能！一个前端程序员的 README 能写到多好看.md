# 前方高能！一个前端程序员的 README 能写到多好看

# 前方高能！一个前端程序员的 README 能写到多好看

---

## 一、文件地址

这个 Markdown 包含了我觉得实用又好看的小组件。

文件地址：[传送门](https://github.com/syy11cn/syy11cn/blob/main/README.md)。

## 二、如何写这样一个花哨的 Markdown 作为个人简介？

### 2.1 居中效果

Markdown 里可以使用 HTML 进行高级样式布局。如果你想让你的内容剧中，只需要在你的内容之外加一个 div 标签，并设置 align 属性。

\# 你的 Markdown 内容

复制代码 ### 2.2 Badges 在我的 Markdown 中，有两处用到了五彩缤纷的 badges，分别是 **联系方式** 和 **前端技术栈**。 两处的 badges 图标均由 [shields.io](https://shields.io) 网站提供。在这个网站上，你只需要提供需要显示的内容，就能得到一个精美的 badge。你还可以自定义样式，可以说非常灵活。 联系方式部分，还使用到了 [SubStats](https://substats.spencerwoo.com/#why-i-did-this)。这是一个致力于提供社交帐号粉丝数统计服务的开源项目，你只需要按照指定格式要求，就能从他们的开放接口获得你在某个平台的粉丝数量。然后在 [shields.io](https://shields.io) 的 Dynamic 部分，提供固定格式的查询信息，就能 **将动态查询到的数据与 badges 结合起来使用**。

### 2.3 仓库卡片 & 分析统计

这两个部分使用到了 [GitHub Readme Stats](https://github.com/anuraghazra/github-readme-stats) 和 [GitHub Profile Trophy](https://github.com/ryo-ma/github-profile-trophy) 开源项目。

你只需复制你想要的卡片链接，在 Markdown 中插入对应图片，修改 **用户名**、**仓库名** 等信息即可。这个开源项目的 README 文件中由具体的使用方式。

### 2.4 阅读数统计

阅读数统计功能使用了[ GitHub Profile Views Counter ](https://github.com/antonkomarev/github-profile-views-counter)开源项目。

只需在 Markdown 中插入 View Counter 提供的 badge，然后在 Markdown 的末尾插入一个由该开源项目提供的 hitter （一个大小为 1 平方像素的透明图片）。

这样，每当有人访问你的 profile，hitter 都会自动被请求，这会帮你为你的 view count 数量加 1。

## 三、作用

GitHub 在之前的一次更新中，为每位 GitHub 用户的仓库加入了魔法——只需要创建一个命名和自己的 **账户名称** 完全一样的仓库，并在这个仓库的根目录放入一个 [README.md](http://README.md) 文件，该文件将展示在个人 GitHub 首页。

效果如下图所示，你也可以访问 [我的 GitHub](https://github.com/yarbei) 进行浏览（欢迎关注 👏 欢迎提出宝贵意见和建议 🙌）。

如果这篇文章对你编写个人 GitHub 主页的介绍文件有帮助的话，烦请帮我点一个宝贵的 👍 你的支持是我创作的最大动力~

外观花哨起来了，下一步就是充实内容了，加油！

```javascript
# This is yarbei 👋 ![Profile View Counter](https://komarev.com/ghpvc/?username=yarbei)


<!--
**yarbei/yarbei** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I'm currently working on ...
- 🌱 I'm currently learning ...
- 👯 I'm looking to collaborate on ...
- 🤔 I'm looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->

- :man_technologist: 𝑾𝒐𝒓𝒌 𝒉𝒂𝒓𝒅 𝒕𝒐 𝒃𝒆 𝒂 𝒈𝒐𝒐𝒅 𝒇𝒓𝒐𝒏𝒕𝒆𝒏𝒅 𝒅𝒆𝒗𝒆𝒍𝒐𝒑𝒆𝒓.
- :email: 𝑅𝑒𝑎𝑐ℎ 𝑚𝑒 𝑎𝑡 [@qq](mailto:949883887@qq.com).
- :house: [𝑩𝒍𝒐𝒈](https:///) • [𝑷𝒓𝒐𝒇𝒊𝒍𝒆](https://yarbei.com) 

<!-- Github Stats -->

![Anurag's GitHub stats](https://github-readme-stats.vercel.app/api?username=yarbei&show_icons=true)

![](https://hit.yhype.me/github/profile?user_id=43692064)
```

```javascript
<div align=center>

<img alt="Yiyang Sun" src="./assets/avatar.png" width=100 />

# Hi, this is Yiyang Sun :wave:

<p>

[![Bilibili](https://img.shields.io/badge/dynamic/json?labelColor=FE7398&logo=bilibili&logoColor=white&label=bilibili%20fans&color=00aeec&query=%24.data.totalSubs&url=https%3A%2F%2Fapi.spencerwoo.com%2Fsubstats%2F%3Fsource%3Dbilibili%26queryKey%3D439734028)](https://space.bilibili.com/439734028)
[![Zhihu](https://img.shields.io/badge/dynamic/json?color=142026&labelColor=0066ff&logo=zhihu&logoColor=white&label=zhihu%20fans&query=%24.data.totalSubs&url=https%3A%2F%2Fapi.spencerwoo.com%2Fsubstats%2F%3Fsource%3Dzhihu%26queryKey%3Dsyy11cn)](https://www.zhihu.com/people/syy11cn)
[![Juejin](https://img.shields.io/badge/juejin-%E5%AD%99%E8%BD%B6%E6%89%AC-1e80ff?logo=bytedance)](https://juejin.cn/user/4010632618185038)
[![Github Stars](https://img.shields.io/github/stars/syy11cn?color=faf408&label=github%20stars&logo=github)](https://github.com/syy11cn)

</p>

<p>

[![Website](https://img.shields.io/badge/personal%20website-syy11.cn-b860ff?logo=html5&logoColor=white&labelColor=red)](https://syy11.cn)
[![Wechat Subscription Account](https://img.shields.io/badge/subscription%20account-%E5%AD%99%E8%BD%B6%E6%89%AC-1e80ff?logo=wechat)](https://mp.weixin.qq.com/mp/profile_ext?action=home&__biz=MzIwNzQxNTgxNQ==&scene=124#wechat_redirect)

</p>

![Profile View Counter](https://komarev.com/ghpvc/?username=syy11cn)

## Introduction :raised_hands:

Student of [@UESTC](https://github.com/uestcer). :school:

Major in Software Engineering. :man_technologist:

I love open source spirit. :heart:

Hope to make more friends in open source projects. :eyes:

## Orientation :dart:

I love coding. :heart:

I love Front End technologys. :heart:

<p>

![HTML5](https://img.shields.io/badge/-HTML5-red?logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/-CSS3-blue?logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/-JavaScript-yellow?logo=javascript&logoColor=white)

</p>

<p>

![TypeScript](https://img.shields.io/badge/-TypeScript-blue?logo=typescript&logoColor=white)
![Vue](https://img.shields.io/badge/-Vue-34495e?logo=vue.js)
![React](https://img.shields.io/badge/-React-282c34?logo=react)
![MiniProgram](https://img.shields.io/badge/-MiniProgram-07c160?logo=wechat&logoColor=white)

</p>

<p>

![Vite](https://img.shields.io/badge/-Vite-646cff?logo=vite&logoColor=white)
![Rollup](https://img.shields.io/badge/-Rollup-ef3335?logo=rollup.js&logoColor=white)
![Webpack](https://img.shields.io/badge/-Webpack-1a6bac?logo=webpack)

</p>

## Projects :computer:

[![Config Router](https://github-readme-stats.vercel.app/api/pin/?username=syy11cn&repo=config-router)](https://github.com/syy11cn/config-router)

[![Course Assistant](https://github-readme-stats.vercel.app/api/pin/?username=syy11cn&repo=course-assistant-miniprogram-fe)](https://github.com/syy11cn/course-assistant-miniprogram-fe)

[![My Blog](https://github-readme-stats.vercel.app/api/pin/?username=syy11cn&repo=my-blog)](https://github.com/syy11cn/my-blog)

[![Typecho Theme](https://github-readme-stats.vercel.app/api/pin/?username=syy11cn&repo=18px-Typecho-Theme)](https://github.com/syy11cn/18px-Typecho-Theme)

## Analysis :point_down:

[![TopLangs](https://github-readme-stats.vercel.app/api/top-langs/?username=anuraghazra&layout=compact)](https://github.com/anuraghazra/github-readme-stats)

![Anurag's GitHub stats](https://github-readme-stats.vercel.app/api?username=syy11cn&show_icons=true&bg_color=30,e96443,904e95&title_color=fff&text_color=fff)

![](https://github-profile-trophy.vercel.app/?username=syy11cn&theme=flat&column=7&margin-w=10)

</div>

![](https://hit.yhype.me/github/profile?user_id=57290456)
```
