PCRE
=========

### 介绍

PCRE是一套实现正则表达式匹配的函数库，使用与Perl基本相同的语法和语意。一些最先出现在Python和PCRE中而不是Perl中的特性使用Python的语法，也有一些支持.NET和Oniguruma语法，另外有一个选项可以更好的兼容JavaScript。

从Release 8.30版本开始，允许编译两个独立的PCRE库：第一个支持8位长字符（包括UTF-8），第二个支持16位长字符（包括UTF-16）。生成过程中允许一个或两个库同时生成。实现这部分的主要工作由Zoltan Herczeg完成。

从Release 8.32版本开始，允许编辑第三个独立的PCRE库，即支持了32位长字符（包括UTF-32）。生成过程允许三个库任意的组合。实现这部分的主要工作有Christian Persch完成。

三个库包含一套一致的函数名，其中16位库使用pcre16\_代替pcre\_,32位库使用pcre32\_代替pcre\_。为了避免过于复杂，减少维护的成本，许多文档将8位的库与16位和32位的库分开来。一种参考的规则就是，对于函数或者结构体，使用pcre\_xxx意味着是8位库，使用pcre16\_xxx意味着16位库，使用pcre32\_xxx意味着32位库。

当前PCRE的实现与Perl5.12基本相同，支持UTF-8/16/32编码和Unicode。虽然UTF-8/16/32和Unicode已被支持，但是这不是默认的。Unicode字符集对应着Unicode-release 6.3.0版本。

除了Perl-compatible matching函数，PCRE包含一个替代的函数，能以另一种方式匹配相同的正则。在特定的情况下，替代函数有些优势。关于这两种匹配算法的讨论，请看pcrematching文档。

PCRE用C编写，并发布了C语言的库。一些人写了各种各样的封装和接口。尤其是Google，为8位库提供了一个完整的C++的封装。并已经成为PCRE发行包的一部分。pcrecpp文档有这些接口的详情。其他人的贡献可以在在后面的FTP站点找到，位于Contrib文件夹：

ftp://ftp.csx.cam.ac.uk/pub/software/programming/pcre

正则表达式相关的以及未被PCRE支持的特性在另外的文档中，详见pcrepattern和pcrecompat文档。语法描述可见pcresyntax文档。

一些特性在PCRE库生成的时候可以被包含、被删除、被修改。pcre_config()函数用来确认哪些特性可用。pcrebuild页面描述了这些特性。在源码包中，README和NON-AUTOTOOLS\_BUILD文件描述了如何在不同的操作系统中生成PCRE库。

库中还包含了一些没有文档的内部函数和数据表，会被一些外部函数使用，但是不会被外部调用。他们的名字以以‘\_pcre\_’或‘\_re16‘或’\_pcre\_‘开头，避免引起命。在一些环境下，在生成库的时候可以控制对外符号被导出（使用export关键字），但是未编写文档的符号将不被导出。

### 安全事项

如果你在一个不支持UTF编码的程序中使用PCRE，但允许用户提供任意的模式进行编译，你该知道这个特性，它允许在模式中打开UTF编码支持，能生成一个支持UTF编码的PCRE。例如，在一个8位的模式中，以\*UTF8或者\*UTF开头的就是打开了UTF-8模式，并将对应的模式和字符串以UTF-8编码的字符解析，而不是独立的8位字符。这将导致表达式和字符串都需要经过UTF-8合格性的检查。如果字符串非常长，可能会占用过多资源而影响程序的性能。

一种防止这个出现的方法是，使用pcre\_fullinfo()函数来检查编译好的模式的与UTF相关的选项。另外，从release 8.33开始，你可以在编译阶段设置PCRE\_NEVER\_UTF选项。如果一个表达式中包含UTF编码的片段，将引起编译时错误。

如果你的程序支持UTF编码，请明白合法性检查也将占用时间。如果同样的字符串被匹配了很多次，你可以使用PCRE\_NO\_UTF[8|16|32]_CHECK选项来避免第二次以及子串的匹配，这样能减少冗余的检查。

另一种大大削减性能的情况是，在一个永远不会被匹配上的字符串上执行搜索树很庞大的表达式。比如无限制的重复，嵌套的表达式。PCRE提供了一些保护措施：请查阅PCRE\_EXTRA\_MATCH\_LIMIT特性，位于pcreapi文档。

### 用户文档

PCRE的用户文档包括了一些其他的章节。在man文档格式中，每个章节都是一个独立的man文档。在HTML文档格式中，每个章节都是一个页面，从首页链接出来。在纯文本格式中，为了方便搜索，所有的章节是连续的，除了pcredemo。章节安排如下：

    pcre 本文档
	pcre-config PCRE安装时配置信息
	pcre16 16位库详情
	pcre32 32位库详情
	pcreapi PCRE原生C语言接口详情
	pcrebuild 生成PCRE
	pcrecallout 标注特性详情
	pcrecompat 讨论Perl兼容性
	pcrecpp 8位库的C++封装库的详情
	pcredemo C程序实例
	pcregrep pcregrep（仅8位库）命令描述
	pcrejit just-in-time优化支持的讨论
	pcrelimits 大小和其他限制的详情
	pcrematching 两种匹配算法的讨论
	pcrepartial 局部匹配技巧的详情
	pcrepattern 正则表达式的语法和语义
	pcreperform 性能问题的讨论
	pcreposix 兼容POSIX风格的C接口（8位库）
	pcreprecompile 保存和重用预编译模式的详情
	pcresample 示例程序的讨论
	pcrestack 堆使用的讨论
	pcresyntax 快速语法参考
	pcretest 测试命令的描述
	pcreunicode Unicode和UTF-8/16/32支持的讨论

另外，在man文档和HTML文档中，有一个C库函数的文档，罗列的参数和返回结果。
