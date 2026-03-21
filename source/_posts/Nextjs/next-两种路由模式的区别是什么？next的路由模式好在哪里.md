---
title: next-两种路由模式的区别是什么？next的路由模式好在哪里?
toc: true
toc_number: 6
tags:
  - next
categories:
  - Next
abbrlink: 6e30716c
date: 2026-03-20 21:58:32
---

Next.js是一个React的SSR框架，目前有两套路由模式，分别是：
- App Router
- Page Router


#  Page Router
Page Router是Next.js 13.3版本之后引入的，它是基于React的，并且支持SSR、SSG、ISR和CSR。Page Router的目录结构如下：
```bash
pages/
  index.tsx
  about.tsx
  user/[id].tsx
```

对应路由情况：
```bash
/        -> index.tsx
/about   -> about.tsx
/user/1  -> user/[id].tsx
```

👍优点：
- ✅ 简单直观
- ✅ 学习成本低
- ✅ 很像传统 MVC


👎缺点：
- ❌ layout 复用麻烦
- ❌ 嵌套路由能力弱
- ❌ 服务端组件不支持


# App Router
App Router是Next.js 13.3版本之后引入的，它是基于React的，并且支持SSR、SSG、ISR和CSR。App Router的目录结构如下：
```bash
app/
  layout.tsx
  dashboard/
    layout.tsx
    page.tsx
  (app)/
    layout.tsx
    page.tsx
```

✅支持：
- ✅ layout 复用
- ✅ 局部布局
- ✅ 自动嵌套

🌟🌟🌟🌟🌟最大价值：**Server Component默认开启**

在这里，我们要区分几个概念：
- **SSR**
首次渲染时，服务端会进行数据的获取，然后插入组件中，最终以HTML字符串形式返回给浏览器；
浏览器得到之后仍然会下载JS脚本，并执行，进行 `hydration水合` 。

- **Server Component**
同样的，Server Component也会在服务器就执行，进行数据获取，插入组件中；
❗️但是，浏览器是不会拿到组件的，也就是不会执行JS，不会进行水合。

- **Client Component**
Client Component与Server Component大致一致，不同的是，若其中有交互的地方，则需要客户端执行，进行 `hydration水合` ，若没有交互的地方，则不需要客户端执行，直接返回HTML字符串给浏览器。

- **CSR**
CSR是纯React，浏览器会下载JS脚本，并执行，进行 `hydration水合` ;
服务器不做任何处理，只返回组件和脚本。

比较：

| 类型                           | 服务器生成HTML | 浏览器拿组件JS | hydration                  |
| ---------------------------- | --------- | -------- | -------------------------- |
| SSR（Page Router）             | ✅         | ✅ 全部     | ✅ 全量                       |
| Server Component             | ✅         | ❌        | ❌                          |
| Client Component（App Router） | ✅（首次）     | ✅ 局部     | ✅ 局部                       |
| CSR（纯 React）                 | ❌         | ✅ 全部     | 不叫 hydration（直接客户端 render） |
