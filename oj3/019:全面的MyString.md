# 019:全面的MyString
[题目链接](http://cxsjsxmooc.openjudge.cn/2023t3spring/019/)

---
# 代码
```CPP
#include <cstdlib>
#include <iostream>
using namespace std;
int strlen(const char * s) 
{	int i = 0;
	for(; s[i]; ++i);
	return i;
}
void strcpy(char * d,const char * s)
{
	int i = 0;
	for( i = 0; s[i]; ++i)
		d[i] = s[i];
	d[i] = 0;
		
}
int strcmp(const char * s1,const char * s2)
{
	for(int i = 0; s1[i] && s2[i] ; ++i) {
		if( s1[i] < s2[i] )
			return -1;
		else if( s1[i] > s2[i])
			return 1;
	}
	return 0;
}
void strcat(char * d,const char * s)
{
	int len = strlen(d);
	strcpy(d+len,s);
}
class MyString
{
// 在此处补充你的代码
private:
    char* p;
public:
    MyString()
    {
        p = new char[1];
        p[0] = 0;
    }
    MyString(const MyString & S)
    {
        p = new char[strlen(S.p) + 1];
        strcpy(p, S.p);
    }
    MyString(const char* str)
    {
        p = new char[strlen(str) + 1];
        strcpy(p, str);
    }
    ~MyString() {delete []p;}

    /* 重载赋值运算符 */
    MyString & operator= (const MyString & S)
    {
        if (this == &S)
            return *this;
        delete []p;
        p = new char[strlen(S.p) + 1];
        strcpy(p, S.p);
        return *this;
    }
    MyString & operator= (const char* str)
    {
        if (p)
            delete []p;
        p = new char[strlen(str) + 1];
        strcpy(p, str);
        return *this;
    }

    /* 重载+运算符 */
    /* string1 + string2 + string3 */
    MyString operator+ (const MyString & S) const
    //const成员函数才能连续相加
    {//返回一个"临时对象"
        MyString temp{};
        delete []temp.p;
        temp.p = new char[strlen(S.p) + strlen(p) + 1];
        strcpy(temp.p, p);
        strcat(temp.p, S.p);
        return temp;/* 只能将临时对象赋值给const的对象 */
    }
    /* string + char* str + char* str */
    MyString operator+ (const char* str) const 
    {
        MyString temp(str);
        return *this + temp;
    }
    /* char* str + string + string */
    friend MyString operator+ (const char* str, const MyString & S)
    {
        MyString temp(str);
        return temp + S;
    }

    /*  重载 << 运算符 */
    friend ostream& operator<< (ostream& os, const MyString & S)
    {
        os << S.p;
        return os;
    }

    /* 重载+= */
    MyString & operator+= (const char* str)
    {
        *this = *this + str;
        return *this;
    }

    /* 重载[] */
    char& operator[] (int idx)
    {
        return p[idx];
    }

    /* 重载 () */
    MyString operator() (int start, int len)
    {
        int i;
        char* sub = new char[len + 1];
        for (i = 0; i < len; ++i)
            sub[i] = p[i + start];
        sub[i] = 0;
        MyString temp(sub);
        return temp;
    }
    
    /* 重载比较运算符 */
    bool operator> (const MyString & S)
    {
        return strcmp(p, S.p) > 0;
    }
    bool operator< (const MyString & S)
    {
        return strcmp(p, S.p) < 0;
    }
    bool operator== (const MyString & S)
    {
        return strcmp(p, S.p) == 0;
    }

};


int CompareString( const void * e1, const void * e2)
{
	MyString * s1 = (MyString * ) e1;
	MyString * s2 = (MyString * ) e2;
	if( * s1 < *s2 )
	return -1;
	else if( *s1 == *s2)
	return 0;
	else if( *s1 > *s2 )
	return 1;
}
int main()
{
	MyString s1("abcd-"),s2,s3("efgh-"),s4(s1);
	MyString SArray[4] = {"big","me","about","take"};
	cout << "1. " << s1 << s2 << s3<< s4<< endl;
	s4 = s3;
	s3 = s1 + s3;
	cout << "2. " << s1 << endl;
	cout << "3. " << s2 << endl;
	cout << "4. " << s3 << endl;
	cout << "5. " << s4 << endl;
	cout << "6. " << s1[2] << endl;
	s2 = s1;
	s1 = "ijkl-";
	s1[2] = 'A' ;
	cout << "7. " << s2 << endl;
	cout << "8. " << s1 << endl;
	s1 += "mnop";
	cout << "9. " << s1 << endl;
	s4 = "qrst-" + s2;
	cout << "10. " << s4 << endl;
	s1 = s2 + s4 + " uvw " + "xyz";
	cout << "11. " << s1 << endl;
	qsort(SArray,4,sizeof(MyString),CompareString);
	for( int i = 0;i < 4;i ++ )
	cout << SArray[i] << endl;
	//s1的从下标0开始长度为4的子串
	cout << s1(0,4) << endl;
	//s1的从下标5开始长度为10的子串
	cout << s1(5,10) << endl;
	return 0;
}
```
---
# 1. 类的基础封装与内存管理（重中之重）
### 提问 1：你在 MyString 类中定义了私有成员 char* p，为什么要使⽤动态内存分配？不动态分配（⽐如⽤固定数组）会有什么问题？

### 提问 2：什么是浅拷⻉？什么是深拷⻉？你实现的MyString为什么必须使⽤深拷⻉？结合你的代码说明。

### 提问 3：你实现的赋值运算符（operator=），为什么要先判断this == &S？不判断会有什么问题？结合代码说明。

### 提问 4：你的代码中，operator+ 返回值是MyString，⽽operator+= 返回值是MyString&，为什么要这样设计？两者的区别是什么？

# 2. 运算符重载⾼频考点（⼤⼚重点追问）
### 提问 1：你的代码中，operator[] 返回char& ⽽不是char，为什么要这样设计？返回char会有什么问题？

### 提问 2：operator<<（输出运算符）为什么必须重载为友元函数，⽽不能作为MyString类的成员函数？结合代码说明。

### 提问 3：你的代码中，为什么要提供“const char* + MyString”的全局友元operator+？成员函数operator+ 不能实现吗？
### 提问 4：operator () 在这⾥实现什么功能？为什么可以像函数⼀样调⽤？
# 3. 内存与异常安全（⼤⼚最爱）
### 提问 10：你的赋值运算符是否异常安全？怎么改进？
### 提问 11：为什么析构要⽤ delete [] ⽽不是 delete？
# 四、与 qsort 结合的考点（⾮常⾼频）
### 提问 12：qsort 为什么需要你写 CompareString ⽐较函数？
### 提问 13：⽐较运算符 < / == / > 为什么建议加 const？
# 五、经典陷阱题（⾯试官最爱挖坑）
### 提问 14：默认构造函数为什么初始化为空串⽽不是 nullptr？
### 提问 15：如果不写拷⻉构造函数，会发⽣什么？
### 提问 16：连续赋值 s1 = s2 = s3 为什么能成⽴？
