---
ID: 67
post_title: 如何撰写文档
post_name: how-to-write-doc
author: 冰寒
post_date: 2018-06-12 17:11:47
layout: post
link: >
  https://devops.vpclub.cn/how-to-write-doc/
published: true
tags: [ ]
categories:
  - 文档
---
# 如何撰写文档

## Fork Git项目

Fork Git项目并克隆到拿到电脑

```bash

# Fork

https://github.com/john-deng/cloud-doc.git

# to

https://github.com/your-name/cloud-doc.git

```

## 创建文件

在_posts目录下面创建文档 yyyy-mm-dd-doc-name.md

## 编写YAML格式文件前置属性

```yaml
---
post_title: 你好，世界
post_name: 'hello-world'
author: 张三
layout: post
published: true
tags: [ 'hello', 'world' ]
categories:
  - 文档
---
```

### 前置属性语法说明

| 属性       | 描述     | 示例       |
|------------|----------|------------|
| post_title | 文档标题 | 你好，世界 |
| post_name | 文档名称，以符号-连接单词| hello-world |
| author | 作者名字 | 张三 |
| layout | 指定特定的模板文件类型，有 post 和 page 两种 | post |
| published | 是否发布本文档？ 有 true 和 false | true |
| tags | 你希望在文档上面打那些标签, 以数组的形式表示 | [ 'hello', 'world' ] |
| categories | 文档分类，你希望你的文档发布在那个分类下面 | 文档 |

## 撰写文档正文

当你写完以上前置属性后，接下来就可以撰写文档正文了，我们采用Markdown语法来撰写文档。

Markdown是一种轻量级的[标记语言]，它的优点很多，目前也被越来越多的写作爱好者，撰稿者广泛使用。看到这里请不要被[标记]、[语言]所迷惑，Markdown的语法十分简单。常用的标记符号也不超过十个，这种相对于更为复杂的HTML标记语言来说，Markdown可谓是十分轻量的，学习成本也不需要太多，且一旦熟悉这种语法规则，会有一劳永逸的效果。

## Mardown 常用语法说明

### 标题

标题是每篇文章都需要也是最常用的格式，在Markdown中，如果一段文字被定义为标题，只须在这段文字前加#号即可

注：# 后面必须保持一个空格

* **代码**

```markdown
# 一级标题
## 二级标题
### 三级标题
#### 四级标题
##### 五级标题
###### 六级标题
```

* **效果**

![header](/images/md-tutorial/md-header.png)

以此类推，总共六级标题，必须在#号后加一个空格，这是最标准的Markdown语法。

### 分级标题

注：= - 最少可以只写一个，兼容性一般

* **代码**

```markdown
一级标题
======================
二级标题
---------------------
```

* **效果**

![header](/images/md-tutorial/md-header-2.png)

### 段落

段落的前后要有空行，所谓的空行是指没有文字内容。若想在段内强制换行的方式是使用两个或以上空格加上回车（引用中换行省略回车）。

### 列表

有序列表与无序列表，在Markdown下，列表的显示只需要在文字前加上-、*或*即可变为无序列表，有序列表则直接在文字前加1. 2. 3.符号和文字之前加上一个字符的空格。

#### 有序列表

* **代码**

```markdown
1. 第一点
2. 第二点
3. 第三点
```

* **效果**

1. 第一点
2. 第二点
3. 第三点

#### 无序列表

* **代码**

```markdown
* 这是无序列表项目
* 这是无序列表项目
* 这是无序列表项目
```

* **效果**

* 这是无序列表项目
* 这是无序列表项目
* 这是无序列表项目

无序列表项目开头使用符号*（或者 - + ）

* **代码**

```markdown
* 一级列表项目1
  * 二级列表项目1
  * 二级列表项目2
  * 二级列表项目3
    * 三级列表项目1
    * 三级列表项目2
    * 三级列表项目3
* 一级列表项目2
```

* **效果**

* 一级列表项目1
  * 二级列表项目1
  * 二级列表项目2
  * 二级列表项目3
    * 三级列表项目1
    * 三级列表项目2
    * 三级列表项目3
* 一级列表项目2

### 引用

#### 单行引用

* **代码**

```markdown
>单行引用
```

* **效果**

>单行引用

#### 多行引用

* **代码**

```markdown
>多行引用
多行引用
多行引用
```

* **效果**

>多行引用
多行引用
多行引用

#### 多层嵌套引用

* **代码**

```markdown
>多层嵌套引用
>>多层嵌套引用
>>>多层嵌套引用
```

* **效果**

>多层嵌套引用
>>多层嵌套引用
>>>多层嵌套引用

### 行内标记

* **代码**

```markdown
行内标记之外`行内标记`行内标记之外
```

* **效果**

行内标记之外`行内标记`行内标记之外

### 代码块

注：与上行距离一空行

注：用```bash生成块shell脚本

* **Bash 代码块**

![codeblock](/images/md-tutorial/md-codeblock-bash.png)

* **Bash 代码效果**

```bash
find . -name "*.java" | grep hello-world
```

同理，注：用```java生成块java脚本

* **Java 代码块**

![codeblock](/images/md-tutorial/md-codeblock-java.png)

* **Java 代码效果**

```java
package com.example.helloworld;

import org.springframework.boot.SpringApplication;

@SpringBootApplication
public class HelloWorldApplication {

  public static void main(String[] args) {
    SpringApplication.run(HelloWorldApplication.class, args);
  }
}
```

### 插入链接

* **代码**

```markdown
点击[微品文档中心](https://devops.vpclub.cn)跳转
```

* **效果**

点击[微品文档中心](https://devops.vpclub.cn)跳转

### 插入图片

* **代码**

```markdown
![devops](/images/md-tutorial/devops-digram-1.png)
```

* **效果**

![devops](/images/md-tutorial/devops-digram-1.png)

### 选项列表

* **代码**

```markdown
[x] 任务列表项目1
[ ] 任务列表项目2
[ ] 任务列表项目3
```

* **效果**

[x] 任务列表项目1
[ ] 任务列表项目2
[ ] 任务列表项目3

### 表格

注： : 代表对齐方式 ,** : 与 | 之间不要有空格**，否则对齐会有些不兼容

* **代码1**

```markdown
|    a    |       b       |      c     |
|:-------:|:------------- | ----------:|
|   居中  |     左对齐    |   右对齐   |
|=========|===============|============|

```

* **效果**

|    a    |       b       |      c     |
|:-------:|:------------- | ----------:|
|   居中  |     左对齐    |   右对齐   |
|=========|===============|============|

* **代码2**

```markdown
a  | b | c  
:-:|:- |-:
居中    |     左对齐      |   右对齐
============|=================|=============
```

* **效果**

a  | b | c  
:-:|:- |-:
居中    |     左对齐      |   右对齐
============|=================|=============

### 语义标记

描述 |	效果 |	代码
:-|:-|:-
斜体 |	斜体 |	``` *斜体* ```
斜体 |	斜体 |	``` _斜体_ ```
加粗 |	加粗 |	``` **加粗** ```
加粗+斜体 | 加粗+斜体	| ```  ***加粗+斜体*** ``` 
加粗+斜体	| 加粗+斜体	| ``` **_加粗+斜体_** ``` 
删除线	| 删除线	| ``` ~~删除线~~ ``` 

### 分隔符

注：最少三个 --- 或 ***

* **代码**

```markdown
***
---
```

* **效果**

***
---

### 脚注

* **代码**

```markdown
Markdown[^1]
[^1]: Markdown是一种纯文本标记语言
```

* **效果**

Markdown[^1]
[^1]: Markdown是一种纯文本标记语言

### 定义型列表

注：解释型定义

* **代码**

```markdown
Markdown 
:   轻量级文本标记语言，可以转换成html，pdf等格式  //  开头一个`:` + `Tab` 或 四个空格

代码块定义
:   代码块定义……

        var a = 10;         // 保持空一行与 递进缩进
```

Markdown 
:   轻量级文本标记语言，可以转换成html，pdf等格式  //  开头一个`:` + `Tab` 或 四个空格

代码块定义
:   代码块定义……

        var a = 10;         // 保持空一行与 递进缩进

### 自动邮箱链接

```markdown
<zhang.san@outlook.com>
```

<zhang.san@outlook.com>
