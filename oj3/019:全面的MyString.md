# 019:全面的MyString
[题目链接](http://cxsjsxmooc.openjudge.cn/2023t3spring/019/)

---

# MyString类 -- 面试问题总结
### 针对以下代码总结常见面试问题
```cpp
#include<iostream>
#include<cstring>
using namespace std;

class MyString
{
private:
    char* p;
public:
    MyString()
    {
        p = new char[1];
        p[0] = '\0';
    }
    MyString(const char* str)
    {
        p = new char[strlen(str) + 1];
        strcpy(p, str);
    }
    MyString(const MyString & S)
    {
        p = new char[strlen(S.p) + 1];
        strcpy(p, S.p);
    }
    ~MyString()
    {
        delete []p;
    }
    MyString & operator= (const char* str)
    {
        if (p == str)
            return *this;
        delete []p;
        p = new char[strlen(str) + 1];
        strcpy(p, str);
        return *this;
    }
    MyString & operator= (const MyString & S)
    {
        if (this == &S)
            return *this;
        delete []p;
        p = new char[strlen(S.p) + 1];
        strcpy(p, S.p);
        return *this;
    }
    friend ostream & operator<< (ostream & os, const MyString & S)
    {
        os << S.p;
        return os;
    }
    MyString operator+ (const MyString & S) const
    {
        MyString temp(p);
        strcat(temp.p, S.p);
        return temp;
    }
    MyString operator+ (const char* str) const
    {
        MyString temp(str);
        return *this + temp;
    }
    friend MyString operator+ (const char* str, const MyString & S)
    {
        MyString temp(str);
        return temp + S;
    }
    MyString & operator+= (const char* str)
    {
        *this = *this + str;
        return *this;
    }
    char & operator[] (int idx)
    {
        return p[idx];
    }
    char operator[] (int idx) const
    {
        return p[idx];
    }
    MyString operator() (int start, int len)
    {
        int size = strlen(p);
        if (start < 0 || start >= size)
            return MyString{};
        int actualLen = start + len - 1 < size ? len : size - start;
        char* str = new char[actualLen + 1];
        strncpy(str, p + start, actualLen);
        str[actualLen] = '\0';
        MyString temp(str);
        delete []str;
        return temp;
    }
    bool operator> (const MyString & S) const
    {
        return strcmp(p, S.p) > 0;
    }
    bool operator< (const MyString & S) const
    {
        return strcmp(p, S.p) < 0;
    }
    bool operator== (const MyString & S) const 
    {
        return strcmp(p, S.p) == 0;
    }
};
/* qsort()自定义比较函数 */
int CompareString( const void * e1, const void * e2)
{
    const MyString* s1 = (const MyString*)e1;
    const MyString* s2 = (const MyString*)e2;
    if (*s1 > *s2)
        return 1;
    else if (*s1 < *s2)
        return -1;
    else
        return 0;
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

### 1. 你在 MyString 类中定义了私有成员 char* p ，为什么要使⽤动态内存分配？不动态分配（⽐如⽤固定数组）会有什么问题？  
>1）使用动态内存分配：可以灵活地根据字符串长度分配内存空间，避免了内存溢出或造成内存大量浪费
>
>2）使用固定数组：
>-  长度受限：字符串过长会造成**内存溢出、程序崩溃**；字符串过短会**大量内存浪费**
>- 无法灵活地进行**赋值/拼接**：固定数组不能扩容/缩容，赋值拼接操作失效
>- 对象体积臃肿：每一个对象都会携带一个固定长度的字符数组，内存占用极高
### 2. 什么是浅拷贝?什么是深拷贝？你的MyString类是如何全面实现深拷贝的？请结合你写的代码说明。
### 3. 什么是 C++ 的三 / 五法则？你的类符合这个法则吗？
### 4. 你写的const char*版本的赋值运算符里的自赋值判断if(p == str)，你觉得有问题吗？
### 5.你实现的赋值运算符 ( operator= ), 为什么要先判断 this == &S ? 不判断会有什么问题？结合代码说明。
### 6.你的代码中， operator+ 返回值是 MyString , ⽽ operator+= 返回值是 MyString& , 为什么要这样设计？两者的区别是什么？
### 7.operator<< (输出运算符) 为什么必须重载为友元函数，⽽不能作为MyString 类的成员函数？结合代码说明。
### 8.你的代码中，为什么要提供 "const char* + MyString" 的全局友元operator+ ? 成员函数 operator+ 不能实现吗？
### 9.operator () 在这⾥实现什么功能？为什么可以像函数⼀样调⽤？
### 10. 为什么析构要⽤ delete [] ⽽不是 delete ?
### 11.qsort 为什么需要你写 CompareString ⽐较函数？
### 12.你实现的operator+运算符，有没有发现缓冲区溢出的问题？
### 13.为什么要实现两个版本的赋值运算符重载？
### 14.为什么要给operator[]实现两个版本？
### 15.比较运算符后面的const是什么意思？为什么要加？
### 16.为什么 qsort 的比较函数要写成全局函数？不能写成类的成员函数吗？
### 17.为什么用strncpy之后还要手动加'\0'？
### 18.你的析构函数不是虚函数，这会带来什么问题？
### 19.你的operator+=的实现有没有可以优化的地方？
### 20.你将成员函数operator+声明为const是为了什么？
### 21.你为C语言的库函数 qsort() 写的比较函数 CompareString 传入的指针类型是 const void* , 你能结合代码说一下将 const void* 转换为 const MyStirng* 有什么具体的变化吗？
### 22. 可以把自己写的无参构造函数去掉，用C++默认的吗？

### 23. 你的类现在支持移动语义吗？怎么添加？有什么好处？
### 24. 你了解小字符串优化（SSO）吗？你的类可以怎么用它来优化？
### 25. 什么是拷贝和交换惯用法？用它来实现赋值运算符有什么好处？
### 26. 你了解写时复制（COW）吗？为什么现在很少用了？
### 27. 赋值运算符为什么要返回MyString&引用？不能返回值吗？
## RAII
### 1. 你了解 RAII 吗？你能默写出 RAII 的英文全称吗？你的 MyString 类是不是用到了 RAII？
### 2. 如果不用 RAII，手动管理这个字符串的内存，会有什么问题？
### 3. 你的 MyString 类的拷贝操作，和 RAII 有什么关系？
### 4. RAII 只能用来管理堆内存吗？
