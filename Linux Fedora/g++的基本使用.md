## g++的基本使用
gcc and g++分别是GNU的c & c++编译器。gcc/g++在执行编译的时候一般有下面4步：   
1 预处理，生成.i的文件[预处理器cpp]。   
2 将预处理后的文件转换成汇编语言，生成文件.s[编译器egcs]。   
3 由汇编变为目标代码（机器代码）生成.o的文件[汇编器as]。    
4 连接目标代码，生成可执行程序[链接器ld]。   
### 各参数说明   
#### 无参数
无参数下，会默认编译（完全步骤）需要编译的文件，并最终生成一个默认为a.out的可执行文件。   
```
g++ main.c
```  
如果有多个文件可以以空格间隔，往后后继续写。   
#### -c  
只激活预处理，编译，和汇编（不进行最后的连接目标代码及生成可执行文件），到汇编这步将会生成obj文件。   
```
g++ -c hello.c   
```   
将生成.o的obj文件。   
#### -S
只激活预处理和编译（并不执行汇编，和最后的连接生成目标文件），就是指把文件编译成为汇编代码。    
```
g++ -S hello.c   
```   
将会生成.s的汇编代码，可以用文本编辑器查看。   
#### -E   
只激活预处理步骤，后面的三步都步发生。   
这个操作并不生成新文件，但是可以重定向到一个输出文件中。   
```
g++ -E hello.c > abc.txt   
g++ -E hello.c | more   
```   
#### -o   
制定目标名称，缺省的情况下，g++编译出来的文件是a.out。   
通过-o可以自定义输出的可执行文件。   
```
g++ hello.c -o hello.exe   
```   
这样同目录下出现的就不是a.out而是hello.exe了   
#### -Idir   
-I后面的dir主要是目录位置。  
这个命令用于手动指定寻找代码中include文件的位置，当没有找到再去系统默认的include路径进行寻找。   
```
g++ -I../include hello.c   
```   
以上命令的意思就是，去上一层目录中的include文件夹寻找hello.c中include的头文件。   
#### -llibrary   
制定编译的时候使用（连接）的库
这个参数分为两部分，第一部分是前面的-l（小写L），第二部分是紧贴着的库文明名。   
库文件名是libxxx.so文件。   
比如一个库文件名为libproto.so。   
那么可以如下连接。   
```
g++ -lproto hello.c   
g++ -llibproto.so hello.c   
```  
以上两种都可以，第一种方法可以省略前面的lib和后面的.so。   
#### -Ldir   
编译中“连接”步骤的时候，搜索库的路径。比如你自己的库，可以用它制定目录，不然编译器将只在标准库的目录找。这个dir就是目录的名称。   
```
g++ -L/usr/lib -lproto hello.c   
```  
以上命令是在/usr/lib目录下寻找proto库（libproto.so库）。   
#### -g
在编译的时候，产生调试信息。即生成的可执行程序可以被gdb所调试。   
```
g++ -g main.c  
```  
生成的a.out可以被gdb调试。   
关于gdb调试内容参考同目录下《gdb调试》。   