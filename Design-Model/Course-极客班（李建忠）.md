## 一、 设计模式简介
变化！！！

复用！！！


## 二、面向对象设计原则
-变化是复用的天敌
面向对象设计最大的优势在于**抵御变化**

### 重新认识面向对象
- 理解隔离变化
	- 从宏观层面来看，面向对象的构建方式能适应软件的变化，能将变化所带来的影响减为最小
- 各司其职
	- 从微观层面来看，面向对象的方式更强调个各类的“责任”
	- 由于需求变化导致的新增类型不应该影响原来类型的实现——是所谓各司其职
- 对象时什么
	- 从语言层面来看，对象封装了代码和数据。
	- 从规格层面讲，对象是一系列可被使用的公共接口
	- 从概念层面讲，对于是某种拥有责任的对象


### 面向对象原则（1） 
- **设计原则比模式更重要**
- 依赖倒置原则（DIP）
	- 高层模块（稳定）不应该依赖于底层（变化），二者都应该依赖抽象
	- 抽象（稳定）不应该依赖于实现细节（变化），实现细节应该依赖于抽象（稳定）
	
### 面向对象原则（2）
- 开放封闭原则（OCP）
	- 对扩展开放，对更改封闭
	- 类模块应该是可扩展的，但是不可修改
	
### 面向对象原则（3）
- 单依职责原则（SRP）
	- 一个类应该仅有一个引起它变化的原因
	- 变化的方向隐含着类的责任

- 有时候，一个类搞太多方法，那么这个类的责任就会很多，容易出现混乱

### 面向对象原则（4）
- Liskov替换原则（LSP）
	- 子类必须能够替换它们的基类（IS-A）
	- 继承表达类型抽象

### 面向对象原则（5）
- 接口隔离原则（ISP）
	- 不应该强迫客户程序依赖它们不用的方法
	- 接口应该小而完备
	
- public太多，外部就会产生依赖，维护起来就很麻烦。合适地使用public、protected、private

### 面向对象原则（6）
- 优先使用对象组和，而不是继承
	- 类继承通常为“白箱复用”，对象组和通常为“黑箱复用”
	- 继承在某种程度上破坏了封装性，子类父类耦合度高
	- 而对象组合则只要求被组合的对象具有良好定义的接口，耦合度低。


### 面向对象原则（7）
- 封装变化点
	- 使用封装来创建对象之间的分界层，让设计者可以在分界层的一侧进行修改，而不会对另一侧产生不良的影响，从而实现层次间的松耦合。

### 面向对象原则（8）
- 针对接口编程，而不是针对实现编程
	- 不将变量类型声明为某个特定的具体类，而是声明为某个接口。
	- 客户程序无需获知对象的具体类型，只需要知道对象所具有的接口
	- 减少系统中各个部分的依赖关系，从而实现“高内聚，松耦合”的类型设计方案
	
- 该原则与依赖倒置原则相辅相成，违背一个基本俩都违背了

- 面向接口设计，是产业强盛的标志。接口标准化！


## 三、模板方法（template method）

### 从封装变化角度对模式分类
- 组件协作：
	- Template Method
	- Strategy
	- Observer / Event
- 单一职责
	- Decorator
	- Bridge
- 对象创建
	- Factory Method
	- Abstract Factory
	- Prototype
	- Builder
- 对象性能
	- Singleton
	- Flyweight
- 接口隔离：
	- Facade
	- Proxy
	- Mediator
	- Adapter
- 状态变化：
	- Memento
	- State
- 数据结构
	- Composite
	- Iterator
	- Chain of
	- Responsibility
- 行为变化
	- Command
	- Visitor
领域问题：
	- Interpreter
	
### 重构关键技法
- 静态 -> 动态
- 早绑定 -> 晚绑定
- 继承 -> 组合
- 编译时依赖 -> 运行时依赖
- 紧耦合 -> 松耦合

### “组件协作”模式：
- 现代软件专业分工之后的第一个结果是“框架与应用程序的划分”，
“组件协作”模式通过晚期绑定，来实现框架与应用程序间的松耦合，是二者之间协作时常用的模式。

- 典型模式
	- Template Method
	- Strategy
	- Observer / Event
   
	
## 四、策略模式
### 动机
- 在软件构建过程中，某些对象使用的算法可能多种多样，经常改变，如果将这些算法都编码到对象中，将会使对象变得异常复杂；而且有时候支持不是用的算法也是一个性能负担。

- 如何在运行时分局需要透明地更改对象的算法？将算法与对象本身解耦，从而避免上述问题？


- 设置模式说的复用性是编译之后的二进制级别的复用，不是代码层面的复用

- 当你的代码出现多个if else时，这就是策略模式需要的特征
- if else 绝对不变的情况下，即情况不会增加，比如一周七天。这时候不用strategy

### 要点总结

- Strategy及其子类为组件提供了一些列可重用的算法，从而可以使得类型在运行时方便的根据需要在各个算法之间切换。
- Strategy模式提供了用条件判断语句以外的另一种选择，消除条件判断语句，就是在解耦合。含有许多条件判断语句的代码通常都需要strategy模式
- 如果Strategy对象没有实例变量，那么各个上下文可以共享同一个Strategy对象，从而节省对象开销


## 五、观察者模式（Observer）
- 广播就是观察者模式
```C++
// C++推荐的形式是：一个主继承类，其他的都是接口或者抽象基类
class MainForm: public Form, public IProgress
{
	TextBox* txtFilePath;
	TextBox* txtFileNumber;
	
	ProgressBar* progressBar;
	
public:
	void Button1_Click()
	{
		stirng filePath = txtFilePath->getText();
		int number = atoi(txtFileNumber->getText().c_str());
		
		FileSplitter splitter(filePath, number);
		
		splitter.add_IProgress(this);
		splitter.add_IProgress(cn);
		
		splitter.split();
		
		splitter.remove_IProgress(this);
	}
	
	virtual void DoProgress(float value)
	{
		progressBar->setValue(value);
	}
}
```

```C++
class IProgress
{
public：
	virtual void DoProgress(float value) = 0;
	virtual ~IProgress(){ }
}

class FileSplitter
{
	string m_filePath;
	int m_fileNumber;
	
	// ProgressBar* m_progressBar;  // 具体通知控件
	List<IProgress*> m_iprogressList;  // 抽象通知机制, 支持多个观察者
	
public:
	FileSplitter(const string& filePath, int fileNumber):
		m_filePath(filePath),
		m_fileNumber(fileNumber),
		{
		
		}
		
	void add_IProgress(IProgress* iprogress)
	{
		m_iprogressList.push_back(iprogress);
	}
	
	void remove_IProgress(IProgress* iprogress)
	{
		m_iprogressList.remove(iprogress);
	}
	
	void split()
	{
		// 1.读取大文件
		
		// 2.分批次向小文件中写入
		for (int i = 0; i < m_fileNumber; i++)
		{
			// ...
			
			
			float progressValue = m_fileNumber;
			progressValue = (i + 1) / progressValue;
			onProgress(progressValue) 
		}
	}
protected:
	void onProgress(float value) 
	{
	List<IProgress*>::Iterator itor = m_iprogressList.begin();
	List<IProgress*>::Iterator end = m_iprogressList.end();
	
	while (itor != end)
	{
		(*itor)->DoProgress(progressValue);  // 更新进度条

		
		itor++;
	}
}
```

#### 模式定义
- 定义对象间的一种一对多的（变化）的依赖关系，以便当一个对象（Subject）的状态发生改变时，所有依赖于它的对象得到通知并自动更新

#### 要点总结
- 使用面向对象的抽象，Observer模式使得我们可以独立地改变目标与观察者，从而使二者之间的依赖关系达至松耦合
- 目标发送通知是，无需指定观察者，通知（可以携带通知信息作为参数）会自动传播
- 观察者自己决定是否需要订阅通知，目标对象对此一无所知。
- Observer 模式是基于事件的UI框架中非常常用的设计模式，也是MVC模式的重要组成部分


## 6.装饰模式

### "单一职责" 模式
- 在软件组件的设计中，如果责任划分的不清晰，使用继承得到的结果往往是随需求的变化，子类急剧膨胀，同时充斥着重复的代码，这时候的关键是划清责任。
	- Decorator
	- Bridge
	
```C++
// 业务操作
class stream
{
public:
	virtual char Read(int number) = 0;
	virtual void Seek(int position) = 0;
	virtual void Write(char data) = 0;
	
	virtual ~Stream() {}
};

// 主体类
class FileStream: public Stream
{
public:
	virtual char Read(int number) {
		// 读文件流
	}
	
	virtual void Seek(int position) {
		// 定位文件流
	}
	
	virtual void Write(char data)
	{
	
	}
}
```


### 要点总结
- 通过采用组合而非继承的手法，Decorator模式实现了在运动时动态扩展对象功能的能力，而且可以根据需要扩展多个功能
。避免了使用继承带来的“灵活性差”和“多子类衍生问题”

- Decorator类在接口上表现为is-a Component的继承关系，
即Decorator类继承了Component类所具有的接口。但在实现上又表现为has-a Component的组合关系，即Decorator类又使用了另外一个Component类

- Decorator模式的目的并非解决“多子类衍生的多继承”问题，Decorator模式应用的要点在于解决“主体类在多个方向上的扩展功能”——是为“装饰”的含义



- 同时继承又同时组合，基本就是Decorator设计模式了


## 7.桥模式

- 由于某些类型的固有的实现逻辑，使得它们具有两个变化的维度，乃至多个维度的变化
- 如何应对这种“多维度的变化



```C++
// 更改前
// 更改前
class Messager {
public:
	virtual void Login(string username, string password) = 0;
	virtual void SendMessage(string message) = 0;
	virtual void SendPicture(Image image) = 0;

	virtual void PlaySound() = 0;
	virtual void DrawShape() = 0;
	virtual void WriteText() = 0;
	virtual void Connect() = 0;


	virtual ~Messager() {}
};


// 平台实现 n
class PCMessagerBase : public Messager
{
public:
	virtual void PlaySound() {
		// ***************
	}

	virtual void DrawShape() {
		// **************
	}

	virtual void WriteText() {
		// **************
	}

	virtual void Connect() {
		// **************
	}
};

class MobileMessagerBase : public Messager
{
public:
	virtual void PlaySound() {
		// ==============
	}

	virtual void DrawShape() {
		// ==============
	}

	virtual void WriteText() {
		// ==============
	}

	virtual void Connect() {
		// ==============
	}
};

// 业务抽象 m
// 类的数量： 1 + n + m*n
class PCMessagerLite : public PCMessagerBase
{
public:
	virtual void Login(string username, string password)
	{
		PCMessagerBase::Connect();
		// ..............
	}

	virtual void SendMessage(string messager) {
		PCMessagerBase::WriteText();
	}

	virtual void SendPicture(Image image) {
		PCMessagerBase::DrawShape();
		// ..............
	}
};

class PCMessagerPerfect : public PCMessagerBase
{
public:
	virtual void Login(string username, string password) {
		PCMessagerBase::PlaySound();
		// ************
		PCMessagerBase::Connect();
		// ..............
	}

	virtual void SendMessage(string messager) {
		PCMessagerBase::PlaySound();
		// ************
		PCMessagerBase::WriteText();
		// ..............
	}

	virtual void SendPicture(Image image) {
		PCMessagerBase::PlaySound();
		// ************
		PCMessagerBase::DrawShape();
		// ..............
	}
};

class MobileMessagerLite : public MobileMessagerBase
{
public:
	virtual void Login(string username, string password)
	{
		MobileMessagerBase::Connect();
		// ..............
	}

	virtual void SendMessage(string messager) {
		MobileMessagerBase::WriteText();
	}

	virtual void SendPicture(Image image) {
		MobileMessagerBase::DrawShape();
		// ..............
	}
};

class MobileMessagerPerfect : public MobileMessagerBase
{
public:
	virtual void Login(string username, string password) {
		MobileMessagerBase::PlaySound();
		// ************
		MobileMessagerBase::Connect();
		// ..............
	}

	virtual void SendMessage(string messager) {
		MobileMessagerBase::PlaySound();
		// ************
		MobileMessagerBase::WriteText();
		// ..............
	}

	virtual void SendPicture(Image image) {
		MobileMessagerBase::PlaySound();
		// ************
		MobileMessagerBase::DrawShape();
		// ..............
	}
};

void Process()
{
	// 编译时装配
	Messager* m = 
		new MobileMessagerPerfect();
}
```

```C++
// 更改后
class Messager {
protected:
	MessagerImp* messagerImp;
public:
	virtual void Login(string username, string password) = 0;
	virtual void SendMessage(string message) = 0;
	virtual void SendPicture(Image image) = 0;

	virtual ~Messager() {}
};

class MessagerImp
{
public:
	virtual void PlaySound() = 0;
	virtual void DrawShape() = 0;
	virtual void WriteText() = 0;
	virtual void Connect() = 0;

	virtual ~MessagerImp() {}
};

// 平台实现 n
class PCMessagerImp : public MessagerImp
{
public:
	virtual void PlaySound() {
		// ***************
	}

	virtual void DrawShape() {
		// **************
	}

	virtual void WriteText() {
		// **************
	}

	virtual void Connect() {
		// **************
	}
}

class MobileMessagerImp : public MessagerImp
{
public:
	virtual void PlaySound() {
		// ==============
	}

	virtual void DrawShape() {
		// ==============
	}

	virtual void WriteText() {
		// ==============
	}

	virtual void Connect() {
		// ==============
	}
};

// 业务抽象 m
// 类的数量： 1 + n + m
class MessagerLite : public Messager
{
public:
	virtual void Login(string username, string password)
	{
		messagerImp->Connect();
		// ..............
	}

	virtual void SendMessage(string messager) {
		messagerImp->WriteText();
	}

	virtual void SendPicture(Image image) {
		messagerImp->DrawShape();
		// ..............
	}
};

class MessagerPerfect : public Messager
{
public:
	virtual void Login(string username, string password) {
		messagerImp->PlaySound();
		// ************
		messagerImp->Connect();
		// ..............
	}

	virtual void SendMessage(string messager) {
		messagerImp->PlaySound();
		// ************
		messagerImp->WriteText();
		// ..............
	}

	virtual void SendPicture(Image image) {
		messagerImp->PlaySound();
		// ************
		messagerImp->DrawShape();
		// ..............
	}
};

void Process()
{
	// 运行时装配
	MessagerImp* mImp = new PCMessagerImp();
	Messager* m = new Messager(mImp);
}
```

- 将抽象部分（业务功能）与实现部分（平台实现）分离，使它们都可以独立地变化

## 8.工厂方法

### “对象创建”模式
- 通过“对象创建”模式绕开new，来避免对象创建（new）过程中所导致的紧耦合（依赖具体类），从而支持对象创建的稳定。它是接口抽象之后的第一步
```C++
class Form;
class ManForm : public Form
{
	SplitterFactory* factory;  // 工厂

public:
	ManForm(SplitterFactory* factory)
	{
		this->factory = factory;
	}

	void Button1_Click() {
		string filePath = txtFilePath;
		int number = atoi(txtFileNumber);


		// 面向接口编程最简单的表现形式是，变量声明为抽象基类
		ISplitter* splitter =
			factory->CreateSplieer();  // 多态 new
			// 把兔子关进笼子里
		splitter->split();
	}
};

// -----------------------------------
// -----------------------------------
class ISplitter
{
public:
	virtual void split() = 0;
	virtual ~ISplitter() {};
};

// 工厂基类
class SplitterFactory {
public:
	virtual ISplitter* CreateSplieer() = 0;
	virtual ~SplitterFactory();
};

//--------------------------------------
// 具体类
class BinarySplitter : public ISplitter {

};

class TxtSplitter : public ISplitter {

};

class PictureSplitter : public ISplitter {

};

class VideoSplitter : public ISplitter {

};

// 具体工厂
class BinarySplitterFactory : public SplitterFactory
{
public:
	virtual ISplitter* CreateSplitter()
	{
		return new BinarySplitter();
	}
};

class TxtSplitterFactory : public SplitterFactory
{
public:
	virtual ISplitter* CreateSplitter()
	{
		return new TxtSplitter();
	}
};

class PictureSplitterFactory : public SplitterFactory
{
public:
	virtual ISplitter* CreateSplitter()
	{
		return new PictureSplitter();
	}
};

class VideoSplitterFactory : public SplitterFactory
{
public:
	virtual ISplitter* CreateSplitter()
	{
		return new VideoSplitter();
	}
};
```

#### 模式定义
- 定义一个用于创建对象的接口，让子类决定实例化哪一个类。Factory Method使得一个类的实例化延迟（目的：解耦，手段：虚函数）到子类

#### 要点总结
- Factory Method模式用于隔离类对象的使用者和具体类型之间的耦合关系。面对一个经常变化的具体类型，紧耦合关系（new）会导致软件的脆弱。

- Factory Method模式通过面向对象的手法，将所要创建
的具体对象工作延迟到子类，从而实现一种扩展（而非更改）的策略，较好地解解决了这种紧耦合关系。

- Factory Method模式解决“单个对象”的需求变化。缺点在于要求创建方法/参数相同。

#### 9.抽象工厂
```C++

```


