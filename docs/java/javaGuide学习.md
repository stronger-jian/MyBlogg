[这是学习JavaGuide的笔记](https://github.com/Snailclimb/JavaGuide)

### 基础

#### 集合

**Arrays.asList()使用指南**

Arrays.asList()将一个数组转成一个list集合。

* 我在实际中遇到过，对转完之后的List不能执行添加元素。

  ```java
  String[] myArray = {"aaa","bbb","ccc"};
  List<String> myList = Arrays.asList(myArray);
  // 等价于 List<String> myList = Arrays.asList("aaa","bbb","ccc");
  myList.add("ddd");
  System.out.println(myList);
  
  Exception in thread "main" java.lang.UnsupportedOperationException
  	at java.util.AbstractList.add(AbstractList.java:148)
  	at java.util.AbstractList.add(AbstractList.java:108)
  	at com.study.Main6.main(Main6.java:17)
  ```

  从曝出的错误可以看出，这个list不提供这种操作。

***使用时的注意事项***

 * **传递的数组必须是对象数组，而不是基本类型。**

   ```java
   int[] myArray = {1,2,3};
   List myList = Arrays.asList(myArray);
   System.out.println(myList.size());//1
   System.out.println(myList.get(0));//数组地址
   System.out.println(myList.get(1));//java.lang.ArrayIndexOutOfBoundsException
   int[] array = (int[])myList.get(0);
   System.out.println(array[0]);//1
   ```

   `Arrays.asList()` 方法返回的并不是 `java.util.ArrayList` ，而是 `java.util.Arrays` 的一个内部类,这个内部类并没有实现集合的修改方法或者说并没有重写这些方法。

***正确将数组转换为ArrayList***

1. 最简单的方法

   ```java
   List list = new ArrayList<>(Arrays.asList("a", "b", "c"))
   ```

2. Java8的Stream流

   ```java
   Integer [] myArray = { 1, 2, 3 };
   List myList = Arrays.stream(myArray).collect(Collectors.toList());
   //基本类型也可以实现转换（依赖boxed的装箱操作）
   int [] myArray2 = { 1, 2, 3 };
   List myList = Arrays.stream(myArray2).boxed().collect(Collectors.toList());
   ```

3. 使用guava

   ```java
   // from collection
   List<String> l1 = Lists.newArrayList(anotherListOrCollection);    
   // from array
   List<String> l2 = Lists.newArrayList(aStringArray);
   // from varargs
   List<String> l3 = Lists.newArrayList("or", "string", "elements"); 
   ```

