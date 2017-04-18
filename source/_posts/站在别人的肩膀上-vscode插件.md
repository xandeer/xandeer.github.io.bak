---
title: 站在别人的肩膀上--vscode插件
date: 2017-04-18 19:26:00
tags:
  - vscode
  - editor
categories:
---

## 缘起

用 vscode 也有一段时间了，也习惯了有什么需求直接在插件市场搜索关键字，
但是上周找一个解析 TODO 的插件，找到一个基本满足我需求的，但是它不支持
`vue`。

怎么搞呢？这时候作为程序员的优点就体现出来了，fork 它的源码，然后看看
怎么满足自己的需求。

## 准备

Fork [vscode-todo-parser](https://github.com/kantlove/vscode-todo-parser)
代码之后要准备环境，非常简单，只需要安装 `vsce` 就可以了。

```shell
npm i -g vsce
# or
yarn global add vsce
```

## 查找切入点

用 vscode 打开源码路径，查看 src 目录结构，看名称只用关注以下两个子目录即可：

```
├── const
│   ├── Icons.ts
│   ├── LanguageExtensions.ts
│   ├── Names.ts
│   ├── Numbers.ts
│   ├── RegexStrings.ts
│   ├── Unsupport.ts
│   └── all.ts
├── types
│   ├── CommandType.ts
│   ├── FileType.ts
│   ├── FileUri.ts
│   ├── LanguageType.ts
│   ├── RegexType.ts
│   ├── TodoType.ts
│   └── all.ts
```

简单浏览之后很容易就找到两个关键文件：

- `RegexStrings.ts`
- `LanguageType.ts`

## 修改源码

`LanguageType.ts` 中有以下一段代码：

```javascript
static ADA          = new LanguageType("ada", new RegexType(RG_ADA));
static C            = new LanguageType("c", new RegexType(RG_JAVA));
static COFFEESCRIPT = new LanguageType("coffeescript", new RegexType(RG_PYTHON));
```

一看这代码就明白，我需要添加一个新对象：

```javascript
static VUE = new LanguageType("vue", new RegexType(RG_VUE));
```

然后看看 `RG_*` 都是什么，就在 `RegexStrings.ts` 中看到了下面的代码：

```javascript
export const RG_JAVA = "/\\*([\\s\\S]*?)\\*/|//([^\\r\\n]+)";
export const RG_PYTHON = "#\\s*(.+)";
export const RG_ADA = "--\\s*(.+)";
```

又因为我还用 `pug`，所以加了一个正则字符串，要不然直接用 `RG_JAVA` 就可以了。

```javascript
export const RG_PUG = "//- ([^\\r\\n]+)";
export const RG_VUE = "//-? ([^\\r\\n]+)|/\\*([\\s\\S]*?)\\*/";
```

代码这就改好了，下面就要安装到自己的机器上了。

## 编译安装

准备阶段安装的 `vsce` 需要上台了，在项目根目录下执行以下命令：

```shell
vsce package
```

在当前目录下就会根据 `package.json` 文件配置生成一个 `*.vsix` 文件。
安装更简单：

![install](http://oag78t4c2.bkt.clouddn.com/xandeer/vscode-plugin.png)

之后 `Reload Window` 就可以了。

`vsce` 还有更多其它功能，比如将修改完的插件发布到插件市场等，
在需要的时候再去探索。

## 感谢

1. vscode 团队；
2. 各插件作者；
3. 更不能忘记开源。