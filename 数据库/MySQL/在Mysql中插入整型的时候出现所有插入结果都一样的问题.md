# 在Mysql中插入整型的时候出现所有插入结果都一样的问题

问题描述：我在一张叫作users的数据表里有一个user_card字段，它的字段类型如下：

user_card INT(10) UNSIGNED NOT NULL

我的目标是在为这个字段添加一条信息，也就是学号，因为我们学校的学号是10位数的，例如6103115021，所以我就把它的类型定义为了INT(10)

但是，当我插入多条学号的时候，例如：6103115021、6103115017、6103115018等等，我发现插入的所有的学号信息都变成了**4294967295**

想了很久都没有想出原因

然后我错误地插入了一条学号信息是9位的，610311503

发现结果竟然是正确的，想了想应该是范围的问题

所以就查看了Mysql的书籍，发现INT(10)和我原来所理解的INT(10)不同

我原来理解的INT(10)  UNSIGNED 的最大值是**9999999999**

但是实际上不是这个意思

正确的意思是显示宽度为10，一般都会配合zerofill使用

举个例子，当我插入一个整型数据111，它是不够10位宽度的（它只有3位），所以，如果配合zerofill的话，就会显示**0000000111**，现在就有10位的宽度

既然解决了我错误理解INT(10)的问题了，那么，INT(10) UNSIGNED的最大范围到底是多少呢？

答案是：2^32  -  1

因为，无论你是INT(5)、INT(8)、INT(7)等等，只要它的类型是INT的，INT  UNSIGNED的最大范围就是4个字节，也就是2^32  -  1即**4294967295**

如果，想要插入学号为6103115021的信息，可以定义user_card的类型为BIGINT，它的字节为8