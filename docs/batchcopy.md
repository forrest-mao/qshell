# 简介

`batchcopy`命令用来将一个空间中的文件批量复制到另一个空间，另外你可以在复制的过程中，给文件进行重命名。

当然，如果所指定的源空间和目标空间相同的话，如果这个时候源文件和目标文件名相同，那么复制会失败（这个操作其实没有意义）。
如果复制的目标空间中存在同名的文件，那么默认情况下针对该文件的复制操作也会失败，如果希望强制覆盖，可以指定`--overwrite`选项。

# 格式

```
qshell batchcopy [-F <Delimiter>] [--force] [--overwrite] [--success-list <SuccessFileName>] [--failure-list <failureFileName>] <SrcBucket> <DestBucket> [-i <SrcDestKeyMapFile>]
```

# 帮助
```
qshell batchcopy -h
```

# 鉴权

需要在使用了`account`设置了`AccessKey`, `SecretKey`和`Name`的情况下使用。

# 参数

|参数名|描述|
|---------|-----------|
|SrcBucket|原空间名，可以为公开空间或私有空间|
|DestBucket|目标空间名，可以为公开空间或私有空间|

**i短选项**
该选项接受一个文件参数， 内容是原文件名和目标文件名对的列表，如果你希望目标文件名和原文件名相同的话，也可以不指定目标文件名，那么这一行就是只有原文件名即可。每行的原文件名和目标文件名之间用空白字符分隔（空格，\t, \n), 如果文件含有空格，可以使用-F选项指定自定义的分隔符。如果没有指定该参数， 从标准输入读取内容。

**success-list选项**
该选项指定一个文件，qshell会把操作成功的文件行导入到该文件

**failure-list选项**
该选项指定一个文件， qshell会把操作失败的文件行加上错误状态码，错误的原因导入该文件

**force选项**

该选项控制工具的默认行为。默认情况下，对于批量操作，工具会要求使用者输入一个验证码，确认下要进行批量文件操作了，避免操作失误的发生。如果不需要这个验证码的提示过程，可以使用`-force`选项。

**overwrite选项**

默认情况下，如果批量复制的文件列表中存在目标空间已有同名文件的情况，针对该文件的复制会失败，如果希望能够强制覆盖目标文件，那么可以使用`-overwrite`选项。

# 示例

1.我们将空间`if-pbl`中的一些文件复制到`if-pri`空间中去。如果是希望原文件名和目标文件名相同的话，可以这样指定`SrcDestKeyMapFile`的内容：

```
data/2015/02/01/bg.png
data/2015/02/01/pig.jpg
```

然后使用如下命令就可以把上面的文件就以和原来文件相同的文件名从`if-pbl`复制到`if-pri`了。

```
$ qshell batchcopy if-pbl if-pri -i tocopy.txt
```

2.如果上面希望在复制的时候，对一些文件进行重命名，那么`SrcDestKeyMapFile`可以是这样：

```
data/2015/02/01/bg.png	background.png
data/2015/02/01/pig.jpg

```
从上面我们可以看到，你可以为你希望重命名的文件设置一个新的名字，不希望改变的就不用指定。然后使用命令就可以将文件复制过去了。

```
$ qshell batchcopy if-pbl if-pri -i tocopy.txt
```

3.如果不希望上面的复制过程出现验证码提示，可以使用 `--force` 选项：

```
$ qshell batchcopy --force if-pbl if-pri -i tocopy.txt
```

4.如果目标空间存在同名的文件，可以使用`--overwrite`选项来强制覆盖：

```
$ qshell batchcopy --force --overwrite if-pbl if-pri -i tocopy.txt
```

5. 如果希望导出复制成功和失败的列表， 可以使用--success-list和--failure-list选项

```
$ qshell batchcopy --success-list success.txt --failure-list failure.txt -i tocopy.txt if-pbl if-pri
```

6.如果源文件或者目的文件中包含了空格，那么文件需要使用其他的分隔符， 需要手动使用-F选项指定分隔符, 假如使用\t

```
data/2015/02/01/bg.png\tbackgrou nd.png
data/2015/02/01/p\tig.jpg

```
可以使用如下命令处理:
```
$qshell batchcopy -i tocopy.txt  -F'\t' if-pbl if-pri
```

# 注意

如果没有指定输入文件的话， 会从标准输入读取同样内容格式

