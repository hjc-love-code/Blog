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
}
```
## FBI树
	我们可以把由“0”和“1”组成的字符串分为三类：全“0”串称为B串，全“1”串称为I串，既含“0”又含“1”的串则称为F串。
	FBI树是一种二叉树，它的结点类型也包括F结点，B结点和I结点三种。由一个长度为2N的“01”串S可以构造出一棵FBI树T，递归的构造方法如下：
	T的根结点为R，其类型与串S的类型相同；
	若串S的长度大于1，将串S从中间分开，分为等长的左右子串S1和S2；由左子串S1构造R 的左子树T1，由右子串S2构造R的右子树T2。
	现在给定一个长度为2N的“01”串，请用上述构造方法构造出一棵FBI树，并输出它的后序遍历序列。
	
	输入：第一行是一个整数N（0≤N≤10），第二行是一个长度为2N的“01”串。
 	输出：一行，这一行只包含一个字符串，即FBI树的后序遍历序列。
	
	【输入样例】 
 	3 10001011 
  	【输出样例】 
   	IBEBBBEIBFIIIFE 

```c++
#include<bits/stdc++.h>
using namespace std;

int n;
string s;


// 这里将使用下表（索引）在原数组中取区间的想法废弃了
// 改为传入字符串作为下一个子树 
void create(string last, int x) {
	if (x < 0) {
		// n代表2 ^ n，但是FBI树包含叶子，也就是没有左右子树，要运行n + 1次递归 
		return;
	} 
	// substr(起始位置，要取的长度)
	create(last.substr(0, last.size() / 2), x - 1); 
	// 左子树 
	create(last.substr(last.size() / 2, last.size() / 2), x - 1);
	// 右子树 
	bool one, zero;
	one = zero = 0;
	// 一定要注意，必须和字符比较，否则始终为true 
	for (int i = 0; i < last.size(); i++) {
		if (last[i] == '1') {
			one = true;
		}
		if (last[i] == '0') {
			zero = true;
		}
	}
	if (one && zero) {
		cout << 'F';
	}
	else if (one == true && zero == false) {
		cout << 'I';
	}
	else if (zero == true && one == false){
		cout << 'B';
	}
	// 输出 
}

int main() {
	cin >> n;
	cin >> s;
	create(s, n);
}
```
## 平衡后缀
	给定长度为 n 的字符串 S 和一个整数 k。求 S 是否存在一种重排是“好的”。
	一个字符串是“好的”，当且仅当：它的任意一个后缀都是“平衡的”。
	一个字符串是“平衡的”，当且仅当：它的任意两个字符出现次数之差不超过 k。
	注意：原串中没出现的字符不需要考虑，原串中出现过的字符在每一个后缀中都需考虑。
	注意：单独一个字符构成的字符串也是“平衡的”。
	如果不存在某个重排是“好的”，则输出-1。
	如果存在，则输出字典序最小的重排。

	输入格式
	第一行一个整数 T 代表数据组数。
	每组数据第一行两个整数代表 n 和 k，第二行一个只包含小写字母的字符串代表 S。
	
	输出格式
	每组数据输出一行，代表答案。

	【样例输入】
	4
	3 1
	aaa
	4 2
	baba
	4 1
	babb
	7 2
	abcbcac
	【样例输出】
	aaa
	aabb
	-1
	abcabcc
```c++
// 可以将问题转换为
// 统计每个字母个数，并记录所有出现过的字符 
// 通过for循环由遍历字串，由长至短，根据ascll码顺序从小到大（尽量让ascll码小的字符排在前面）尝试可以相较于上一字串可以缺少的一个字符，若可行，输出;
// 不可能的情况:原串中某个字符的次数 - 另一个字符的次数 > k 
#include <bits/stdc++.h>
using namespace std;
#define int long long

int T, n, k;
int sum[30]; // 数组统计每个字母的个数 
int check[30]; // 记录原串中出现过的字符 

signed main() {
	cin >> T;
	while (T--) {
		string s;
		memset(sum, 0, sizeof(sum)); //重置数组 
		cin >> n >> k;
		cin >> s;
		for (int i = 0; i < n; i++) {
			sum[s[i] - 'a']++; // 压缩小写字母至0 ~ 26 
			check[s[i] - 'a']++;
		}
		int mx = INT_MIN, mn = INT_MAX;
		for (int i = 0; i < 30; i++) {
			if (sum[i]) {
				mx = max(mx, sum[i]);
				mn = min(mn, sum[i]);
			}
		}
		if (mx - mn > k) { // 若字母最大出现次数 - 最小出现次数 > k 则整个串不是平衡的 
			cout << -1 << endl;
			continue;
		}
		for (int i = 0; i < n; i++) {// 遍历所有字串（i即减少的字符数） 
			for (int j = 0; j < 30; j++) {// 由ascll码从小到大进行遍历尝试 
				if (sum[j]) { // 若原串中的字符个数还有剩余 
					sum[j]--; // 若新字串相比上一字串缺少这个字符，判断符不符合出现最多字符数 - 出现最少字符数(包含原串中所有出现过的字符，子串中没出现的则为0) > k; 
					mx = INT_MIN;
					mn = INT_MAX;
					for (int k = 0; k < 30; k++) {
						if (check[k]) { //这里不能直接判断sum中这个字符是否>0，有出现某个字符用完的可能，需在之前另开数组记录下原串中出现过的字符 
							mx = max(mx, sum[k]);
							mn = min(mn, sum[k]);
						}
					}
					if (mx - mn > k) {
						sum[j]++; // 若不行，还原 
						continue;
					}
					else {
						cout << char(j + 'a'); // 若可以，输出第一个可行的字符（由于是按照ascll码从小到大进行遍历，所以不用考虑字典序） 
    					break; // 一定要跳出
                    }
				}
			}
		}
		cout << endl;
	}
	return 0;
}
```
