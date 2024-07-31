## github混合网站的底层代码指南 v1.2

#### 关于这个代码：

这些代码是基于mkdocs与github静态网页功能（github pages），采用Action工作流的方式实现网站部署。以下是其内容与代码的介绍：

#### 网站框架结构

网站的主要框架为：

```markdown
main
├─ README.md  #主页页面
├─ LICENCE  #许可证
├─ doc/     #存放的各个页面及其文件
│	├─index.md
│	├─courses/
│		├─ ···
│		····
│
├─ .github/
│	└─ workflows
│		└─ static.yml  #工作流，配套github Action使用
│
├─ .gitattributes
│
├─ css #css配置文件，暂未配置
│
├─ js  #JavaScript配置文件，暂未配置
│
└─ mkdocs.yml #mkdocs配置文件
```

#### docs部分

docs部分存放了网页所需的所有markdown文件及其相关配套的资源，并通过markdown连接的代码语句实现资源的查看与下载。

- 新生指南：用于存放新生入学、报到等前期工作的指导信息
- 混合课程：用于混合班特有课程的学习资料与课程介绍等，以及对于物理课在图灵的基础上进一步补充
- 学院与专业：介绍各个学院并提供一定的专业课程资源，以免出现院外祖传代码而院内自己手搓的情况
- 导师制和教授学术小组：介绍相关政策及系统的操作等
- 科研训练：介绍SRTP项目等本科生科研训练项目、介绍部分竞赛
- 乐活混合：关于在校园生活的进一步信息
- 师长分享：（待定）找愿意分享的人说两句
- 深造资讯：保研推免以及考研的经验分享

其中，学院与专业模块按照：学院-专业-课程类别（若是选修课，按照课程归属模块拆分）与课程资源（选修、必修分开）的结构。对于课程资源较大的文件，使用github release上传并附带下载链接。

例如：
```markdown
信电学院
├─ 微电子
├─ 电科     
│   ├─ 专业必修课
│   ├─ 专业选修课-公共基础类
│   ├─ 专业选修课-XXX类
│   ├─ 必修课程资源
│   │   ├─ 资源1
│   │   ├─ 资源2
│   │   └─ ···
│   └─ 选修课程资源
```


#### mkdocs部分

网站采用了较为方便简洁的mkdocs进行基本的配置。相关资料可参考：

- [MkDocs](https://www.mkdocs.org/)
- [Material for MkDocs (squidfunk.github.io)](https://squidfunk.github.io/mkdocs-material/)

mkdocs的主要网页设置是mkdocs.yml文件。文件的所有解释均已添加于原文件代码中。



#### .github/workflows部分

workflows是github生成网页所需的设置。在github pages功能中，选择了构建与部署的源来自Action。

![github pages设置](figure/屏幕截图%202024-07-12%20125510.png)

本项目中的workflows来自github提供的通用模版，并在其中加入本项目使用mkdocs所需的部分代码。

workflows中的action文件的执行情况是：

- 在main中进行修改并提交
- Action自动执行workflows文件夹中的文件
- static.yml文件中的各项语句以虚拟机的形式进行执行并生成网页

在每次提交时、修改workflows文件夹中的工作流文件时，Action均会自动执行，若执行中断会显示报错结果并以邮件形式同步提醒



#### .gitattributes部分

该部分主要作用有两个：

- 设置代码使得github能够识别markdown语言（对应文件的代码第一行），并在仓库的右下角language部分显示整个项目的语言情况。若无此代码，markdown文件将不会被认为是代码文件

![markdown语言识别效果](figure/屏幕截图%202024-07-12%20130538.png)

- （已撤销）设置较大文件的LFS代理，以期望能够在仓库上传大文件限制的情况下给予大文件下载功能



#### css部分（待完善）

css部分用于调整mkdocs生成的网页中的字体、颜色等信息，实现个性化定制。所有配置css的文件放于统一的文件夹并在mkdocs.yml文件中加入配置语句



#### js部分（待完善）

js部分用于存放JavaScript文件，这些文件用于给markdown提供特殊的逻辑指示，从而实现通过特定的markdown语法实现较为多样化的呈现方式。所有配置的JavaScript文件放于统一的文件夹并在mkdocs.yml文件中加入配置语句



#### markdown特殊语法

由于mkdocs的markdown拓展语法的加入，支持特殊的markdown语法。举例如下：

```markdown
!!! [字符名] {“文本”}
```

注意空格。正文部分缩进四个空格。用于生成卡片效果

```markdown
???{+} [字符名] {"文本"}
```

注意空格。正文部分缩进四个空格。用于生成answer卡片效果。若加上"+"，则会在页面加载时自动展现卡片内容，否则内容会折叠，需要点击打开。

```markdown
=== ["文本"]
```

注意空格，正文部分缩进四个空格。用于生成项目选项卡。多个选项卡内容并列会被视为一个选项卡的不同选项。