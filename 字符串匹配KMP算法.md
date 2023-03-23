# 字符串匹配KMP算法

避免重复进行匹配，降低时间复杂度

例如

```c++
 A： abababaababacb
 B： ababacb
          //第6位匹配失败
 A： abababaababacb
       ababacb //j指针指向第四位进行匹配（下标为3），第6位匹配失败
 A： abababaababacb
         ababacb //j指针指向第四位进行匹配（下标为3），第4位匹配失败
 A： abababaababacb
           ababacb //j指针指向第二位进行匹配(下标为1)，第二位匹配失败
 A： abababaababacb    
            ababacb //j指针指向第一位进行匹配(下标为0)，匹配成功
     
     原理：匹配失败时，失败位前面均成功匹配，我们找到主串的后缀与比较成功模式串的前缀的最大公共缀，然后把模式串移动至其前缀与主串后缀一致，从一致部分后面一位进行比较
     主串的后缀也是公共部分模式串的后缀，故这个部分的寻找只与模式串自身相关，我们可在比较浅完成这项任务，把最大公共缀长度储存在next[]数组中。这个值就是比较失败后j指针指向字符数组中的下标。
     此过程中i指针没有回溯过程，降低了时间复杂度。
  
匹配函数代码实现
     
//匹配字符串，匹配成功返回首元位置，失败则返回0
     int kmt_string_search(char* a,char* b)
 {
     //获取b字符串的next数组
     int *next=build_next(b);
     int LengthOfB=strlen(b);
     int LenthOfA=strlen(a);
     int i=0;
     int j=0;
     while(i<LenthOfA)//i一直右移未曾回溯
     {
         if(a[i]==b[j])
         {
             i++;
             j++;
         }//匹配则指针均右移
         else if(j>0)//不匹配（且不是第一位就不匹配）
         {
             j=next[j-1]; //j指针回溯
         }
         else //第一位就不匹配，只需要i右移
         {
             i++;
         }
     }
     if(j==LenthOfB) return i-j;
     else return 0;
     
 }


int* build_next(char* b) //生成字符串的next数组，实质上还是字符串匹配问题
{                        //但是我们可以通过递推提高速度
  int c=strlen(b);
    int* next=(int*)malloc(sizeof(int)*c);
    next[0]=0;
    int prefix_len=0;
    int i=1;
    int j=1;
    while(i<c)
    {
        if(b[prefix_len]==b[i]) //i始终比prefix_len大1
        {
            prefix_len++;
            next[j]=prefix_len;
            j++;
            i++;
        }
        else
        {
            if(prefix_len==0)
            {
                next[j]=0;
                j++;
                i++;
            }
            else
                prefix_len=next[prefix_len-1];
        }
    }
    return next;
}
12212
next[0]=0;
next[1]=0;
next[2]=0;
next[3]=1;
next[4]=2;
```





