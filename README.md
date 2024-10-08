# 量化Agent

------

在进行方案文档的编写过程中，发现了先前实现路径的一些bug点（现有模型很难解决）。

1. 由于现有大模型的逻辑性不高，不能让其修改核心的执行代码。只能由其进行 text to action类型的操作。

------

## 实现路径

### 核心功能

#### 传统模型

1. 构建文件 ==目录文档== 
   其中包含整个文档项目的目录结构，以及每一个策略文件的作用、适用范围/条件。
2. RAG
   - **如果是执行模式：** 
     （暂时做不出完整的形态，因为需要每种策略都有相应的执行代码，并且代码要统一封装）
     例如，输入用户开户的券商的key（身份认证的标识）作为变量，就能够直接执行
     1. 让大模型根据这个==目录文件== 联合输入进行RAG，返回执行的策略文件的路径。
     2. 根据路径选取文件执行。
   - **如果是问答模式：**
     1. 让大模型根据这个==目录文件== 联合输入进行RAG，返回执行的策略文件的路径。
     2. 把路径中的文件合并到一起，形成新的RAG文档。
     3. 进行RAG，结束后删除对应文件。

#### 深度学习模型

除了能进行上述的方式之外，在==执行模式==之中，可以在执行代码的过程中，有以下优化策略：

1. 利用大模型在新信息中进行信息抽取/分类（可以抽取到一个文件之中，把文件路径和key作为参数直接执行文件）
2. 把大模型抽取到的关键信息用作==量化深度学习模型==中的一个输入

### 增加功能

在策略改变的时候，给用户发送邮件提醒。（function_call）

## 补充

我现在搜集到了的量化策略大概有160个，但是其中没有直接可以执行的代码。

所以目前只做一下RAG还行。