# Qt入门

## 第一个Qt程序

- 包含一个pro（project）文件，一个用户自定义头文件，和两个源文件

### 各文件的简单解释

- project文件

```c++
#加载模块 core核心模块 gui 界面模块
QT       += core gui

#当Qt版本大于4 Qt5需要加上widgets模块
greaterThan(QT_MAJOR_VERSION, 4): QT += widgets

#配置C++11 
CONFIG += c++11

#使用过时的函数会警告
# The following define makes your compiler emit warnings if you use
# any Qt feature that has been marked deprecated (the exact warnings
# depend on your compiler). Please consult the documentation of the
# deprecated API in order to know how to port your code away from it.
DEFINES += QT_DEPRECATED_WARNINGS

# You can also make your code fail to compile if it uses deprecated APIs.
# In order to do so, uncomment the following line.
# You can also select to disable deprecated APIs only up to a certain version of Qt.
#DEFINES += QT_DISABLE_DEPRECATED_BEFORE=0x060000    # disables all the APIs deprecated before Qt 6.0.0

#项目里的源文件
SOURCES += \
    main.cpp \
    mywidget.cpp

#项目里的头文件
HEADERS += \
    mywidget.h

# Default rules for deployment.
qnx: target.path = /tmp/$${TARGET}/bin
else: unix:!android: target.path = /opt/$${TARGET}/bin
!isEmpty(target.path): INSTALLS += target

```

- 头文件

```c++
#ifndef MYWIDGET_H
#define MYWIDGET_H

#include <QWidget>		//包含头文件 Qwidget 窗口类

class myWidget : public QWidget		//myWidget类 公有继承 QWidget类
{
    Q_OBJECT		// Q_OBJECT宏，允许类中使用信号和槽机制

public:
    myWidget(QWidget *parent = nullptr);		//构造函数
    ~myWidget();		//析构函数
};
#endif		// MYWIDGET_H

```

- 含main()的源文件

```c++
#in#include "mywidget.h"       //自定义的头文件
#include <QApplication>     //包含一个应用程序类的头文件

int main(int argc, char *argv[])    //mian程序入口  argc命令行变量的数量，argv命令行变量的数组
{
    
    QApplication a(argc, argv);     //a应用程序对象，在Qt中，应用程序对象有且只有一个
 
    myWidget w;     //窗口对象 mywidget父类 ->Qwidget
  
    w.show();       //窗口对象 默认不会显示，用成员函数show()显示窗口

    return a.exec();        //让应用程序对象进入消息循环
}
```

- 与头文件相对应的源文件

```c++
#include "mywidget.h"

myWidget::myWidget(QWidget *parent)		//构造函数
    : QWidget(parent)		//用初始化列表把parent初始化
{
}

myWidget::~myWidget()		//析构函数
{
}
```

------

## 快捷键

```c++
//注释 						ctrl + /
//运行						ctrl + r
//编译						ctrl + b
//查找	 					ctrl + f
//整行移动	 				   ctrl + shift + ↑ 或者 ↓
//帮助文档	 				   F1
//自动对其	 				   ctrl + i
//同名之间的.h 和 .cpp切换 		 F4
```

------

## 创建按钮

```c++
#include "mywidget.h"		//自定义的头文件
#include<QPushButton> 		//控制按钮头文件

myWidget::myWidget(QWidget *parent)
    : QWidget(parent)		//用初始化列表把parent初始化
{
    QPushButton *btn = new QPushButton;		//创建一个按钮

    //btn->show();     //show()是以顶层的方式弹出窗口控件
        
    btn->setParent(this);			//设置按钮在窗口中弹出
  
    btn->setText("第一个按钮");		//显示文本

    QPushButton *btn2 = new QPushButton("第二个按钮",this);		//创建第二个按钮，（此时btn1与btn2会重合）
    
    btn2->move(100,100);		//移动btn2按钮
    
    btn2->resize(100,100);		//重置按钮大小
    
    resize(600,400);		//重置窗口大小, 用户可通过拉伸改变窗口大小
    
    setFixedSize(600,400);		//设置固定窗口大小，用户不可通过拉伸改变窗口大小
        
    setWindowTitle("第一个窗口");		//设置窗口标题

}

myWidget::~myWidget()
{
}
```

------

## 对象树

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fwww.pianshen.com%2Fimages%2F945%2F5495fafa182a909a59b6e1236f707061.png&refer=http%3A%2F%2Fwww.pianshen.com&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=jpeg?sec=1629515875&t=11fe3f0e34cc3282aad5157a843650a3)

### 样例

- 右击pro文件，添加C++ Class文件（添加类），这里我的类名是：MyPushbutton，设置父类，选择**QPushButton**类（ QPushButton是QWidget的子类 ）

*myPushbutton.h*

```c++
#ifndef MYPUSHBUTTON_H
#define MYPUSHBUTTON_H

#include <QPushButton>

class MyPushButton : public QPushButton//继承了按钮类
{
    Q_OBJECT
public:
    explicit MyPushButton(QWidget *parent = nullptr);

    ~MyPushButton();
signals:

};

#endif // MYPUSHBUTTON_H
```

*myPushbutton.cpp*

```c++
#include "mypushbutton.h"
#include<QDebug>

MyPushButton::MyPushButton(QWidget *parent) : QPushButton(parent)
{
    qDebug()<<"按钮类的构造调用";
}

MyPushButton::~MyPushButton()
{
    qDebug()<<"按钮类的析构调用";
}
```

*mywidget.cpp*

```c++
#include "mywidget.h"		//自定义的头文件
#include<QPushButton> 		//控制按钮头文件
#include<mypushbutton.h>    //自定义的继承按钮类的子类头文件

myWidget::myWidget(QWidget *parent)
    : QWidget(parent)		//用初始化列表把parent初始化
{
    QPushButton *btn = new QPushButton;		//创建一个按钮

    //btn->show();     //show()是以顶层的方式弹出窗口控件

    btn->setParent(this);		//设置按钮在窗口中弹出

    btn->setText("第一个按钮");		//显示文本

    QPushButton *btn2 = new QPushButton("第二个按钮",this);		//创建第二个按钮，（此时btn1与btn2会重合）

    btn2->move(100,100);		//移动btn2按钮

    btn2->resize(100,100);		//重置按钮大小

    resize(600,400);		//重置窗口大小, 用户可通过拉伸改变窗口大小

    setFixedSize(600,400);		//设置固定窗口大小，用户不可通过拉伸改变窗口大小

    setWindowTitle("第一个窗口");		//设置窗口标题

    //创建一个自定义按钮对象
    MyPushButton *myBtn = new MyPushButton;
    myBtn->setText("取消");
    myBtn->move(200,0);
    myBtn->setParent(this);

}

myWidget::~myWidget()
{
}
```

*当关闭窗口后，析构函数被调用，释放内存*

------



## 信号和槽

参考网址：[Qt 之 emit、signals、slot的使用 - 黄树超 - 博客园 (cnblogs.com)](https://www.cnblogs.com/schips/p/12537360.html)

### 信号

​		当某个信号对其客户或所有者发生的内部状态发生改变，信号被一个对象发射。只有定义过这个信号的类及其派生类能够发射这个信号。
​		**当一个信号被发射时，与其相关联的槽将被立刻执行，就象一个正常的函数调用一样**。信号-槽机制完全独立于任何 GUI 事件循环。只有当所有的槽返回以后发射函数（emit）才返回。
**如果存在多个槽与某个信号相关联，那么，当这个信号被发射时，这些槽将会一个接一个地执行，但是它们执行的顺序将会是随机的、不确定的，我们不能人为地指定哪个先执行、哪 个后执行。**

​		信号的声明是在头文件中进行的，QT 的 signals 关键字指出进入了信号声明区，随后即可声明自己的信号。例如，下面定义了三个信号：

```cpp
signals:
    void mySignal();
    void mySignal(int x);
    void mySignalParam(int x,int y);
```

在上面的定义中，signals 是 QT 的关键字，而非 C/C++ 的。
接下来的一行 `void mySignal()` 定义了信号 `mySignal`，这个信号没有携带参数；
接下来的一行 void mySignal(int x) 定义 了重名信号 mySignal，但是它携带一个整形参数，这有点类似于 C++ 中的虚函数。
从形式上讲信号的声明与普通的 C++ 函数是一样的，**但是信号却没有函数体定义**，**另外，信号的返回类型都是 void，不要指望能从信号返回什么有用信息**。信号由 moc 自动产生，它们不应该在 .cpp 文件中实现。

------

### 槽

​		槽是普通的 C++ 成员函数，可以被正常调用，它们唯一的特殊性就是很多信号可以与其相关联。当与其关联的信号被发射时，这个槽就会被调用。槽可以有参数，但槽的参数不能有缺省值。既然槽是普通的成员函数，因此与其它的函数一样，它们也有存取权限。槽的存取权限决定了谁能够与其相关联。同普通的 C++ 成员函数一样，槽函数也分为三种类型，即 public slots、private slots 和 protected slots。

> public slots：在这个区内声明的槽意味着任何对象都可将信号与之相连接。这对于组件编程非常有用，你可以创建彼此互不了解的对象，将它们的信号与槽进行连接以便信息能够正确的传递。
>
> protected slots：在这个区内声明的槽意味着当前类及其子类可以将信号与之相连接。这适用于那些槽，它们是类实现的一部分，但是其界面接口却面向外部。
>
> private slots：在这个区内声明的槽意味着只有类自己可以将信号与之相连接。这适用于联系非常紧密的类。

​		槽也能够声明为虚函数，这也是非常有用的。

​		槽的声明也是在头文件中进行的。例如，下面声明了三个槽：

```cpp
public slots:
   void mySlot();
   void mySlot(int x);
   void mySignalParam(int x,int y);
```

------

### 信号与槽的关联

#### connect()

**功能：**通过调用 QObject 对象的 connect 函数来将某个对象的信号与另外一个对象的槽函数相关联，这样当发射者发射信号时，接收者的槽函数将被调用。

**原型：**

```cpp
//原版
bool QObject::connect ( const QObject * sender, const char * signal,const QObject * receiver, const char * member ) [static]
 //简化版   
    bool connect(sender, signal, receiver, slot);
```

**参数：**

sender：发出信号的对象

signal：发送对象发出的信号

receiver：接收信号的对象

slot：接收对象在接收到信号之后所需要调用的函数		

​		这个函数的作用就是将发射者 sender 对象中的信号 signal 与接收者 receiver 中的 member 槽函数联系起来。当指定信号 signal 时必须使用 QT 的宏 SIGNAL()，当指定槽函数时必须使用宏 SLOT()。**如果发射者与接收者属于同一个对象的话，那么在 connect 调用中接收者参数可以省略**。

​		例如，下面定义了两个对象：标签对象 label 和滚动条对象 scroll，并将 valueChanged() 信号与标签对象的 setNum() 相关联，另外信号还携带了一个整形参数，这样标签总是显示滚动条所处位置的值。

```cpp
QLabel     *label  = new QLabel;
QScrollBar *scroll = new QScrollBar;
QObject::connect( scroll, SIGNAL(valueChanged(int)),label,  SLOT(setNum(int)) );//Qt4
```

------

### 自定义信号和槽

**样例**

*需求：*

- 下课后，老师会触发一个信号，饿了，学生响应信号，请客吃饭

**创建老师类**

teacher.h

```c++
#ifndef TEACHER_H
#define TEACHER_H

#include <QObject>

class Teacher : public QObject
{
    Q_OBJECT
public:
    explicit Teacher(QObject *parent = nullptr);

signals:    //信号
    //自定义信号 写道signals下
    //返回值是void，只需要声明，不需要实现
    //可以有参数，可以重载
    void hungry();

    void hungry(QString foodName);

public slots:   //槽
};

#endif // TEACHER_H
```

teacher.cpp

```c++
#include "teacher.h"

Teacher::Teacher(QObject *parent) : QObject(parent)
{

}
```

**创建学生类**

student.h

```c++
#ifndef STUDENT_H
#define STUDENT_H

#include <QObject>

class Student : public QObject
{
    Q_OBJECT
public:
    explicit Student(QObject *parent = nullptr);

signals:    //信号

public slots:   //槽
    //早期Qt版本必须要写到public slots，高级版本可以写到public或者全局下
    //返回值void，需要声明，也需要实现
    //可以有参数，可以发生重载
    void treat();		//无参

    void treat(QString foodName);		//有参
};

#endif // STUDENT_H
```

student.cpp

```c++
#include "student.h"
#include<QDebug>
Student::Student(QObject *parent) : QObject(parent)
{

}
void Student::treat()
{
    qDebug()<<"请老师吃饭";
}
void Student::treat(QString foodName)
{
    //QString -> char * 先转成 QByteArry (.toUtf8()) 再转char * (char* （data())
    qDebug()<<"请老师吃饭，老师要吃："<<foodName.toUtf8().data();
}
```

**窗口类**

widget.h

```c++
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include"student.h"
#include"teacher.h"

QT_BEGIN_NAMESPACE
namespace Ui { class Widget; }
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

public:
    Widget(QWidget *parent = nullptr);
    ~Widget();

private:
    Ui::Widget *ui;

    Teacher *zt;
    Student *st;

    void classIsOver();
};
#endif // WIDGET_H
```

widget.cpp

```c++
#include "widget.h"
#include "ui_widget.h"
#include<QPushButton>
//Teacher Class
//Student Class
//下课后，老师会触发一个信号，饿了，学生响应信号，请客吃饭


Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);

    //创建老师对象
    this->zt= new Teacher(this);

    //创建学生对象
    this->st = new Student(this);

    //老师饿了 学生请客的连接
    //connect(zt,&Teacher::hungry,st,&Student::treat);

    //调用下课函数
    //classIsOver();
        
 /* 连接带参数的信号和槽
    指针 ->地址
    函数指针->函数地址 */
    void(Teacher:: *teacherSignal)(QString) = &Teacher::hungry;
    void(Student:: *studentSlot)(QString) = &Student::treat;
    connect(zt,teacherSignal,st,studentSlot);
    classIsOver();

    //点击一个 下课按钮 再触发下课
    QPushButton *btn = new QPushButton("下课",this);

    //重置窗口大小
    this->resize(600,400);

    //点击按钮 触发下课
    //connect(btn,&QPushButton::clicked,this,&Widget::classIsOver);

    //无参信号和槽连接
    void(Teacher::*teacherSignal2)(void) = &Teacher::hungry;
    void(Student::*studentSlot2)(void) = &Student::treat;
    connect(zt,teacherSignal2,st,studentSlot2);

   //信号连接信号
   connect(btn,&QPushButton::clicked,zt,teacherSignal2);

   //断开信号
   //disconnect(zt,teacherSignal2,st,studentSlot2);

}

void Widget::classIsOver()
{
    //下课函数，调用后 触发老师饿了的信号
    //emit zt->hungry();
    emit zt->hungry("宫保鸡丁");//emit 关键字作用：发射信号
}

Widget::~Widget()
{
    delete ui;
}
```

**main()**

```c++
#include "widget.h"
#include <QApplication>

int main(int argc, char *argv[])
{
    QApplication a(argc, argv);
    Widget w;
    w.show();
    return a.exec();
}
```

### Lambda表达式

​		C++中，一个lambda表达式表示一个可调用的代码单元。我们可以将其理解为一个未命名的内联函数。它与普通函数不同的是，lambda必须使用尾置返回来指定返回类型。

例如调用<algorithm>中的std::sort，ISO C++ 98 的写法是要先写一个compare函数：

```c++
bool compare(int& a,int& b){
    return a>b;
}
```

然后，再这样调用：

```
sort(a, a+n, compare);
```

然而，用ISO C++ 11 标准新增的Lambda表达式，可以这么写：

```c++
sort(a, a+n, []( int a,int b){return a>b;});//降序排序
```

这样一来，代码明显简洁多了。

由于Lambda的类型是单一的，不能通过类型名来显式声明对应的对象，但可以利用auto关键字和类型推导：

```c++
auto f=[](int a,int b){return a>b;};
```

和其它语言的一个较明显的区别是Lambda和C++的类型系统结合使用，如：

```c++
auto f=[x](int a,int b){return a>x;};//x被捕获复制
int x=0, y=1; auto g=[&]( int x){ return ++y;};//y被捕获引用，调用g后会修改y，需要注意y的生存期
bool (*fp)(int,int)=[](int a, int b){return a>b;};//不捕获时才可转换为函数指针
```

### 项目一

- 点击“open”按钮打开新窗口，点击“close”按钮关闭打开的窗口

```c++
#include"widget.h"
#include"ui_widget.h"
#include <QWidget>
#include <QPushButton>

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);
    QWidget*one=new QWidget;
    QPushButton*brt=new QPushButton;
    QPushButton*brt2=new QPushButton;
    brt->setParent(this);
    brt2->setParent(this);
    resize(500,500);
    brt->move(100,200);
    brt2->move(300,200);
    brt->setParent(this);
    brt->setText("open");
    brt2->setParent(this);
    brt2->setText("close");
    setWindowTitle("作业");
    connect(brt,&QPushButton::clicked,one,[=](){
            one->show();
    });
    connect(brt2,&QPushButton::clicked,one,[=](){
            if(brt2->text()=="close"){
             one->close();
             brt2->setText("open");
            }
            else{
                one->show();
                brt2->setText("close");
            }
    });
}
Widget::~Widget()
{
    delete ui;
}
```

------



## QMainWindow

​		Qt中的顶层窗口称为**MainWindow**，属于类QMainWindow，QMainWindow也是**继承于QWidget**。通过子类化QMainWindow可以创建一个应用程序的窗口。

​		MainWindow的结构分为五个部分：菜单栏（MenuBar）、工具栏（Toolbars）、浮动窗口（DockWidgets）、状态栏（StatusBar）和中央窗口（CentralWidget）。

------

### MenuBar

```c++
 	//创建菜单栏 只能有一个
    QMenuBar *bar = menuBar();
    setMenuBar(bar);        //将菜单栏放入到窗口中

	//创建菜单
    QMenu *fileMenu = bar->addMenu("文件");
    QMenu *editMenu = bar->addMenu("编辑");

    //创建文件项
    QAction * newAction = fileMenu->addAction("新建");
    fileMenu->addSeparator();       //添加分割线
    QAction *openAction = fileMenu->addAction("打开");
```

------

### ToolBar

```c++
	//创建工具栏 可以有多个
    QToolBar * toolBar = new QToolBar(this);
    addToolBar(Qt::LeftToolBarArea,toolBar);

    //固定工具栏 只允许左右停靠
    toolBar->setAllowedAreas(Qt::LeftToolBarArea | Qt::RightToolBarArea);//运算符重载

    //设置浮点
    toolBar->setFloatable(false);

    //设置移动 (总开关)
    toolBar->setMovable(false);

    //设置工具栏中内容
    toolBar->addAction(newAction);
    toolBar->addAction(openAction);

    //工具栏中添加控件
    QPushButton * btn = new QPushButton("aaa",this);
    toolBar->addWidget(btn);
```

------

### StatusBar

```c++
    //状态栏 最多一个
    QStatusBar *stBar = statusBar();
    setStatusBar(stBar);        //设置到窗口中

	//设置标签控件
    QLabel *label1 = new QLabel("提示信息",this);   
    stBar->addWidget(label1);
    QLabel *label2 = new QLabel("提示信息",this);
    stBar->addPermanentWidget(label2);
```

------

### DockWidgets

```c++
	//铆接窗口 （浮动窗口）可有多个
    QDockWidget * dockWidget = new QDockWidget("浮动",this);
    addDockWidget(Qt::BottomDockWidgetArea,dockWidget);

    //固定浮动窗口停靠位置
    dockWidget->setAllowedAreas(Qt::TopDockWidgetArea | Qt::BottomDockWidgetArea);//上下停靠
```

------

### CentralWidget

```c++
	//设置中心部件
    QTextEdit *edit = new QTextEdit;//设置文字编辑
    setCentralWidget(edit);
```

------

### 资源文件添加

- 访问电脑中的文件来设置菜单图标

```c++
ui->actionnew->setIcon(QIcon("D:/Picture/000.jpg"));
```

- 调用项目中的文件来设置菜单图标

```c++
 	//添加资源文件 ": + 前缀名 + 文件名"
    ui->actionnew->setIcon(QIcon(":/Image/000.jpg"));
    ui->actionopen->setIcon(QIcon(":/Image/001.jpg"));
```

------

## 对话框

**样例：点击按钮 弹出对话框**

- 模态对话框 （不可以对其他窗口进行操作）

```c++
	connect(ui->actionnew,&QAction::triggered,[=](){
        //模态创建 阻塞 exec()
        QDialog dlg(this);
        dlg.resize(200,100);
        dlg.exec();
        qDebug()<<"模块对话框弹出";
    });
```

- 非模态对话框（可以对其他窗口进行操作）

```c++
 	connect(ui->actionopen,&QAction::triggered,[=](){
        //非模态对话框 展示 show()
          QDialog * dlg2 = new QDialog(this);
          dlg2->resize(200,100);
          dlg2->show();
          dlg2->setAttribute(Qt::WA_DeleteOnClose);//防止多次操作生成非模态对话框导致内存溢出
          qDebug()<<"非模态对话框弹出";
    });
```

------

### 消息对话框

属于：QMessageBox类。

#### critical()

```c++
	//错误对话框
    QMessageBox::critical(this,"critical","错误");
```

------

#### information()

```c++
	//信息对话框
    QMessageBox::information(this,"info","信息");
```

------

#### question()

```c++
    //提问对话框
    //参数：父亲，标题，提示内容，按键类型，默认关联回车按键
    QMessageBox::question(this,"ques","提问",QMessageBox::Save|QMessageBox::Cancel,QMessageBox::Save);
```

------

#### warning()

```c++
    //警告对话框
    QMessageBox::warning(this,"warning","警告");
```

------

### 其他对话框

#### getColor()

```c++
    //颜色对话框
    QColor color = QColorDialog::getColor(QColor(255,0,0));
    qDebug()<<"r = "<<color.red()<<"g = "<<color.green()<<" b = "<<color.blue();
```

------

#### getOpenFileName()

```c++
	//文件对话框
    //返回值 选取的路径
    QString str = QFileDialog::getOpenFileName(this, "打开文件", "D:\\","(*.txt)");
	qDebug()<<str<<endl;
```

------

#### getFont()

```c++
	//字体对话框
    bool flag;
    QFont font = QFontDialog::getFont(&flag,QFont("仿宋",36));
    qDebug()<<"字体："<<font.family()<<"字号"<<font.pointSize()<<"是否加粗"<<font.italic();
```

------

