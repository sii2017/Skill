## SQLSERVR 转换为数字类型numeric时出现算数溢出错误
在插入numeric的时候，出现算数溢出错误，原因是插入的数值太大了。   
numeric有自己的长度，比如numeric(6,4)   
这意味着这个数值**一共可以有6位，其中小数最长为4位**。   
因此可以推断出，**整数部分为2位，小数部分为4位**。   
如果超出这个范围则会插入失败。   