继承体系中protected属性只能在类内部使用，不能对外使用， 对外使用只有public属性修饰的可以

## 1. 指针与引用的区别

**注意**：本质上，引用底层是一个指针常量`(int* const p)`,只是c++编译器隐藏了处理细节，所以指针的话占用内存大小就由编译环境的的位数决定。如果编译环境为32位指针占字节大小就为4字节，64位的话就为8字节

1. 两者定义和性质不同

   指针是一个变量，存储的是一个地址，指向内存的一个存储单元；

   引用是原变量的一个别名，跟原来的变量实质上是同一个东西。

   ```cpp
   int a = 996;
   int *p = &a; // p是指针, &在此是求地址运算
   int &r = a; // r是引用, &在此起标识作用
   ```

2. 指针可以有多级，引用只有一级

   ```cpp
   int **p; // 合法
   int &&a; // 不合法，这是右值引用
   ```

3. 指针可以在定义的时候不初始化，引用必须在定义的时候初始化

   ```cpp
   int *p; // 合法
   int &r; // 不合法
   int a = 996;
   int &r = a; // 合法
   ```

4. 指针可以指向NULL，引用不可以为NULL

   ```cpp
   int *p = NULL; // 合法
   int &r = NULL; // 不合法
   ```

5. 指针初始化之后可以再改变，引用不可以

   ```cpp
   int a = 996;
   int *p = &a; // 初始化, p 是 a 的地址
   int &r = a; // 初始化, r 是 a 的引用
   
   int b = 885;
   p = &b;	// 合法, p 更改为 b 的地址
   r = b; 	// 不合法, r不可以再变 ; r引用的还是a,只不过是把a的值变为了b的值而已
   ```

6. `sizeof` 的运算结果不同

   ```cpp
   int a = 996;
   int *p = &a;
   int &r = a;
   
   cout << sizeof(p); // 返回 int* 类型的大小
   cout << sizeof(r); // 返回 int 类型的大小
   ```

   `sizeof` 是C/C++ 中的一个操作符（operator），其作用就是返回一个对象或者类型所占的内存字节数。

7. 自增运算意义不同

   如下图所示，p++之后指向a后面的内存，r++相当于a++。

   ```cpp
   int a = 996;
   int *p = &a;
   int &r = a;
   std::cout << p << std::endl;
   std::cout << r << std::endl;
   p++;
   r++;
   std::cout << p << std::endl;
   std::cout << r << std::endl;
   
   /*
   输出：
   0x62f89c
   996
   0x62f8a0
   997
   */
   ```

8. 指针和引用作为函数参数时，指针需要检查是否为空，引用不需要

   注意：指针和引用都可以作为函数参数，改变实参的值

   ```cpp
   void fun(int *p){
       if(p == nullptr){ // 对于指针需要判空
           
       }
   }
   
   void fun(int &a){
       // 对于引用不需要判空
   }
   ```

## 2. 如何理解面向对象

- 面向对象编程，即OOP，是一种编程范式，满足面向对象编程的语言，一般会提供类、封装、继承等语法和概念来辅助我们进行面向对象编程。

- 面向对象是基于万物皆对象这个哲学观点. 所谓的面向对象就是将我们的程序模块化，对象化，把具体事物的特性属性和通过这些属性来实现一些动作的具体方法放到一个类里面。

- 面向对象的三大特征：**继承**，**封装**，**多态**

  - 继承

    - 概念：一个类继承另一个类，则称继承的类为子类，被继承的类为父类。
    - 目的：实现代码的复用。
    - 理解：子类与父类的关系并不是日常生活中的父子关系，子类与父类而是一种特殊化与一般化的关系，是**is-a**的关系，子类是父类更加详细的分类。如 class dog 继承于 animal,就可以理解为dog is a animal.注意设计继承的时候。
    - 结果：继承后子类自动拥有了父类的属性和方法，子类可以写自己特有的属性和方法，目的是实现功能的扩展，子类也可以复写父类的方法即方法的重写。

  - 封装

    - 概念1：封装也称为信息隐藏，是**指利用抽象数据类型将数据和基于数据的操作封装在一起**，使其构成一个不可分割的独立实体，数据被保护在抽象数据类型的内部，尽可能地隐藏内部的细节，只 保留一些对外接口使之与外部发生联系。也就是说，用户无需知道对象内部方法的实现细节，但可以根据对象提供的外部接口(对象名和参数)访问该对象。
    - 概念2：将我们的程序模块化，对象化，把具体事物的特性属性和通过这些属性来实现一些动作的具体方法放到一个类里面；封装的目标就是要实现软件部件的**“高内聚、低耦合”**，防止程序相互依赖性而带来的变动影响。在面向对象的编程语言中，对象是封装的最基本单位。面向对象的封装就是把描述一个对象的属性和行为的代码封装在一个“模块”中，也就是一个类中，属性用变量定义，行为用方法进行定义，方法可以直接访问同一个对象中的属性。
    - 好处：**将各个独立功能设计成一个个独立的单元，形成一个有规划设计的整体，减小耦合，提高内聚。**

  - 多态

    - 概念：相同的事物，调用其相同的方法，参数也相同时，但表现的行为却不同。

    - 理解：子类以父类的身份出现，但做事情时还是以自己的方法实现。子类以父类的身份出现需要**向上转型(up-cast)**，其中向上转型是由编译器自动实现的， 是安全的，但**向下转型(down-cast)**是不安全的，需要强制转换。**子类以父类的身份出现时自己特有的属性和方法将不能使用**（见如下例子）。

      > 对于普通函数，当发生多态性行为时，调用跟这个等号左边的类型走，虚函数当子类没有时调用父类，子类有时，调用子类。
      >
      > 普通函数如果子类没有，父类有，那么子类对象指针调用时会调用父类的函数。可以在子类中使用using使父类的普通函数在子类可见，从而形成重载。
      
      ```cpp
      #include <bits/stdc++.h>
      using namespace std;
      
      class A{
          public:
              int age;
          
              A(int a):age(a){}
              virtual ~A(){}
              void print(){
                  cout << "age: " << age << endl;
              }
      };
      
      
      class B : public A{
          public:
              int name;
          
          B(int a, int b):name(a), A(b){}
          ~B(){}
      
          void print(){
              cout << "name: " << name << endl;
          }
      
          void gre(){
              std::cout << __FUNCTION__ << endl;
          }
      };
      
      int main(){
          A *pa = new B(12, 23);
      	pa->print(); // age: 23,,如果print为虚函数，结果为name: 12
          return 0;
  }
      ```
      
      父类有print方法，子类对其进行重写，而此时子类又有自己独有的方法`gre`，当使用该语句`A *pa = new B(12, 23);`时pa向上转型为父类对象，所以可见性为父类的一些方法，自己特有的方法无法看到，pa无法调用`gre`方法。上述print调用的是父类的函数实现。
      
      对于虚函数，如果上面的print函数声明为虚函数，那么调用的子类的print实现。

- 五大基本原则

  - 单一职责原则

    一个类应该有且只有一个去改变它的理由，这意味着**一个类应该只有一项工作**。

  - 开放封闭原则

    对象或实体应该对**扩展**开放，对**修改**封闭。

    > 更改封闭即是在我们对模块进行扩展时，勿需对源有程序代码和DLL进行修改或重新编译文件！这个原则对我们在设计类的时候很有帮助，坚持这个原则就必须尽量考虑接口封装，抽象机制和多态技术！

  - 里氏替换原则

    **任何基类可以出现的地方，子类一定可以出现**

  - 依赖倒置原则

    高层次的模块不应该依赖于低层次的模块，他们都应该依赖于抽象。具体实现应该依赖于抽象，而不是抽象依赖于实现。

  - 接口隔离原则

    1. 客户端不应该依赖它不需要的接口。
    2. 类间的依赖关系应该建立在最小的接口上。

- 总结

  > 封装是为了模块化（重用）；
  > 继承是为了代码扩展和重用；
  > 多态是为了接口重用。

## 3. 如何理解面向过程

面向过程注重实现过程，讲究一步一步实现。

> 面向过程和面向对象优缺点比较：
>
> 面向过程：
>
> - 优点：性能比面向对象高，因为类调用时需要实例化，开销比较大，比较消耗资源，比如单片机、嵌入式开发、Linux/Unix等一般采用面向过程开发，性能是最重要的因素。
> - 缺点：没有面向对象易维护、易复用、易扩展
>
> 面向对象：
>
> - 优点：易维护、易复用、易扩展，由于面向对象有封装、继承、多态性的特性，可以设计出低耦合的系统，使系统更加灵活、更加易于维护
> - 缺点：性能比面向过程低

## 4. C和C++的区别

### 4.1 C是面向过程语言，C++是面向对象语言

1. C是一个结构化语言，它的重点在于算法和数据结构。C程序的设计首要考虑的是如何通过一个过程，对输入（或环境条件）进行运算处理得到输出（或实现过程（事务）控制）。
2. C++是面向对象语言，首要考虑的是如何构造一个对象模型，让这个模型能够契合与之对应的问题域，这样就可以通过获取对象的状态信息得到输出或实现过程（事务）控制。 所以C与C++的最大区别在于它们的用于解决问题的思想方法不一样。

### 4.2 具体语言区别

1. 关键字数量

   C语言有32个关键字，C++语言有63个关键字

2. 返回值

   C语言中，如果一个函数没有指定返回值类型，默认返回int类型；C++中，如果一个函数没有返回值则必须指定为void。

   > c语言

   <img src="C:\Users\whello\Desktop\王小宁\cpp语言\c语言默认函数返回值类型.jpeg" alt="c语言默认函数返回值类型" style="zoom:50%;" />

   > c++语言

   默认返回值类型，也是int；上面话的意思是如果没有返回值，就得指定为void。

   <img src="C:\Users\whello\Desktop\王小宁\cpp语言\cpp语言默认函数返回值类型.jpeg" alt="cpp语言默认函数返回值类型" style="zoom:50%;" />

3. 参数列表

   在C语言中，函数没有指定参数列表时，默认可以接收任意多个参数；但在C++中，因为严格的参数类型检测，没有参数列表的函数，默认为 void，不接收任何参数。

   > c++语言

   <img src="C:\Users\whello\Desktop\王小宁\cpp语言\cpp语言函数参数.png" alt="cpp语言函数参数" style="zoom:50%;" />

   > c语言

   <img src="C:\Users\whello\Desktop\王小宁\cpp语言\c语言函数参数.png" alt="c语言函数参数" style="zoom:50%;" />

4. 缺省参数

   缺省参数是声明或定义函数时为函数的参数指定一个默认值。在调用该函数时，如果没有指定实参则采用该默认值，否则使用指定的参。（C语言不支持缺省参数）

   **注意：**

   · 在半缺省的情况下，带缺省值的参数必须放在参数列表的最后面。

   · 缺省参数不能同时在函数的声明和函数定义中出现，二者只能选其一。

   · 缺省值必须是常量或者全局变量。

   · 缺省参数必须通过值参或常参传递。

5. 函数重载

   > 原理参考：[关于extern “C“](https://blog.csdn.net/A315776/article/details/124051339)

   函数重载：函数重载是函数的一种特殊情况，指在同一作用域中，声明几个功能类似的同名函数，这些同名函数的形参列表（参数个数、类型、顺序）必须不同，返回值类型可以相同也可以不同，常用来处理实现功能类似数据类型不同的问题。（C语言没有函数重载，C++支持函数重载）。

   C语言中产生函数符号的规则是根据名称产生，这也就注定了c语言不存在函数重载的概念。而C++生成**函数符号**则考虑了函数名、参数个数、参数类型及顺序。需要注意的是函数的返回值并不能作为函数重载的依据，也就是说`int sum()`和`double sum()`这两个函数是不能构成重载的！

   我们的函数重载也属于多态的一种，这就是所谓的**静态多态**。

   **静态多态**：函数重载，函数模板

   **动态多态**（运行时的多态）：继承中的多态（虚函数）。

   > **注意**：使用重载的时候需要注意作用域问题：请看如下代码。

   ![cpp函数重载1](cpp函数重载1.png)

   在全局作用域定义了两个函数，它们由于参数类型不同可以构成重载，此时main函数中调用则可以正确的调用到各自的函数。

   但是请看main函数中被注释掉的一句代码。如果将它放出来，编译器调试`fun(1.2,1.2)`使用的函数声明为`fun(int,int)`,如下图

   ![cpp函数重载2](cpp函数重载2.png)

   这就意味着我们编译器针对下面两句调用都调用了参数类型int的fun。由此可见，**编译器调用函数时优先在局部作用域搜索，若搜索成功则全部按照该函数的标准调用。若未搜索到才在全局作用域进行搜索。**

   > **总结**：C语言不存在函数重载，C++根据函数名参数个数参数类型判断重载，属于静多态，必须同一作用域下才叫重载。

6. const

   C语言中被const修饰的变量不是常量，叫做常变量或者只读变量，这个常变量是无法当作数组下标的。然而在C++中const修饰的变量可以当作数组下标使用，成为了真正的常量，这就是C++对const的扩展。

   C语言中的const：被修饰后不能做左值，可以不初始化，但是之后没有机会再初始化。不可以当数组的下标，可以通过指针修改。

   简单来说，它和普通变量的区别只是不能做左值而已，其他地方都是一样的。

   C++中的const：真正的常量。定义的时候必须初始化，可以用作数组的下标。**const在C++中的编译规则是替换（和宏很像）**，所以它被看作是真正的常量。也可以通过指针修改。需要注意的是，C++的指针有可能退化成C语言的指针。比如以下情况：

   <img src="const属性变量例子.png" alt="const属性变量例子" style="zoom: 67%;" />

   这时候的a就只是一个普通的C语言的const常变量了，已经无法当数组的下标了。（引用了一个编译阶段不确定的值）

   const在生成符号时，是local符号。即在本文件中才可见。如果非要在别的文件中使用它的话，在文件头部声明：`extern const int data = 10`；这样生成的符号就是global符号。

   > **总结**：C中的const叫只读变量，只是无法做左值的变量；C++中的const是真正的常量，但也有可能退化成c语言的常量，默认生成local符号。

   验证const可通过指针修改的情况。参考链接[const变量通过指针修改 详解](https://blog.csdn.net/hyqsong/article/details/50867456)

7. 引用

   > `g++ -S -fverbose-asm file.cpp`

   <img src="C:\Users\whello\Desktop\王小宁\cpp语言\引用与指针的区别1.png" alt="引用与指针的区别1" style="zoom:67%;" />

   由汇编代码可以看出，引用和指针没有区别，引用相当于`typename * const p`

   可以看到底层实现完全一致，取a的地址放入`eax`寄存器，再将`eax`中的值存入引用b/指针p的内存中。

   我们看到对a的值的修改，指针p的做法是*p = 20；即进行解引用后替换值。

   再来看看引用修改：

   我们看到修改a的值的方法也是一样的，也是解引用。只是我们在调用的时候有所不同：调用p时需要*p解引用，b则直接使用就可以。由此我们推断出：**引用在直接使用时是指针解引用。p直接使用则是它自己的地址。**

   这样，**我们给引用开辟的这块内存是根本访问不到的。如果直接用就直接解引用了。即使打印&b，输出的也是a的地址。**

   > 创建数组的引用

   ```cpp
   	int arr[10] = {1,2,3,4,5,6,7,8,9,10};
       int (*pa)[10] = &arr; // 
       cout << pa << " " << arr << " " << &arr << endl; // 三个值一样
       cout << (*pa)[1] << endl; // 2
       cout << *(*pa + 1) << endl; // 2 
       int (&b)[10] = arr;
       cout << b[1] << endl; // 2
   ```

8. malloc/free和new/delete

   malloc()和free()是C语言中动态申请内存和释放内存的标准库中的函数。而new和delete是C++运算符、关键字。new和delete底层其实还是调用了malloc和free。它们之间的区别有以下几个方面：

   - malloc和free是函数，new和delete是运算符。

   - malloc在分配内存前需要大小，new不需要。malloc分配内存需要进行初始化

     ```cpp
     int *p1 = (int *)malloc(sizeof(int));  // 注意sizeof表示的对象占内存的多少 对于arr[]数组，sizeof（arr）是数组所占内存的大小
     
     int *p2 = new int;     //int *p3 = new int(10);
     ```

     malloc时需要指定大小，还需要类型转换。new时不需要指定大小因为它可以从给出的类型判断，并且还可以同时赋初始值。

   - malloc不安全，需要手动类型转换，new不需要类型转换。

   - free只释放空间，delete先调用析构函数再释放空间（如果需要）。

   - new是先配置内存，然后调用构造函数【侯捷 STL源码剖析】

   - malloc失败返回0，new失败抛出bad_alloc异常。

   - malloc开辟在堆区，new开辟在自由存储区域（比如placement new）。

9. 作用域

   C语言中作用域只有两个：局部，全局。C++中则是有：局部作用域，类作用域，名字空间作用域三种。

   >所谓名字空间就是namespace，我们定义一个名字空间就是定义一个新作用域。访问时需要以如下方式访问（以`std`为例）
   >
   >`std::cin<<"123" <<std::endl;`

   例如我们有一个名字空间叫name，其中有一个变量叫做data。如果我们希望在其他地方使用data的话，需要在文件头声明：`using name::data`；这样一来data就使用的是name中的值了。可是这样每个符号我们都得声明岂不是很麻烦？

   我们只要`using namespace name`；就可以将其中所有符号导入了。这样就能理解`using namespace std`了。

## 5. C++三大特性和什么是多态

[2. 如何理解面向对象](#2. 如何理解面向对象)

## 6. 多态的底层实现原理

多态分为静态多态和动态多态，静态多态（编译器就已经确定）是通过函数重载和模板来实现的，动态多态（运行时决定）是通过虚函数来实现的

> 在C++中动态绑定是通过虚函数实现的，是多态实现的具体形式。而虚函数是通过**虚函数表**实现的。这个表中记录了虚函数的地址，解决继承、覆盖的问题，保证动态绑定时能够根据对象的实际类型调用正确的函数。这个虚函数表在什么地方呢？C++标准规格说明书中说到，编译器必须要保证虚函数表的指针存在于对象实例中最前面的位置（这是为了保证正确取到虚函数的偏移量）。也就是说，我们可以通过对象实例的地址得到这张虚函数表，然后可以遍历其中的函数指针，并调用相应的函数。

## 7. 虚函数原理（什么是虚函数，虚函数指针、虚函数表,虚函数表存放的内容,虚表指针存放的位置） 

### 7.1 什么是虚函数

在C++中是使用`virtual`关键字修饰的函数就是虚函数，虚函数又分纯虚函数和普通虚函数，纯虚函数语句如下中注释1

```cpp
class A{ // 接口
  	virtual void v_fun1() = 0;  // 1
    virtual void v_fun2(){ 		// 2
        // TODO
    }
};
class B : public A{ // 未实现v_fun1纯虚函数，此类也是接口，无法实例化
    
}
```

没有实现，并且后面有`=0`修饰，一个类如果存在至少一个这种纯虚函数，那么这个类我们也称接口。这个纯虚函数在子类中必须得到实现。

注释2普通虚函数，一般都会提供默认动作。如果没有父类或子类的实例化对象则可以不实现，否则，有实例化对象却没有实现，在链接时会报错。测试如下：

```cpp
class A {
    public:
        virtual void v_a();
};

class B : public A{
    public:
        virtual void v_a() override {}
};
int main(){
    A a;
    B b;
    return 0; 
}
```

上面程序没有出现A（父类）或B（子类）的实例化对象，所以父类的虚函数没有实现，编译没有任何问题。但是，当把其中随便一个注释去掉后，会出现连接错误。错误如下（属于链接错误）：

```shell
test.cpp:(.text+0x18)：对‘vtable for A’未定义的引用
/tmp/cc18yAiA.o:(.rodata._ZTI1B[_ZTI1B]+0x10)：对‘typeinfo for A’未定义的引用
collect2: error: ld returned 1 exit status
```

注意：当该类不会实例化时，不提供动作没有问题，但如果该类会实例化，则必须提供动作，否则编译报错】。

如果某个类可能会被当作父类，那么这个类的析构函数应该为虚函数，使用`virtual`修饰。否则会出现问题。如下：

```cpp
#include <stdio.h>
class A {
    public:
    A(){
        printf("A constructor!\n");
    }
    ~A(){
        printf("A destructor!\n");
    }
    
};

class B : public A{
    public:
    B(){
        printf("B constructor!\n");
    }
    ~B(){
        printf("B destructor!\n");
    }
};
// __attribute__((unused)) int a = 10; 用于避免如果a变量没有用到会报警告。
int main(){
    A *pa = new B;
    delete pa;
    return 0; 
}
/*
	A constructor!
    B constructor!
	A destructor!
*/
```

由执行结果可以看出，如果基类的析构函数不是虚函数，那么出现多态行为的时候（父类指针指向子类对象，向下转型），**子类的析构函数得不到调用，造成内存泄露**。如果修改A的析构函数为`virtual ~A(){printf("A destructor!\n");}`,执行结果如下：

```shell
A constructor!
B constructor!
B destructor!
A destructor!
```

可以看到析构函数都得到了调用。

>总结：
>
>1. 父类的非纯虚函数一般提供默认实现。但是如果保证程序中不会出现该类的实例化对象，则可以不实现，否则会出现链接错误。
>2. 父类中定义了纯虚函数，那么子类继承后没有实现父类的纯虚函数，那么该子类依旧是接口，不能进行实例化。
>3. 如果一个类有可能被继承，那么该类的析构函数应该设为虚函数（父类析构函数被virtual修饰时，虚表中会有两个本对象的虚析构函数指针）。
>4. 虚函数只有在父类指针或引用指向子类对象时发生动态绑定行为，也叫多态。

### 7.2 多态实现原理

C++中虚函数的实现一般是通过虚函数表实现的（C++规范中没有规定具体用哪种方法，但大部分的编译器厂商都选择此方法）。类的虚函数表是一块连续的内存，每个内存单元中记录一个JMP指令的地址。编译器会为每个有虚函数的类创建一个虚函数表，该虚函数表将被该类的所有对象共享。 类的每个虚成员占据虚函数表中的一行。如果类中有N个虚函数，那么其虚函数表将有(N+2)*4字节的大小。

> N是虚函数个数，虚函数表表首还有两个指针变量，第一个为空指针，第二个为类型信息type_info，4表示的是指针变量所占内存，与操作系统位数有关

如下实例:

> 单继承

```cpp
class A {
    public:
        virtual void v_fun1(){}
};

class B : public A{
    public:
        void v_fun1() override{}

        virtual void v_fun2(){}
};
```

使用编译命令生成虚函数表关系`g++ -fdump-lang-class .cpp`产生的文件内容如下：

```txt
Vtable for A 						# 虚函数表
A::_ZTV1A: 3 entries 				# A::_ZTV1A表示虚函数表明，可以看成一个 typedef int (*pt_name)(...); pt_name _ZTV1A[];
0     (int (*)(...))0 				# 空指针（此处保存某个偏移信息），可看出这里的指针变量的大小为8字节，所以上面所提到的公式为(N+2)*8字节
8     (int (*)(...))(& _ZTI1A) 		# type_info指针，包含一些额外信息，后续有说明
16    (int (*)(...))A::v_fun1 		# 自己的虚函数指针

Class A
   size=8 align=8 					# 表示类对象的大小信息
   base size=8 base align=8
A (0x0x7fb62f297420) 0 nearly-empty
    vptr=((& A::_ZTV1A) + 16) 		# 虚函数表指针，编译器处理后我们看到的虚函数表是从我们定义的虚函数指针开始，其实编译器做了偏移。

Vtable for B
B::_ZTV1B: 4 entries
0     (int (*)(...))0
8     (int (*)(...))(& _ZTI1B)
16    (int (*)(...))B::v_fun1		# 重写父类的虚函数指针
24    (int (*)(...))B::v_fun2		# B类自己的虚函数指针

Class B
   size=8 align=8					# 表示类对象的大小信息
   base size=8 base align=8
B (0x0x7fb62f1431a0) 0 nearly-empty # 因为两个类我都没有写数据成员，所以有个空基类优化的东西，如果有数据成员，此处就不一样了,0表示数据成员相对于首地址的偏移
    vptr=((& B::_ZTV1B) + 16) 		# 虚函数表指针，编译器处理后我们看到的虚函数表是从我们定义的虚函数指针开始，其实编译器做了偏移。
  A (0x0x7fb62f297540) 0 nearly-empty
      primary-for B (0x0x7fb62f1431a0)

```

> 多继承（非虚多继承）

```cpp
class A {
    public:
        virtual void v_fun1(){}

        virtual void v_fun_(){}
};

class B{
    public:
        virtual void v_fun2(){}

        virtual void v_fun_er(){}
};

class C : public A, public B{
    void v_fun2() override{}
    void v_fun1() override{}

    virtual void v_fun3(){}
};
int main(){
    return 0; 
}
```

生成的类层次关系数据文件如下,有的不做介绍，参考上面的：

```txt
Vtable for A
A::_ZTV1A: 4 entries
0     (int (*)(...))0
8     (int (*)(...))(& _ZTI1A)
16    (int (*)(...))A::v_fun1 		# A类有两个虚函数
24    (int (*)(...))A::v_fun_

Class A
   size=8 align=8
   base size=8 base align=8
A (0x0x7f1803ea5420) 0 nearly-empty
    vptr=((& A::_ZTV1A) + 16)

Vtable for B
B::_ZTV1B: 4 entries
0     (int (*)(...))0
8     (int (*)(...))(& _ZTI1B)
16    (int (*)(...))B::v_fun2 		# B类有两个虚函数
24    (int (*)(...))B::v_fun_er

Class B
   size=8 align=8
   base size=8 base align=8
B (0x0x7f1803ea5540) 0 nearly-empty
    vptr=((& B::_ZTV1B) + 16)

Vtable for C
C::_ZTV1C: 10 entries 				# 此时C的虚函数表大小还是那个公式计算得到的吗？当然不是
0     (int (*)(...))0
8     (int (*)(...))(& _ZTI1C)
16    (int (*)(...))C::v_fun1 		# C类重写了A的v_fun1
24    (int (*)(...))A::v_fun_ 		# A的v_fun_，因为没有重写，所以此处会造成C对象调用v_fun_时表现得是A中定义得行为
32    (int (*)(...))C::v_fun2 		# C类重写B的v_fun2
40    (int (*)(...))C::v_fun3 		# C类自己的v_fun3
48    (int (*)(...))-8				# 这个应该是某个位置的偏移
56    (int (*)(...))(& _ZTI1C)
64    (int (*)(...))C::_ZThn8_N1C6v_fun2Ev
72    (int (*)(...))B::v_fun_er		# B的虚函数，未被重写

Class C
   size=16 align=8
   base size=16 base align=8
C (0x0x7f1803eb6000) 0
    vptr=((& C::_ZTV1C) + 16)
  A (0x0x7f1803ea5660) 0 nearly-empty
      primary-for C (0x0x7f1803eb6000)
  B (0x0x7f1803ea56c0) 8 nearly-empty # 此处的数值8表示该指针该位置到基地址也就是C对象的地址的偏移，因为其之前保存了一个vptr，大小为8，int为4字节
      vptr=((& C::_ZTV1C) + 64)
```

> 所以可以看到，多继承时会有多个虚函数表指针，但其实底层只有一个虚函数表。

> 包含数据的多继承

```cpp
class A {
    int a;
    public:
        virtual void v_fun1(){}

        virtual void v_fun_(){}
};

class B{
    int b;
    public:
        virtual void v_fun2(){}

        virtual void v_fun_er(){}
};

class C : public A, public B{
    int c;
    void v_fun2() override{}
    void v_fun1() override{}

    virtual void v_fun3(){}
};
int main(){
    return 0;
}
```

```txt
Vtable for A
A::_ZTV1A: 4 entries
0     (int (*)(...))0
8     (int (*)(...))(& _ZTI1A)
16    (int (*)(...))A::v_fun1
24    (int (*)(...))A::v_fun_

Class A
   size=16 align=8
   base size=12 base align=8
A (0x0x7f16cecc6420) 0
    vptr=((& A::_ZTV1A) + 16)

Vtable for B
B::_ZTV1B: 4 entries
0     (int (*)(...))0
8     (int (*)(...))(& _ZTI1B)
16    (int (*)(...))B::v_fun2
24    (int (*)(...))B::v_fun_er

Class B
   size=16 align=8
   base size=12 base align=8
B (0x0x7f16cecc6540) 0
    vptr=((& B::_ZTV1B) + 16)

Vtable for C
C::_ZTV1C: 10 entries
0     (int (*)(...))0
8     (int (*)(...))(& _ZTI1C)
16    (int (*)(...))C::v_fun1
24    (int (*)(...))A::v_fun_
32    (int (*)(...))C::v_fun2
40    (int (*)(...))C::v_fun3
48    (int (*)(...))-16
56    (int (*)(...))(& _ZTI1C)
64    (int (*)(...))C::_ZThn16_N1C6v_fun2Ev
72    (int (*)(...))B::v_fun_er

Class C
   size=32 align=8
   base size=32 base align=8
C (0x0x7f16cecd8000) 0
    vptr=((& C::_ZTV1C) + 16)
  A (0x0x7f16cecc6660) 0 				# 偏移0
      primary-for C (0x0x7f16cecd8000)
  B (0x0x7f16cecc66c0) 16 				# 偏移16 有没有发现这里的数值和虚函数表中的数值有对应关系
      vptr=((& C::_ZTV1C) + 64) 		# 这个多继承中B的虚函数表为什么定在64偏移不懂，因为64偏移的内容是C::_ZThn16_N1C6v_fun2Ev,实际定义的虚函数在72偏移处
```

内容不做过多解释，来看数据存储，g++默认将虚指针放置在内存对象的开始。

所以此时C对象的数据存储模型应该为如下：

<img src="C:\Users\whello\Desktop\王小宁\cpp语言\多继承内存布局.png" alt="多继承内存布局" style="zoom: 67%;" />

虚函数（virtual）是通过虚函数表来实现的，**在这个表中，主要是一个类的虚函数的地址表**，这张表解决了继承、覆盖的问题，保证其真实反映实际的函数。这样，在有虚函数的类的实例中分配了指向这个表的指针的内存（位于对象实例的最前面），所以，当用父类的指针来操作一个子类的时候，这张虚函数表就显得尤为重要，指明了实际所应调用的函数。



当然程序中也是可以去直接访问的,参见如下代码。

```cpp
#include <iostream>
using namespace std;

class Base {
 public:
 	virtual void fun1(){
 	 	cout << "Base::fun1()" << endl;
 	}
 	virtual void fun2(){
 		cout << "Base::fun2()" << endl;
 	}
 	virtual void fun3(){
 		cout << "Base::fun3()" << endl;
 	}
};

class Base_new {
	public:
		virtual void fun_new1(){
			cout << "Base_new::fun_new1()" << endl;
		}
		virtual void fun_new2(){
			cout << "Base_new::fun_new2()" << endl;
		}
		virtual void fun_new3(){
			cout << "Base_new::fun_new4()" << endl;
		}
};

class A : public Base{
	public:
		virtual void fun1() override{
			cout << "A::fun1()" << endl;
		}
		
		virtual void fun4(){
			cout << "A::fun4()" << endl;
		}
};

class B : public Base, public Base_new{ // 继承顺序是先Base,再Base_new
	public:
		virtual void fun1() override{
			cout << "B::fun1()" << endl;
		}
		virtual void fun_new2() override{
			cout << "B::fun_new2()" << endl;
		}
		virtual void func_B(){
			cout << "B::func_B()" << endl;
		}
};

int main(){
	//基类指针或引用指向子类对象时，调用子类的方法
	Base *base = new A;
	base->fun1(); // 结果 A::fun1()
	A a;
	Base &r_A = a;
	r_A.fun1();  // 结果 A::fun1()
	
	// 单继承继承体系中的内存模型，测试虚函数
	A aa;
	long *pvptr = (long*)(&aa); // 首先拿到内存模型中第一个变量指针,该变量的内容为虚函数表的起始地址
	long *vptr = (long*)(*pvptr); // 将内容取出，转换为指针，该指针就指向虚函数表起始位置
	typedef void (*pfunc)();// using pfunc = void(*)();
	for(unsigned i = 0; vptr + i /*或vptr[i]*/; ++i){
		((pfunc)vptr[i])();
	}
	/*
		结果：
			A::fun1()
			Base::fun2()
			Base::fun3()
			A::fun4()
	*/
	
	// 多继承内存模型
	B bb;
	long *pvptr1 = (long*)(&bb); 
	long *pvptr2 = (long*)(pvptr1 + 1);
	long *vptr1 = (long*)(*pvptr1); 
	long *vptr2 = (long*)(*pvptr2); 
#if 0
	for(unsigned i = 0; vptr1 + i /*或vptr[i]*/; ++i){
		((pfunc)vptr1[i])();
	}
#endif
	for(unsigned i = 0; vptr2 + i /*或vptr[i]*/; ++i){
		((pfunc)vptr2[i])();
	}
	/*
	vptr1:
		B::fun1()
		Base::fun2()
		Base::fun3()
		B::fun_new2()
		B::func_B()
	vptr2://打印vptr2时，注释掉vptr1的for循环，我的测试环境为ubuntu，会出现段错误，有些地方（虚函数表最后两个位置）编译器不允许访问
		Base_new::fun_new1()
		B::fun_new2()
		Base_new::fun_new4()
	可以看出与继承顺序有关，子类的虚函数在第一个虚函数表中（这句话有问题，多继承有多个虚函数表吗？答案是，依旧只有一张）
	从上面可以看出vptr1指向的表中有5个函数指针，那如果使用第一个指针访问第6个呢？其实，只有一张虚函数表，这两张表是连在一起的，每张表的最后有两个信息指针，第一个为空指针，第二个为type_info;可以确定的是多继承有多个虚函数表指针。
	*/
	//验证
	printf("%lu, %lu %d\n", vptr1, vptr2, vptr2 - vptr1); // 单位为字节 4197544 4197600 7
	// vptr1中函数指针有5个，每个占8字节，4197544 + 5 * 8 = 4197584
	// 4197600 - 4197584 = 16 = 2 * 8 ,最后那个7表示vptr1除去5个虚函数指针外最后还有两个指针空间，有关资料说第一个是空，第二个是type_info。
	return 0;
}
```

## 8. 虚继承原理（虚基类指针和虚基类表）

当出现多继承的钻石继承时，共同继承的类中的变量会冲突（有重复），存在二义性问题，如下例子。

```cpp
class A {
    int a;
};

class B : public A{
    int b;
};


class C : public A{
    int c;
};

class D : public B, public C{
    int d;
};
int main(){
    return 0; 
}
```

```txt
Class A
   size=4 align=4
   base size=4 base align=4
A (0x0x7efcb2819420) 0

Class B
   size=8 align=4
   base size=8 base align=4
B (0x0x7efcb26c51a0) 0
  A (0x0x7efcb2819480) 0

Class C
   size=8 align=4
   base size=8 base align=4
C (0x0x7efcb26c5208) 0
  A (0x0x7efcb28194e0) 0

Class D
   size=20 align=4 # 里面有5个int变量，所以A中变量a存在两个备份
   base size=20 base align=4
D (0x0x7efcb2828000) 0
  B (0x0x7efcb26c5270) 0
    A (0x0x7efcb2819540) 0
  C (0x0x7efcb26c52d8) 8
    A (0x0x7efcb28195a0) 8
```

> 上面的非虚多继承会出现两个最终基类的成员变量有二义性问题，所以引入了虚继承
>
> 再看虚继承的情况

```cpp
class A { // 4
    public:
    int a;
};

class B : virtual public A{ // 16
    int b;
};


class C : virtual public A{ // 16
    int c;
};

class D : public B, public C{ // 40
    int d;
};
int main(){
    return 0; 
}
```

```txt
Class A 									# 基类A对象
   size=4 align=4
   base size=4 base align=4
A (0x0x7f5b3eb60420) 0

Vtable for B 								# B的虚函数表，测试虚继承时，成员中一律皆没有虚函数
B::_ZTV1B: 3 entries
0     12
8     (int (*)(...))0
16    (int (*)(...))(& _ZTI1B) 				# 和VTT表中的字段对应

VTT for B
B::_ZTT1B: 1 entries
0     ((& B::_ZTV1B) + 24) 					# 自己的虚函数开始地址，因为没有虚函数，所以会偏移到24，没有地址

Class B  									# B的大小为16，内存分布为vptr:8Byte, A::a,B::b各占4字节
   size=16 align=8
   base size=12 base align=8
B (0x0x7f5b3ea0c1a0) 0
    vptridx=0 vptr=((& B::_ZTV1B) + 24)  	# vptridx表示在虚函数表中的偏移，vptr虚函数表指针
  A (0x0x7f5b3eb60480) 12 virtual
      vbaseoffset=-24 						# this指针相对于基类的偏移，用于共享基类

Vtable for C
C::_ZTV1C: 3 entries
0     12 									# 某种偏移，目前不清楚，有的资料说0位置是vbase_offset,8位置是top_offset
8     (int (*)(...))0 						# 某种偏移
16    (int (*)(...))(& _ZTI1C)

VTT for C 									# C类含义与B类相似，因为对A的继承关系一致
C::_ZTT1C: 1 entries
0     ((& C::_ZTV1C) + 24)

Class C
   size=16 align=8
   base size=12 base align=8
C (0x0x7f5b3ea0c208) 0
    vptridx=0 vptr=((& C::_ZTV1C) + 24)
  A (0x0x7f5b3eb604e0) 12 virtual
      vbaseoffset=-24

Vtable for D 												# D类的虚函数表
D::_ZTV1D: 6 entries
0     32													
8     (int (*)(...))0
16    (int (*)(...))(& _ZTI1D)  							# 对应于VTT表地址
24    16													# 推断这个应该是VTT中的偏移
32    (int (*)(...))-16
40    (int (*)(...))(& _ZTI1D) 								# VTT表地址

Construction vtable for B (0x0x7f5b3ea0c270 instance) in D  # 在D中给B构造一个虚函数表
D::_ZTC1D0_1B: 3 entries
0     32
8     (int (*)(...))0
16    (int (*)(...))(& _ZTI1B)

Construction vtable for C (0x0x7f5b3ea0c2d8 instance) in D	# 在D中给B构造一个虚函数表
D::_ZTC1D16_1C: 3 entries
0     16
8     (int (*)(...))0
16    (int (*)(...))(& _ZTI1C)

VTT for D 													# VTT表保存该类的所有虚函数表地址
D::_ZTT1D: 4 entries
0     ((& D::_ZTV1D) + 24)
8     ((& D::_ZTC1D0_1B) + 24)
16    ((& D::_ZTC1D16_1C) + 24)
24    ((& D::_ZTV1D) + 48)

Class D # D类size=40，分为2*vptr = 2 * 8 = 16, a,b,c,d其中B中的变量存在对齐，因为多继承B在前，size为(pad|b)+4+4+(pad|a) = 24; 所以总size=40,最后的那个对齐是基类的数据成员
   size=40 align=8
   base size=32 base align=8
D (0x0x7f5b3eb70000) 0
    vptridx=0 vptr=((& D::_ZTV1D) + 24)
  B (0x0x7f5b3ea0c270) 0
      primary-for D (0x0x7f5b3eb70000)
      subvttidx=8
    A (0x0x7f5b3eb60540) 32 virtual
        vbaseoffset=-24
  C (0x0x7f5b3ea0c2d8) 16
      subvttidx=16 vptridx=24 vptr=((& D::_ZTV1D) + 48) 
    A (0x0x7f5b3eb60540) alternative-path
```

> 虚继承总结：
>
> VTT表和vtable表是连接在一起的，Vtable在前，VTT在后
>
> 此时的虚表可能就不是一个连续的了，向上面有为B和C构造的虚表，然后在VTT中有对应的地址

最后附一张别人的多继承的内存分析,与上面的程序，数据变量名不一样，其他均一样，参考链接：[[C++内存问题，看这篇就够了](https://segmentfault.com/a/1190000039348996)](https://segmentfault.com/a/1190000039348996)

<img src="C:\Users\whello\Desktop\王小宁\cpp语言\菱形虚继承的内存分布.webp" alt="菱形虚继承的内存分布" style="zoom: 67%;" />

## 9. type_info

> 该部分内容对应于vtable中typeinfo参阅[C++ typeid关键字详解](https://blog.csdn.net/gatieme/article/details/50947821)
>
> 用于运行时的类型检查，类中有至少一个虚函数时，在运行时决定（vtable中的type_info那块内存的信息），否则编译时决定。

## 10. 构造函数可以是虚函数吗

类构造时先构造vptr.

<img src="C:\Users\whello\Desktop\王小宁\cpp语言\构造函数有虚函数的汇编代码.png" alt="构造函数有虚函数的汇编代码" style="zoom:67%;" />

答案是**不可以**，原因如下：

>从存储空间角度分析

虚函数对应一个vtable，可是这个vtable其实是存储在对象的内存空间的。问题出来了，如果构造函数是虚函数，就需要通过 vtable来调用，可是对象还没有实例化，也就是内存空间还没有，无法找到vtable，所以构造函数不能是虚函数。

> 从实用角度分析

虚函数主要用于在信息不全的情况下，能使重载的函数得到对应的调用。构造函数本身就是要初始化实例，那使用虚函数也没有实际意义。所以构造函数没有必要是虚函数。
虚函数的作用在于通过父类的指针或者引用来调用它的时候能够变成调用子类的那个成员函数。而构造函数是在创建对象时自动调用的，不可能通过父类的指针或者引用去调用，因此也就规定构造函数不能是虚函数。

## 11. 构造函数可以是内联的吗

> 可以但不建议
>
> Effective里面说，构造函数和析构函数往往是inline的糟糕候选人。因为编译器在编译期间会给你的构造函数和析构函数额外加入很多的代码，像成员函数的构造析构等代码，所以通常构造析构函数比表面上看起来的要多，并不适合作为内联函数。

>**inline造成代码膨胀会导致额外的换页行为，降低指令高速缓存装置的命中率，以及伴随这些而来的效率降低。**——-摘自effective

## 12. 智能指针及实现原理

> 参考链接：[c++中的四个智能指针：shared_ptr, unique_ptr, weak_ptr, auto_ptr](https://blog.csdn.net/A315776/article/details/105171512)

野指针不是为null的指针，而是指向`垃圾`内存的指针

> shared_ptr参考：https://mp.weixin.qq.com/s/rx5QvFHCacC7SHtXlV_C8w

## 13. C++11新特性

[cpp11常用特性.md](./cpp11常用特性.md)

## 14. 预处理、编译、汇编、链接都干了什么

> 预处理：主要处理以#号开头的那些东西。`#pragma`会保留。
>
> 编译：就是将预处理后的文件进行词法、语法、语义分析,生成汇编代码。
>
> 汇编：将汇编代码变为机器可执行的指令。
>
> 链接：主要将各个模块之间相互引用的部分都处理好，使得各个模块之间能正确的衔接。主要过程包括：**地址和空间分配**、**符号决议**、**重定位等**。

链接是最重要的，符号解析，重定位一系列动作。

**延迟绑定**

> 在动态链接下，程序加载的模块中包含了大量的函数调用，因此动态链接器会耗费很多的时间用于解决模块之间的函数引用的符号查找以及重定位，而实际上只有很少的一部分符号会被立刻访问。延迟绑定通过**将函数地址的绑定推迟到第一次调用这个函数时**，从而避免动态链接器在加载时处理大量函数引用的重定位。

## 15. 传值、传指针、传引用的区别

> 传值和传指针本质都是传值，需要实参到形参的拷贝；传引用本质**没有任何实参的拷贝（不会调用拷贝构造函数）**，一句话，就是让另外一个变量也执行该实参。就是两个变量指向同一个对象。这是对形参的修改，必然反映到实参上。

## 16. malloc 和 new 的区别，free 和 delete 的区别

[8.malloc/free和new/delete](#4. C和C++的区别)

[Linux源码分析之：malloc、free](https://www.cnblogs.com/lit10050528/p/8619977.html)

- new操作符从**自由存储区**（free store）上为对象动态分配内存空间，而malloc函数从堆上动态分配内存。自由存储区是C++基于new操作符的一个抽象概念，凡是通过new操作符进行内存申请，该内存即为自由存储区。而堆是操作系统中的术语，是操作系统所维护的一块特殊内存，用于程序的内存动态分配，C语言使用malloc从堆上分配内存，使用free释放已分配的对应内存。那么自由存储区是否能够是堆（问题等价于new是否能在堆上动态分配内存），这取决于operator new 的实现细节。自由存储区不仅可以是堆，还可以是静态存储区，这都看operator new在哪里为对象分配内存。
- new操作符内存分配成功时，返回的是对象类型的指针，类型严格与对象匹配，无须进行类型转换，故new是符合类型安全性的操作符。而malloc内存分配成功则是返回`void *` ，需要通过强制类型转换将`void*`指针转换成我们需要的类型。
- new内存分配失败时，会抛出bac_alloc异常，会返回NULL；malloc分配内存失败时返回NULL。
- 使用new操作符申请内存分配时无须指定内存块的大小，编译器会根据类型信息自行计算，而malloc则需要显式地指出所需内存的尺寸。
- new，delete会调用对象的构造和析构函数同时申请或释放内存，而malloc，free则不会调用构造或析构函数。
- C++提供了new[]与delete[]来专门处理数组类型。
- operator new / operator delete的实现可以基于malloc，而malloc的实现不可以去调用new。
- 如果是使用malloc分配的内存则释放时一定要调用free，而使用new分配的内存则释放时一定要调用delete。

## 17. malloc, realloc, calloc区别

> `malloc`：`void* malloc(size_t n); `申请n大小的内存，如果成功返回内存首地址，否则返回返回NULL。
>
> 其中，形参n为要求分配的字节数。如果函数执行成功，`malloc`返回获得内存空间的首地址；如果函数执行失败，那么返回值为`NULL`。由于`malloc`函数值的类型为`void`型指针，因此，可以将其值类型转换后赋给任意类型指针，这样就可以通过操作该类型指针来操作从堆上获得的内存空间。
> 需要注意的是,`malloc`函数分配得到的内存空间是未初始化的。
> **注意**：通过`malloc`函数得到的堆内存必须使用`memset`函数来初始化。



> `calloc`：`void *calloc(int n,int size);`申请`n * size`大小的内存,并且初始化为0
>
> 函数返回值为`void`型指针。如果执行成功，函数从堆上获得`size * n`的字节空间，并返回该空间的首地址。如果执行失败，函数返回`NULL`。该函数与malloc函数的一个显著不同时是，`calloc`函数得到的内存空间是经过初始化的，其内容全为0。`calloc`函数适合为数组申请空间，可以将`size`设置为数组元素的空间长度，将`n`设置为数组的容量。



> `relloc`：`void * realloc(void * p,int n);`从p内存开始为止申请n大小的内存
>
> 其中，指针p必须为指向堆内存空间的指针，即由`malloc`函数、`calloc`函数或`realloc`函数分配空间的指针。`realloc`函数将指针p指向的内存块的大小改变为n字节。如果n小于或等于p之前指向的空间大小，那么。保持原有状态不变。如果n大于原来p之前指向的空间大小，那么，系统将重新为p从堆上分配一块大小为n的内存空间，同时，将原来指向空间的内容依次复制到新的内存空间上，p之前指向的空间被释放。`realloc`函数分配的空间也是未初始化的。

## 18. 程序大量malloc和free会有什么后果，怎么解决? 

>可能会造成大量内存碎片，内存管理模块，避免使用系统内存管理api,可以使用内存管理较好的第三方，或自己编写内存管理模块

**频繁的动态分配不定大小的内存会引起很大的问题以及堆破碎的风险。**

## 19. 函数参数变量前有无const是否可以重载

> 普通类型不可以，但当参数类型为指针或引用时可以。

## 20. 全局变量，静态全局变量可以被其他文件调用么？

> 全局变量整个程序可见，加static当前文件可见，， 想要调用的话需要使用extern关键字修饰

## 21. const 的作用和用法

const强调只读属性，constexpr强调常量属性

## 22. 堆和栈的区别

### 管理方式不同

> 对于栈来讲，是由编译器自动管理，无需我们手工控制；对于堆来说，释放工作由程序员控制，容易产生memory leak。

### 空间大小不同

> 一般来讲在32位系统下，堆内存可以达到4G的空间，从这个角度来看堆内存几乎是没有什么限制的。但是对于栈来讲，一般都是有一定的空间大小的.linux下栈大小8MB

### 能否产生碎片不同

> 对于堆来讲，频繁的new/delete势必会造成内存空间的不连续，从而造成大量的碎片，使程序效率降低。对于栈来讲，则不会存在这个问题，因为栈是先进后出的队列，他们是如此的一一对应，以至于永远都不可能有一个内存块从栈中间弹出，在他弹出之前，在他上面的后进的栈内容已经被弹出.

### 生长方向不同

> 对于堆来讲，生长方向是向上的，也就是向着内存地址增加的方向；对于栈来讲，它的生长方向是向下的，是向着内存地址减小的方向增长。

### 分配方式不同

> 堆都是动态分配的，没有静态分配的堆。栈有2种分配方式：静态分配和动态分配。静态分配是编译器完成的，比如局部变量的分配。动态分配由`alloca`函数进行分配，但是栈的动态分配和堆是不同的，他的动态分配是由编译器进行释放，无需我们手工实现。

### 分配效率不同

> 栈是机器系统提供的数据结构，计算机会在底层对栈提供支持：分配专门的寄存器存放栈的地址，压栈出栈都有专门的指令执行，这就决定了栈的效率比较高。堆则是C/C++函数库提供的，它的机制是很复杂的，例如为了分配一块内存，库函数会按照一定的算法（具体的算法可以参考数据结构/操作系统）在堆内存中搜索可用的足够大小的空间，如果没有足够大小的空间（可能是由于内存碎片太多），就有可能调用系统功能去增加程序数据段的内存空间，这样就有机会分到足够大小的内存，然后进行返回。显然，堆的效率比栈要低得多。



## 23. 写一个宏，输出数组的大小

```cpp
#define SIZE(tar) ({sizeof((tar))/sizeof(*(tar));})
// test
int arr[] = {12, 23, 34, 45};
cout << SIZE(arr) << endl; // 4

// fun(int arr[]){}// arr会自动退化为指针 sizeof(arr)为指针变量所占字节大小
```

## 24. C语言柔性数组

```cpp
struct A{
    int a;
    int arr[]; // 柔性数组，动态确定大小
};
sizeof(A); // 4
//使用
A *p = (A*)malloc(sizeof(A) + sizeof(char) * 10);
memset(p->arr, 97, 10); // 97是字符'a'
cout << p->arr << endl; // aaaaaaaaaa
```

## 25. static关键字的使用和作用

### 变量

#### 局部变量

在任何一个函数内部定义的变量（没有static修饰）的都属于这个范畴，编译器一般不对普通局部变量进行初始化，也就是说它的初值是不确定的，除非显式赋值。

> 普通局部变量存储于进程栈空间，使用完毕会立即释放。

静态局部变量使用static修饰符定义，即使在声明时未赋初值，编译器也会把它初始化为0。且静态局部变量存储于进程的全局数据区，即使函数返回，它的值也会保持不变。

>变量在全局数据区分配内存空间；编译器自动对其初始化
>其作用域为局部作用域，当定义它的函数结束时，其作用域随之结束

#### 全局变量

全局变量定义在函数体外部，在全局数据区分配存储空间，且编译器会自动对其初始化。

**普通全局变量对整个工程可见**，其他文件可以使用extern外部声明后直接使用。也就是说其他文件不能再定义一个与其相同名字的变量了（否则编译器会认为它们是同一个变量）。

**静态全局变量仅对当前文件可见**，其他文件不可访问，其他文件可以定义与其同名的变量，两者互不影响。

> 在定义不需要与其他文件共享的全局变量时，加上static关键字能够有效地降低程序模块之间的耦合，避免不同文件同名变量的冲突，且不会误使用。

### 函数

函数的使用方式与全局变量类似，在函数的返回类型前加上static，就是静态函数。其特性如下：

- 静态函数只能在声明它的文件中可见，其他文件不能引用该函数
- 不同的文件可以使用相同名字的静态函数，互不影响

> 非静态函数可以在另一个文件中直接引用，甚至不必使用extern声明

### 面向对象

#### 静态数据成员

在类内数据成员的声明前加上static关键字，该数据成员就是类内的静态数据成员。其特点如下：

- 静态数据成员存储在全局数据区，静态数据成员在定义时分配存储空间，所以不能在类声明中定义
- 静态数据成员是类的成员，无论定义了多少个类的对象，静态数据成员的拷贝只有一个，且对该类的所有对象可见。也就是说任一对象都可以对静态数据成员进行操作。而对于非静态数据成员，每个对象都有自己的一份拷贝。
- 由于上面的原因，静态数据成员不属于任何对象，在没有类的实例时其作用域就可见，在没有任何对象时，就可以进行操作
- 和普通数据成员一样，静态数据成员也遵从`public, protected, private`访问规则【访问规则与数据成员一样】
- 静态数据成员的初始化格式：`<数据类型><类名>::<静态数据成员名>=<值>`
- 类的静态数据成员有两种访问方式：`<类对象名>.<静态数据成员名> `或 `<类类型名>::<静态数据成员名>`

同全局变量相比，使用静态数据成员有两个优势：

- 静态数据成员没有进入程序的全局名字空间，因此不存在与程序中其它全局名字冲突的可能性
- 可以实现信息隐藏。静态数据成员可以是`private`成员，而全局变量不能

#### 静态数据成员

与静态数据成员类似，静态成员函数属于整个类，而不是某一个对象，其特性如下：

- 静态成员函数没有this指针，它无法访问属于类对象的非静态数据成员，也无法访问非静态成员函数，它只能调用其余的静态成员函数或静态成员变量。
- 出现在类体外的函数定义不能指定关键字static
- 非静态成员函数可以任意地访问静态成员函数和静态数据成员

## 26. 结构体内存对齐规则

【【程序员老秦】谈一谈C++的内存对齐】 https://www.bilibili.com/video/BV1YG411M7xR/?share_source=copy_web&vd_source=a62b785a83479ae5d8d46682f507d384

> 1. **变量**的**起始地址**能够被其对齐值整除，结构体变量的对齐值为最宽的成员大小。
> 2. 结构体每个成员相对于**起始地址的偏移**能够被其**自身对齐值整除**，如果不能则在**前一个成员后面**补充字节。
> 3. 结构体总体大小能够**被最宽的成员的大小**整除，如不能则在**后面**补充字节。

> 为什么要对齐？
>
> 1. 为了减少使用的内存
> 2. 为了提升数据读取的效率

C++中可以使用`alignof`获取类型的对齐值，`char`类型的对齐值为1，` int`的对齐值为4, `short`的对齐值为2，整个结构体的对齐值为4。

## 27. RAII原则

> 在构造函数中分配资源，在析构函数中释放资源。

## 28. C++模板解析机制

> 模板是在编译器根据程序的相关实现进行实例化的，相当于宏替换，比如有一个`template<class T> class A{};`那么当程序中使用了`A<int>`或`A<double>`等时，编译器才会去构建一个与之相对应的类，这里编译器会构建两个类，第一个中将所有T全部替换为int，第二个全部替换为double。

## 29. C++程序内存布局是怎么样的？堆和栈有什么区别？栈和堆各有什么优缺点？

> 栈空间大小（`ulimit -s `linux是8MB）
>
> 代码区`.text`、只读数据区`.rodata`、`.data`区、`.bss`区、堆区、文件映射区、栈区

> 参考链接：
>
> [C/C++程序内存布局](https://blog.csdn.net/weixin_41969690/article/details/106871662)
>
> [C/C++程序占用内存分析](https://blog.csdn.net/xluren/article/details/8150723?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-8150723-blog-106871662.pc_relevant_multi_platform_whitelistv3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7Edefault-1-8150723-blog-106871662.pc_relevant_multi_platform_whitelistv3&utm_relevant_index=2)

## 30. 2 GB 内存的操作系统中，可以分配4 GB 的数组吗？（虚拟内存）

> 参考：[在2G物理内存的机器上申请4G会怎么样？ -- Linux内存管理](http://www.manongjc.com/detail/39-xbxmsiarihseegc.html)

## 31. 动态链接和静态链接有什么区别？一般什么情况用动态链接，什么情况用静态链接？

> 最大的区别是：静态链接是在形成可执行程序前，动态链接是在程序执行时。
>
> 动态链接在编译时如果发现函数是一个动态链接符号，则会在此时保存一个**符号链接的标记**，在程序运行过程中，如果用到会进行动态装载。
>
> > 参考链接：[深入浅出静态链接和动态链接](https://blog.csdn.net/kang___xi/article/details/80210717)

## 32. gcc的各个优化级别

> `gcc -O[0,1,2,3]`四个优化级别，默认-O0不进行任何优化，现在有一个`-Og`让编译器决定级别
>
> [gcc -O0 -O1 -O2 -O3 四级优化选项及每级分别做什么优化](https://blog.csdn.net/qq_31108501/article/details/51842166)

## 33. 为什么要字节对齐

> <img src="C:\Users\whello\Desktop\王小宁\cpp语言\字节对齐原则.jpg" alt="字节对齐原则" style="zoom: 33%;" />
>
> 原则：
>
> 1. 起始地址需要是成员变量中内存最大的那个的倍数
> 2. 每个成员地址相对于起始地址的偏移是min(对齐数,成员内存最大）的倍数
> 3. 总长度也要满足倍数关系

> cpu访问数据的效率问题
>
> 参考链接：[地址对齐和非对齐是指什么？](https://blog.csdn.net/zmfmfking/article/details/120061407#:~:text=%E5%A6%82%E6%9E%9C%E6%95%B0%E6%8D%AE%E4%BC%A0%E8%BE%93%EF%BC%88data%20transfer%EF%BC%89%E7%9A%84%E6%89%80%E6%9C%89%E6%95%B0%E6%8D%AE%E8%8A%82%E6%8B%8D%EF%BC%88data%20beats%EF%BC%89%E5%88%A9%E7%94%A8%E4%BA%86%E6%80%BB%E7%BA%BF%E7%9A%84%E6%89%80%E6%9C%89%E5%AD%97%E8%8A%82%E9%80%9A%E9%81%93%EF%BC%88byte%20lanes%EF%BC%89%EF%BC%8C%E5%88%99%E7%A7%B0%E5%85%B6%E6%98%AF%E2%80%9C%E5%AF%B9%E9%BD%90%E7%9A%84%E2%80%9D%E3%80%82,%E4%B8%80%E4%B8%AA%E2%80%9C%E6%95%B0%E6%8D%AE%E8%8A%82%E6%8B%8D%E2%80%9D%E8%A2%AB%E5%AE%9A%E4%B9%89%E4%B8%BA%EF%BC%8C%E5%9C%A8%E4%B8%80%E4%B8%AA%E6%97%B6%E9%97%B4%E9%97%B4%E9%9A%94%E6%88%96%E6%97%B6%E9%92%9F%E5%91%A8%E6%9C%9F%E5%86%85%EF%BC%8C%E9%80%9A%E8%BF%87%E6%80%BB%E7%BA%BF%E7%9A%84%E5%85%A8%E5%AE%BD%E5%BA%A6%E6%88%96%E6%9B%B4%E5%B0%8F%E5%AE%BD%E5%BA%A6%EF%BC%88full%20width%20or%20less%EF%BC%89%E7%9A%84%E6%95%B0%E6%8D%AE%E4%BC%A0%E8%BE%93%EF%BC%88data%20transfer%EF%BC%89%E3%80%82)

## 34. vector底层，vector如何释放内存，clear()的作用

底层是数组结构，连续的内存，clear的作用底层调用erase人，erase底层调用`destory`来调用析构函数（没有内存释放操作），只是把当前要插入的位置置为了开始，总容量没变

## 35. 函数重载是怎样实现的

> c语言不支持函数重载，c++支持函数重载，表示函数签名不同，签名包括函数名+参数列表，编译器解析方式不同，所以造成c++支持函数重载
>
> 参考链接：[关于extern “C“](https://blog.csdn.net/A315776/article/details/124051339)

## 36. 深拷贝是用什么函数？

`strcpy或strncpy`以及`memcpy`,因为`strcpy`会有内存泄漏的风险， `memcpy`是内存位的拷贝

## 37. 栈的生长方向

由高向低生长

## 38. c++内存访问顺序有哪些种类

C++11 提供6 种可以应用于原子变量的内存次序

```shell
momory_order_relaxed,
memory_order_consume,
memory_order_acquire,
memory_order_release,
memory_order_acq_rel,
memory_order_seq_cst
```

详解参考：[[C++内存问题，看这篇就够了](https://segmentfault.com/a/1190000039348996)](https://segmentfault.com/a/1190000039348996)

## 39. new的对象使用delete [ ] 删除行吗，为什么？new 的数组使用delete删除可以吗，为什么？

> 不可以(看对象是不是内置对象，如果是内置对象就可以混用)

## 40. new出来的对象，使用delete时它怎么知道这个内存的大小?没答出来，然后问如果是你来设计会怎么设计？

在返回地址的前面额外使用内存保存了所需释放内存的次数和大小信息。

## 41. 宏实现max(a,b)

```cpp
#define MAX(a,b)  (((a)>(b))?(a):(b))
```

## 42. vector原理

> 参见《STL源码剖析》

## 43. C++虚函数实现的目的是什么？ 底层原理是什么？

> [C++中虚函数的作用和虚函数的工作原理](https://www.cnblogs.com/zkfopen/p/11061414.html)

## 44. 读程序

```cpp
class Base { 
public:  
    virtual void test() {

    }
    Base() {
        test();
    }  
    void func() {
        test();
    }
}; 
class A : public Base { 
public:
    A() {
        test();
    }   
    void test() {

    }
};
```

我说的是Base里的构造函数调用的test是静态绑定，`func`里的test不是静态绑定，需要查表，然后就问我为什么，讨论了挺久。面试完经过汇编验证，我的结论是：构造函数调用虚函数时，构造的是本对象，所以直接静态绑定本对象的函数。而`func`函数可能被子类继承，调用方对象不明确，需要查表。

> 如果注释掉子类中的test函数，那么A构造函数中test为父类的test，因为子类并没有重载该函数

## 45. 说说大小字节序，用一个简单函数判别大小序

```cpp
#include<bits/stdc++.h>
using namespace std;
int main(){
    //检测大端小端
    union{
        int n;
        char c;
    }data;
    data.n = 1;
    if(data.c == 1){
        cout<<"little"<<endl;
    }else{
        cout<<"big"<<endl;
    }
    return 0;
}
//little 表示小端方式
```

大端：数据的低位放到高地址

小端：数据的低位放到低地址

> 以一个两字节short型变量0x0102的存储举例：
>
> - 大端字节序：高位字节在前，低位字节在后，01|02，符合人们的读写习惯。
> - 小端字节序：低位字节在前，高位字节在后，02|01。
> - ![大小端数据测试举例](大小端数据测试举例.gif)
> - 一个四字节数据0x01234567分别用大小端字节序存储的结果如上图所示。

## 46. 什么是函数指针？

```cpp
函数指针是指向函数的指针变量。

通常我们说的指针变量是指向一个整型、字符型或数组等变量，而函数指针是指向函数。

函数指针可以像一般函数一样，用于调用函数、传递参数。

函数指针变量的声明：

int func(int, int);

typedef int (*fun_ptr1)(int,int); // 声明一个指向同样参数、返回值的函数指针类型
或 using func_ptr2 = int(*)(int, int);
或 typedef int func_ptr3();
或 using func_ptr4 = int();

使用方式
func_ptr1 f1 = func;
func_ptr2 f2 = func;
func_ptr3 *f3 = func;
func_ptr4 *f4 = func;


指针函数：返回值为指针变量的函数
```

## 47. 写个函数在main函数执行前先运行?

1. 使用`attribute`关键字，声明`constructor`和`destructor`函数(gcc中，注意：VC中不支持attribute)。
2. 利用全局对象的构造函数会在main函数之前执行的特点。
3. 将函数注册到`.ctors`和`.dtors`段中。
4. `atexit(参数)`参数为`void(*)(void)`类型的函数指针，使得该函数在`main`之后执行。

> 方式1

```cpp
#include <iostream>

// attribute方式
__attribute__((constructor)) void before() {
    printf("before main\n"); //注意此处不要使用std::cout,会出现段错误
}

__attribute__((destructor)) void after(){
    printf("after main\n");
}

int main(){
    std::cout << "main\n";
    return 0;
}
```

> 方式2

```cpp
#include <iostream>

class A{
    public:
        A(){
            printf("A\n");
        }
};

static A a;

int main(){
    std::cout << "main\n";
    return 0;
}
```

> 方式3

```cpp
void fun1(){
    printf("ctor\n");
}
void fun2(){
    printf("dtor\n");
}
typedef void(*init_r)(void);

init_r __attribute__((section(".ctors"))) mptr1 = &fun1; // __attribute((section(".ctors"))) 也可以
init_r __attribute__((section(".dtors"))) mptr2 = &fun2; // __attribute((section(".dtors")))

int main(){
    
}
```

## 48. C++中拷贝赋值函数的形参能否进行值传递？

> 可以！但在实参到形参的过程中会调用一次拷贝构造函数

> 正常情况

```cpp
#include <iostream>
class A{
    public:
        A(){
            printf("构造函数\n");
        };
    A(const A& a){
        printf("拷贝构造\n");
    }
    A& operator=(const A &a){
        printf("拷贝赋值\n");
        return *this;
    }
};
int main(){
    A a;
    A b;
    b = a;
    return 0;
}
```

>构造函数
>构造函数
>拷贝赋值

> 将拷贝赋值函数的形参变为A a

    class A{
        public:
            A(){
                printf("构造函数\n");
            };
    
            A(const A& a){
                printf("拷贝构造\n");
            }
            A& operator=(const A a){
                printf("拷贝赋值\n");
                return *this;
            }
    };

 ```cpp
    int main(){
​        A a;
​        A b;
​        b = a;
​        return 0;
​    } 
 ```

>构造函数
构造函数
拷贝构造
拷贝赋值

> 总结，可以看到多调用了一次拷贝构造函数！

## 49. include头文件的顺序以及双引号""和尖括号<>的区别？

>双引号查找顺序

1. 先在当前源文件所在的工作目录中进行查号
2. 编译器设置的头文件查找路径，编译器有默认的头文件查找路径（当然编译器可使用`-I`显式指定搜索路径）
3. 系统变量`CPLUS_INCLUDE_PATH/C_INCLUDE_PATH`指定的头文件路径

> 尖括号查找顺序

1. 编译器设置的头文件查找路径，编译器有默认的头文件查找路径（当然编译器可使用`-I`显式指定搜索路径）
2. 系统变量`CPLUS_INCLUDE_PATH/C_INCLUDE_PATH`指定的头文件路径

## 50. C++ 程序到可执行文件的过程？

> 预处理、编译、汇编、链接

- 预处理

  预处理过程主要处理那些源代码中的以`#`开头的预编译指令。比如`#include`、`#define`等，主要处理规则如下：

  - 将所有的`#define`删除，并且展开所有的宏指令
  - 处理所有条件预编译指令，比如`#if`、`#ifdef`、`#elif`等
  - 处理`#include`预编译指令，将被包括的文件插入到该预编译指令的位置
  - 删除所有注释
  - 添加行号和文件名标识，便于调试
  - 保留所有`#pragma`编译指令，因为编译器需要使用他们。

- 编译

  - 编译过程就是把预处理完的文件进行一系列词法分析、语法分析、语义分析以及优化后生成相应的汇编代码。

- 汇编

  - 汇编器将汇编代码转变成机器可以执行的指令，每一个汇编语句几乎都对应一条指令。

- 链接

  - 链接的主要内容就是把各个模块之间**相互引用**的部分都处理好，使各个模块之间能够正确地衔接。
  - 链接过程主要包括**地址和空间分配**，**符号决议**和**重定位**等。

> [14. 预处理、编译、汇编、链接都干了什么](#14. 预处理、编译、汇编、链接都干了什么)

## 51.c++构造函数抛出异常？尽量不要抛出异常在构造函数，那样会导致析构函数不被调用，容易造成内存泄漏，但部分内存会逆序释放！

> 1. 在继承体系中，对象的部分构造是很常见的，异常的发生点也完全是随机的，程序员要谨慎处理这种情况；
> 2. 当对象发生部分构造时，已经构造完毕的子对象将会逆序地被析构（即异常发生点前面的对象）；而还没有开始构建的子对象将不会被构造了（即异常发生点后面的对象），当然它也就没有析构过程了；还有正在构建的子对象和对象自己本身将停止继续构建（即出现异常的对象），并且它的析构是不会被执行的。
> 3. C++中通知对象构造失败的唯一方法那就是在构造函数中抛出异常；
> 4. 构造函数中抛出异常将导致对象的析构函数不被执行；
## 52. 回调函数

> A "callback" is any function that is called by another function which takes the first function as a parameter。 ---from stackoverflow.com
>
> 函数 F1 调用函数 F2 的时候，函数 F1 通过参数给 函数 F2 传递了另外一个函数 F3 的指针，在函数 F2 执行的过程中，函数F2 调用了函数 F3，这个动作就叫做回调（Callback），而先被当做指针传入、后面又被回调的函数 F3 就是回调函数。

> 为什么要有回调函数？`解耦`

<img src="C:\Users\whello\Desktop\王小宁\cpp语言\回调函数.png" alt="回调函数" style="zoom:67%;" />

> 怎么使用？

```cpp
#include <bits/stdc++.h>
using namespace std;

void fun(int, int, string){
    // TODO
}

void call(function<void(int,int,string)> func, int a, int b, string str){
    func(a, b, str);
}

int main(){
    call(fun, 1, 2, "hello");
}
```

>异步回调：必须是多进程或多线程
>同步回调：可以是多线程也可以是单线程

## 53. volatile关键字

> volatile 关键字和const 一样是一种类型修饰符，用它修饰的变量表示可以被某些编译器未知的因素更改，比如操作系统、硬件或者其它线程等。遇到这个关键字声明的变量，编译器对访问该变量的代码就不再进行优化，从而可以提供对特殊地址的稳定访问。不优化，并且每次读都会直接从内存取值。

## 54. restrict关键字

> restrict关键字只用于限定和约束指针;它告诉编译器,所有修改该指针所指向内存中内容的操作,全都必须基于(base on)该指针,即:不存在其它进行修改操作的途径;换句话说,**所有修改该指针所指向内存中内容的操作都必须通过该指针来修改**,而不能通过其它途径(其它变量或指针)来修改;这样做的好处是,能帮助编译器进行更好的优化代码,生成更有效率的汇编代码;

> 在该指针的生命周期内，其指向的对象不会被别的指针所引用。

```cpp
int add(int * restrict a, int * restrict b); // 这样声明的话，a和b不能指向同一块内存。
```

## 55. 把常量字符串，复制给指针、数组，可以修改吗？

>指针不能修改，数组可以修改
>
>char *ptr = "hello";// 本身"hello"是一个常量字符串，ptr只是进行了指向，所以不能通过ptr进行字符串的修改, 此时属于字面量，只读
>
>char arr[] = "hello"; // 进行了**复制**操作，所以可以通过arr下标访问来修改字符串内容



```cpp
char name[] = "hello";
cout << sizeof(name)  // 6
     << " " << strlen(name) << endl; // 5
```



## 56. c++中两个unique_ptr互指会有什么问题？

```cpp
#include <bits/stdc++.h>
using namespace std;
class B;

class A{
public:
    unique_ptr<B> _pb;
    ~A(){
        cout << "delete A" << endl;
    }
};

class B{
public:
    unique_ptr<A> _pa;
    ~B(){
        cout << "delete B" << endl;
    }
};

void func(){
    A* a = new A;
    B* b = new B;
    a->_pb = unique_ptr<B>(b);
    b->_pa = unique_ptr<A>(a);
    delete a;
    // delete b;
    
}

int main(){
    func();
    return 0;
}
```

>因为`delete a `时，`_pb` 的生命周期也要结束了，因此`_pb`会析构，就会去调用指向的原始对象（b）的析构函数，而 b 析构，`_pa` 的生命周期也结束了， 因此会调用指向的原始对象（a）的析构函数。如此反复……

## 57. 带返回值的宏定义编写

```cpp
#include <bits/stdc++.h>
using namespace std;

#define BFS_ADD(x,y,z) ({\
    int ret = 0;\
    ret = (x) + (y) + (z);\
    ret *= 2;\
    ret;\
})


int main(){
    cout << BFS_ADD(1,2,3) << endl;
    return 0;
}
```

格式为 `#define MACRO(参数) ({sy1; sy2;...;syn;})`最后一个`syn`表示返回值

## 58. map如何迭代？迭代器类型是什么？

> 双向迭代器， 底层是红黑树
>
> 不能修改key值，因为涉及到排序规则

```cpp
map<int,int> mp;
mp.begin();//指向红黑树最左下的那个位置
mp.end();//指向红黑树最右下的那个位置
```

怎么迭代参考路径：`./相关参考/CppEngineering.pdf` 根据父节点进行指向

## 59. float和int分别以什么形式存储的？

> 参考：[别在int和float上栽跟头](https://wenku.baidu.com/link?url=Kf2PqZZfBoXaYWsZ7r7d8lr8Q6LYf-zkp08l9tCj4-EZ1mwAIWIhS2Enqrb6ZhbrymiwzJS7_Sz_lQ3i3NPBOQf-idzbNj7GLPi9huRDIwm)

> float:4字节  int：4字节

> float占4字节（32位），第一位是**符号位**(sign)，符号位后边8位是**指数**(exponent)，最后23位是**尾数**(mantissa)。
>
> float值的二进制表示形式是: `sign * mantissa * (2 ^ exponent)`。这个表达式是对应上述存储结构的二进制。

![float的二进制形式](C:\Users\whello\Desktop\王小宁\cpp语言\float的二进制形式.png)

> **符号位**，表述浮点数的正或者负，0代表正，1代表负。

> **指数位**，实际也是有正负的，但是没有单独的符号位，在计算机的世界里，进位都是二进制的，指数表示的也是2的N次幂，8位指数表达的范围是0到255，而对应的实际的指数是-127到128。也就是说实际的指数等于指数位表示的数值减127。这里特殊说明， -127和+128这两个指数数值在IEEE当中是保留的用作多种用途的。

> **尾数位**，只代表了二进制的小数点后的部分，小数点前的那位被省略了，当指数位全部为0时省略的是0否则省略的是1，为什么呢，看个例子:
> 二进制11.11表示成指数形式是`1.111 * 2 ^ 1`，0.1111表示成指数形式是`1.111 * 2 ^ -1`。由此可见，正常情况下二进制的指数形式是肯定有一个1的，所以存储的时候直接省略。但是在指数位全部为0时，指数是-127，这个数字是有特殊含义的，在尾数全部为O时代表的数值是0，省略的那位是0，如果省略的是1那么0这个数字就没法用float表示了。

> 举例

3.75的内存结构到底是什么样子的。首先转化成二进制形式`11.11`。转化成二进制指数形式`1.111 * 2 ^ 1`。由此我们可以得知尾数部分是111(将1省略掉了)，不足23位的后边补0，指数部分是`1+127=128`，对应二进制`10000000`。所以存储结构就是`01000000011100000000000000000000`。
反过来转换一下，比如某个`float`的存储结构是`01000000011100000000000000000000`，符号位是正的，指数位是128，实际的指数是`128-127=1`，尾数是`111`，再加上省略的那位就是`1.111`。所以对应的二进制指数形式是`1.111 * 2 ^ 1`，对应的二进制是`11.11`，对应的十进制是`3.75`。
到这里我们就可以看出，实际上尾数决定了浮点数的精度，尾数只有23位，加上省略的那位就是24位。如果一个int类型的值小于`2 ^ 24`，那么float是完全可以表示的。如果int类型大于`2 ^ 24`就不一定能表示。假如一个int数值的二进制表示形式是`100000000000000000000000`，表示成指数形式是
`1.00000000000000000000000 * 2 ^ 23`，对应的float的类型，尾数位全部为0，指数位是`23+127=150`。这句不正确了。

## 60. 函数闭包

> 参考链接：[C++的闭包(closure)](https://zhuanlan.zhihu.com/p/121628510)

> 闭包是带有上下文的函数。说白了，就是有状态的函数。更直接一些，不就是个类吗？换了个名字而已。**像函数而已**

> 一个函数，带上了一个状态，就变成了闭包了。那什么叫 “带上状态” 呢？ 意思是这个闭包有属于自己的变量，这些个变量的值是创建闭包的时候设置的，并在调用闭包的时候，可以访问这些变量。

> 函数是代码，状态是一组变量，将代码和一组变量捆绑 (bind) ，就形成了闭包。

> **内部包含 static 变量的函数，不是闭包， 因为这个 static 变量不能捆绑。你不能捆绑不同的 static 变量，这个在编译的时候已经确定了。**

> **闭包的状态捆绑，必须发生在运行时。**

### 闭包实现1：重载operator()

> 因为闭包是一个函数+一个状态， 这个状态通过隐含的 this 指针传入，所以闭包必然是一个函数对象，因为成员变量就是极好的用于保存状态的工具，因此实现 operator() 运算符重载，该类的对象就能作为闭包使用。默认传入的 this 指针提供了访问成员变量的途径。

```cpp
class MyFunctor{
    private:
        int data_inner;
    public:
        MyFunctor(int data_):data_inner(data_){}
        int operator()(int data1, int data2){
            return data_inner + data1 + data2;
        }
};

int main(){
    // MyFunctor mf(12);
    // cout << mf(23, 24) << endl;
    cout << MyFunctor(12)(23, 24) << endl;
    return 0;
}
```

> 如上就是一种闭包，状态量可以看为`data_inner`，就是包括一个内部状态变量，该变量不能是静态变量

### 闭包实现2：lambda表达式

```cpp
int data_status = 23;
auto fun = [data_status](int data1, int data2){
	return data_status + data1 + data2;
};
cout << fun(23, 24) << endl;
```

### 闭包实现3：bind函数

```cpp
int func(int a, int b, int c){
    return a + b + c;
}

int main(){
    auto funcc = bind(func, placeholders::_2, 12, placeholders::_1); // auto为函数对象functional<int(int,int)>
    cout << funcc(1, 2) << endl;
}
```

## 61. c++模板是如何实现的

>来自：http://t.csdn.cn/3gC1M
>
>1.编译器并不是把函数模板处理成能够处理任意类的函数
>
>2.编译器从函数模板通过具体类型产生不同的函数
>
>3.编译器会对函数模板进行**两次编译。**
>
>**在声明的地方对模板代码本身进行编译；在调用的地方对参数替换后的代码进行编译**

## 62. c++中auto关键字寻找类型的原理

> 参考链接：[C++11中的auto和decltype的原理?](https://www.zhihu.com/question/294048058)

auto使用的是**模板实参推断**（Template Argument Deduction）的机制。auto被一个虚构的模板类型参数T替代，然后进行推断，即相当于把变量设为一个函数参数，将其传递给模板并推断为实参，auto相当于利用了其中进行的实参推断，承担了模板参数T的作用。比如。

```cpp
template<typename Container>
void useContainer(const Container& container)
{
    auto pos = container.begin();
    while (pos != container.end())
    {
        auto& element = *pos++;
        … // 对元素进行操作
    }
}
```

其中第一个auto的初始化相当于下面这个模板传参时的情形，T就是为auto推断的类型

```cpp
template<typename T>
void deducePos(T pos);

deducePos(container.begin());
```

而**auto类型变量不会是引用类型*（模板实参推断的规则）***，所以要用auto&（C++14支持直接用`decltype(auto)`推断原始类型），第二个auto推断对应于下面这个模板传参时的情形，同样T就是为auto推断的类型

```cpp
// auto& element = *pos++的推断等价于如下调用模板的推断
template<typename T>
void deduceElement(T& element);

deduceElement(*pos++);
```

## 63. 构造、析构是否可以是虚函数？

> 构造函数不可以，析构函数可以，并且如果一个类可能被当作父类，那么该类的析构函数就应该是虚函数，否则在多态行为下，子类的析构函数不会被调用。

```cpp
class A{
    public:
    ~A(){
        cout << "A dtor\n";
    }
};

class B : public A{
    public:
    ~B(){
        cout << "B dtor\n";
    }
};

int main(){
    A *a = new B;
    delete a;
}
/*
A dtor
*/
```

> 造成子类的析构函数没有被调用，可能造成内存泄漏

## 64. c++内联、宏、普通函数的区别（内联和宏分别如何实现）

宏定义不检查函数参数，返回值什么的，只是展开，相对来说，内联函数会检查参数类型，所以更安全。. 内联函数和宏很类似，而区别在于，宏是由预处理器对宏进行替代，而内联函数是通过编译器控制来实现的。. 而且内联函数是真正的函数，只是在需要用到的时候，内联函数像宏一样的展开，所以取消了函数的参数压栈，减少了调用的开销。

## 65. 函数调用过程

|  调用惯例  |   出栈方   |                           参数传递                           |                           名字修饰                           |
| :--------: | :--------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
|  `cdecl`   | 函数调用方 |                   从右至左的顺序压参数入栈                   |                        下划线+函数名                         |
| `stdcall`  |  函数本身  |                   从右至左的顺序压参数入栈                   | 下划线+函数名+@+参数的字节数。如函数`int fun(int a, double b)`的修饰名为`_fun@12` |
| `fastcall` |  函数本身  | 头两个DWORD（4字节）类型或者占更少字节的参数被放入寄存器，其他剩下的参数按从右到左的顺序压入栈 |                    @+函数名+@+参数字节数                     |
|  `pascal`  |  函数本身  |                   从左至右的顺序压参数入栈                   |                              --                              |

> `cdecl`是C语言的默认调用惯例

> 栈帧：`ebp`基址指针，`esp`栈指针
>
> 第一步：先根据调用规则将函数参数进行压栈
>
> 第二步：会在上一步完成后的`esp`位置保存返回地址
>
> 第三步：调用call,指令地址入栈
>
> 第四步：开辟新的栈帧，保护现场
>
> 第五步：执行函数

## 66. c++函数返回值传递

1. 函数将返回值存储在`eax`中，返回后函数的调用方读取`eax`，获取返回值。【`eax`有大小限制，只有4字节，所以返回的值不能超过4字节】
2. 【返回大于4字节，小于等于8字节】采用`eax`和`edx`联合返回方式，`eax`保存返回值的低4字节，`edx`保存返回值的高4字节.
3. 【超过8字节】调用方会额外申请一块（**在栈上**）临时区域用于保存返回值，将该临时区域的地址传给被调函数，被调函数将数据拷贝至该区域，并将临时区域的地址保存在`eax`中传出，函数返回后，调用方`eax`指向的临时位置中的对象拷贝给返回值接收的对象。【在此过程中有**两次**拷贝动作】【所以此处有**返回值优化RVO**的操作，直接将对象构造在传出时使用的临时对象上】细节参考《程序员的自我修养》

## 67. 程序运行时，存储区如何划分？

## 68. static特性，static变量初始化的时机。

在C++中，静态变量分为**全局静态变量**（又称全局变量）、**局部静态变量**（函数中的静态变量）和**类中静态成员变量**。按照初始化的类型分为**静态初始化**（static initialization）和**动态初始化**(dynamic initialization)。

> static初始化时机

### 静态初始化

> 指的是用常量来对静态变量进行初始化，包括zero initialization和const initialization，其中zero initialization的变量会保存在.bss段（未初始化静态变量，以及初始化为0的静态变量）；const initialization的变量保存在.data段(已经初始化为非0的静态变量)。对于静态初始化的变量（请注意：包括在函数中采用静态初始化的静态变量），是在程序加载时完成的初始化。

### 动态初始化

> 指的是需要调用函数才能完成的初始化，比如类的构造函数。对于全局或者类的静态成员变量，是在main()函数执行前由运行时调用相应的代码进行初始化的。而对于局部静态变量，是在函数执行至此初始化语句时才开始执行的初始化。

>static的作用：

>1). 在函数体，一个被声明为静态的变量在这一函数被调用过程中维持其值不变。
>2). 在模块内（但在函数体外），一个被声明为静态的变量可以被模块内所用函数访问，但不能被模块外其它函数访问。它是一个本地的全局变量。
>3). 在模块内，一个被声明为静态的函数只可被这一模块内的其它函数调用。那就是，这个函数被限制在声明它的模块的本地范围内使用。 

## 69. C++程序的运行过程

- 操作系统创建进程后，把控制权交到了程序的入口，这个入口往往是运行库中的某个入口函数。
- 入口函数对运行库和程序运行环境进行初始化，包括对、I/O、线程、全局变量构造等等。
- 入口函数在完成初始化后，调用`main`函数，正式开始执行程序主体部分。
- main函数执行完毕后，返回到入口函数，入口函数进行清理工作，包括全局变量的析构。堆销毁、关闭I/O等，然后进行系统调用结束进程。

## 70. 智能指针 share_ptr是否线程安全,队列是否线程安全 share_ptr构造方式 make_share(stl)

> shared_ptr由两部分组成，一个是原生指针，一个是引用计数的模板类，其中包括引用计数等，这个引用计数模板是线程安全的，因为在其模板中锁策略，而对于操作shared_ptr来说，多个读是没有问题的，但对于多个写的话需要加锁，不是线程安全的。

## 71. constexpr

[constexpr常量表达式](./cpp11常用特性.md#constexpr常量表达式)

## 72. 了解内存泄漏吗？堆内存泄漏和栈内存泄漏？

https://blog.csdn.net/qq_23350817/article/details/90641856

https://www.eet-china.com/mp/a123486.html

> 堆栈内存泄漏原因

1. **函数调用层次太深。**函数递归调用时，系统要在栈中不断保存函数调用时的现场和产生的变量，如果递归调用太深，就会造成栈溢出，这时递归无法返回。再有，当函数调用层次过深时也可能导致栈无法容纳这些调用的返回地址而造成栈溢出。
2. **动态申请空间使用之后没有释放**。由于C语言中没有垃圾资源自动回收机制，因此，需要程序主动释放已经不再使用的动态地址空间。申请的动态空间使用的是堆空间，动态空间使用不会造成堆溢出。
3. **数组访问越界。**C语言没有提供数组下标越界检查，如果在程序中出现数组下标访问超出数组范围，在运行过程中可能会内存访问错误。
4. **指针非法访问**。指针保存了一个非法的地址，通过这样的指针访问所指向的地址时会产生内存访问错误。

## 73. 为什么引入移动语义？

>c++为了解决不必要的内存拷贝带来的性能消耗，引入了“移动语义”，为了实现“移动语义”，在类中增加了移动拷贝构造和移动赋值操作，同时引入了新的参数类型：右值引用，而在真实使用场景，一般参数传递的都是左值，所以有了`std::move`函数，它可以将一个左值转化为右值引用，从而可以很方便的使用“移动语义”。

## 74. 元组实现原理

> 自身泛化模板的递归继承。

> github地址：https://github.com/Humorly/wstd-tuple/blob/master/wtuple.h
>
> 参考链接：[剖析std::tuple原理并实作一个](https://zhuanlan.zhihu.com/p/143715615)

## 75. fread、fwrite过程中操作系统做了哪些事情

首先`fread`和`fwrite`是c库函数，底层使用系统调用`read`和`write`函数实现。

- `fread`是**自带缓冲**的,`read`**不带缓冲**。
- `fread`可以读一个结构.`read`在`linux/unix`中读二进制与普通文件没有区别.
- `fopen`是标准c里定义的,`open`是POSIX中定义的
- `fopen`不能指定要创建文件的权限.`open`可以指定权限
- `fopen`返回指针,`open`返回文件描述符(整数).
- `linux/unix`中任何设备都是文件,都可以用`open,read`.

>如果文件的大小是8k。
>
>如果用`read/write`，且只分配了2k的缓存，则要将此文件读出需要做4次系统调用来实际从磁盘上读出。
>
>如果用`fread/fwrite`，则系统自动分配缓存，则读出此文件只要一次系统调用从磁盘上读出。
>
>也就是用`read/write`要读4次磁盘，而用`fread/fwrite`则只要读1次磁盘。效率比`read/write`要高4倍。
>
>如果程序对内存有限制，则用`read/write`比较好。
>
>都用`fread` 和`fwrite`,它自动分配缓存,速度会很快,比自己来做要简单。如果要处理一些特殊的描述符,用`read` 和`write`,如套接口,管道之类的
>
>系统调用write的效率取决于`buf`的大小和你要写入的总数量，如果`buf`太小，你进入内核空间的次数大增，效率就低下。而`fwrite`会替你做缓存，减少了实际出现的系统调用，所以效率比较高。
>
>如果只调用一次(可能吗?)，这俩差不多，严格来说write要快一点点(因为实际上`fwrite`最后还是用了`write`做真正的写入文件系统工作)，但是这其中的差别无所谓。

## 76. typedef和const相遇时

```cpp
typedef string* str_type_name;

int main(){    
    const str_type_name p = new string("hello"); // 这其实展开是这样的string * const p;表示p的指向是不能变的
    p = new string("1111");//error: expression must be a modifiable lvalue
    str_type_name const tp = new string("world"); // 这其实展开是这样的string * const p;表示p的指向是不能变的
    cout << *p << " " << *tp << endl; // hello hujk
    (*tp)[1] = 'c'; // success
    (*p)[1] = 'f'; // success
    cout << *p << " " << *tp << endl; //hfllo hejk
    return 0;
}
```

## 77. 类型转换 dynamic_cast 失败返回null会抛异常吗？  隐式类型转换规则

> dynamic_cast主要用于安全的向下转型。

用于动态类型转换。只能用于**含有虚函数的类**，用于类层次间的向上和向下转化。只能转**指针或引用**。<u>向下转化时，如果是非法的，对于指针返回NULL；对于引用，抛出异常</u>

## 78. 有指针为什么还要有迭代器

### 迭代器

Iterator（迭代器）模式又称Cursor（游标）模式，**用于提供一种方法顺序访问一个聚合对象中各个元素, 而又不需暴露该对象的内部表示**。或者这样说可能更容易理解：**Iterator模式是运用于聚合对象的一种模式，通过运用该模式，使得我们可以在不知道对象内部表示的情况下，按照一定顺序（由iterator提供的方法）访问聚合对象中的各个元素。**

### 迭代器和指针的区别

迭代器不是指针，是类模板，表现的像指针。它只是模拟了指针的一些功能，重载了指针的一些操作符，`->、*、++、--`等。**迭代器封装了指针**，是一个“可遍历STL（ Standard Template Library）容器内全部或部分元素”的对象，**本质是封装了原生指针**，是指针概念的一种提升（lift），提供了比指针更高级的行为，相当于一种智能指针，他可以根据不同类型的数据结构来实现不同的++，--等操作。

### 迭代器产生原因

Iterator类的访问方式就是把不同集合类的访问逻辑抽象出来，使得不用暴露集合内部的结构而达到循环遍历集合的效果。

### 为什么有指针了还要迭代器

1. 通过迭代器访问容器，可以避免许多错误，同时还能隐藏容器的具体实现。
2. 迭代器可以保证对所有容器的基本遍历方式都是一样的，实现算法时若需要遍历，则使用迭代器，则可以不用关注容器的具体类型，实现数据结构和算法的分离。

## 79. 如何不拷贝数据实现vector的扩容？

1. 改变底层的实现结构，改用链表，使用hash表用于下标和链表节点的映射关系。
2. 使用类似deque的实现原理：`中控制器+局部数组化`

## 80. C++重载前置++/--和后置++/--运算符

> 前置和后置是对于++来说的，`++a`表示前置++, `a++`表示后置++

> 假设A中有个变量i,现对++操作符进行重载

### 前置++

```cpp
// 前置++括号中不要参数
A& operator++(){
   	this->i += 1;
    return *this;
}
```

### 后置++

```cpp
// 后置++括号中需要参数，但不会使用
A operator++(int){
    A tmp = *this;
    ++(*this); // 利用前置
    return tmp;
}
```

## 81. linux下malloc实现

> 参考链接
>
> https://zhuanlan.zhihu.com/p/448535361
>
> https://bbs.pediy.com/thread-257742.htm

口水话概述：

首先明确malloc实现是借助三个系统调用函数实现的，分别是：`brk(),sbrk(),mmap()匿名映射`

<img src="linux进程内存模型.jpg" alt="linux进程内存模型" style="zoom:67%;" />

关键的一个数据结构是chunk。如下

<img src="chunk.jpg" alt="chunk" style="zoom:67%;" />

- P表示前一块chunk是否正在使用

- M表示从哪个区域获取的虚拟内存，1为mmap，其他值为heap

- A表示主分配区和非主分配区

  <img src="chunk_detail.jpg" alt="chunk_detail" style="zoom:67%;" />

> malloc通过维护一个有128个bins的数组，每个bin保存一个链表，链表节点为不同大小的chunk块，第一个为unsorted bin，从2开始的64个为small bin,其中维护的chunk大小为16开始，每个间隔8B，在small中从chunk为16到80为fast bin，后64个位large bin，每个chunk间隔64B。
>
> fast bin中的空间在特定时候会进行合并，并放入unsorted bin。
>
> 特定时候为：
>
> > 在申请large bin时
> >
> > 当申请的chunk需要申请新的top chunk时。
>
> > free的堆块大小大于fast bin中的最大size时，这个最大size是定值为64B
>
> 用户申请过程：
>
> 用户在申请内存时，首先根据大小在bin中找
>
> 1. fast bin主要保存小的内存的释放，对于小内存先在fast bin中找，如果没找到会去unsorted bin中去找，unsorted bin中保存的是fast合并或释放大于64B的内存。
> 2. 如果在unsorted bin中找到了，就返回，没有的话，会使用下面的三个内存区方式：
>    1. 如果fast bin和bins中都找不到，malloc会设法在top chunk中分配一块内存给用户；top chunk为在mmap区域分配一块较大的空闲内存模拟sub-heap
>    2. 当chunk足够大，fast bin和bins都不能满足要求，甚至top chunk都不能满足时，malloc会从mmap来直接使用内存映射来将页映射到进程空间，这样的chunk释放时，直接解除映射，归还给操作系统。
>    3. Last remainder是另外一种特殊的chunk，就像top chunk和mmaped chunk一样，不会在任何bins中找到这种chunk。当需要分配一个small chunk,但在small bins中找不到合适的chunk，如果last remainder chunk的大小大于所需要的small chunk大小，last remainder chunk被分裂成两个chunk，其中一个chunk返回给用户，另一个chunk变成新的last remainder chunk。

## 82. 如何限制类的对象只能在堆上创建？如何限制对象只能在栈上创建？

> C++中类的对象的建立分为两种：静态建立和动态建立。
>
> - 静态建立：由编译器为对象在栈空间上分配内存，直接调用类的构造函数创建对象。如`A a;`
> - 动态建立：使用new关键字在堆空间上创建对象，底层调用`operator new()`函数，在堆上寻求合适的内存并分配；然后调用类的构造函数创建对象。如`A* p = new A();`

> 限制对象只能在堆上创建？

1. **将析构函数设为私有**。静态建立是在栈上进行，是由编译器分配和释放内存空间，编译器为对象分配内存时，会对类的非静态函数进行检查，即编译器会检查析构函数的访问性。当析构函数为私有时，编译器创建的对象就无法访问析构函数进行释放，因此编译器就不会在栈上创建对象。

   该办法的问题是，需要显式调用释放内存的函数。

   ```cpp
   class A{
       public:
           A(){}
           void destory(){
           }
       private:
           ~A(){}
   };
   int main(){
       // A a; // 该语句无法通过编译，错误原因报析构函数是私有的
       A *pa =  new A();
       pa->destory();
       return 0;
   }
   ```

2. 将构造函数和析构函数设为protected。设为protected属性保证了子类也能正常创建。类似单例模式，提供一个静态创建对象的公有函数。该方法也需要定义公有显式释放内存的方法。

   ```cpp
   class A{
       public:
           static A* create(){
               return new A();
           }
       	
       protected:
           A(){}
           ~A(){}
   };
   int main(){
       A *pa = A::create();
       return 0;
   }
   ```

> 限制对象只能在栈上创建？

将`operator new`和`operator delete`函数设为私有，防止在堆上建立。

```cpp
class A{
    public:
        A(){}
        ~A(){}
    private:
        void* operator new(size_t t){} // 固定写法
        void operator delete(void *){} // 固定写法
};
int main(){
    A a;
    // A *pa =  new A(); // 报错，不能用
    return 0;
}
```

## 83. mmap

> https://blog.csdn.net/Holy_666/article/details/86532671

## 84.epoll，select，poll

从实现原理上来说，select和poll采用的都是轮询的万式,即每次调用都要扫描整个注册文件描述符集合，并将其中就绪的文件描述符返回给用户程序，因此它们检测就绪事件的算法的时间复杂度是O(n)。epoll_wait则不同，它采用的是回调的方式。内核检测到就绪的文件描述符时，将触发回调函数，回调函数就将该文件描述符上对应的事件插入内核就绪事件队列。内核最后在适当的时机将该就绪事件队列中的内容拷贝到用户空间。因此epoll_wait无须轮询整个文件描述符集合来检测哪些事件已经就绪，其算法时间复杂度是O(1),**但是，当活动连接比较多的时候，epoll_wait 的效率未必比select和 poll高，因为此时回调函数被触发得过于频繁。所以epoll_wait适用于连接数量多，但活动连接较少的情况。**

## 85.netstat命令

`-n,-a,-p等含义`

<img src="C:\Users\whello\Desktop\王小宁\cpp语言\netstat常用参数.png" alt="netstat常用参数" style="zoom: 67%;" />

## 86.awk，grep, sed 大致了解即可

awk就是把文件读入，按每行来处理，格式`awk -F '分割符（默认是空格，制表符） 'BEGIN{} {action} END{} filename'`

grep匹配文本中的符合条件的行，并输出出来，常用参数如下：

<img src="C:\Users\whello\Desktop\王小宁\cpp语言\grep常用参数.png" alt="grep常用参数" style="zoom:60%;" />

sed常用于替换，删除编辑操作

## 87. 如何查看哪个进程在监听指定端口

<img src="C:\Users\whello\Desktop\王小宁\cpp语言\查看端口被哪些进程监听.png" alt="查看端口被哪些进程监听" style="zoom:67%;" />

## 88. 如何查看cpu核数

`/proc/cpuinfo `中的信息如下：

```objectivec
processor       : 0
vendor_id       : GenuineIntel
cpu family      : 6
model           : 62
model name      : Intel(R) Xeon(R) CPU E5-2650 v2 @ 2.60GHz
stepping        : 4
microcode       : 0x428
cpu MHz         : 1200.062 # cpu频率
cache size      : 20480 KB # 缓存大小
physical id     : 0
siblings        : 16
core id         : 0
cpu cores       : 8 # cpu核数
apicid          : 0
initial apicid  : 0
fpu             : yes
fpu_exception   : yes
cpuid level     : 13
wp              : yes
bogomips        : 5200.34
clflush size    : 64
cache_alignment : 64
address sizes   : 46 bits physical, 48 bits virtual
power management:
```

其中包括`cpu cores`，只要把其中的8拿出来就行了

`echo /proc/cpuinfo | awk -F ':' '{if($1=="cpu cores") printf("%d, $2")}'`

## 89. 磁盘剩多少，内存剩多少

磁盘使用`df`,内存使用`free或top`

`cat /proc/meminfo`查看内存信息

## 90. cpp中模板传参后类型的推导

```cpp
template<typename T>
size_t df(T){
    return sizeof(T);
}

template<typename T>
size_t df2(T &&){
    return sizeof(T);
}

int main(){
    uint32_t a = 12;
    uint32_t b[] = {a};
    uint32_t *p = b;
    
    cout << df(a) << " " << df(b) << " " << df(p) << endl;
    cout << df2(a) << " " << df2(b) << " " << df2(p) << endl;
}
/*
4 8 8
4 4 8
*/
```

问题：当数组传入后，T&&推导后T为什么类型；

> 查阅资料 万能引用对数组参数的推导，万能引用对指针变量的推导
>
> 对于`df2(b)`会推导成`size_t df2<uint32_t (&)[1]>(uint32_t (&a)[1])`属实忘记了，

- 不同的模板参数对数组的推导

  > 参考：https://zhuanlan.zhihu.com/p/404741698

  ```cpp
  template<typename T>
  void copy_d1(T param){}
  
  template<typename T>
  void copy_d2(T& param){}
  
  template<typename T>
  void copy_d3(T* param){}
  
  template<typename T>
  void copy_d4(const T& param){}
  
  template<typename T>
  void copy_d5(const T* param){}
  
  template<typename T>
  void copy_d6(const T param){}
  
  int        arr[10]{1};
  copy_d1(arr); // void copy_d1<int *>(int *param)： T： int*    数组退化为指针     sizeof(T) = sizeof(int *) = 8
  copy_d2(arr); // void copy_d2<int [10]>(int (&param)[10]) T：int [10]            sizeof(T)为sizeof(int) * 数组有多少个元素 sizeof(int[10]) = 4 * 10 = 40
  copy_d3(arr); // void copy_d3<int>(int *param)  T[int]                           sizeof(T) = sizeof(int) = 4
  copy_d4(arr); // void copy_d4<int [10]>(const int (&param)[10])                  sizeof(int[10]) = 4 * 10 = 40
  copy_d5(arr); // void copy_d5<int>(const int *param)                             sizeof(T) = sizeof(int) = 4
  copy_d6(arr); // void copy_d6<int *>(int *param)                                 sizeof(T) = sizeof(int *) = 8
  
  
  // 对于万能引用
  template<typename T>
  void copy_d(T&& param){}
  
  copy_d(arr); // void copy_d<int (&)[10]>(int (&param)[10])  sizeof(T)为sizeof(int) * 数组有多少个元素 sizeof(int[10]) = 4 * 10 = 40
  ```


## 91.a++和++a以及b--,--b左值、右值分析

牢记：右值不能赋值给左值引用。

后置是右值，前置是左值

> `a++`是右值，以及`b--`也是右值
>
> `++a`是左值，以及`--b`也是左值
>
> ```cpp
> void f(int &a){}
> void fg(const int &b){}
> 
> int a = 1;
> f(a++);// 报错
> fg(a++); // 正确
> ```

> 变量距离等号越远为右值，靠近等号为左值。

## 92. memcmp可以比较两个结构体是否相等吗？

- 如果结构体不存在内存对齐，那么可以
- 如果存在内存对齐，那么不可以

但可以在使用结构体之前都是用`memset`将内存设置为0，那么不管涉不涉及内存对齐都可以使用。

比如提供默认构造函数进行内存设置，如下代码

```cpp
typedef struct D{
    char ch;
    int a;
    D(){
        memset(this, 0, sizeof(*this));
    }
}data;
// 通过这样声明后就可以使用memcmp进行比较是否相等
data ad1, ad2;
ad1.a = 1;
ad1.ch = 'c';
ad2.a = 1;
ad2.ch = 'c';
cout << memcmp(&ad1, &ad2, sizeof(data)) << endl; // 0
```

```shell
原型：int memcmp(const void *buf1, const void *buf2, unsigned int count);
用法：#include <string.h>或#include<memory.h>
功能：比较内存区域buf1和buf2的前count个字节。
说明：
当buf1<buf2时，返回值<0
当buf1=buf2时，返回值=0
当buf1>buf2时，返回值>0
```

## 93. 强符号和弱符号、强引用和弱引用

> 编译器<u>默认函数</u>和<u>初始化了的全局变量</u>为**强符号**；
>
> <u>未初始化的全局变量</u>为**弱符号**。

> 对外部目标文件的符号引用被最终链接成可执行文件时，它们需要被正确决议，如果没有找到该符号的定义，链接器就会报符号未定义错误，这种称为**强引用**。
>
> 链接器处理强引用和**弱引用**的过程几乎一样，只是对于未定义的弱引用，链接器不认为它是一个错误，一般对于未定义的弱引用，链接器默认其为0，或者是一个特殊的值，以便程序代码能够识别。

## 94. 条件变量

> 注意条件变量的几个问题：信号丢失，虚假唤醒问题

> 什么是条件变量？

条件变量是多线程程序中用来实现等待和唤醒逻辑常用的方法。通常有wait和notify两个动作，wait用于阻塞挂起线程A，直到另一个线程B通过通过notify唤醒线程A，唤醒后线程A会继续运行。

> 条件变量的使用(以生产者消费者为例！)

```cpp
std::mutex mutex;
std::condition_variable cv;
std::vector<int> vec;

void Consume() {
  std::unique_lock<std::mutex> lock(mutex);
  cv.wait(lock);
  std::cout << "consume " << vec.size() << "\n";
}

void Produce() {
  std::unique_lock<std::mutex> lock(mutex);
  vec.push_back(1);
  cv.notify_all();
  std::cout << "produce \n";
}

int main() {
  std::thread t(Consume);
  t.detach();
  Produce();
  return 0;
}
```

上面的代码存在**信号丢失**问题：

```txt
如果先执行的Produce()，后执行的Consume()，生产者提前生产出了数据，去通知消费者，但是此时消费者线程如果还没有执行到wait语句，即线程还没有处于挂起等待状态，线程没有等待此条件变量上，那通知的信号就丢失了，后面Consume()中才执行wait处于等待状态，但此时生产者已经不会再触发notify，那消费者线程就会始终阻塞下去，出现bug。
```

解决办法：通过**增加附加条件**可以解决信号丢失的问题，但这里还有个地方需要注意，消费者线程处于wait阻塞状态时，即使没有调用notify，操作系统也会有一些概率会唤醒处于阻塞的线程，使其继续执行下去，这就是**虚假唤醒**问题，当出现了虚假唤醒后，消费者线程继续执行，还是没有可以消费的数据，出现了bug。 

```cpp
std::mutex mutex;
std::condition_variable cv;
std::vector<int> vec;

void Consumer() {
  std::unique_lock<std::mutex> lock(mutex);
  if (vec.empty()) { // 加入此判断条件
      cv.wait(lock);
  }
  std::cout << "consumer " << vec.size() << "\n";
}

void Produce() {
  std::unique_lock<std::mutex> lock(mutex);
  vec.push_back(1);
  cv.notify_all();
  std::cout << "produce \n";
}

int main() {
  std::thread t(Consumer);
  t.detach();
  Produce();
  return 0;
}
```

解决虚假唤醒问题，可以将条件判断的`if`变为`while`，如`while (vec.empty())`即可解决虚假唤醒问题。

上面两个信号丢失和虚假唤醒问题可以通过使用c++新标准的一条语句进行解决，新的生产者消费者模型如下：

```cpp
std::mutex mutex;
std::condition_variable cv;
std::vector<int> vec;

void Consumer() {
  std::unique_lock<std::mutex> lock(mutex);
  cv.wait(lock, [&](){ return !vec.empty(); }); // 这里可以直接使用C++的封装.wait中传入的lock需要有手动释放锁的能力
  std::cout << "consumer " << vec.size() << "\n";
}

void Produce() {
  std::unique_lock<std::mutex> lock(mutex);
  vec.push_back(1);
  cv.notify_all();
  std::cout << "produce \n";
}

int main() {
  std::thread t(Consumer);
  t.detach();
  Produce();
  return 0;
}
```

## 95. 多维数组的使用

```cpp
template <typename T, size_t R, size_t... C>
struct Matrix {
    private:
    using Col = typename Matrix<T, C...>::type;
    public:
    using type = std::array<Col, R>;
};

template <typename T, size_t R>
struct Matrix<T, R> {
    using type = std::array<T, R>;
};

Matrix<int, 2, 2, 2>::type arr;
for(int i = 0; i< 2; ++i){
	for(int j = 0; j < 2; ++j){
		for(int k = 0; k < 2; ++k){
			arr[i][j][k] = 2;
        }
    }
}
```

## 96. 虚函数为什么会慢？

> 虚函数通常通过虚函数表来实现，在虚表中存储函数指针，实际调用时需要间接访问，这需要多一点时间。
>
> 其实上面的过程并不是虚函数慢的主要原因。
>
> 主要原因如下：
>
> 虚函数其实最主要的性能开销在于**它阻碍了编译器内联函数和各种函数级别的优化**，导致性能开销较大，在普通函数中`log(10)`会被优化掉，它就只会被计算一次，而如果使用虚函数，`log(10)`不会被编译器优化，它就会被计算多次。如果代码中使用了更多的虚函数，编译器能优化的代码就越少，性能就越低。[**待确定**]

## 97. const函数重载

类内的const函数可以重载，重载的方式不同,只有当const修饰的是指针或引用时才会发生重载。

```cpp
void fun__r(const int &a){
    cout << "const int &\n";
}

void fun__r(int &a){
    cout << "int &\n";
}

void fun__p(const int *a){
    cout << "const int *\n";
}

void fun__p(int *a){
    cout << "int *\n";
}

int a = 10;
fun__r(a); // int &
const int b = 10;
fun__r(b); // const int&

fun__p(&a); // int *
fun__p(&b); // const int *
```



## 98. 为什么构造析构里不要调用虚函数？

> https://www.jianshu.com/p/46090def718a
>
> 可以使用，但没有多态行为。

```kotlin
    #include "iostream"

    using namespace std;


    class Base{
        public:
            Base(){
                cout << "Base::Base()\n";
                fun();
            }
            virtual ~Base(){
                cout << "Base::Base()\n";
                fun();
            }
            virtual void fun(){
                cout << "Base::fun() virtual\n";
            }
    };

    // 派生类
    class Derive: public Base{
        public:
            Derive(){
                cout << "Derive::Derive()\n";
                fun();
            }
            ~Derive(){
                cout << "Derive::Derive()\n";
                fun();
            }
            virtual void fun(){
                cout << "Derive::fun() virtual\n";
            }
    };

    int main(){
        Base *b = new Base();
        delete b;
        cout << "-----------------------------------\n";
        Derive *d = new Derive();
        delete d;
        cout << "-----------------------------------\n";
        Base *bd = new Derive();  // 基类Base的指针bd指向的是派生类Derive的对象
        delete bd;
        return 0;
    }
```

- 解释: 第一段代码不加说明，第二段代码Derive *d = new Derive();先调用类的构造函数，这个构造函数是基类Base的fun()函数，因为此时派生类Derive还不存在,然后才构造派生类Derive。析构时，先析构派生类Derive,并调用派生类的fun()函数，再析构基类。第三段代码Base *bd = new Derive();基类Base的指针bd指向派生类对象。构造时，先调用基类Base的构造函数，此时构函数中调用基类中的fun()函数，此时**虚函数的动态绑定机制并没有会生效**,这是因为**此时的派生类还不存在**。析构时，先析构派生类，派生类中的fun()函数调用的是自己的fun()，然后析构基类Base,基类析构函数中的fun()调用的是基类Base自己的fun()函数，这里**虚函数的动态绑定机制也没有生效**，因为**此时派生类已经不存在了。**

### 总结

- 不要在构造函数中调用虚函数的原因：因为父类对象会在子类之前进行构造，此时子类部分的数据成员还未初始化， 因此调用子类的虚函数是不安全的，故而C++不会进行动态联编。
- 不要在析构函数中调用虚函数的原因：析构函数是用来销毁一个对象的，在销毁一个对象时，先调用子类的析构函数，然后再调用基类的析构函数。所以在调用基类的析构函数时，派生类对象的数据成员已经“销毁”，这个时再调用子类的虚函数已经没有意义了。



## 99. 重写、重载、隐藏

1. 函数重写：也称作覆盖，是用于类的继承中，函数名、参数个数、类型都相同，仅函数体不同。
2. 函数重载：是指**同一作用域**的不同函数使用相同的函数名，但是参数个数或类型不同。
3. 函数隐藏：既不是重载也不是重写，例如：函数名及参数完全相同却又不是虚函数，却在子类中重新实现该函数，也就是所谓的隐藏。
   

## 100. 析构函数能抛出异常吗？

不能，原因如下：

1. 如果析构函数抛出异常，则异常点之后的程序不会执行，如果析构函数在异常点之后执行了某些必要的动作比如释放某些资源，则这些动作不会执行，会造成诸如资源泄漏的问题。
2. 通常异常发生时，c++的机制会调用已经构造对象的析构函数来释放资源，此时若析构函数本身也抛出异常，则前一个异常尚未处理，又有新的异常，会造成程序崩溃的问题。

> 无法保证在析构函数中不发生异常时， 该怎么办?

```cpp
~ClassName()
{

  try{

      do_something();

  }
  catch(){  //这里可以什么都不做，只是保证catch块的程序抛出的异常不会被扔出析构函数之外。
		# 在此处处理可能的资源释放
   }
}
```

## 101.  空类里有哪些函数 

> 默认构造，析构，拷贝构造赋值那些

## 102. 多线程加锁有性能问题，怎么解决？

> 自旋锁通过CPU提供的CAS，在用户态完成加锁和解锁操作，不会主动产生线程上下文切换，所以相比互斥锁来说，会快一些开销小一些。

1. 降低锁粒度
2. 尽量减少锁持有时间
3. 用读写分离锁替换独占锁
4. 如果确定锁持有锁持有时间很短，可以使用自旋锁

## 103. sizeof一些问题

> sizeof求所占内存的大小

```cpp
char name[] = "hello"; // 因为方括号没有指定数组大小，所以长度根据实际而定，并且sizeof时会将\0加进去
char nm[10] = "hellohe"; // 里面的字符个数需满足 个数 + 1 = 10否则会编译报错！
cout << sizeof(name) << " " << sizeof(nm) << endl; // 6 10
```

> strlen时直接算实际长度，不包括\0

```cpp
cout << strlen(name) << " " << strlen(nm) << endl; // 5 7
```

## 104. 子类继承父类的函数

- 如果对于父类函数（virtual/非virtual），如果子类没有同名函数，则正常继承。
- 对于父类函数（virtual、非virtual），子类有同名函数，无同型函数，则不能调用父类函数。
- 对于父类函数（virtual、非virtual），如果有同型函数：
  非virtual函数由指针类型决定调用哪个；
  virtual函数由指针指向的对象决定调用哪个（运行时决定）。

```cpp
class BaseT{
public:
    void func(){
        printf("BaseT: func\n");
    }
    void func1(){
        printf("BaseT: func1\n");
    }
    virtual void func2(){
        printf("BaseT: func2\n");
    }
};

class SubTest : public BaseT{  //注意public要有
public:
    void func1(){
        printf("SubTest: func1\n");
    }

    void func2(){
        printf("SubTest: func2\n");
    }
};

int main()
{
	SubTest *mSubTest = new SubTest();
    mSubTest->func();		   	//非virtual , 且无同型 调用父类函数
    mSubTest->func1();       	//非virtual , 有同型 指针类型SubTest, 调用SubTest函数
    mSubTest->func2();       	//非virtual , 有同型 指针类型SubTest, 调用SubTest函数
    delete mSubTest;
    printf("=============\n");
    BaseT *mSubTest1 = new SubTest();
    mSubTest1->func();			//非virtual , 且无同型 调用父类函数
    mSubTest1->func1();  		//非virtual , 有同型 指针类型BaseT, 调用BaseT函数
    mSubTest1->func2(); 		//virtual , 有同型  对象类型SubTest, 调用SubTest函数
}
```

```shell
# 执行结果
BaseT: func
SubTest: func1
SubTest: func2
=============
BaseT: func
BaseT: func1
SubTest: func2
```

## 105. gcc相关

https://www.cnblogs.com/schips/p/gcc-about.html

## 106. 野指针

1. 未初始化的指针，指针指向不明确
2. 释放了指针，未置空却使用

## 107. const 和define的区别

1. 就**定义常量**说的话：
   const 定义的常数是变量 也带类型， #define 定义的只是个常数 不带类型。

2. 就**起作用的阶段**而言：
   define是在编译的预处理阶段起作用，而const是在 编译、运行的时候起作用。

3. 就**起作用的方式**而言：
   define只是简单的字符串替换，没有类型检查。而const有对应的数据类型，是要进行判断的，可以避免一些低级的错误。

4. 就**空间占用**而言：

   define预处理后占代码段空间，const占数据段空间。

5. 从**代码调试的方便程度**而言：
   const常量可以进行调试的，define是不能进行调试的，因为在预编译阶段就已经替换掉了

6. 从**是否可以再定义**的角度而言：
   const不足的地方，是与生俱来的，const不能重定义，而`#define`可以通过`#undef`取消某个符号的定义，再重新定义。

7. 特殊功能

   define可以防止头文件重复包含，const不能。

## 108. cpp如何保证一个类不能被继承

1. 将构造函数设置为私有。
2. 使用c++11关键字final修饰。

## 109. 静态链接和动态链接的区别

<img src="静态链接和动态链接的区别.png" alt="静态链接和动态链接的区别" style="zoom:67%;" />

## 110. 除了堆内存和栈内存，还有什么内存？

答案：静态内存

**静态内存用来保存局部static对象、类的static数据成员、以及定义在任何函数之外的变量。**

## 111. 多态分类

![多态分类](多态分类.png)

多态分为两类：**通用多态**和**特定多态**。两者的区别是前者对工作的**类型不加限制**，允许对不同类型的值执行相同的代码；后者只对**有限数量**的类型有效，而且对不同类型的值可能要执行不同的代码。

**通用多态**：

- **参数多态**（parametric）：**模板**

  采用**参数化模板**，通过给出不同的类型参数，使得一个结构有多种类型。(类似模板类吧！)

- **包含多态**（Inclusion Polymorphism）子类多态 **类继承**

  同样的操作可用于一个类型及其子类型和继承。（注意是子类型，不是子类。）包含多态一般需要进行**运行时的类型检查**。

  需要注意的地方：包含多态的操作存在着逆单调（Anti-Mornotonic）。即一个类型t上的操作，当其定义域缩小成t的一个子类型时，其值域应不小于t.

**特定多态：**

- **重载多态**（overloading）**函数重载**

  正常的重载概念，根据cpp的对函数生成的符号表进行解释。

- **强制多态**（coercion）**类重载operator type**

  > 有点类型转换的意思，回想类中可以定义operator int那种隐式转换操作。

  编译程序通过语义操作，把操作对象的类型强行加以变换，以符合函数或操作符的要求。程序设计语言中基本类型的大多数操作符，在发生不同类型的数据进行混合运算时，编译程序一般都会进行强制多态。程序员也可以显示地进行强制多态的操作(Casting)。举个例子，比如，int+double，编译系统一般会把int转换为double，然后执行double+double运算，这个int->double的转换，就实现了强制多态，即可是隐式的，也可显式转换。

## 112. 指针函数和函数指针

> 指针函数：表示的是一个函数，所以可以写作`void* func(typename...);`

> 函数指针：表示的是一个指针，所以可以写作`void (*ptr)(typename...);`

## 113. 招银二面总结

10分钟项目，20分钟折腾类和其中修饰符的差异。

> 构造函数

```cpp
class Base{
    public:
    	Base(const Base& b);
    	Base& operator=(const Base& b);
};
```

## 114. 静态对象和全局对象构造和析构顺序

静态对象比全局对象先析构。

## 115. 实现memcpy函数的迭代思路

> 先按CPU位宽进行拷贝，剩余的按字节去拷贝

```cpp
void * memcpy(void *dst,const void *src,size_t num)
{
    // sizeof(dst)在64位下为8字节，32位下为4字节
	int nchunks = num/sizeof(dst);   /*按CPU位宽拷贝*/
	int slice =   num%sizeof(dst);   /*剩余的按字节拷贝*/
	
	unsigned long * s = (unsigned long *)src;
	unsigned long * d = (unsigned long *)dst;
	
	while(nchunks--)
	    *d++ = *s++;
	    
	while (slice--)
	    *((char *)d++) =*((char *)s++);
	    
	return dst;
}
```

> 上述代码问题，如果地址不对齐，效率很低。

> 内存重叠问题

```cpp
void *memcpy(void *dest, const void *src, size_t count){
 char *d;
 const char *s;
 
 if (dest > (src + count)) || (dest < src))    {
    d = dest;
    s = src;
    while (count--)
        *d++ = *s++;        
    } else /* overlap */ {
    d = (char *)(dest + count - 1); /* offset of pointer is from 0 */
    s = (char *)(src + count -1);
    while (count --)
        *d-- = *s--;
    }
  
 return dest;
}
```

## 116. 顶层const和底层const

从右向左对应顶层和底层

## 117. 继承体系下的构造函数和析构函数的调用顺序

构造函数：先基类，再子类

析构函数：先子类，再基类

对于多继承，如`class A : public B, public C;`这种构造函数调用顺序是冒号后面按顺序，最后是当前类，所以为`B,C,A`

## 118. cpp中的5个存储区

在c++中，内存分为5个区域。分别是**堆，栈，自由存储区，全局/静态存储区和常量存储区**
     **栈** ：由编译器在需要的时候分配，在不需要的时候自动清除的变量存储区。里面通常是局部变量，函数参数等。
     **堆** ：由new分配的内存块，他们的释放编译器不去管，由我们的应用程序去控制，一般一个new对应一个delete。如果程序员没有释放掉，那么在程序结束后，操作系统会自动回收。
     **自由存储区** ：由malloc等分配的内存块，和堆十分相似，不过它使用free来结束自己的生命。
     **全局/静态存储区** ：全局变量和静态变量被分配到同一块内存中，**在以前的c语言中。全局变量又分为初始化的和未初始化的，在c++里面没有这个区分了，他们共同占用同一块内存。**
     **常量存储区** ：这是一块比较特殊的存储区，里面**存放的是常量，不允许修改。**

## 119. unordered_map中的哈希表

首先会根据key值使用hash函数计算一个哈希值，但由于该值不一定在所对应的桶的下标索引范围内，所以需要进行映射，也就是对桶个数取余（默认桶个数为1，有扩容机制1，3，7， 17（下一个素数）这种规律）

## 120. 跨平台设计

[c++跨平台开发技术总结 ](https://www.cnblogs.com/end-emptiness/p/15083829.html)