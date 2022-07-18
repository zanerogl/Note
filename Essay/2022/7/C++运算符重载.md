# C++运算符重载

重载的运算符是带有特殊名称的函数，函数名是由关键字 **operator** 和其后要重载的运算符符号构成的。与其他函数一样，重载运算符有一个返回类型和一个参数列表。

```c++
Box operator+(const Box&);
```

声明加法运算符用于把两个 Box 对象相加，返回最终的 Box 对象。大多数的重载运算符可被定义为普通的非成员函数或者被定义为类成员函数。如果我们定义上面的函数为类的非成员函数，那么我们需要为每次操作传递两个参数，如下所示：

```c++
Box operator+(const Box&, const Box&);
```

**示例代码**

```c++
#include <iostream>
using namespace std;

class Box
{
public:
	void setLength(double Length)	//设置长度
	{
		length = Length;
	}
	void setBreadth(double Breadth)	//设置宽度
	{
		breadth = Breadth;
	}
	void setHeight(double Height)	//设置高度
	{
		height = Height;
	}
	double getVolume(void)			//计算体积
	{
		return length * breadth * height;
	}
	
	//重载 + 运算符，用于两个Box的相加
	Box operator+(const Box& b)
	{
		Box box;
		box.length = this->length + b.length;
		box.breadth = this->breadth + b.breadth;
		box.height = this->height + b.height;
		return box;
	}
	
	//重载 = 运算符，用比较两个Box是否相等
	bool operator==(const Box& b)
	{
		bool yon;
		if(this->length == b.length && this->breadth == b.breadth && this->height == b.height)
		{
			yon = true;
		}else{
			yon = false;
		}
		return yon;
	}
	
private:
	double length;
	double breadth;
	double height;
};


int main()
{
	Box box0, box1;
	
	//设置box0的属性
	box0.setLength(10.0);
	box0.setBreadth(15.0);
	box0.setHeight(10.0);
	
	//设置box1的属性
	box1.setLength(15.0);
	box1.setBreadth(20.0);
	box1.setHeight(10.0);
	
	//获取box0和box1的体积
	cout<<"The volume of box0: "<<box0.getVolume()<<endl;
	cout<<"The volume of box1: "<<box1.getVolume()<<endl;
	
	//计算box0和box1相加后的体积
	cout<<(box0+box1).getVolume()<<endl;
	
	//判断box0和box1是否相等
	if(box0 == box1)
	{
		cout<<"box0==box1"<<endl;
	}
	else
	{
		cout<<"box0!=box1"<<endl;
	}
	return 0;
}
```

*小结：目前理解为运算符重载主要服务于类对象之间的逻辑运算*