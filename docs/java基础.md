基础决定上层建筑！

这些都是廖雪峰java教程的笔记。

名词解释：

方法签名：方法名+参数列表（形参类别、个数、顺序）

### 面向对象编程

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

#### 多态

是指，针对某个类型的方法调用，其真正执行的方法取决于运行时期实际类型的方法。

1. 子类可以覆写父类的方法（Override），覆写在子类中改变了父类方法的行为；
2. Java的方法调用总是作用于运行期对象的实际类型，这种行为称为多态；
3. `final`修饰符有多种作用：
   1. `final`修饰的方法可以阻止被覆写；
   2. `final`修饰的class可以阻止被继承；
   3. `final`修饰的field必须在创建对象时初始化，随后不可修改。