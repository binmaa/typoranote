[TOC]

#### 1.IOC(Inversion of Control):控制反转

	##### DI

依赖注入（Dependency Inversion）：把底层类作为参数传递给上层类，实现上层对下层的”控制“。

##### IOC、DI、DL的关系

​	![image-20210429081832432](C:\GitOut\typoranote\image\IOCDIDL关系.png)

​		

##### 依赖倒置原则、IOC、DI、IOC容器关系

![image-20210429082257252](C:\GitOut\typoranote\image\依赖倒置原则与IOCDIIOC关系.png)

#### 2.Spring IOC支持的功能

+ 依赖注入
+ 依赖检查
+ 自动装配
+ 支持集合
+ 指定初始化及销毁方法
+ 支持回调方法（需实现spring接口，有侵入性）

#### 3. getBean方法的代码逻辑

+ 转换beanName

+ 从缓存中加载实例

+ 实例化Bean

+ 检测parentBeanFactory

+ 初始化依赖的bean

+ 创建bean

  

#### 4.SpringNeam的作用域

