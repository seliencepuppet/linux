sed是一个流编辑器

sed默认不编辑原文件

```shell
sed 'AddressCommand' file ....
```

表示起始行到结束行比如1, 100
1) ###### $: 表示最后一行
2) ###### $-1: 表示倒数第二行

/Regexp/使用正则表达式来匹配的模式
/^root/

/pattern1/, /pattern2/ 表示第一次被pattern1匹配到的行至第一次被pattern2匹配到的行结束

4.LineNumber
特定的行

5.Startline +N
从startline开始向后隔n行

Command：
d：通常表示删除符合条件的行

```shell
sed '/oot/' /etc/fstab
sed '1d' /etc/passwd
sed '1,+2d' /etc/passwd
```

sed是一个很好的文件处理工具本身是一个管道命令主要是以行为单位进行处理,可以将数据行进行替换, 删除, 新增, 选取等特定工作下面先了解一下sed的用法
sed命令行格式为
```shell
sed [-nefri] 'command' 输入文本       
```

常用选项
-n: 使用安静(silent)模式, 在一般sed的用法中所有来自STDIN的资料一般都会被列出到萤幕上但如果加上-n参数后则只有经过sed特殊处理的那一行(或者动作)才会被列出来
-e: 直接在指令列模式上进行sed的动作编辑
-f: 直接将sed的动作写在一个档案内 -f filename 则可以执行filename内的sed动作
-r: sed的动作支援的是延伸型正规表示法的语法 (预设是基础正规表示法语法)
-i: 直接修改读取的档案内容，而不是由萤幕输出。       
	
常用命令
a: 新增 a 的后面可以接字串而这些字串会在新的一行出现(目前的下一行)～
c: 取代 c 的后面可以接字串这些字串可以取代 n1, n2 之间的行!
d: 删除因为是删除啊, 所以d后面通常不接任何玩意儿
i: 插入 i 的后面可以接字串, 而这些字串会在新的一行出现(目前的上一行)
p: 列印亦即将某个选择的资料印出, 通常 p 会与参数 sed -n 一起运作～
s: 取代可以直接进行取代的工作通常这个 s 的动作可以搭配正规表示法 例如 1,20s/old/new/g

举例: (假设我们有一文件名为ab)
```shell
sed '2,$d' ab	删除2行到最后一行 
sed '$d' ab	删除最后一行
```



	显示符合条件的行

	sed -n '1p' zhangyz             显示第一行
	sed -n '$p' zhangyz                显示最后一行
	sed -n '1,2p' zhangyz               显示一行到第二行
	sed -n '2,$p' zhangyz               显示2行到最后一行

	增加一行或多行字符串
	sed '1a hello world' zhangyz


	-n：表示只打印，只显示符合条件的行

	使用模式进行查询
	sed -n '/root/p' zhangyz            查询root所在的所有行
	sed -n '/\$/p' zhangyz              查询包括关键字$所在所有行，使用\屏蔽特殊含义


	====================================================================

	a：表示追加内容，表示在指定的行后面追加新行，内容为“string”
	i：表示在string指定的行前面添加新行，内容为string
	\n  可用于换行
	r file：将指定的文件跟内容添加到符合条件的行处        
	sed '2r /etc/issue'  /etc/fstab
	w file：将指定范围内的内容另存至指定的位置
	sed '/oot/w /tmp/oot.txt' /etc/fstab                        默认打印模式空间
	s/pattern/string/： 查找并替换            默认只替换每行中第一次被模式匹配到的串
	加修饰符
	q：全局替换
	i：忽略字符大小写

	s# # #
	s@ @ @
	\(\) \1, \2

	&：引用模式匹配

	sed 's#\(1..e\)#\1r#'  sed.txt   

	sed 's#l\(..e\)#L\1#'  sed.txt


	sed '1a drink tea' ab  #第一行后增加字符串"drink tea"

	sed '1,3a drink tea' ab #第一行到第三行后增加字符串"drink tea"

	sed '1a drink tea\nor coffee' ab   #第一行后增加多行，使用换行符\n


	代替一行或多行
	sed '1c Hello World!'    zhangyz            第一行代替为hello world！

	sed '1,2c Hello World'      zhangyz         第一行到第二行代替为hello world！


	替换某一行的某部分
	格式：sed 's/要替换的字符串/新的字符串/g'   （要替换的字符串可以用正则表达式）

	sed -n '/root/p' zhangyz  | sed 's/root/caonima/g'

	sed -n '/root/p' zhangyz  | sed 's/root/ /g'

	插入
	sed -i '$a bye' zhangyz         在文件中最后一行直接输入“bye”




	 删除匹配行
	      sed -i '/匹配字符串/d'  filename  （注：若匹配字符串是变量，则需要“”，而不是‘’。记得好像是）
	      替换匹配行中的某个字符串
	      sed -i '/匹配字符串/s/替换源字符串/替换目标字符串/g' filename


	sed -e :表示支持多个操作同时进行

	-e scirpt -e script   可以同时执行


	sed -f  /path/to/sed_script ： 表示指定的一个文件

	sed -f /path/to/somefile    file 


	-r：表示使用扩展正则表达式

	history | sed 's#^[[:space:]]*##g' | cut -d ' ' -f1


	sed '/oldboy/d' test.txt

	-a的使用：
	sed '2r  /etc/issue'   /etc/fstab

	sed 's/\//#/'  /etc/fstab

	sed 's/\/#/g'  /etc/fstab

	sed 's#^[[:space:]]*##g'   |  cut -d " " -f1


