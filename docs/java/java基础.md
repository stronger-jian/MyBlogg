基础决定上层建筑！

这些都是廖雪峰java教程的笔记。

名词解释：

方法签名：方法名+参数列表（形参类别、个数、顺序）

### 面向对象编程

#### 方法

参数绑定

```java
//基本类型参数绑定
class Person{
	private int age;

	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}
	
}
public class Main {
	public static void main(String[] args) {
		Person p = new Person();
		int n = 15;
		p.setAge(n);
		System.out.println(p.getAge());
		n = 20;
		System.out.println(p.getAge());
	}
}
```

结果：15,15

基本类型参数的传递，是调用方值的复制。双方各自的后续修改，互不影响。

```java
// 引用类型参数绑定
class Person1 {
	private String[] name;

	public String getName() {
		return name[0] + " " + name[1];
	}

	public void setName(String[] name) {
		this.name = name;
	}

}

public class Main1 {
	public static void main(String[] args) {
		Person1 p1 = new Person1();
		String[] fullname = new String[] { "a", "jian" };
		p1.setName(fullname);
		System.out.println(p1.getName());
		fullname[0] = "A";
		System.out.println(p1.getName());
	}
}
```

结果:

a jian
A jian

结论：引用类型参数的传递，调用方的变量，和接收方的参数变量，指向的是同一个对象。双方任意一方对这个对象的修改，都会影响对方（因为指向同一个对象嘛）。

#### 继承

1. 继承是面向对象编程的一种强大的代码复用方式；

   你创建了Person类：

   ```java
   public class Person {
       private String name;
       private int age;
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public int getAge() {
           return age;
       }
   
       public void setAge(int age) {
           this.age = age;
       }
   }
   ```

   你还需要创建一个Student类：

   ```java
   public class Student {
       private String name;
       private int age;
       private int score;
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public int getAge() {
           return age;
       }
   
       public void setAge(int age) {
           this.age = age;
       }
   
       public int getScore() {
           return score;
       }
   
       public void setScore(int score) {
           this.score = score;
       }
   }
   ```

   此时你发现，Student就是Person(学生就是人)。

   所以你可以使用继承

   ```java
   public class Person {
       private String name;
       private int age;
   
       public String getName() {
           return name;
       }
   
       public void setName(String name) {
           this.name = name;
       }
   
       public int getAge() {
           return age;
       }
   
       public void setAge(int age) {
           this.age = age;
       }
   }
   public class Student extends Person {
       private int score;
   
       public int getScore() {
           return score;
       }
   
       public void setScore(int score) {
           this.score = score;
       }
   }
   ```

   * 子类自动获得父类所有字段，禁止定义与父类重名的字段。

2. Java只允许单继承，所有类最终的根类是`Object`；
   
* Object特殊，它没有父类。
  
3. protected 允许子类访问父类的字段和方法；
   
* 子类不能访问父类的private字段和方法。
  
4. 子类的构造方法可以通过`super()`调用父类的构造方法；
   * 一个类有默认的无参构造方法。当你重载构造方法时，这个无参构造方法会无效掉。
   * 程序编译，构建子类时，如果没有用super()指明父类特殊的构造方法，那子类会默认使用父类的无参构造方法，此时若父类有特殊构造方法，没有无参构造方法，就会报错。这也就是为什么我们有事要定义，一个无参的看似无用的构造方法。

5. 可以安全地向上转型为更抽象的类型；

   * 这种把一个子类类型安全地变为父类类型的赋值，被称为向上转型（upcasting）。

     Person p = new Student();

6. 可以强制向下转型，最好借助`instanceof`判断；

   * 和向上转型相反，如果把一个父类类型强制转型为子类类型，就是向下转型（downcasting）。

   Person p = new Student();

   if(p instanceof Student){

   ​	Student s = (Student)p;

   }

7. 子类和父类的关系是is，has关系不能用继承。

#### 多态(Polymorphic)

是指，针对某个类型的方法调用，其真正执行的方法取决于运行时期实际类型的方法。

1. 子类可以覆写父类的方法（Override），覆写在子类中改变了父类方法的行为；

   ```java
   // 多态(Polymorphic)
   // 定义一种收入，需要报税
   class Income {
   	protected double income;
   
   	public Income(double income) {
   		this.income = income;
   	}
   	
   	public double getTax() {
   		return income * 0.1;
   	}
   }
   // 工资报税
   class Salary extends Income{
   	
   	public Salary(double income) {
   		super(income);
   	}
   
   	@Override
   	public double getTax() {
   		if(income <= 5000) {
   			return 0;
   		}
   		return (income - 5000) * 0.2;
   	}
   	
   }
   // 津贴免税
   class Allowance extends Income{
   	public Allowance(double income) {
   		super(income);
   	}
   	@Override
   	public double getTax() {
   		return 0;
   	}
   	
   }
   public class Main2 {
   	public static void main(String[] args) {
   		// 给普通收入，工资收入和享受津贴的算税
   		Income[] incomes = new Income[] {
   				new Income(3000),
   				new Salary(7500),
   				new Allowance(10000)
   		};
   		System.out.println(totalTax(incomes));
   	}
   	public static double totalTax(Income... incomes) {
   		double total = 0;
   		for(Income income: incomes) {
   			total += income.getTax();
   		}
   		return total;
   	}
   }
   ```

   结果：800.0

2. Java的方法调用总是作用于运行期对象的实际类型，这种行为称为多态；

3. `final`修饰符有多种作用：
   1. `final`修饰的方法可以阻止被覆写；
   2. `final`修饰的class可以阻止被继承；
   3. `final`修饰的field必须在创建对象时初始化，随后不可修改。

#### 抽象类

如果父类本身不需要实现任何功能，仅仅是为定义方法签名，目的是让子类复写，那么，可以把父类的方法定义为抽象方法，抽象方法用abstract修饰。

因为无法执行这个抽象方法，所以这个类也必须申明为抽象类(abstract class)。我们无法实例化一个抽象类。

抽象方法相当于规范，子类必须重写父类定义的抽象方法。

```java
abstract class Person3{
	public abstract void run();
}
class Student extends Person3{

	@Override
	public void run() {
		System.out.println("Setudent.run!!!!!");
	}
	
}
public class Main3 {
	public static void main(String[] args) {
		Person3 p = new Student();
        // 不关心Person变量的具体子类型:
        // 同样不关心新的子类是如何实现run()方法的：
		p.run();
	}

}
```

#### 接口

如果一个抽象类没有字段，所有方法全部都是抽象方法。

就可以把该抽象类改写为接口：`interface`。

接口默认所有的方法都是public abstract。

接口可以定义default方法，实现类可以不必覆写`default`方法。

```java
interface Person4{
	String getName();
	default void run() {
		System.out.println(getName() + "   run!!!");
	}
}
class Student4 implements Person4{
	private String name;

	public Student4(String name) {
		this.name = name;
	}
	
	public String getName() {
		return this.name;
	}
}
public class Main4 {
	public static void main(String[] args) {
		Person4 p = new Student4("ajian");
		p.run();
	}
}
```

#### 静态字段和静态方法

静态由static修饰。静态字段和方法都属于类。

接口是纯抽象类，不能定义实例字段，但可以有静态字段，并且该字段必须是final类型。

编译器会省略public static final。

#### 包

在Java中，我们使用`package`来解决名字冲突。

真正的完整类名是 `包名.类名`。

#### Java核心类

**StringJoiner**

```java
public class Main5 {
	public static void main(String[] args) {
		String[] names = {"Bob", "Alice", "Grade"};
		StringJoiner sj = new StringJoiner(", ");
		for(String name: names) {
			sj.add(name);
		}
		System.out.println(sj.toString());
		StringJoiner sj1 = new StringJoiner(", ", "Hello ", "!");
		for(String name: names) {
			sj1.add(name);
		}
		System.out.println(sj1.toString());
	}
}
```

结果：

Bob, Alice, Grade
Hello Bob, Alice, Grade!

### 集合

Java的集合类定义在`java.util`包中，支持泛型，主要提供了3种集合类，包括`List`，`Set`和`Map`。

### 多线程

#### 创建多线程

方法一：从`Thread`派生一个自定义类，然后覆写`run()`方法：

```java
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("start new thread");
    }
}
public class Main {
    public static void main(String[] args) {
        Thread t = new MyThread();
        t.start(); // 启动新线程
    }
}
```

方法二：创建`Thread`实例时，传入一个`Runnable`实例：

```java
class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("start new thread!");
    }
}
public class Main {
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable());
        t.start(); // 启动新线程
    }
}
```

用Java8引入的lambda语法进一步简写为：

```java
public class Main {
    public static void main(String[] args) {
        Thread t = new Thread(() -> {
            System.out.println("start new thread!");
        });
        t.start(); // 启动新线程
    }
}
```

* Java用`Thread`对象表示一个线程，通过调用`start()`启动一个新线程；
* 一个线程对象只能调用一次`start()`方法；
* 线程的执行代码写在`run()`方法中；
* 线程调度由操作系统决定，程序本身无法决定调度顺序；
* `Thread.sleep()`可以把当前线程暂停一段时间。

```java
public static void main(String[] args) {
    System.out.println("main start!");
    Thread t = new Thread(){
        @Override
        public void run() {
            System.out.println("thread run...");
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("thread end...");
        }
    };
    t.start();
    try {
        Thread.sleep(20);
    } catch (InterruptedException e) {
        e.printStackTrace();
    }
    System.out.println("main end!");
}
```

#### 线程的状态

```java
Thread t = new Thread(()->{
    System.out.println("hello");
});
System.out.println("start");
t.start();
// 让主线程等待t线程结束,join(long)的重载方法也可以指定一个等待时间，超过等待时间后就不再继续等待
t.join();
System.out.println("end");
```

* Java线程对象`Thread`的状态包括：`New`、`Runnable`、`Blocked`、`Waiting`、`Timed Waiting`和`Terminated`；
  * New-创建新的线程
  * Runnable-运行中的线程，正在执行run()方法的java代码
  * Blocked-运行中的线程，因某些操作而被挂起
  * Waiting-运行中的线程，因某些操作在等待中
  * Timed Waiting-运行中的线程，因为执行`sleep()`方法正在计时等待；
  * Terminated-线程已终止，因为`run()`方法执行完毕。

#### 中断线程

```java
class MyThread extends Thread {
    @Override
    public void run() {
        int n = 0;
        while (!interrupted()){
            n++;
            System.out.println(n+" hello!");
        }
    }
}
public class Main2 {
    public static void main(String[] args) throws InterruptedException {
        Thread t = new MyThread();
        t.start();
        Thread.sleep(1);// 暂停1毫秒
        t.interrupt();// 中断t线程
        t.join();// 等待t线程结束
        System.out.println("end");
    }
}
```

对目标线程调用`interrupt()`方法可以请求中断一个线程，目标线程通过检测`isInterrupted()`标志获取自身是否已中断。如果目标线程处于等待状态，该线程会捕获到`InterruptedException`；

目标线程检测到`isInterrupted()`为`true`或者捕获了`InterruptedException`都应该立刻结束自身线程；

```java
class HelloThread extends Thread {
    public volatile boolean running = true;// volatile 可变的 易变的
    @Override
    public void run() {
        int n = 0;
        while (running){
            n++;
            System.out.println(n+"  hello");
        }
        System.out.println("end");
    }
}
public class Main2 {
    public static void main(String[] args) throws InterruptedException {
        HelloThread t = new HelloThread();
        t.start();
        Thread.sleep(1);
        t.running=false;
    }
}
```

通过标志位判断需要正确使用`volatile`关键字；

`volatile`关键字解决了共享变量在线程间的可见性问题。

#### 守护线程

守护线程适用于定时器。

```java
Thread t = new MyThread();
t.setDaemon(true);
t.start();
```

守护线程是为其他线程服务的线程；

所有非守护线程都执行完毕后，虚拟机退出；

守护线程不能持有需要关闭的资源（如打开文件等，因为守护线程没有机会关闭文件）。



