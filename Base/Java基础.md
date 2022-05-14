# JAVA EE
## 一、简介
### 1、介绍  
继承了C++语⾔⾯向对象技术的核⼼，舍弃了容易引起错误的指针，以引⽤取代；移除了C++中
的运算符重载和多重继承特性，⽤接⼝取代；增加垃圾回收器功能。
1998年12月8日，Sun公司发布了第二代Java平台（简称为Java2）的3个版本：J2ME（微型版），J2SE（标准版），J2EE（企业版）

> JDK：java开发工具包SDk，包含了jre和开发工具，稳定版为jdk1.8版本（2014年，即jdk8、JavaSE8）。 开发java程序所需。
> jre：java运行环境，包含了JVM和lib。 运行java程序所需。
> JVM：java虚拟机。   

2005年，Java SE 6正式发布。此时，Java的各种版本已经更名，如J2SE更名为JavaSE。

原理：先用javac命令编译.java文件,生成了JVM可以识别的.class运行字节码文件,再用java命令执行运行.class文件。 
特点：
一个源文件中只能有一个public修饰的类，其他类个数不限；一个源文件有n个类时，编译结果的class文件就有n个；源文件的名字必须和public修饰的类名相同；main方法为入口；每一句以分号;结束。 
注释单行用//多行用/* */  
第一个HelloWorld代码：

    public class HelloWorld{
    	public static void main(String[] args){
    		System.out.println("Hello!");
    	}}
### 2、安装
Windows下安装：

    先下载好合适的jdk1.8文件,再在环境变量的系统变量中设置3项属性：JAVA_HOME为jdk1.8文件所在目录  
    CLASSPATH为.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar  
    Path为%JAVA_HOME%\bin;%JAVA_HOME%\jre\bin
Linux下安装：

    1、下载好jdk1.8的tar.gz文件，使用命令tar -zxvf jdk-8u-linux-x64.tar.gz -C /usr/local/src/解压到src目录下。  
    2、修改配置文件，vim /etc/profile，在文尾按i进行编辑，添加：
    export JAVA_HOME=/usr/local/src/jdk1.8.0（根据自己的完整路径修改）  
    export PATH=$PATH:$JAVA_HOME/bin:$JAVA_HOME/jre/bin:$PATH  
    export CLASSPATH=.:$JAVA_HOME/lib:$JAVA_HOME/jre/lib  
    在按esc再输入：wq保存退出。  
    3、执行命令:source /etc/profile让配置文件生效。
## 二、基础语法

关键字、标识符、常量、变量（作用域、生命周期、数据类型、运算符）、  
语句：if-else、do、while、for、switch、break、continue、return  
switch(){case :...;break;...default:xxx;}  
函数
## 三、面向对象
**对象**：类的实例  
构造方法：类会默认有  
**封装**：  
this：代表本类对象 super：代表父类对象  
static：被静态修饰的对象，可以直接被类名所调用，静态方法中不能使用this，super关键字。  
设计模式  
**访问权限**：  
public完全公开，protected仅子类可用，private仅类内部可用，默认default仅同一个包package内可用。  
**继承**：所有类都继承（派生）于Object类，有toString()方法  
**多态**：用**extends**关键字来继承父类，不能用属性，没有构造方法，可以@Override重写父类的方法，并动态绑定

    public class Animal{      //类的封装
    	public int weight;    //共有属性
    	private String type;    //私有属性
    	public Animal(int weight){
    		this.weight = weight;  //构造函数，可省略
    	}
    	public void move(){    //方法
    		System.out.println("animal can move!");
    	}
    }
    
    public class Tiger extends Animal{   //继承
    	public String roar = "aoo";
    	public Tiger(int weight,String roar){
    		super(weight);            //继承父类属性
    		//super(type);            //父类的私有属性无法调用
    		this.roar = roar;
    	}
    	@Override                 //重写父类方法
    	public void move(){
    		System.out.println("tiger can run!");
    	}
    }
    public class Fish extends Animal{
    	public String livein = "water";   //多态
    	@Override
    	public void move(){
    		System.out.println("fish can swim!");
    	}
    }
    //新建Main.class
    public class Main{
    	public static void main(String[] args){
    		Tiger tiger1 = new Tiger(500,"aooo");  //创建子类的实例对象
    		tiger1.move();
    	}
    }
**抽象**：用abstract关键字修饰，此类不能创建实例，只能派生子类  
**接口**：用**interface**关键字定义，用**implements**实现，与类相似但不同，不能实例化，只能被类实现。  

    interface A{//定义一个接口
         		public static final String MESSAGE="Hello！";//全局常量
         		public abstract void print();//定义抽象方法
         		default public void otherprint(){//定义可以带方法体的默认方法
         					System.out.println("默认方法");
         			}
         		}
接口中成员属性默认是public static final修饰，成员方法是public abstact修饰，接口中允许定义默认default方法。
**packge和import**：  
package：在类的第一行声明来管理类，格式：package 域名倒写.项目名.模块名;   
import：导入包或者类 

## 四、容器

    import java.util.*
容器也称集合，都是继承Collection接口的子类(List,Set,Map,Queue),Stack已被淘汰。
### 数组
在容器类前，有数组。  

    String[] s1 = new String[5]
    s1[0] = 1
### List
ArrayList读取性能好,LinkedList插入性能好,Vector线程安全，性能差一些。

    String[] s1 = {"one","two","three"};
    ArrayList<String>list1 = new ArrayList<String>();
    List<String>list2 = new LinkedList<String>();
    list1.add("a");    list1.get(0);    list1.contain("a");
    list1.set(0,"d");  list1.indexof("d");  list1.remove(3);
    list1.isEmpty();
    //集合转为数组  数组转集合
    Object[] obj = set.toArray();
    List<Object> list = Arrays.asList(obj);
### Set
Set是无序的集，值是唯一的。常用的有HashSet、TreeSet。

    HashSet<String> = new Hashset<String>();
    方法同List
### Map
Map通过键值对储存。
	

    HashMap<String,Object>map1 = new HashMap<>();
    Object res1 = map1.put("a":1);
    map1.remove(key = "a");   map1.size();
### Queue
Queue是特殊的线性表，只能在头部插入，尾部删除。
### 迭代器Iterator
用于遍历集合
	

    Iterator iter1 = list1.iterator();
    while(iter.hasNext()){
    	System.out.println((String)iter.next());
    }
### 泛型、通识符
使用参数化可以实现多种类型。  
用List<? extends Fruit>来代替List<Apple>
## 五、异常
Exception和Error都是继承自Throwable类，Error异常比较严重，可能关于JVM虚拟机。 

Exception分为可检查和不检查的，不检查的(运行时)异常，是RuntimeException类及其子类.可检查的(编译时)异常程序员必须显示处理，否则程序就会发生错误无法通过编译。  

可以用try-catch-finally捕获异常，用throw来抛出异常。

### 自定义异常

    public class MyException extends Exception {
    	private String message;//异常信息
    	public MyException(String message){//构造函数
    		super(message);
    		this.message = message;
    }
    //在要抛出异常的地方使用异常类
    public class UseMyException {
    	private String name;
    	private String password;
    	public UseMyException(String name,String password){
    		this.name = name;
    		this.password = password;
    	}
    	//在抛异常的函数使用throws关键字，内部用throw关键字
    	public void throwException(String password) throws MyException{
    		if (!this.password.equals(password)){
    			throw new MyException("密码不正确!");
    		}
    	}
    }
    public class TestException {   //测试异常
    	public void test(){
    		UseMyException e1 = new UseMyException("admin","123");
    		try{
    			e1.throwException("123456");
    		}catch (MyException e){
    			System.out.println("MyException:"+e.getMessage());
    		}
    	}
    }
## 六、I/O流
​    import java.io.*;
1、流的分类  
读取源：    字节流 InputStream  文本流 Reader  
写出到目的：字节流 OutputStream  文本流 Writer  
2、输出设备
硬盘：文件 File开头,内存：数组 字符串,键盘：System.in,网络：Socket
​    

    文件字节流：
    　　FileOutputStream fos = new FileOutputStream("D://xxx.xxx");
    　　fos.write("dsfdsf".getBytes());//写入字节数组
    　　fos.close();   //用完后需要关闭流，释放资源。字节流不需要Flush
    　　FileInputStream fis = new FileInputStream("D://xxx.xxx");
    　　fis.read();   //读取一个字节
    　　fis.close();
    文本字符流：
    　　FileWriter fw = new FileWriter("D:\\xxx.txt");
    　　fw.write("sdfsdfsdf");//可以直接写入字符串
    　　fw.flush(); //写完后需要flush，才能真正写道输出设备
    　　fw.close(); //close()时也会flush
    　　FileReader fr = new FileReader("D:\\xxx.txt");
    　　fr.read(char[] ch);//可以读取一个字符数组的内容
    　　fr.close();
## 七、多线程
进程是应用程序，线程是一条执行路径。进程有独立的内存空间，崩溃不会影响其他程序，线程没有独立的空间，多个线程在同一个进程的空间，可能会影响其他线程，一个进程中，至少有一个线程。
实现：实现Runnable接口或者继承Thread类。

	//1、实现Runnable接口方法
	class MyThread implements Runnable{ // 实现Runnable接口，作为线程的实现类
		private String name ;       // 表示线程的名称
		public MyThread(String name){
			this.name = name ;      // 通过构造方法配置name属性
		}
		public void run(){  // 覆写run()方法，作为线程 的操作主体
			for(int i=0;i<10;i++){
				System.out.println(name + "运行，i = " + i) ;
			}
		}
	};
	public class RunnableDemo01{
		public static void main(String args[]){
			MyThread mt1 = new MyThread("线程A ") ;    // 实例化对象
			MyThread mt2 = new MyThread("线程B ") ;    // 实例化对象
			Thread t1 = new Thread(mt1) ;       // 实例化Thread类对象
			Thread t2 = new Thread(mt2) ;       // 实例化Thread类对象
			t1.start() ;    // 启动多线程
			t2.start() ;    // 启动多线程
		}
	};
	//2、继承Thread类
	class MyThread extends Thread{  // 继承Thread类，作为线程的实现类
		private String name ;       // 表示线程的名称
		public MyThread(String name){
			this.name = name ;      // 通过构造方法配置name属性
		}
		public void run(){  // 覆写run()方法，作为线程 的操作主体
			for(int i=0;i<10;i++){
				System.out.println(name + "运行，i = " + i) ;
			}
		}
	};
	public class ThreadDemo02{
		public static void main(String args[]){
			MyThread mt1 = new MyThread("线程A ") ;    // 实例化对象
			MyThread mt2 = new MyThread("线程B ") ;    // 实例化对象
			mt1.start() ;   // 调用线程主体
			mt2.start() ;   // 调用线程主体
		}
	};
## 八、内存和垃圾回收
### 内存
Java是在JVM所虚拟出的内存环境中运行的。内存分为栈(stack)和堆(heap)两部分。  
每个线程拥有一个栈。在某个线程的运行过程中，如果有新的方法调用，那么该线程对应的栈就会增加一个存储单元，即帧(frame)。在frame中，保存有该方法调用的参数、局部变量和返回地址。  
堆是JVM中一块可自由分配给对象的区域。当我们谈论垃圾回收(garbage collection)时，我们主要回收堆(heap)的空间。
### OOM
报OutOfMemoryError错，当JVM因为没有足够的内存来为对象分配空间并且垃圾回收器也已经没有空间可回收时，就会抛出这个Error。  
堆溢出：Java Heap的最值设置：-Xms 最小值(默认为物理内存的1/64)  -Xmx 最大值(默认为物理内存的1/4,一般小于1G)
### 垃圾回收
垃圾回收GC，Java在空闲时自动清空堆中不再使用的对象来释放内存。
## 九、反射、注解
### 反射
反射Reflection是一种机制，可以在程序运行期间，去调用类中的各种成员，可以不创建对象，就是用里面的方法。  

	//获取Class对象的3种方式：
	class Person{...}
	Person p = new Person();
	
	Class c1 = Class.forName("Person");
	Class c2 = Person.class;
	Class c3 = p.getClass();
	
	//反射生成对象：
	Class<?> c = Person.class
	Object obj = c.newInstance();
### 注解
注解Annotation是以@注解名在代码中存在，不是程序本身，可以对程序作出解释。  
内置注解：  
**@Override**重写或实现一个父类的方法，**@Deprecated** 不建议使用，**@SuppressWarnings**用于抑制编译时的警告信息。  
元注解：  
**@Target** 用于描述注解的适用范围，**@retention**表示需要在什么级别保存该注解信息，**@Documented** 用于描述其它类型的注解应该被作为被标注的程序成员的公共API，可被javadoc工具文档化，**@Inherited**  阐述了某个被标注的类型是被继承的。  
自定义注解：public @interface 注解名{...}
## 十、JDBC
JDBC是最基础的ORM工具，是Java程序与数据库系统通信的标准API，使得开发人员可以使用纯Java的方式来连接数据库，并执行SQL操作。  
JDBC这套接口的实现，称为 数据库驱动 ，由各个数据库厂商提供。  
关系：数据库->数据库驱动Driver->DriverManager->JDBC API->Java应用程序  
以Mysql为例使用：  
1、安装好Mysql数据库，再下载驱动文件mysql-connector-java-5.1.37-bin.jar，放在项目的lib目录下。  
在项目上右击选择"Build Path"-->"Configure Build Path..."，点击"Add JARs..."按钮，选择项目下的jar包，单击OK完成jar包的导入。还可以先单击"Add External JARs..."，然后导入计算机中任意目录的jar包。  
2、编写代码

	import java.sql.*
	public class DbUtil {
		public static final String URL = "jdbc:mysql://localhost:3306/imooc";
		public static final String USER = "liulx";
		public static final String PASSWORD = "123456";
		public static void main(String[] args) throws Exception {
			//1.加载驱动程序
			Class.forName("com.mysql.jdbc.Driver");
			//2.获得数据库连接
			Connection con = DriverManager.getConnection(URL, USER, PASSWORD);
			//3.操作数据库，实现增删改查
			Statement st = con.createStatement();
			ResultSet r = st.executeQuery("SELECT user_name, age FROM imooc_goddess");
			//如果有数据，r.next()返回true
			while(r.next()){
				System.out.println(r.getString("user_name")+"年龄："+r.getInt("age"));
			}
		}
	}
