本仓库共包含三个工具：FullwidthConverter(全角片假名转换器)、SubCleaner(字幕清理器)、Caption2Txt，三者为包含关系。

SubCleaner中包含了FullwidthConverter的功能，Caption2Txt中包含了SubCleaner的功能。

# 程序下载

在[Releases](https://github.com/zhimengsub/SubtitleCleaner/releases)页面选择最新版本的程序下载。

# FullwidthConverter / 全角片假名转换器

可以将半角：片假名、空格以及`｡` `｢` `｣` `､` `･`等符号转换为全角；全角数字转换为半角

## 使用方法

1. 将待转换文件直接拖放到本程序上即可。

2. 也可以使用命令行进行更多配置：
   - `InputFile`：设置待处理文件名
   - `-o FILE, --output FILE`：设置输出文件名，默认为`<输入文件名>_out.txt`。
   - `-q, --quit`：结束后不暂停程序直接退出，方便命令行调用。不加该参数程序结束时会暂停。
   - `--log`：记录日志，把执行结果输出到`<输入文件名>_log.txt`

## TODO List

- [ ] 制作GUI界面

---
# SubCleaner / 字幕清理器

输入`.ass`字幕文件，提取对话文本（跳过样式为Rubi的注音台词），进行处理后输出为文本文件，具体处理内容如下：

1. 台词合并（为避免合并后过长，每两行合并一次）：
   - 对连续多行对白**包含以下配对符号**的进行合并，用全角空格隔开(并删除该符号)：
     `《》` `<>` `＜＞` `〈〉` `「」`(不删除) `｢｣`(不删除)
   - 以`→`结尾的对白，和下一行合并，并用全角空格隔开(并删除该符号)。
   - ~~对连续多行对白的**开始和结束时间相同**的进行合并，并(只有有效对白才)用半角空格隔开~~（多数情况下为不同说话人，故去掉）
2. 台词清理：
   - 直接删除：`…` `｡`(半角) `。`(全角) `！` `!` `？` `?` `~` `～` `∼` `・` `(...)` `[...]` `{...}` `\N` `空行`
   - `、` `､`替换为全角空格
   - `『』`替换为`「」`
   - 删除拟声词（具体见[拟声词](#拟声词)）
   - 每一行开头添加`\N`
3. 假名转换
   - 每一行使用前述的片假名转换器`FullWidthConverter`处理半角片假名、半角符号、全角数字

## 使用方法

1. 将`.ass`文件直接拖放到本程序上即可。

2. 也可以使用命令行进行更多配置：
   - `InputFile`：设置待处理文件名
   - `-o FILE, --output FILE`：设置输出文件名，默认为`<输入文件名>.txt`
   - `-q, --quit`：结束后不暂停程序直接退出，方便命令行调用。不加该参数程序结束时会暂停
   - `--log`：记录日志，把执行结果输出到`<输入文件名>_log.txt`

## 拟声词

### V0.2

以下只有单独出现时才删除

```text
ん,
うむ, ええ, わあ, うわ,
あぁ, はぁ, うわぁ,
んっ, うっ, よっ, はっ, ひっ, ほっ, あっ, えっ, なっ, わっ,
えへへへ,
あ（>=1个）,
う（>=1个）,
は（>=2个）,
うん（>=1个）
```

## TODO List

- [ ] 制作GUI界面

---

# Caption2Txt.bat

说明：调用`Caption2Ass`(请自行搜素下载)、`SubCleaner`，提取ts中的ass，然后生成清理后的台词文本文件。

使用方法：拖入ts文件，会先生成ass，然后生成处理后的txt文件。

注意：必须把本脚本与Caption2Ass_PCR.exe、SubCleaner.exe放在同一目录下才能正常工作！

# 提出修改建议 / 运行时的错误和BUG

请给我提出[Issue](https://github.com/zhimengsub/SubtitleCleaner/issues)，看到后我会及时处理。

# Changelog

## FullwidthConverter

- v1.0.4
  - 空格全部变为全角

- v1.0.3
  - 修改全角数字为半角数字

- v1.0.2
  - 添加`-q, --quit` `--log`参数，方便命令行调用

- v1.0.1
  - 优化直接运行程序时的提示

- v1.0
  - 实现基本功能
  - 支持UTF-8 (with BOM)、GBK编码格式文件
  - 一次只支持单个文件

## SubCleaner

- v2.4.4.002:
  - 使用假名转换器v1.0.4
  - 不含拟声词删除功能

- v2.4.3
  - 顿号替换为全角空格
  - v2.4.3.002: 拟声词误删较多，暂时关闭该功能

- v2.4.2
  - 使用假名转换器v1.0.3

- v2.4.1
  - 完善台词清理符号

- v2.4.0
  - 完善台词清理符号
  - 台词清理新增删除拟声词v0.2
  - 完善台词合并行为（每两行合并一次）

- v2.3.1
  - 转移仓库至zhimengsub

- v2.3.0
  - 改进台词合并的规则
  - 跳过Rubi(注音)的台词

- v2.2.0
  - 优化输出内容（存在bug：时间相同的台词合并时也会以全角空格隔开）

- v2.1.0
  - 新增几对合并标志符号，优化输出内容

- v2.0.0
  - 修改台词合并逻辑
  - 修复合并及清理时的一些bug
  - 修改默认输出文件名为`<输入文件名>.txt`
  - 添加`-q, --quit` `--log`参数，方便命令行调用

- v1.0.1
  - 优化直接运行程序时的提示

- v1.0
  - 实现基本功能 
  - 一次只支持单个文件
