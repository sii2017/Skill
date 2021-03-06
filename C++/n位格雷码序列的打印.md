# n位格雷码序列的打印   
关于格雷码的信息，参考上级目录《数学》下的《格雷码》。   
## 题目概述
来自leetcode89，格雷编码。   
> 给定一个代表编码总位数的非负整数n，打印其格雷编码序列。格雷编码序列必须以0开头。    
> 输入: 2  
> 输出: [0,1,3,2]  
> 解释:  
> 00 - 0  
> 01 - 1   
> 11 - 3  
> 10 - 2   

> 对于给定的 n，其格雷编码序列并不唯一。  
> 例如，[0,2,3,1] 也是一个有效的格雷编码序列。   

> 00 - 0  
> 10 - 2   
> 11 - 3   
> 01 - 1  
  
即通过输入的n来输出**总位数为n**的格雷编码。    
## 思路
先贴一段4位格雷码的码图。  
0000    
0001    
0011    
0010     
0110    
0111    
0101    
0100    
1100    
1101    
1111    
1110    
1010    
1011    
1001    
1000    
我们可以清晰的看到最后一位，先空1个0，然后两个1，两个0；倒数第二位先空2个0，然后四个1，四个0；倒数第三位空4个0，然后8个1和0这样……   
因此我们可以在外层遍历整个数字，从0到2的n次方-1，内部再从最后一位开始往前分别对每一位进行处理。   
总的效率位n*pow(2,n)。   
**这个思路是错误的。**   
真是太遗憾了，我花一个下午想出来的思路是错误的。  
## 解题代码   
```c
class Solution {  
public:  
    vector<int> grayCode(int n)   
    {  
        vector<int> res;  
        int size = pow(2,n);   
        for(int i=0;i<size;i++)  
        {   
            int graycode = i ^ (i>>1);  
            res.push_back(graycode);  
        }   
        return res;  
    }  
};   
```  
如代码所示，先算出整个数字的数量，然后依次遍历。依靠着格雷码特有的一种规律，即：  
```c
int graycode= i^(i>>1);   
```   
既可以直接依次获取格雷码。   
## 总结
就我目前对数学的理解，还没有办法重新正推出这个公式。所以记下来这个公式，多看看吧。   
**int graycode= i^(i>>1);**   
