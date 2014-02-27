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

