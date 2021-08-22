# Lambda & Stream

- 个人不喜欢**方法引用**，可读性较差，尽量写 **Lambda 表达式**

### Lambda 表达式

- `(o1,o2) -> Integer.compare(o1,o2);`
- 格式：
  - **->** : lambda操作符 或 箭头操作符
  - ->左边：lambda形参列表 （其实就是接口中的抽象方法的形参列表）
  - ->右边：lambda体 （其实就是重写的抽象方法的方法体）
- 使用情况：
  - ->左边：lambda形参列表的参数类型可以省略(类型推断)；如果lambda形参列表只有一个参数，其一对()也可以省略
  - ->右边：lambda体应该使用一对{}包裹；如果lambda体只有一条执行语句（可能是return语句），省略这一对{}和return关键字
- Lambda表达式的本质：作为函数式接口的实例
- 如果一个接口中，只声明了**一个**抽象方法，则此接口就称为**函数式接口**。我们可以在一个接口上使用 `@FunctionalInterface` 注解，来检查它是否是一个函数式接口

```java
@FunctionalInterface
public interface MyInterface {
    void method1();
//    void method2();   报错
}
```

- 参考示例

```java
public class LambdaTest1 {
    //语法格式一：无参，无返回值
    @Test
    public void test1() {
        Runnable r1 = new Runnable() {
            @Override
            public void run() {
                System.out.println("我爱北京天安门");
            }
        };

        r1.run();

        System.out.println("***********************");

        Runnable r2 = () -> {
            System.out.println("我爱北京故宫");
        };

        r2.run();
    }

    //语法格式二：Lambda 需要一个参数，但是没有返回值。
    @Test
    public void test2() {

        Consumer<String> con = new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        };
        con.accept("谎言和誓言的区别是什么？");

        System.out.println("*******************");

        Consumer<String> con1 = (String s) -> {
            System.out.println(s);
        };
        con1.accept("一个是听得人当真了，一个是说的人当真了");

    }

    //语法格式三：数据类型可以省略，因为可由编译器推断得出，称为“类型推断”
    @Test
    public void test3() {

        Consumer<String> con1 = (String s) -> {
            System.out.println(s);
        };
        con1.accept("一个是听得人当真了，一个是说的人当真了");

        System.out.println("*******************");

        Consumer<String> con2 = (s) -> {
            System.out.println(s);
        };
        con2.accept("一个是听得人当真了，一个是说的人当真了");

    }

    @Test
    public void test4() {

        ArrayList<String> list = new ArrayList<>();//类型推断

        int[] arr = {1, 2, 3};//类型推断

    }

    //语法格式四：Lambda 若只需要一个参数时，参数的小括号可以省略
    @Test
    public void test5() {
        Consumer<String> con1 = (s) -> {
            System.out.println(s);
        };
        con1.accept("一个是听得人当真了，一个是说的人当真了");

        System.out.println("*******************");

        Consumer<String> con2 = s -> {
            System.out.println(s);
        };
        con2.accept("一个是听得人当真了，一个是说的人当真了");


    }

    //语法格式五：Lambda 需要两个或以上的参数，多条执行语句，并且可以有返回值
    @Test
    public void test6() {

        Comparator<Integer> com1 = new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                System.out.println(o1);
                System.out.println(o2);
                return o1.compareTo(o2);
            }
        };

        System.out.println(com1.compare(12, 21));

        System.out.println("*****************************");
        Comparator<Integer> com2 = (o1, o2) -> {
            System.out.println(o1);
            System.out.println(o2);
            return o1.compareTo(o2);
        };

        System.out.println(com2.compare(12, 6));


    }

    //语法格式六：当 Lambda 体只有一条语句时，return 与大括号若有，都可以省略
    @Test
    public void test7() {

        Comparator<Integer> com1 = (o1, o2) -> {
            return o1.compareTo(o2);
        };

        System.out.println(com1.compare(12, 6));

        System.out.println("*****************************");

        Comparator<Integer> com2 = (o1, o2) -> o1.compareTo(o2);

        System.out.println(com2.compare(12, 21));

    }

    @Test
    public void test8() {
        Consumer<String> con1 = s -> {
            System.out.println(s);
        };
        con1.accept("一个是听得人当真了，一个是说的人当真了");

        System.out.println("*****************************");

        Consumer<String> con2 = s -> System.out.println(s);

        con2.accept("一个是听得人当真了，一个是说的人当真了");

    }

}
```

------

### java内置的4大核心函数式接口

- 消费型接口 `Consumer<T>:     void accept(T t)`
- 供给型接口 `Supplier<T>:     T get()`
- 函数型接口 `Function<T,R>:   R apply(T t)`
- 断定型接口 `Predicate<T>:    boolean test(T t)`

- 参考示例

```java
public class LambdaTest2 {

    // Consumer
    @Test
    public void test1() {

        happyTime(500, new Consumer<Double>() {
            @Override
            public void accept(Double aDouble) {
                System.out.println("学习太累了，去天上人间买了瓶矿泉水，价格为：" + aDouble);
            }
        });

        System.out.println("********************");

        happyTime(400, money -> System.out.println("学习太累了，去天上人间喝了口水，价格为：" + money));
    }

    public void happyTime(double money, Consumer<Double> con) {
        con.accept(money);
    }


    // Predicate
    @Test
    public void test2() {
        List<String> list = Arrays.asList("北京", "南京", "天津", "东京", "西京", "普京");

        List<String> filterStrs = filterString(list, new Predicate<String>() {
            @Override
            public boolean test(String s) {
                return s.contains("京");
            }
        });

        System.out.println(filterStrs);

        System.out.println("********************");

        List<String> filterStrs1 = filterString(list, s -> s.contains("京"));
        System.out.println(filterStrs1);
    }

    //根据给定的规则，过滤集合中的字符串。此规则由Predicate的方法决定
    public List<String> filterString(List<String> list, Predicate<String> pre) {

        ArrayList<String> filterList = new ArrayList<>();

        for (String s : list) {
            if (pre.test(s)) {
                filterList.add(s);
            }
        }

        return filterList;
    }
}
```

------

### 构造器引用 & 数组引用

```java
public class ConstructorRefTest {
    // 构造器引用
    // Supplier中的 T get()
    // Employee的空参构造器：Employee()
    @Test
    public void test1() {

        Supplier<Employee> sup = new Supplier<Employee>() {
            @Override
            public Employee get() {
                return new Employee();
            }
        };
        System.out.println("*******************");

        Supplier<Employee> sup1 = () -> new Employee();
        System.out.println(sup1.get());

        System.out.println("*******************");

        Supplier<Employee> sup2 = Employee::new;
        System.out.println(sup2.get());
    }

    // Function中的 R apply(T t)
    @Test
    public void test2() {
        Function<Integer, Employee> func1 = id -> new Employee(id);
        Employee employee = func1.apply(1001);
        System.out.println(employee);

        System.out.println("*******************");

        Function<Integer, Employee> func2 = Employee::new;
        Employee employee1 = func2.apply(1002);
        System.out.println(employee1);

    }

    // BiFunction中的 R apply(T t,U u)
    @Test
    public void test3() {
        BiFunction<Integer, String, Employee> func1 = (id, name) -> new Employee(id, name);
        System.out.println(func1.apply(1001, "Tom"));

        System.out.println("*******************");

        BiFunction<Integer, String, Employee> func2 = Employee::new;
        System.out.println(func2.apply(1002, "Tom"));

    }

    // 数组引用
    // Function中的 R apply(T t)
    @Test
    public void test4() {
        Function<Integer, String[]> func1 = length -> new String[length];
        String[] arr1 = func1.apply(5);
        System.out.println(Arrays.toString(arr1));

        System.out.println("*******************");

        Function<Integer, String[]> func2 = String[]::new;
        String[] arr2 = func2.apply(10);
        System.out.println(Arrays.toString(arr2));
    }
}
```

------

### 方法引用

- 使用情境：当要传递给Lambda体的操作，已经有实现的方法了，可以使用方法引用
- 方法引用，本质上就是Lambda表达式，也是函数式接口的实例
- 使用格式：  类(或对象) :: 方法名
- 使用情况：
  - 对象 :: 非静态方法
  - 类 :: 静态方法
  - 类 :: 非静态方法
- 使用要求：
  - 要求接口中的抽象方法的 **形参列表** 和 **返回值类型** 与方法引用的方法的形参列表和返回值类型相同（针对于情况1和情况2）

```java
public class MethodRefTest {

    // 情况一：对象 :: 实例方法
    //Consumer中的void accept(T t)
    //PrintStream中的void println(T t)
    @Test
    public void test1() {
        Consumer<String> con1 = str -> System.out.println(str);
        con1.accept("北京");

        System.out.println("*******************");

        PrintStream ps = System.out;
        Consumer<String> con2 = ps::println;
        con2.accept("beijing");
    }

    //Supplier中的 T get()
    //Employee中的 String getName()
    @Test
    public void test2() {
        Employee emp = new Employee(1001, "Tom", 23, 5600);

        Supplier<String> sup1 = () -> emp.getName();
        System.out.println(sup1.get());

        System.out.println("*******************");

        Supplier<String> sup2 = emp::getName;
        System.out.println(sup2.get());
    }

    // 情况二：类 :: 静态方法
    //Comparator中的 int compare(T t1,T t2)
    //Integer中的 int compare(T t1,T t2)
    @Test
    public void test3() {
        Comparator<Integer> com1 = (t1, t2) -> Integer.compare(t1, t2);
        System.out.println(com1.compare(12, 21));

        System.out.println("*******************");

        Comparator<Integer> com2 = Integer::compare;
        System.out.println(com2.compare(12, 3));
    }

    //Function中的 R apply(T t)
    //Math中的 Long round(Double d)
    @Test
    public void test4() {
        Function<Double, Long> func = new Function<Double, Long>() {
            @Override
            public Long apply(Double d) {
                return Math.round(d);
            }
        };

        System.out.println("*******************");

        Function<Double, Long> func1 = d -> Math.round(d);
        System.out.println(func1.apply(12.3));

        System.out.println("*******************");

        Function<Double, Long> func2 = Math::round;
        System.out.println(func2.apply(12.6));
    }

    // 情况三：类 :: 实例方法  (有难度)
    // Comparator中的 int comapre(T t1,T t2)
    // String中的 int t1.compareTo(t2)
    @Test
    public void test5() {
        Comparator<String> com1 = (s1, s2) -> s1.compareTo(s2);
        System.out.println(com1.compare("abc", "abd"));

        System.out.println("*******************");

        Comparator<String> com2 = String::compareTo;
        System.out.println(com2.compare("abd", "abm"));
    }

    //BiPredicate中的 boolean test(T t1, T t2);
    //String中的 boolean t1.equals(t2)
    @Test
    public void test6() {
        BiPredicate<String, String> pre1 = (s1, s2) -> s1.equals(s2);
        System.out.println(pre1.test("abc", "abc"));

        System.out.println("*******************");

        BiPredicate<String, String> pre2 = String::equals;
        System.out.println(pre2.test("abc", "abd"));
    }

    // Function中的 R apply(T t)
    // Employee中的 String getName();
    @Test
    public void test7() {
        Employee employee = new Employee(1001, "Jerry", 23, 6000);

        Function<Employee, String> func1 = e -> e.getName();
        System.out.println(func1.apply(employee));

        System.out.println("*******************");

        Function<Employee, String> func2 = Employee::getName;
        System.out.println(func2.apply(employee));
    }
}
```

------

### Stream

- Stream关注的是对数据的运算，与CPU打交道；集合关注的是数据的存储，与内存打交道
- ①Stream 自己不会存储元素。
- ②Stream 不会改变源对象。相反，他们会返回一个持有结果的新Stream。
- ③Stream 操作是延迟执行的。这意味着他们会等到需要结果的时候才执行
- 执行流程：
  - 创建 Stream： 一个数据源(如：集合、数组)，获取一个流
  - 中间操作： 一个中间操作链，对数据源的数据进行处理
  - 终止操作(终端操作)： 一旦执行终止操作，就执行中间操作链，并产生结果。之后，不会再被使用