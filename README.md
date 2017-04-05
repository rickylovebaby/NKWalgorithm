
# 牛客网编程训练

## 在线编程

### 小米（懂二进制）

世界上有10种人，一种懂二进制，一种不懂。那么你知道两个int32整数m和n的二进制表达，有多少个位(bit)不同么？ 


```cpp
class Solution {
public:
    /**
     * 获得两个整形二进制表达位数不同的数量
     * 
     * @param m 整数m
     * @param n 整数n
     * @return 整型
     */
    int countBitDiff(int m, int n) {
        int dif = m ^ n;
        int cnt = 0;
        while(dif){
            dif = dif&(dif-1);
            cnt++;
        }
        return cnt;
    }
};
```
### 风口的猪-中国牛市
风口之下，猪都能飞。当今中国股市牛市，真可谓“错过等七年”。 给你一个回顾历史的机会，已知一支股票连续n天的价格走势，以长度为n的整数数组表示，数组中第i个元素（prices[i]）代表该股票第i天的股价。 假设你一开始没有股票，但有至多两次买入1股而后卖出1股的机会，并且买入前一定要先保证手上没有股票。若两次交易机会都放弃，收益为0。 设计算法，计算你能获得的最大收益。 输入数值范围：2<=n<=100,0<=prices[i]<=100 

`code`

```cpp
class Solution {
public:
    /**
     * 计算你能获得的最大收益
     * 
     * @param prices Prices[i]即第i天的股价
     * @return 整型
     */
    int calculateMax(vector<int> prices) {
        int hold1 =-101,hold2 = -101;
        int release1 = 0, release2= 0;
        for(int i : prices){
            release2 = max(release2,hold2 + i);//第二次卖
            hold2 = max(hold2,release1 - i);//第二次买
            release1 = max(release1,hold1 + i);//第一次卖
            hold1 = max(hold1,- i);//第一次买
        }
        return release2;
    }
};
```


## 腾讯

### 2016年研发工程师算法题
#### 1.生成格雷码


```cpp
class GrayCode {
public:
    vector<string> getGray(int n) {
        vector<string> gray;
        if(n == 1){
            gray.push_back("0");
            gray.push_back("1");
            return gray;
        }
        vector<string> last_gray = getGray(n-1);
        for(int i = 0; i < last_gray.size(); i++)
            gray.push_back("0"+last_gray[i]);
        for(int i = last_gray.size()-1; i >= 0; i--)
            gray.push_back("1"+last_gray[i]);

        return gray;
    }
};
```
#### 2.微信红包
春节期间小明使用微信收到很多个红包，非常开心。在查看领取红包记录时发现，某个红包金额出现的次数超过了红包总数的一半。请帮小明找到该红包金额。写出具体算法思路和代码实现，要求算法尽可能高效。
给定一个红包的金额数组gifts及它的大小n，请返回所求红包的金额。若没有金额超过总数的一半，返回0。


```cpp
class Gift {
public:
    int getValue(vector<int> gifts, int n) {
        int candi = 0,count = 0;
        for(int i = 0; i < n;i++){
            if(count == 0){
                candi = gifts[i];
                count = 1;
            }
            else{
                if(candi == gifts[i]){
                    count++;
                }else{
                    count--;
                }
            }      
        }
        int cntCan = 0;
        for(int i = 0; i < n;i++){
            if(candi == gifts[i])
                cntCan++;
        }
        return cntCan > n/2 ? candi : 0;
        
    }
};
```

## 网易

### 2017年春招算法题
#### 1.双核处理
一种双核CPU的两个核能够同时的处理任务，现在有n个已知数据量的任务需要交给CPU处理，假设已知CPU的每个核1秒可以处理1kb，每个核同时只能处理一项任务。n个任务可以按照任意顺序放入CPU进行处理，现在需要设计一个方案让CPU处理完这批任务所需的时间最少，求这个最小的时间

输入描述：
输入包括两行：
>第一行为整数n(1 ≤ n ≤ 50)
第二行为n个整数length[i](1024 ≤ length[i] ≤ 4194304)，表示每个任务的长度为length[i]kb，每个数均为1024的倍数


```cpp
#include<iostream>
#include<algorithm>
#include<set>
using namespace std;
int n;
int a[51];
set<int> s;
int sum=0;
int main(){
    cin>>n;
    for(int i=0;i<n;i++){
        int tmp;
        cin>>tmp;
        a[i]=tmp/1024;
        sum+=a[i];
    }
    s.insert(0);
    for(int i=0;i<n;i++){
        set<int> added;
        for(set<int>::iterator it=s.begin();it!=s.end();it++){
            added.insert(*it+a[i]);
        }
        s.insert(added.begin(),added.end());
    }
    int ans=sum;
    for(set<int>::iterator it=s.begin();it!=s.end();it++){
        ans=min(ans,max(*it,sum-*it));
    }
    cout<<ans*1024<<endl;
}
```
#### 3.消除重复元素
小易有一个长度为n序列，小易想移除掉里面的重复元素，但是小易想是对于每种元素保留最后出现的那个。小易遇到了困难,希望你来帮助他。

```cpp
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;

int main(){
	int n;
	cin >> n;
	vector<int> vec;
	int count[1001] = {0};
    for(int i = 0; i < n;i++){
        int num;
		cin >> num;
		vec.push_back(num);
		count[num]++;
	}
	bool flag = false;
	for(int i = 0; i < n; i++){
		if(count[vec[i]] == 1){
			if(flag == false)
			{
				cout << vec[i];
				flag = true;
			}
			else{
				cout << " " <<vec[i];
			}
			count[vec[i]] = 0;
		}else{
			count[vec[i]]--;
		}
	}
	return 0;

}
```

#### 7.集合
小易最近在数学课上学习到了集合的概念,集合有三个特征：1.确定性 2.互异性 3.无序性.
小易的老师给了小易这样一个集合：
S = { p/q | w ≤ p ≤ x, y ≤ q ≤ z }
需要根据给定的w，x，y，z,求出集合中一共有多少个元素。小易才学习了集合还解决不了这个复杂的问题,需要你来帮助他。

```cpp
#include <iostream>
#include <set>
#include <utility>
using namespace std;

int gcd(int a,int b){
	if(b == 0){
		return a;
	}
	a %= b;
	return gcd(b,a);
}
int main(){

	int w,x,y,z;
    int ii,jj;
	int d;
	pair<int,int> res;
	set<pair<int,int>> iset;
 	while(cin >> w >> x >> y >> z){
		for(int i = w; i <= x;i++){
			for(int j = y;j <=z;j++){
                ii = i,jj = j;
				d = gcd(i,j);
				ii/=d;jj/=d;
				iset.insert(make_pair(ii,jj));
			}
		}
		cout << iset.size()<<endl;
	}
	return 0;
}
```
#### 10.小易记单词
小易参与了一个记单词的小游戏。游戏开始系统提供了m个不同的单词，小易记忆一段时间之后需要在纸上写出他记住的单词。小易一共写出了n个他能记住的单词，如果小易写出的单词是在系统提供的，将获得这个单词长度的平方的分数。注意小易写出的单词可能重复，但是对于每个正确的单词只能计分一次。 

输入描述：
>输入数据包括三行：
第一行为两个整数n(1 ≤ n ≤ 50)和m(1 ≤ m ≤ 50)。以空格分隔
第二行为n个字符串，表示小易能记住的单词，以空格分隔，每个单词的长度小于等于50。
第三行为m个字符串，系统提供的单词，以空格分隔，每个单词的长度小于等于50。

```cpp
#include <iostream>
#include <string>
#include <set>
using namespace std;

int main(){
	int n,m;
	int sum = 0;
	string nstr,mstr;
	set<string> res1,res2;
	cin >> n >> m;
	
	while(n--){
		cin >> nstr;
		res1.insert(nstr);
	}
	while(m--){
		cin >> mstr;
		res2.insert(mstr);
	}
	for(auto &c : res1){
		if(res2.find(c) != res2.end())
			sum += c.size() * c.size();
	}
	cout << sum << endl;
	
	return 0;
}
```


## 阿里

### 2016年研发工程师算法题
#### 1

