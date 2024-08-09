## 对称二叉树
    如果二叉树的左右子树的结构是对称的，即两棵子树皆为空，或者皆不空，则称该二叉树是对称的。编程判断给定的二叉树是否对称.
    
    例：如下图中的二叉树T1是对称的，T2是不对称的。
    
    
    
    二叉树用顺序结构给出，若读到#则为空，二叉树T1=ABCDE，T2=ABCD#E，如果二叉树是对称的，输出“Yes”，反之输出“No”。
    
    输入：二叉树用顺序结构给出，若读到#则为空。
    
    输出：如果二叉树是对称的，输出“Yes”，反之输出“No”
    
    【输入样例】
    
    ABCDE 
    
    【输出样例】 
    
    Yes 
    
    
    
    对称ABC##DE
    
    不对称ABCD##E
    
    不对称ABCD#E

```c++
#include<iostream>
using namespace std;
string str;
int main(){
  cin>>str;
  int flag=0;
  int len=str.size();
  str[len] = '#';
;    for(int i=1;i<len;i+=2){
      if ( (str[i] =='#' && str[i+1] =='#') || (str[i] !='#' && str[i+1] !='#') ) continue;
      //判断对称的情况，正着判断在此处更加方便
      else {
          flag=1;
          break;
      }
  }
  if(flag) cout<<"No";
  else cout<<"Yes";
  return 0;
}
```
