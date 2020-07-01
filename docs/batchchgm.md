# 简介

`batchchgm`命令用来批量修改七牛空间中文件的MimeType。

# 格式

```
qshell batchchgm [--force] [--sucess-list <SuccessFileName>] [--failure-list <FailureFileName>] <Bucket> [-i <KeyMimeMapFile>]

```

# 帮助
qshell batchchgm -h

# 鉴权

需要在使用了`account`设置了`AccessKey`, `SecretKey`和`Name`的情况下使用。

# 参数

|参数名|描述|
|---------|-----------|
|Bucket|空间名，可以为公开空间或私有空间|

**success-list选项**
该选项指定一个文件，qshell会把操作成功的文件行导入到该文件

**failure-list选项**
该选项指定一个文件， qshell会把操作失败的文件行加上错误状态码，错误的原因导入该文件

**force选项**

该选项控制工具的默认行为。默认情况下，对于批量操作，工具会要求使用者输入一个验证码，确认下要进行批量文件操作了，避免操作失误的发生。如果不需要这个验证码的提示过程，可以使用`--force`选项。

**i选项**
该选项指定输入文件, 文件内容是文件名称和新的MimeType对的列表，每一行是`Key\tNewMimeType`格式，注意格式中间的Tab, 如果没有指定该参数，从标准输入读取内容

# 示例

比如我们要将空间`if-pbl`中的一些文件的MimeType修改为新的值。
那么提供的`KeyMimeMapFile`的内容有如下格式：

```
data/2015/02/01/bg.png	image/png
data/2015/02/01/pig.jpg	image/jpeg
```

注意：上面文件名和MimeType中间的书写方式不是空格，而是制表符“tab”键，否则执行的时候不会报错，但也不会把MimeType(文件类型)批量修改成功。在上面的列表中，`data/2015/02/01/bg.png`的新MimeType就是`image/png`，诸如此类。

把上面的内容保存在文件`tochange.txt`中，然后使用如下的命令：

```
$ qshell batchchgm if-pbl -i tochange.txt
```

如果执行过程中遇到任何错误，会输出到终端，如果没有的话，则没有任何输出。

# 注意

如果没有指定输入文件的话, 默认会从标准输入读取同样格式的内容
