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
# MyString程序填空题 大厂面试高频提问完整答案

## 一、核心考点（对应面试官高频提问\+完整答案）

### 1\. 类的基础封装与内存管理（重中之重）

#### 提问 1：你在 MyString 类中定义了私有成员 char\* p，为什么要使用动态内存分配？不动态分配（比如用固定数组）会有什么问题？

答：使用动态内存分配的核心原因是 **灵活适配字符串长度**，满足程序中字符串可变、长度不确定的需求，这也是C\+\+中自定义字符串类的核心设计思路，更是大厂面试考察内存管理能力的重点。具体来说，MyString类需要支持多种字符串操作（如题目中的赋值、拼接、子串提取、对象拷贝等），这些操作都会导致字符串长度发生变化，例如s1初始值为\&\#34;abcd\-\&\#34;（长度5），s3初始值为\&\#34;efgh\-\&\#34;（长度5），两者拼接后s3长度变为10，若后续再进行\+=操作，长度会进一步增加。动态内存分配（通过new\[\]）可以根据当前字符串的实际长度，精准申请所需内存空间，既不会造成内存浪费，也不会出现内存不足的问题，同时契合C\+\+ RAII（资源获取即初始化）的设计原则，通过析构函数（delete\[\] p）释放内存，避免内存泄漏。此外，动态内存分配的内存位于堆区，生命周期由程序员通过new/delete控制，能够满足MyString对象在函数间传递、赋值、拷贝时的内存管理需求，确保对象在不同作用域下都能正常使用，这也是固定数组无法实现的核心优势。

若不动态分配，改用固定数组（如char p\[100\]），会出现两个无法规避的核心问题，这也是面试官常追问的“内存管理漏洞”，具体如下：① 内存浪费严重：固定数组的大小一旦定义就无法修改，若存储的字符串长度远小于数组长度（例如仅存储单个字符\&\#39;a\&\#39;，却占用100字节的数组空间），剩余99字节的内存无法被其他程序或对象利用，造成堆内存（或栈内存）的无效消耗，不符合C\+\+“高效利用内存”的设计理念，面试中会被判定为内存管理考虑不周全；② 内存不足导致越界：若字符串长度超过固定数组的大小（例如题目中s1=\&\#34;abcd\-\&\#34;与s3=\&\#34;efgh\-\&\#34;拼接后长度为10，若固定数组大小为8，拼接后的字符串就会超出数组边界），会触发数组越界访问，属于未定义行为，具体表现为程序崩溃、数据错乱（例如覆盖其他变量的内存），甚至可能引发安全隐患。此外，固定数组无法适配题目中“任意长度字符串赋值、拼接”的需求，例如将一个长度为200的字符串赋值给MyString对象，固定数组会直接无法容纳，导致程序运行失败，这也是固定数组无法替代动态内存分配的关键原因。同时，从面试考点来看，面试官通过这个问题，核心考察我们对“动态内存分配的必要性”“内存浪费与越界风险”“C\+\+内存管理核心原则”的理解，这也是区分基础扎实与否的重要考点。

#### 提问 2：什么是浅拷贝？什么是深拷贝？你实现的MyString为什么必须使用深拷贝？结合你的代码说明。

答：这是大厂面试考察MyString类的核心必考题，核心是考察内存管理的严谨性，结合我的代码具体说明如下：① 浅拷贝：仅拷贝指针变量本身（即char\* p的地址），不拷贝指针指向的内存内容，也就是说，两个MyString对象的p成员会指向同一块堆内存，没有重新开辟新的内存空间。比如，若不写自定义拷贝构造函数，编译器会生成默认的浅拷贝构造，此时MyString s4\(s1\)会让s4\.p = s1\.p，两者共享同一块内存；② 深拷贝：不仅拷贝指针变量，还会根据原指针指向的内存内容，重新开辟一块大小相同的新内存，将原内容完整复制到新内存中，两个对象的指针指向各自独立的内存，互不影响。我在代码中实现的拷贝构造函数（MyString\(const MyString \&amp; S\)）和赋值运算符（operator=），都是典型的深拷贝实现——拷贝构造中，先通过strlen\(S\.p\)获取原字符串长度，再用new\[\]开辟对应大小的新内存，最后用strcpy将S\.p指向的内容复制到当前对象的p中，确保s4\(s1\)后，s4\.p和s1\.p指向不同内存，修改其中一个不会影响另一个。

MyString必须使用深拷贝的核心原因，是避免内存泄漏和程序崩溃，这也是面试官重点关注的“踩坑点”：若使用浅拷贝，会出现两个致命问题：1\. 重复释放内存（double free）：当两个对象生命周期结束时，都会调用析构函数delete\[\] p，而两者指向同一块内存，第一次delete\[\]会释放该内存，第二次delete\[\]会释放已经无效的内存，触发未定义行为，直接导致程序崩溃；2\. 数据相互干扰：一个对象修改字符串内容（比如s1\[2\] = \&\#39;A\&\#39;），会直接修改共享内存中的内容，导致另一个对象的字符串也被意外修改，违背封装的设计原则，造成数据错乱。结合我的代码，s4初始是s1的拷贝，若用浅拷贝，s4\.p和s1\.p指向同一块内存，当s4 = s3时，delete\[\] s4\.p会释放s1\.p指向的内存，后续s1使用时就会出现野指针，程序崩溃，因此深拷贝是MyString类实现的核心前提。

#### 提问 3：你实现的赋值运算符（operator=），为什么要先判断this == \&amp;S？不判断会有什么问题？结合代码说明。

答：判断this == \&amp;S的核心目的，是**防止自赋值**，这是赋值运算符重载的基础规范，也是大厂面试高频扣分点，结合我的代码具体分析如下：this指针指向当前对象，\&amp;S是传入的右操作数对象的地址，判断两者是否相等，本质是判断“当前对象是否给自己赋值”（比如s1 = s1）。

若不判断自赋值，会导致严重的内存错误，结合我的代码中的赋值运算符实现（operator=\(const MyString \&amp; S\)），具体问题如下：我的赋值运算符逻辑是“先释放当前对象的内存（delete\[\] p），再开辟新内存拷贝内容”，若发生自赋值（s1 = s1），会先执行delete\[\] p，此时当前对象的p指向的内存被释放，变成野指针；后续执行strcpy\(p, S\.p\)时，S\.p就是当前对象的p（因为自赋值），此时拷贝的是已经被释放的野指针指向的内容，会导致内存访问错误（段错误），程序崩溃。此外，即使没有直接崩溃，也会造成内存泄漏——原本的内存被释放，却没有正确拷贝内容，当前对象的p变成野指针，后续无法释放，不符合C\+\+内存管理的规范。

补充说明：判断自赋值后，若this == \&amp;S，直接返回\*this，避免了无效的内存释放和拷贝操作，既提升了效率，又避免了内存错误，这也是面试官判断赋值运算符实现是否严谨的重要标准。

#### 提问 4：你的代码中，operator\+ 返回值是MyString，而operator\+= 返回值是MyString\&amp;，为什么要这样设计？两者的区别是什么？

答：这道题考察运算符重载的返回值设计逻辑，核心是区分“修改自身”和“产生新对象”，结合MyString的操作场景和面试考点，具体说明如下：

首先明确两者的核心区别：operator\+ 是“拼接两个字符串，产生一个新的字符串对象”，不修改原有对象；operator\+= 是“将另一个字符串拼接到当前对象末尾，修改当前对象本身”，这一区别决定了两者的返回值类型。

1\. operator\+ 返回MyString（值返回）的原因：operator\+ 执行时，会创建一个临时对象（比如s1 \+ s3，会创建一个新的MyString对象，存储拼接后的内容），这个临时对象是局部对象，生命周期仅限于operator\+ 函数内部。若返回MyString\&amp;（引用返回），则返回的是局部对象的引用，函数结束后局部对象被销毁，引用就变成了悬空引用，后续使用该引用（比如s3 = s1 \+ s3）会访问无效内存，导致程序崩溃。同时，为了支持连续拼接（比如s1 \+ s2 \+ s3），必须返回值——因为s1 \+ s2返回一个新对象，才能继续和s3执行operator\+ 操作，链式调用才能成立。我的代码中，operator\+ 的实现的就是如此：创建temp临时对象，拼接后返回temp，确保连续拼接和赋值的正确性。

2\. operator\+= 返回MyString\&amp;（引用返回）的原因：operator\+= 是修改当前对象本身（比如s1 \+= \&\#34;mnop\&\#34;，是将\&\#34;mnop\&\#34;拼接到s1的末尾，修改s1的p指向的内存内容），当前对象（this指向的对象）在函数执行期间是存在的，不会被销毁，返回其引用不会出现悬空引用的问题。同时，返回引用可以支持链式赋值（比如s1 \+= s2 \+= s3），因为s2 \+= s3返回s2的引用，才能继续和s1执行\+=操作，提升代码的灵活性和效率。我的代码中，operator\+= 的实现是“\*this = \*this \+ str”，修改当前对象后返回\*this（引用），符合这一设计原则。

面试补充：这是面试官区分“基础扎实与否”的考点，重点考察对“局部对象生命周期”“引用返回的风险”“运算符重载设计逻辑”的理解，若混淆两者的返回值类型，会直接被判定为基础不牢固。

### 2\. 运算符重载高频考点（大厂重点追问）

#### 提问 1：你的代码中，operator\[\] 返回char\&amp; 而不是char，为什么要这样设计？返回char会有什么问题？

答：这道题考察运算符重载的实用性设计，核心是实现“字符串的可读可写”，结合题目中的场景和面试考点，具体说明如下：

operator\[\] 的核心功能是“通过下标访问字符串中的单个字符”，题目中存在“s1\[2\] = \&\#39;A\&\#39;”这样的操作，要求通过下标修改字符串的内容，这就决定了operator\[\] 必须返回char\&amp;（字符引用）。具体原因：引用是变量的别名，返回char\&amp; 意味着返回的是当前对象p指向的内存中，对应下标的字符本身，而非值拷贝——这样，对返回值进行赋值（比如s1\[2\] = \&\#39;A\&\#39;），本质是修改当前对象p指向的内存中的字符，能够真正实现“通过下标修改原字符串”的需求。

若返回char（值返回），则会出现“只能读、不能写”的问题：值返回时，会将p\[idx\]的值拷贝一份，返回的是一个临时的char变量，而非原内存中的字符。此时，对返回值赋值（比如s1\[2\] = \&\#39;A\&\#39;），修改的是临时变量，而非原字符串中的字符，无法实现题目中“修改下标对应字符”的需求，程序虽然不会崩溃，但不符合题目要求和实际使用场景。

面试补充：进阶追问（高频）：若想实现“const对象只能读、不能写”，该如何优化operator\[\]？答案是重载const版本的operator\[\]：char operator\[\]\(int idx\) const; 这样，const MyString对象调用operator\[\] 时，只能返回char值，无法修改；非const对象调用时，返回char\&amp;，支持读写，这是工业级string类的标准实现，也是面试官常追问的进阶考点。

#### 提问 2：operator\&lt;\&lt;（输出运算符）为什么必须重载为友元函数，而不能作为MyString类的成员函数？结合代码说明。

答：这是运算符重载的经典考点，核心是理解“运算符的左操作数类型”，结合cout的使用场景和类的封装特性，具体说明如下：

首先明确cout的本质：cout是ostream类的一个对象，我们使用的“cout \&lt;\&lt; s1”，本质是调用“operator\&lt;\&lt;\(cout, s1\)”，其中左操作数是ostream对象（cout），右操作数是MyString对象（s1）。

若将operator\&lt;\&lt; 作为MyString类的成员函数，根据类成员函数的调用规则，成员函数的第一个参数默认是this指针（指向当前MyString对象），此时operator\&lt;\&lt; 的调用形式会变成“s1\.operator\&lt;\&lt;\(cout\)”，对应的使用方式就是“s1 \&lt;\&lt; cout”，这与我们习惯的“cout \&lt;\&lt; s1”完全相反，不符合C\+\+的语法使用习惯，也无法满足题目中“cout输出MyString对象”的需求。

而将operator\&lt;\&lt; 重载为友元函数，就可以打破类的封装，直接访问MyString类的私有成员p，同时可以自由定义两个参数的类型——我的代码中，友元函数的定义是“friend ostream\&amp; operator\&lt;\&lt; \(ostream\&amp; os, const MyString \&amp; S\)”，第一个参数是ostream\&amp;（接收cout），第二个参数是const MyString\&amp;（接收要输出的MyString对象），调用形式就是“operator\&lt;\&lt;\(cout, s1\)”，对应“cout \&lt;\&lt; s1”，符合使用习惯。同时，返回ostream\&amp; 可以支持链式输出（比如cout \&lt;\&lt; s1 \&lt;\&lt; s2 \&lt;\&lt; s3），因为每次调用都会返回os（cout的引用），可以继续执行下一次\&lt;\&lt; 操作。

面试补充：友元函数的核心作用是“打破封装，访问类的私有成员”，但面试官会追问：友元函数会破坏封装性，有没有替代方案？答案是：可以提供public的成员函数（比如c\_str\(\)），返回p的指针，然后在全局operator\&lt;\&lt; 中调用该函数，避免使用友元，但实际开发中，为了简洁，operator\&lt;\&lt; 通常重载为友元，这也是面试中需要掌握的权衡思路。

#### 提问 3：你的代码中，为什么要提供“const char\* \+ MyString”的全局友元operator\+？成员函数operator\+ 不能实现吗？

答：这道题考察运算符重载的覆盖场景，核心是“左操作数的类型限制”，结合题目中的场景（比如s4 = \&\#34;qrst\-\&\#34; \+ s2），具体说明如下：

成员函数operator\+ 的调用规则是：左操作数必须是当前类的对象，右操作数可以是其他类型（比如MyString、const char\*）。我的代码中，成员函数operator\+ 实现了“MyString \+ MyString”“MyString \+ const char\*”两种场景（比如s1 \+ s3、s1 \+ \&\#34;mnop\&\#34;），但无法实现“const char\* \+ MyString”的场景（比如题目中的\&\#34;qrst\-\&\#34; \+ s2）。

原因很简单：“qrst\-”是const char\* 类型，不是MyString对象，无法调用MyString类的成员函数operator\+，也就是说，左操作数是const char\* 时，无法触发MyString的成员函数operator\+，导致编译错误，无法满足题目中的需求。

因此，必须提供全局友元operator\+，重载“const char\* \+ MyString”的场景：我的代码中，友元函数的定义是“friend MyString operator\+ \(const char\* str, const MyString \&amp; S\)”，实现逻辑是先将const char\* 类型的str转换为MyString临时对象（MyString temp\(str\)），再调用已经实现的成员函数operator\+，让临时对象temp与MyString对象S进行拼接（temp \+ S），最终返回拼接后的新MyString对象，这样就完美实现了“const char\* \+ MyString”的操作场景，满足题目中s4 = \&\#34;qrst\-\&\#34; \+ s2的需求，避免了编译错误。

面试补充：这种实现方式的核心优势是“代码复用”——无需重新编写拼接逻辑，直接复用成员函数operator\+的实现，既减少了代码冗余，又保证了拼接逻辑的一致性，这也是面试官考察的“代码设计能力”。此外，面试官可能会追问：若要支持“const char\* \+ const char\*”的拼接，该如何实现？答案是：再重载一个全局友元operator\+，参数为两个const char\*，内部先将两个字符串拼接为一个新的char\*，再构造MyString对象返回，但题目中未涉及该场景，因此无需额外实现，重点掌握“const char\* \+ MyString”的友元重载即可。

#### 提问 4：operator \(\) 在这里实现什么功能？为什么可以像函数一样调用？

答：\(\) 是C\+\+中的函数调用运算符，该运算符支持被类重载，这也是C\+\+仿函数（functor）的核心基础。结合本题MyString类的实现，operator \(\) 在这里的核心功能是**提取字符串的子串**，调用形式为s\(start, len\)，其中start表示子串的起始下标，len表示子串的长度，最终返回一个包含该子串的MyString临时对象。

之所以MyString对象可以像函数一样调用（如s1\(0,4\)），核心原因是我们重载了函数调用运算符operator \(\)：当我们对MyString对象执行“对象名\(参数列表\)”的操作时，编译器会自动调用我们重载的operator \(\) 成员函数，将参数（start和len）传入函数内部，执行子串提取逻辑，本质是运算符重载带来的语法便捷性。这种设计让对象具备了“函数般的行为”，也是仿函数的入门应用，面试官通过这道题，重点考察对C\+\+运算符重载拓展功能的理解，以及仿函数的基础概念。

### 三、内存与异常安全（大厂最爱）

#### 提问 10：你的赋值运算符是否异常安全？怎么改进？

答：我当前实现的赋值运算符**不具备异常安全性**，这是面试中高频扣分点，结合代码和底层逻辑具体说明如下：

当前赋值运算符（operator=）的核心逻辑是“先释放当前对象的内存（delete\[\] p），再通过new\[\]开辟新内存，最后拷贝内容”，即：

```cpp
MyString & operator= (const MyString & S)
{
    if (this == &S)
        return *this;
    delete []p;  // 先释放当前内存
    p = new char[strlen(S.p) + 1];  // 可能抛异常
    strcpy(p, S.p);
    return *this;
}
```

存在的核心问题：new\[\] 申请内存时，可能因为内存不足等原因抛出异常（bad\_alloc）。而此时，当前对象的p已经被delete\[\]释放，变成了野指针；一旦new\[\]抛异常，后续的strcpy无法执行，当前对象的p将一直是野指针，后续使用该对象（如访问p、析构时delete\[\] p）会导致内存访问错误、程序崩溃，不符合C\+\+异常安全的设计原则。

最优改进方案：采用**copy and swap（拷贝交换）**模式，修改后的赋值运算符实现如下：

```cpp
MyString& operator=(MyString rhs) {  // 注意这里是值传递，自动触发深拷贝
    swap(p, rhs.p);  // 交换当前对象和临时对象的指针
    return *this;
}

```

改进后的异常安全逻辑：

1\.  值传递参数rhs：当调用赋值运算符时，会先调用MyString的拷贝构造函数，将右操作数深拷贝到rhs中——若拷贝过程中（new\[\]）抛异常，当前对象的p未被修改，依然有效，不会出现野指针；

2\.  交换指针：通过swap函数交换当前对象的p和rhs的p，此时当前对象的p指向rhs深拷贝后的内存（即原右操作数的内容），而原当前对象的内存被转移到rhs中；

3\.  自动释放：赋值运算符执行结束后，临时对象rhs生命周期结束，会调用析构函数delete\[\] 其p指向的内存（即原当前对象的内存），无需手动释放，避免内存泄漏。

该方案不仅实现了异常安全，还简化了代码，避免了自赋值判断（值传递的rhs与当前对象不可能是同一个），是工业级C\+\+代码中赋值运算符的标准实现，也是面试官最认可的改进方式。

#### 提问 11：为什么析构要用 delete \[\] 而不是 delete？

答：这道题考察new/delete、new\[\]/delete\[\]的匹配使用规则，核心是“分配与释放的方式必须一致”，结合MyString的实现具体说明如下：

1\.  匹配原则：C\+\+中，new与delete对应、new\[\]与delete\[\]对应，二者不能混用——new用于分配单个对象的内存，delete用于释放单个对象的内存；new\[\]用于分配数组内存（本题中是char数组），delete\[\]用于释放数组内存。

2\.  delete\[\] 的作用：delete\[\] 执行时，会先遍历数组中的每个元素，调用每个元素的析构函数（本题中char是基本数据类型，析构函数无实际操作），然后释放整个数组占用的整块内存，确保内存完全释放，无泄漏。

3\.  误用delete的后果：若用delete替代delete\[\] 释放new\[\]分配的数组内存，会导致“部分内存泄漏”或“未定义行为”。具体来说，delete只会释放数组的第一个元素的内存，剩余数组元素的内存无法被释放，造成内存泄漏；对于自定义类型数组，还会导致除第一个元素外，其他元素的析构函数无法被调用，引发更严重的资源泄漏（如自定义类型中包含其他动态内存），最终可能导致程序崩溃。

结合我的代码，MyString的p是通过new\[\]分配的char数组（如new char\[strlen\(str\)\+1\]），因此析构函数必须用delete\[\] p，确保数组内存完全释放，这也是面试官判断内存管理是否严谨的基础考点。

### 四、与 qsort 结合的考点（非常高频）

#### 提问 12：qsort 为什么需要你写 CompareString 比较函数？

答：核心原因是qsort是C标准库中的通用排序函数，**不了解自定义类（MyString）的比较规则和内存结构**，必须由用户提供比较回调函数，才能完成排序，具体说明如下：

1\.  qsort的通用性：qsort可以排序任意类型的数据（int、char、自定义类等），其底层实现是快速排序，设计时无法预知用户要排序的数据类型，因此无法内置比较逻辑——对于int类型，比较逻辑是“大于、小于、等于”，但对于MyString类，比较逻辑是“字符串内容的字典序比较”，这是qsort无法默认实现的。

2\.  比较回调函数的作用：CompareString函数是用户提供给qsort的“比较规则接口”，qsort在排序过程中，会不断调用该函数，传入两个待比较元素的地址（const void\* e1、const void\* e2），由该函数判断两个元素的大小关系，并返回对应的值：

\- 返回 \&lt; 0：表示第一个元素小于第二个元素，排序时放在前面（升序）；

\- 返回 0：表示两个元素相等，排序时位置不变；

\- 返回 \&gt; 0：表示第一个元素大于第二个元素，排序时放在后面。

3\.  结合本题：我的CompareString函数中，将const void\* 强制转换为MyString\*，然后通过重载的比较运算符（\&lt;、==、\&gt;）判断两个MyString对象的大小，本质是将MyString的比较规则告知qsort，让qsort能够正确排序MyString数组（如本题中的SArray）。

面试补充：面试官可能会追问：C\+\+中的sort函数与qsort有什么区别？答案是：sort是C\+\+ STL中的模板函数，支持函数对象（仿函数）和lambda表达式，无需强制类型转换，类型安全；qsort是C库函数，依赖void\*，无类型安全，效率略低，本题用qsort是为了考察回调函数的使用和自定义比较规则。

#### 提问 13：比较运算符 \&lt; / == / \&gt; 为什么建议加 const？

答：建议给比较运算符添加const修饰，核心原因是**支持const对象的比较，保证函数的常量正确性**，这是面试中高频扣分点，结合本题代码和qsort的使用场景具体说明如下：

1\.  const对象的调用规则：C\+\+中，const修饰的对象（常量对象）只能调用const修饰的成员函数，不能调用非const成员函数——因为非const成员函数可能会修改对象的内容，而const对象的内容是不可修改的，编译器会直接报错。

2\.  结合qsort的场景：本题中，qsort的比较回调函数CompareString中，传入的参数是const void\* e1、const void\* e2，强制转换后是const MyString\* s1、const MyString\* s2（虽然代码中未加const，但逻辑上qsort不会修改待比较元素）。此时，当我们通过\*s1 \&lt; \*s2调用比较运算符时，\*s1和\*s2是const MyString对象，若比较运算符未加const修饰（如我的代码中bool operator\&lt;\(const MyString \&amp; S\)），编译器会报错，因为const对象无法调用非const成员函数。

3\.  正确写法及意义：给比较运算符的成员函数添加const修饰，表明该函数不会修改当前对象的任何成员变量（即不会修改p指向的字符串内容），符合“比较操作不改变对象”的逻辑，同时支持const对象的比较，代码更严谨、通用。正确写法如下：

```cpp
bool operator<(const MyString& s) const { return strcmp(p, s.p) < 0; }
bool operator==(const MyString& s) const { return strcmp(p, s.p) == 0; }
bool operator>(const MyString& s) const { return strcmp(p, s.p) > 0; }

```

我的代码中未给比较运算符加const，属于基础疏漏，是面试官判断代码严谨性的重要扣分点，面试时必须修正。

### 五、经典陷阱题（面试官最爱挖坑）

#### 提问 14：默认构造函数为什么初始化为空串而不是 nullptr？

答：核心原因是**避免野指针，保证字符串操作函数的安全性**，结合本题中用到的字符串操作函数（strlen、strcpy、strcat）和代码实现，具体说明如下：

1\.  字符串操作函数的要求：本题中我们使用的strlen、strcpy、strcat等函数，其参数要求是“以\&\#39;\\0\&\#39;结尾的有效字符串指针”，若传入nullptr（空指针），这些函数会直接访问nullptr指向的内存，触发段错误（程序崩溃），这是C/C\+\+中常见的内存错误。

2\.  我的默认构造函数实现：MyString\(\) \{ p = new char\[1\]; p\[0\] = 0; \}，这里将p初始化为一个长度为1的char数组，且p\[0\] = 0（即\&\#39;\\0\&\#39;），本质是一个空串——这样的初始化方式，确保p是一个有效指针（指向一块合法的堆内存），即使调用strlen\(p\)，也会返回0（空串长度），不会触发段错误，保证了后续所有字符串操作的安全性。

3\.  若初始化为nullptr的后果：若默认构造函数写成MyString\(\) \{ p = nullptr; \}，则p是一个空指针。当创建空的MyString对象（如本题中的s2）时，调用cout \&lt;\&lt; s2会触发operator\&lt;\&lt; 函数中的strlen\(p\)，此时传入的是nullptr，会直接导致程序崩溃，无法满足题目中“输出空对象”的需求。

面试补充：这是面试官考察“空指针规避”和“字符串操作安全性”的经典陷阱题，核心是要理解“空串（p指向\&\#39;\\0\&\#39;）和空指针（p=nullptr）的区别”，空串是有效指针，空指针是无效指针，这也是基础内存管理的重点。

#### 提问 15：如果不写拷贝构造函数，会发生什么？

答：若不写自定义拷贝构造函数，编译器会自动生成**默认浅拷贝构造函数**，导致内存错误和数据错乱，具体问题结合本题场景说明如下：

1\.  默认浅拷贝的行为：编译器生成的默认拷贝构造函数，仅会拷贝对象的成员变量本身，对于MyString类的char\* p，就是拷贝p的地址（即新对象的p与原对象的p指向同一块堆内存），不会重新开辟新内存，也不会拷贝字符串内容。

2\.  引发的两个致命问题：

①  重复释放内存（double free）：当两个对象（如s1和s4，s4通过默认拷贝构造拷贝s1）的生命周期结束时，都会调用析构函数delete\[\] p，而两者指向同一块内存——第一次delete\[\]会释放该内存，第二次delete\[\]会释放已经无效的内存，触发未定义行为，直接导致程序崩溃。结合本题，s4初始是s1的拷贝，若用默认浅拷贝，s4\.p和s1\.p指向同一块内存，当s4 = s3时，delete\[\] s4\.p会释放s1\.p指向的内存，后续s1使用时会出现野指针，程序崩溃。

②  数据相互干扰：一个对象修改字符串内容，会直接影响另一个对象。例如，s1\[2\] = \&\#39;A\&\#39;会修改s1\.p指向的内存中的字符，而s4\.p与s1\.p指向同一块内存，因此s4的字符串也会被意外修改，违背类的封装原则，造成数据错乱。

总结：自定义拷贝构造函数（深拷贝）是MyString类的必备成员，不写会导致程序崩溃，这也是面试官考察“浅拷贝危害”的核心陷阱题。

#### 提问 16：连续赋值 s1 = s2 = s3 为什么能成立？

答：核心原因是**赋值运算符（operator=）的返回值是MyString\&amp;（当前对象的引用）**，支持链式赋值，具体逻辑如下：

1\.  赋值运算符的结合性：C\+\+中，赋值运算符（=）是右结合的，即连续赋值s1 = s2 = s3会被解析为s1 = \(s2 = s3\)。

2\.  引用返回的作用：当执行s2 = s3时，调用operator=函数，将s3的值赋给s2，函数返回s2的引用（\*this）——此时，这个返回值（s2的引用）作为左操作数，继续执行s1 = （s2的引用），将s2的值赋给s1，最终完成连续赋值。

3\.  若返回值是MyString（值返回）的后果：若operator=返回值是MyString（值返回），则s2 = s3会返回一个s2的临时拷贝对象，此时s1 = 临时对象，虽然也能完成赋值，但会多创建一个临时对象，效率降低，且无法支持更复杂的链式操作（如s1 = s2 = s3 = s4）。

结合我的代码，operator=返回MyString\&amp;，因此能够支持连续赋值，这也是赋值运算符重载的标准设计，面试官通过这道题考察对“引用返回作用”和“运算符结合性”的理解。

### 六、你代码里的面试扣分点（一定要会改）

结合我的MyString代码，面试官会重点指出以下4个扣分点，这些都是基础疏漏，面试时必须修正，否则会被判定为“内存管理和代码严谨性不足”：

1\.  比较运算符未加const修饰（最严重扣分点）：我的代码中，operator\&lt;、operator==、operator\&gt; 均未加const，导致const MyString对象无法调用这些函数，结合qsort场景会编译报错，正确写法如下：

```cpp
bool operator<(const MyString & S) const { return strcmp(p, S.p) < 0; }
bool operator==(const MyString & S) const { return strcmp(p, S.p) == 0; }
bool operator>(const MyString & S) const { return strcmp(p, S.p) > 0; }

```

2\.  operator \(\) 未做边界检查：我的代码中，operator \(\) 实现子串提取时，未检查start（起始下标）和len（子串长度）的合法性，若start超出原字符串长度（如原字符串长度为5，start=6），或len过大（如原字符串剩余长度为3，len=10），会导致数组越界访问，触发未定义行为（程序崩溃）。改进方案：添加边界判断，若start越界，返回空串；若len超出剩余长度，取剩余全部字符，示例如下：

```cpp
MyString operator() (int start, int len)
{
    int strLen = strlen(p);
    // 边界检查：start越界，返回空串
    if (start < 0 || start >= strLen) {
        return MyString("");
    }
    // 若len超出剩余长度，取剩余全部字符
    int actualLen = (start + len) > strLen ? (strLen - start) : len;
    char* sub = new char[actualLen + 1];
    for (int i = 0; i < actualLen; ++i)
        sub[i] = p[i + start];
    sub[actualLen] = 0;
    MyString temp(sub);
    delete[] sub;  // 补充：之前遗漏了sub的释放，导致内存泄漏
    return temp;
}

```

3\.  赋值运算符中“if \(p\) delete\[\] p”多余且危险：我的代码中，赋值运算符（operator=）里有“if \(p\) delete\[\] p”，但MyString的所有构造函数（默认构造、拷贝构造、带参构造）都保证了p是有效指针（不会为nullptr），因此if \(p\)判断多余，反而可能因逻辑疏漏（如后续修改构造函数导致p为nullptr）引发问题，直接写delete\[\] p即可。

4\.  赋值运算符未实现异常安全：如提问10所述，当前赋值运算符的实现（先delete\[\] p，再new\[\]）存在异常安全问题，若new\[\]抛异常，会导致p变成野指针，建议改用copy and swap模式改进，提升代码严谨性。

### 七、终极面试压轴题

#### 提问 17：实现一个工业级 string 需要考虑哪些点？

答：工业级string类（如C\+\+ STL中的std::string），需要兼顾安全性、效率、通用性和可扩展性，结合MyString的基础实现，核心需要考虑以下7个关键点，也是面试官考察“高级C\+\+设计能力”的压轴考点：

1\.  基础内存管理：必须实现深拷贝，避免浅拷贝带来的内存错误；同时支持copy and swap模式，保证赋值运算符的异常安全，避免野指针和内存泄漏。

2\.  短字符串优化（SSO）：这是工业级string的核心优化点。对于短字符串（如长度\&lt;=16），直接存储在栈上（而非堆上），避免频繁调用new\[\]/delete\[\]带来的性能开销，提升程序运行效率；对于长字符串，再使用堆内存分配，兼顾效率和灵活性。

3\.  容量（capacity）与预留空间（reserve）：区分“长度（size）”和“容量（capacity）”——size是当前字符串的实际长度，capacity是当前分配的内存可容纳的最大长度。提供reserve\(n\)函数，允许用户提前预留n个字符的内存，减少字符串拼接、扩容时的频繁内存分配（避免多次new\[\]和拷贝），提升效率。

4\.  迭代器支持：实现iterator和const\_iterator迭代器，支持\.begin\(\)、\.end\(\)等接口，适配STL算法（如sort、for\_each），提升类的通用性；同时保证迭代器的有效性（如扩容后迭代器不失效，或明确告知失效场景）。

5\.  线程安全：在多线程环境下，保证string对象的操作（如赋值、拼接、修改）是线程安全的，可通过加锁（如mutex）或原子操作实现，避免多线程并发修改导致的数据错乱。

6\.  不抛异常版本（noexcept）：为常用的、不会抛异常的成员函数（如size\(\)、capacity\(\)、operator\[\]的const版本）添加noexcept修饰，告知编译器该函数不会抛异常，便于编译器优化代码，同时提升程序的稳定性。

7\.  兼容C风格字符串：保证string对象以\&\#39;\\0\&\#39;结尾，提供c\_str\(\)成员函数，返回const char\*指针，兼容C语言中的字符串操作函数（如strlen、strcpy），提升类的兼容性和实用性。

补充：此外，还需考虑拷贝优化（如移动构造、移动赋值，避免深拷贝的开销）、边界检查（所有访问下标、子串提取的操作都需做边界判断）、丰富的成员函数（如insert、erase、find、substr等），满足实际开发中的各种需求。

面试补充：面试官可能会追问“std::string的实现原理”，核心就是以上几点，尤其是SSO优化和异常安全，这是区分初级和高级C\+\+开发者的关键。

额外福利：若需要，我可以将你当前的MyString错误代码（含扣分点），与修正后的满分面试版代码（含所有优化点）对比整理，标注每处修改的原因和面试考点，方便你直接背诵、应对面试。

面试补充：这种实现方式的核心优势是“代码复用”——无需重新编写拼接逻辑，直接复用成员函数operator\+的实现，既减少了代码冗余，又保证了拼接逻辑的一致性，这也是面试官考察的“代码设计能力”。此外，面试官可能会追问：若要支持“const char\* \+ const char\*”的拼接，该如何实现？答案是：再重载一个全局友元operator\+，参数为两个const char\*，内部先将两个字符串拼接为一个新的char\*，再构造MyString对象返回，但题目中未涉及该场景，因此无需额外实现，重点掌握“const char\* \+ MyString”的友元重载即可。

