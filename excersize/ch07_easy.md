# 设计并测试一个名为 Rectangle 的矩形类，其属性为矩形的左下角与右上角两个点的坐标，根据坐标能计算矩形的面积。

```cpp
#include <iostream>
#include <string>
using std::cin; using std::cout; using std::endl; using std::string;

typedef struct Point {
    double x;
    double y;
} Point;

class Rectangle{
    public:
        double get_area ();
        void set_point_1(double x, double y){
            this->point_1.x = x;
            this->point_1.y = y;
        };
        void set_point_2(double x, double y){
            this->point_2.x = x;
            this->point_2.y = y;
        };
        Rectangle(double x_1, double y_1, double x_1, double y_1){
            point_1.x = x_1;
            point_1.y = y_1;
            point_2.x = x_2;
            point_2.y = y_2;
        }
    private:
        Point point_1;
        Point point_2;
};

double Rectangle::get_area(){
    return (this->point_2.y - this->point_1.y) * (this->point_2.x - this->point_1.x)
};
```

# 设计一个用于人事管理的“人员”类。由于考虑到通用性，这里只抽象出所有类型人员都具有的属性：编号、性别、出生日期、身份证号等。其中“出生日期”声明为一个“日期”类内嵌子对象。用成员函数实现对人员信息的录人和显示。要求包括：构造函数和析构函数、复制构造函数、内联成员函数、带默认形参值的成员函数、类的组合

```cpp
#include<iostream>
#include<stdlib.h>
using namespace std;

class Date {
private:
    int year;
    int month;
    int day;
public:
    Date() {}
    Date(int year, int month, int day);
    void showDate();
    void setDate(int year, int month, int day);
    ~Date() { }
};

class Person {
public:
    Person(long num, char sex, long id, int year, int month, int day): date(year, month, day)
    {
        this->num = num;
        this->sex = sex;
        this->id = id;
    }
    Person(int year, int month = 1, int day = 1) : date(year, month, day) {}
    Person() {}
    Person(Person &p);
    void setPerson();
    void showPerson();
    ~Person() { }               // 析构函数

private:
    long num;
    char sex;
    Date date;
    long id;
};

Date::Date(int year, int month, int day)
{
    this->year = year;
    this->month = month;
    this->day = day;
}
void Date::showDate()
{
    cout << "year-month-day:  " << year << "-" << month << "-" << day << endl;
}
void Date::setDate(int year, int month, int day)
{
    this->year = year;
    this->month = month;
    this->day = day;
}

Person::Person(Person &p)
{
    this->num = p.num;
    this->id = p.id;
    this->sex = p.sex;
}

void Person::showPerson()
{
    cout << "num: " << num << endl;
    cout << "sex: " << sex << endl;
    cout << "date: ";
    date.showDate();
    cout << "id: " << id << endl;
}

void Person::setPerson()
{
    int year;
    int month;
    int day;
    cout << "请输入编号: ";
    cin >> this->num;
    cout << "请输入性别: ";
    cin >> this->sex;
    cout << "请输入身份证号: ";
    cin >> this->id;
    cout << "请输入出生年月日:" << endl;
    cin >> year >> month >> day;
    this->date.setDate(year,month,day);
}

int main()
{
    Person p;
    p.setPerson();
    p.showPerson();

    system("pause");
    return 0;
}
```

# 定义一个 DataType(数据类型)类，能处理包含字符型、整型、浮点型 3 种类型的数据，给出其构造函数

```cpp
#include<iostream>
#include<stdlib.h>
using namespace std;

class DataType {
private:
    // 枚举类型, 作类型判断
    enum {
        character,
        integer,
        floating_point
    } vartype;
    // 无名联合体做类的内嵌成员, 联合体内部成员可直接访问
    union {
        char c;
        int i;
        float f;
    };
public:
    DataType(char ch)
    {
        this->vartype = character;
        this->c = ch;
    }
    DataType(int li)
    {
        this->vartype = integer;
        this->i = li;
    }
    DataType(float ff)
    {
        this->vartype = floating_point;
        this->f = ff;
    }
    void print();
 };

void DataType::print()
{
    switch (vartype)
    {
    case character:
        cout << "字符型: " << c << endl;
        break;
    case integer:
        cout << "整型: " << i << endl;
        break;
    case floating_point:
        cout << "浮点型: " << f << endl;
        break;
    }
}

int main()
{
    DataType d1('a'), d2(12), d3(1.5f);
    d1.print();
    d2.print();
    d3.print();

    system("pause");
    return 0;
}
```
