# 面向对象技术
上午单选11分，下午2道案例分析30分（UML+设计模式）
## 基础
### 什么是面向对象（OO）
（1）面向对象和面向过程的区别？

`面向过程`：将问题分解为具体**步骤**，按序执行，需亲力亲为每个环节

`面向对象`：不关注具体实现细节，只关注**对象**的操作和交互（对象仍需面向过程来实现功能）

思维：一切皆对象，所有对象又被赋予了`属性`和`方法`来模拟现实世界，属性即名词，方法即动词

（2）**面向对象的底层依赖面向过程**

（如我们给洗衣机洗衣服，我们面向对象，洗衣机内部仍需按步骤完成洗涤的过程）

（3）什么是面向对象？

`概述-特点-举例-总结`

概述：面向对象是一种 *编程思维*，强调 *以对象为基础* 完成各种操作，它是 *基于面向过程* 的。

三大特点：更符合人们的思考习惯、把复杂问题简单化、把程序员从执行者变成指挥者

举例：选择环境中的实际对象举例

总结：一切皆对象
<br><br>

### 核心概念
对象：包括数据(属性)和作用于数据的操作(行为)

`属性`：名词，描述特征，相当于变量

`方法`：动词，描述操作，相当于函数

可以说，对象把属性和行为封装为一个整体

`对象 = 对象名 + 属性 + 方法（行为在代码层的具体实现）`

通信：对象间通过消息传递（对象引用）进行通信

类：把一组对象的共同特征加以抽象，存储在类中

>类分为：实体类、接口类、控制类
>
>实体类对象：具体实体
>
>接口类对象：接口（用户和系统交互的接口），比如显示屏、窗口等
>
>类比一下，实体类相当于菜、顾客，接口类相当于服务员、菜单，控制类相当于调度员

类和对象的关系:

类：创建对象的模板，是对对象的抽象，看不见摸不着

对象：类的具体化，是类的实现，看得见摸得着
<br><br>

### 基本语法

```python
# 定义类
class Car():
    def run(self): # 定义类的方法时必须包含self（谁调用，self就代表谁）
        print("run")
    def work(self):
        self.run() # 类内部，通过self.的方式调用其他成员
        print("work")

# 测试代码写main里，只在运行该文件时执行，其他文件导入模块时不执行
if __name__ == '__main__': # 输入main回车就会自动补全
    # 创建对象
    car = Car()
    # 调用方法
    car.work()
    # 类外添加的属性只属于当前对象，其他同类对象不具备
    car.color = "red"
    print(f"汽车颜色:{car.color}")
```

注意：
1.定义类有三种格式：不带括号、带括号、括号里是object，三者功能完全相同，其中object是所有类的父类

2.只能在类外给对象添加属性，一个个添加很麻烦，因此后续会学习通过`__init__`魔法方法在创建对象时自动初始化属性

2.快捷键技巧：`Ctrl+D`复制当前行，`Alt+Shift+方向键`移动当前行，按住`Ctrl`再点击变量会跳转到定义变量的位置（用于debug）
<br><br>

### 三大特性
封装、继承、多态
#### 1. 封装
##### 1.1 基本概念
`封装`：将属性和方法封装成类，隐藏实现细节，仅提供公共接口

##### 1.2 私有化
`私有化`：隐藏父类的某些属性或方法，让子类无法访问

私有化：`__属性名`或 `__方法名()` （双下划线）

获取私有化属性：定义`get_××`方法

修改私有化属性：定义`set_××`方法

获取私有化方法：定义一个公有方法来访问私有方法（相当于给了一个公有接口）

例子：

```python
# 私有化
class Prentice(object):
    def __init__(self):
        self.kongfu = '独创煎饼果子技术'
        self.__money = 500 #私有化money
    def make_cake(self):
        print(f'采用{self.kongfu}制作煎饼果子')
    # 提供公共接口访问money
    def get_money(self):
        return self.__money
    # 提供公共接口修改money
    def set_money(self, money):
        self.__money = money
class TuSun(Prentice):
    pass

if __name__ == '__main__':
    ts = TuSun()
    print(ts.get_money())
    ts.set_money(600)
    print(ts.get_money())
```

##### 1.3 优缺点
优点：

1.简化编程

2.提高安全性（类）

3.提高复用性（函数）

缺点：

增加代码量
<br>

#### 2. 继承
##### 2.1 基本概念
`继承`：允许一个类（子类）获取另一个类（父类）的属性和方法

`父类`又称基类/超类，`子类`又称派生类/扩展类

继承（子类角度）：获取父类成员特性(公共特性）

`派生`（父类角度）：产生新的子类

##### 2.2 基本格式
```python
class 父类(object):
    定义属性和方法
```

```python
class 子类(父类1，父类2...):
    pass
```
所有类默认继承自object类（顶级基类）

##### 2.3 继承的种类
`单（重）继承`：单一父类

`多（重）继承`：多个父类

当多个父类有重名属性或方法时，*优先使用第一个父类*（遵循MRO规则）

MRO规则：Method Resolution Order（方法解析顺序），先找第一个父类，如果没有再找第二个父类，以此类推

（子类自己-->第一个父类-->第二个父类-->...-->object）

MRO调用方式：`类名.__mro__`或`类名.mro()`，查看实际顺序

`多层继承`：继承链（继承关系的传递性），A-->B-->C-->D

实际开发中建议继承深度不超过3层，避免过度耦合

##### 2.4 重写（覆盖）
子类在继承父类的同时想对父类进行改造，有自己的独有需求时，可以*重新定义父类的属性或方法*，即`重写`或`方法重写`，实际更常用的是方法重写

根据MRO规则，重写后*优先使用子类自己的属性或方法*

重写的优点：提高扩展性

##### 2.5 super关键字
重写后子类如何访问父类的成员？

1.`父类名.父类方法名(self)`  用于多继承，精准初始化某个父类属性

2.`super().父类方法名()` 单继承

这里需要用到super关键字

super关键字：

（1）概述：代表 本类当前对象 父类的引用

（2）简单理解：**self代表自己，super代表父类**

（3）适用情况：super()只能初始化第一个父类的成员，所以super不适合多继承，更适合单继承

（4）重点：随着代码不断往下执行，**后面的初始化覆盖前面的，self也会变成不断被覆盖**

例子：
（1）单继承

```python
# 摊煎饼案例（单继承 + 重写）
# 师傅类
class Master(object):
    # 构造方法，给类的对象初始化属性
    def __init__(self): 
        self.kongfu = '古法摊煎饼果子技术'
    # make_cake方法
    def make_cake(self): 
        print(f'采用{self.kongfu}制作煎饼果子')
# 徒弟类
class Prentice(Master):
    # 重写属性和方法
    def __init__(self):
        self.kongfu = '独创煎饼果子技术'
    # 调用徒弟自己的方法，可以不写，直接继承
    def make_cake(self):
        print(f'采用{self.kongfu}制作煎饼果子')
    # 调用老师傅的方法
    def make_master_cake(self):
        super().__init__() # 一定要先初始化父类的属性！！！确保读到正确的属性值(kongfu)
        super().make_cake()

if __name__ == '__main__':
    p = Prentice() # kongfu是独创
    p.make_cake() # kongfu是独创
    p.make_master_cake() # 调用super().__init__(),kongfu变为古法
```

（2）多继承

```python
# 摊煎饼案例（多继承 + 重写）
# 师傅类
class Master(object):
    def __init__(self):
        self.kongfu = '古法摊煎饼果子技术'
    def make_cake(self):
        print(f'采用{self.kongfu}制作煎饼果子')
# 学校类
class School(object):
    def __init__(self):
        self.kongfu = '黑马AI摊煎饼果子技术'
    def make_cake(self):
        print(f'采用{self.kongfu}制作煎饼果子')
# 徒弟类
class Prentice(School, Master):
    # 重写属性和方法
    def __init__(self):
        self.kongfu = '独创煎饼果子技术'
    # 调用徒弟自己的方法，可以不写，直接继承
    def make_cake(self):
        print(f'采用{self.kongfu}制作煎饼果子')
    # 调用老师傅的方法
    def make_master_cake(self):
        Master.__init__(self) # 一定要先初始化父类的属性！！！
        Master.make_cake(self)
    # 调用学校的方法
    def make_school_cake(self):
        School.__init__(self)
        School.make_cake(self)

if __name__ == '__main__':
    p = Prentice()
    p.make_cake()
    p.make_master_cake()
    p.make_school_cake()
```
##### 2.6 优缺点
优点：

1.提高复用性（子类继承父类功能）

2.提高可维护性（修改父类即修改所有子类）

缺点：

强耦合（父类缺陷子类不可避免，除非是私有）
<br>

#### 3. 多态
##### 3.1 基本概念
`多态`：**同一事物**在不同环境中表现不同的形态（如水的三态、人的多重身份）

在代码中体现为：

（1）同一函数，传入不同的对象，会实现不同的结果

（2）不同对象收到同一消息，会产生不同的结果

##### 3.2 多态分类
<img width="1387" height="314" alt="image" src="https://github.com/user-attachments/assets/f2b068f5-c7fd-4ea9-8116-c8840a7df794" />

参数多态：采用参数化模板，传递不同类型的对象，参数不一样，执行的代码一样（如cpp中的函数模板和类模板）

包含多态：最常见的就是子类型化，即一个类型是另一个类型的子类型，本质上是覆盖（如cpp中的虚函数）

过载多态：同一个名（操作符、函数名）在不同的上下文中有不同的含义，本质上就是重载（如cpp中的函数重载，函数名相同，传入参数的个数和类型不同）

强制多态：将一个对象的类型转化为另一个对象的类型（如cpp中的隐式类型转换，显式也行）

##### 3.3 实现多态的三个条件
（1）有继承

>多态的实现受到继承的支持
>
>利用类的继承的层次关系，把具有通用功能的消息存放在高层次，而不同的实现这一功能的行为放在较低层次，在这些低层次上生成的对象能够给通用消息以不同的响应

（2）有重写

（3）要有父类引用指向子类对象（例如Animal = Dog() ）

例子：

```python
# 多态案例（包含多态）
# 有继承
class Animal(object):
    def speak(self):
        pass
class Dog(Animal):
    def speak(self):
        print('汪汪汪')
class Cat(Animal):
    def speak(self):
        print('喵喵喵')
# 有多态
def make_noise(an : Animal): # 意思是an必须是Animal的对象，或者其子类对象
    an.speak()

if __name__ == '__main__':
    # 创建类对象
    d = Dog()
    c = Cat()
    make_noise(d)
    make_noise(c)
```
理论上，make_noise只能传入Animal的对象，但是python中即使传入别的也可以执行，不会报错，所以python的多态也称为“伪多态”

原因：Python类型注解不会强制检查参数类型，而Java就是强制的

##### 3.4 优缺点
优点：

高内聚（提高类独立处理问题的能力）、低耦合（减少类间依赖）

提高扩展性（一个函数多种效果）
<br><br>

### 魔法方法
#### 1. 魔法方法
（1）命名格式：`__方法名__`（双下划线）
（2）**自动调用**：满足特定条件时会自动触发，无需手动执行
（3）核心作用：为Python类添加特殊功能
（4）==常用魔法方法==：`__init__`，`__str__` ，`__del__`
<br>

#### 2.__ init __
==用于初始化对象的属性值，创建对象时自动调用==

（1）无参数：所有对象默认属性相同，扩展性差
```python
class Car():
    def __init__(self):  # 类内用init魔法方法初始化属性
        self.color = 'red'
        self.number = 4

if __name__ == '__main__':
    car1 = Car()
    print(f"汽车颜色:{car1.color},汽车轮胎数:{car1.number}")
    car2 = Car()
    car2.color = 'black'# 修改属性值
    print(f"汽车颜色:{car2.color},汽车轮胎数:{car2.number}")
```
（2）有参数：属性值由外部传入，提高扩展性

```python
# 有参数
class Car():
    def __init__(self, color, number):  # 参数名就和属性名相同，便于理解
        self.color = color
        self.number = number

if __name__ == '__main__':
    car = Car('red', 4) # 在类外创建对象时传入参数
    print(f"汽车颜色:{car.color},汽车轮胎数:{car.number}")
```
内存图演示：

![在这里插入图片描述](https://i-blog.csdnimg.cn/direct/9a447a2a9ac74108acf1eba1b30e43ef.png)
<br>

#### 3. __ str __
==用于快速打印对象的属性，输出语句打印对象时自动调用==
（print 默认打印对象的内存地址）


```python
class Car():
    def __init__(self, color, number):  # 参数名就和属性名相同，便于理解
        self.color = color
        self.number = number
    def __str__(self):
        return f"汽车颜色:{self.color},汽车轮胎数:{self.number}"

if __name__ == '__main__':
    car = Car('red', 4) # 在类外创建对象时传入参数
    print(car) # 将car作为self参数传给__str__()方法
```
<br>

#### 4. __ del __
==用于销毁对象，删除对象或者main函数执行完毕时自动调用==
（释放对象占用的系统资源，节省内存空间）

```python
class Car():
    def __init__(self, color, number):  # 参数名就和属性名相同，便于理解
        self.color = color
        self.number = number
    def __str__(self):
        return f"汽车颜色:{self.color},汽车轮胎数:{self.number}"
    def __del__(self):
        print(f"{self}对象被释放了")

if __name__ == '__main__':
    car = Car('red', 4) # 在类外创建对象时传入参数
    print(car) # 将car作为self参数传给__str__()方法
```
<br><br>

### 抽象类
`抽象类`：有抽象方法的类
在Python中抽象类与接口完全等价（与Java不同）

`抽象方法`：方法体是空实现（pass）的方法
例如：

```python
class Animal(object):
    def speak(self):
        pass
```

注意：抽象类不能直接实例化，只能作为父类被继承
总结：==父类确定有哪些方法，子类完成方法的实现；父类制定标准，子类负责实现；父类考虑要做什么，子类考虑怎么做==
<br><br>

### 类属性和对象属性
`类属性`：类的所有对象共享，A对象修改了类属性，其他对象再使用的就是修改后的

定义类属性：
定义在类中，函数外的变量就是类属性
获取类属性：
推荐使用：类名.类属性名
可用但不推荐：对象名.类属性名

`对象属性`：每个对象特有的，对象之间互不影响

定义对象属性：
类内：self.属性名=属性值
类外：对象名.属性名=属性值
获取对象属性：
类内：self.属性名
类外：对象名.属性名
<br><br>

### 类方法和静态方法
`实例方法`：
==第一个参数必须是self，实例本身==
调用方法：推荐 对象名.
例如：

```python
class Student(object):
    # 类属性
    teacher_name = '王老师'
    # 实例方法
    def method00(self,stu_name):
        print(f'{self.teacher_name}的学生是{stu_name}') # 通过self访问

if __name__ == '__main__':
    stu = Student()
    stu.method00('张三') # 推荐对象名
    Student.method00(stu,'张三') # 也可以
```

`类方法`：
==第一个参数必须是cls(class)，类本身==
通过@classmethod装饰器修饰
调用方法：推荐 类名.
例如：

```python
class Student(object):
    # 类属性
    teacher_name = '王老师'
    # 类方法
    @classmethod
    def method01(cls, stu_name):
        print(f'{cls.teacher_name}的学生是{stu_name}') # 通过cls访问

if __name__ == '__main__':
    stu = Student()
    Student.method01('张三') # 推荐类名
    stu.method01('张三') # 也可以
```

`静态方法`：
==第一个参数无硬性要求，可不传参==
通过@staticmethod装饰器修饰
调用方法：推荐 类名.
例如：

```python
class Student(object):
    # 类属性
    teacher_name = '王老师'
    # 静态方法
    @staticmethod
    def method02(stu_name):
        print(f'{Student.teacher_name}的学生是{stu_name}') # 通过类名访问

if __name__ == '__main__':
    stu = Student()
    Student.method02('张三') # 推荐类名
    stu.method02('张三') # 也可以
```
<br><br>

### 面向对象分析（OOA）
五个核心活动：
对象认定：识别系统中的具体对象（找名词，且要是具体实体）
对象组织：建立对象间的结构关系（抽象出类）
对象交互：分析对象间的消息传递机制
操作定义：明确对象的方法和属性
信息封装：定义对象的内部数据表示

### 面向对象设计（00D）
将分析模型转化为设计模型

#### 面向对象设计的原则

