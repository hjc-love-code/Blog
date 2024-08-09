## 对称二叉树
    如果二叉树的左右子树的结构是对称的，即两棵子树皆为空，或者皆不空，则称该二叉树是对称的。编程判断给定的二叉树是否对称.
    二叉树用顺序结构给出，若读到#则为空，二叉树T1=ABCDE，T2=ABCD#E，如果二叉树是对称的，输出“Yes”，反之输出“No”。
    二叉树T1是对称的，T2是不对称的。
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
    for(int i=1;i<len;i+=2){
        if ( (str[i] =='#' && str[i+1] =='#') || (str[i] !='#' && str[i+1] !='#') )
            continue;
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
## 树的统计
    给定一棵树，输出树的根root，孩子最多的结点max以及他的孩子。
    输入：第一行：n（结点个数≤100），m（边数≤200）。
    以下m行：每行两个结点x和y，表示y是x的孩子(x,y≤1000)。
    输出：第一行：树根：root；
    第二行：孩子最多的结点max；
    第三行：max的孩子（按编号由小到大输出）。
    
    【输入样例】
    8 7
    4 1
    4 2
    1 3
    1 5
    2 6
    2 7
    2 8
    【输出样例】
    4
    2 
    6 7 8

```c++
#include <bits/stdc++.h>
using namespace std;

int tree[1005][1005];
// 邻接表在这里不方便使用，改为邻接矩阵 
int maxnode;
int maxchild = INT_MIN;
int root;
int check[1005];


int main() {
    int n, m;
    cin >> n >> m;
    cout << m << endl;
    for (int i = 1; i <= m; i++) {
        int x, y;
        cin >> x >> y;
        // 如果这个数在y中出现，标记为0，说明是孩子 
		// 如果没有，并在x中出现，标记为1，说明是父亲
        check[x] = 1;
        check[y] = 0;
        tree[x][y] = 1;
    }
    for (int i = 1; i <= n; i++) {
        int temp = 0; //统计孩子个数 
        for (int j = 1; j <= n; j++) {
            if (tree[i][j]) {
                temp++;
            }
        }
        // 比较并赋值 
        if (temp > maxchild) {
            maxnode = i; 
            maxchild = temp;
        }
    }
   	// 寻找标记为1（根节点） 
    for (int i = 1; i <= n; i++) {
        if (check[i] == 1) {
            root = i;
        }
    }
    cout << root << endl << maxnode << endl;
    for (int i = 1; i <= n; i++) {
        if (tree[maxnode][i]) {
            cout << i << ' ';
        }
    }
    return 0;
}
```
## 二叉树遍历转换（important）
    输入一棵二叉树的先序和中序遍历序列，输出其后序遍历序列。
    输入：共两行，第一行一个字符串，表示树的先序遍历，第二行一个字符串，表示树的中序遍历。树的结点一律用小写字母表示。
    输出：一行，表示树的后序遍历序列。
    
    【输入样例】
    abdec 
    dbeac 
    【输出样例】 
    debca 

```c++
#include <bits/stdc++.h>
using namespace std;

string prestring; // 前序遍历字符串 
string midstring; // 中序遍历字符串 

void func(int prel, int prer, int midl, int midr) {
	// prel 前序遍历起始位置（相当于本次处理的树的根）
	// prer 前序遍历终止位置
	// midl 中序遍历起始位置
	// midr 中序遍历终止位置 
	int pos = 0; 
	for (int i = 0; i < midstring.size(); i++) {
		if (midstring[i] == prestring[prel]) {
			pos = i;
			// 寻找前序遍历起始位置(本次处理树的根)在中序遍历中的位置 
			break;
		}
	}
	if (pos > midl) {
		// 如果存在中序遍历起始位置在根的左边，说明有左子树 
		func(prel + 1, prer + pos - midl, midl, pos - 1);
		// prel + 1 本次树根节点 + 1（前序遍历"根""左""右"的特性） 
		// pos - midl 中序遍历根节点位置 - 中序遍历起始位置 = 处理子树的节点数
		// prer + pos - midl 前序遍历终止位置
		// midl 由于是处理左子树， 所以中序遍历起始位置不变
		// pos - 1 中序遍历结束位置（去掉本次的根的位置） 
		 
	}
	if (pos < midr) {
		// 如果有右子树 
		func(prel + pos - midl + 1, prer, pos + 1, midr);
		// prel + pos - midl + 1 寻找前序中新树根节点（若不+1，则是取左子树中最右的叶子节点，无法取到根）
		// prer 前序遍历终止位置不变
		// pos + 1 中序遍历起始位置
		// midr 中序遍历结束位置不变 
	}
	cout << prestring[prel]; 
	retrun;
}

int main() {
	cin >> prestring;
	cin >> midstring;
	func(0, prestring.size() - 1, 0, midstring.size() - 1);
	// 传入整个树 

```
