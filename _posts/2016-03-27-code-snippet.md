---
layout: post
title: "个人积累的一些代码块(Code Snippet)"
description: ""
category: articles
tags: [Xcode]
comments: true
---

## My Code Snippet

不得不承认我是个很懒的人，相同的代码不想敲第三遍，所以尽可能的让代码自动化。以此为前提积累了一些代码块。

不过代码块的同步是一个问题，主要有两个方式

1. 使用[ACCodeSnippetRepositoryPlugin](https://github.com/acoomans/ACCodeSnippetRepositoryPlugin)来管理Code Snippet。
2. 使用`github`进行代码的版本管理，然后在本地进行快捷方式的链接。我就是使用这种方式。


### 注意点：
- 可以使用脚本`build_snippets.sh`：

```
$ ./build_snippets.sh
done
```

- `Completion Shortcut`（自动补全快捷键）是最重要的一项，设置好关键字后就可以在编辑器中输入相应关键字后出现补全提示，否则代码片段只能拖拽到编辑器中使用。`Title`和`Summary`会在补全提示时显示。指定`Language`和`Completion Scopes`可以让代码片段只在特定区域显示补全提示，在你拖拽创建代码片段时，Xcode 会根据你的拖拽区域自动指定它们。

- 代码片段中的占位符可以按`<#占位符提示#>`格式添加。

- 自动补全快捷键可以包含小部分特殊符号，已知的有`@ $`。除`@`外，其他符号虽然也可以填入，但会从其所在位置起导致快捷键失效（比如快捷键为`$+`，输入`$`弹出提示后，再输入`+`，则提示消失）。

- 用户自定义的代码片段存放于`~/Library/Developer/Xcode/UserData/CodeSnippets`。

- 一些快捷键：  

```
//Title : Property Assign
//Completion Shortcut : @pa
//Completion Scopes : All
@property (nonatomic, assign) <#type#> <#name#>;

//@pc
@property (nonatomic, copy) <#type#> <#name#>;

//@pr
@property (nonatomic, readonly) <#type#> <#name#>;

//@prw
@property (nonatomic, readwrite) <#type#> <#name#>;

//@ps
@property (nonatomic, strong) <#type#> <#name#>;

//@pw
@property (nonatomic, weak) <#type#> <#name#>;

//@pb
@property (nonatomic, copy) void (^<#block name#>)(void);

//@pi	
@property (nonatomic, weak) IBOutlet <#type#> <#name#>;

```

- 补全完整方法：`@`作为开头。


具体可以安装后查看。

### 代码：
文章中的代码都可以从我的GitHub [`MyCodeSnippet`](https://github.com/tonyh2021/MyCodeSnippet)找到。

