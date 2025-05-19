# lambda表达式

https://blog.csdn.net/huangjhai/article/details/107110182?fromshare=blogdetail&sharetype=blogdetail&sharerId=107110182&sharerefer=PC&sharesource=yc5338835&sharefrom=from_link

1.1 相关背景
Lambda表达式是Java 8引入的一个重要特性，它是函数式编程在Java中的一种体现。在Java之前的版本中，Java主要采用面向对象的编程风格，而Lambda表达式的引入使得Java具备了函数式编程的能力。

1.2 函数式编程
函数式编程是一种编程范式，它将计算过程视为函数应用的连续组合。函数式编程强调使用纯函数（Pure Function），避免使用可变状态和副作用，倡导将计算过程抽象为函数，便于代码的理解、测试和并行化。

Lambda表达式允许将函数作为方法的参数，或者将代码块作为数据进行传递。它的引入主要解决了以下几个问题：

匿名内部类的冗余代码：在Java之前的版本中，为了实现函数的传递，常常需要使用匿名内部类来定义一个函数接口的实现。这导致代码冗长、可读性较差。Lambda表达式的引入简化了匿名内部类的语法，让代码更加简洁明了。

函数式编程的支持：函数式编程强调将函数作为第一类对象进行传递和操作。Lambda表达式提供了一种便捷的语法形式，使得函数可以作为参数传递给方法，或者作为返回值返回。

并行编程的支持：函数式编程中的纯函数天然具备无副作用的特性，这使得在并行编程中更容易实现可靠的多线程和并行处理。Lambda表达式的引入使得Java在并行编程方面具备了更好的支持。

1.3 匿名内部类和Lambda表达式
匿名内部类和Lambda表达式都是在Java中用于实现函数式编程的机制，它们可以用来传递行为（函数）作为参数或返回值。

匿名内部类：

匿名内部类是在Java早期引入的一种机制，用于创建一个没有命名的、实现了某个接口或抽象类的类的实例。通过匿名内部类，可以在使用某个接口或抽象类的地方，直接定义一个实现该接口或抽象类的实例。匿名内部类通常通过创建一个子类来实现，并且在实例化的同时定义实现的方法。例如，以下是使用匿名内部类实现Runnable接口的例子：

```
Thread thread = new Thread(new Runnable() {
    public void run() {
        System.out.println("Hello from anonymous inner class");
    }
});
thread.start();
```


Lambda表达式：

Lambda表达式是Java 8中引入的一种更简洁、更直观的方式来表示函数式接口（只有一个抽象方法的接口）的实例。Lambda表达式提供了一种更简洁的语法，使得可以以更紧凑的方式传递行为。使用Lambda表达式，可以直接定义一个函数式接口的实例，而无需创建匿名内部类。以下是使用Lambda表达式实现相同功能的例子：

```
Thread thread = new Thread(() -> {
    System.out.println("Hello from Lambda expression");
});
thread.start();

```

Lambda表达式使用箭头->将参数和函数体分隔开，参数可以有零个或多个，函数体可以是一个表达式或一段代码块。Lambda表达式的引入让Java代码更具表达力和简洁性，提高了代码的可读性和可维护性。

总结来说，匿名内部类和Lambda表达式都可以用来实现函数式编程，传递行为作为参数或返回值。匿名内部类是一种更早引入的机制，Lambda表达式是Java 8中引入的更简洁、更直观的方式。根据具体的场景和需求，可以选择使用匿名内部类或Lambda表达式来实现相应的功能。

二、Lambda表达式的使用
2.1 基本语法
Lambda表达式的基本语法如下：

```
(parameters) -> expression
```


或

```
(parameters) -> { statements; }
```


其中，参数可以是任意合法的Java参数列表，可以为空或包含一个或多个参数。箭头（->）将参数与Lambda主体分隔开来。

Lambda主体可以是一个表达式，也可以是一个代码块。如果主体是一个表达式，它将直接返回该表达式的结果。如果主体是一个代码块，它将按照常规的Java语法执行，并且您可能需要使用return语句来返回值。

下面是一些Lambda表达式的示例：

无参数的Lambda表达式：

```
() -> System.out.println("Hello, Lambda!");
```


带有参数的Lambda表达式：

```
(x, y) -> System.out.println(x + y);
带有多行代码的Lambda表达式：
```

```
(x, y) -> {
    int sum = x + y;
    System.out.println("Sum: " + sum);
    return sum;
}
```


Lambda表达式可以与函数式接口一起使用，函数式接口是只包含一个抽象方法的接口。在Lambda表达式中，根据上下文的要求，可以自动匹配函数式接口的抽象方法，并创建接口的实例。

2.2 使用案例

    //无返回值无参数
    @FunctionalInterface // 函数式接口，检查作用
    interface NoParameterNoReturn {
        void test();
    }
    
    //无返回值一个参数
    @FunctionalInterface
    interface OneParameterNoReturn {
        void test(int a);
    }
    
    //无返回值多个参数
    @FunctionalInterface
    interface MoreParameterNoReturn {
        void test(int a, int b);
    }
    
    //有返回值无参数
    @FunctionalInterface
    interface NoParameterReturn {
        int test();
    }
    
    //有返回值一个参数
    @FunctionalInterface
    interface OneParameterReturn {
        int test(int a);
    }
    
    //有返回值多参数
    @FunctionalInterface
    interface MoreParameterReturn {
        int test(int a, int b);
    }
    
    public class Test1 {
    public static void main(String[] args) {
        MoreParameterReturn moreParameterReturn = (x, y) -> x + y;
        int sum = moreParameterReturn.test(1, 2);
        System.out.println(sum);
    }
    
    public static void main6(String[] args) {
        OneParameterReturn oneParameterReturn = (x) -> x + 1;
        int ret = oneParameterReturn.test(10);
        System.out.println(ret);
    }
    
    public static void main5(String[] args) {
        // NoParameterReturn noParameterReturn = () -> {return 10;};
        NoParameterReturn noParameterReturn = () -> 10;
        int ret = noParameterReturn.test();
        System.out.println(ret);
    }
    
    public static void main4(String[] args) {
        MoreParameterNoReturn moreParameterNoReturn = (x, y) -> System.out.println(x + y);
        moreParameterNoReturn.test(10, 20);
    }
    
    public static void main3(String[] args) {
        // OneParameterNoReturn oneParameterNoReturn = (x) -> System.out.println("x: " + x);
        OneParameterNoReturn oneParameterNoReturn = x -> System.out.println("x: " + x);
        // OneParameterNoReturn oneParameterNoReturn = System.out::println;
        oneParameterNoReturn.test(10);
    }
    
    public static void main2(String[] args) {
    
        // 相当于一个匿名内部类实现了一个接口，重写了接口当中的方法
        NoParameterNoReturn noParameterNoReturn = () -> System.out.println("test()");
        noParameterNoReturn.test();
    }
    
    public static void main1(String[] args) {
        // 匿名内部类
        NoParameterNoReturn noParameterNoReturn = new NoParameterNoReturn() {
            @Override
            public void test() {
                System.out.println("test()");
            }
        };
    
        noParameterNoReturn.test();
    
    }
    }


上述代码展示了在Java中使用Lambda表达式的各种情况，涵盖了无返回值无参数、无返回值一个参数、无返回值多个参数、有返回值无参数、有返回值一个参数、有返回值多个参数的情况。

三、变量捕获
3.1 匿名内部类的变量捕获
在Java中，匿名内部类可以捕获外部变量，即在匿名内部类中引用并访问外部作用域的变量。这种行为称为变量捕获（Variable Capturing）。

在匿名内部类中，可以捕获以下类型的变量：

实例变量（Instance Variables）：如果匿名内部类位于一个实例方法中，它可以捕获并访问该实例的实例变量。

静态变量（Static Variables）：匿名内部类可以捕获并访问包含它的类的静态变量。

方法参数（Method Parameters）：匿名内部类可以捕获并访问包含它的方法的参数。

本地变量（Local Variables）：匿名内部类可以捕获并访问声明为final的本地变量。从Java 8开始，final关键字可以省略，但该变量实际上必须是最终的（即不可修改）。

当匿名内部类捕获变量时，它们实际上是在生成的字节码中创建了一个对该变量的副本。这意味着即使在外部作用域中的变量发生改变，匿名内部类中捕获的变量仍然保持其最初的值。

以下是一个示例，展示了匿名内部类捕获外部变量的用法：

```
public class OuterClass {
    private int instanceVariable = 10;
    private static int staticVariable = 20;

public void method() {
    final int localVar = 30; // 或者直接使用 Java 8+ 的隐式 final

    Runnable runnable = new Runnable() {
        @Override
        public void run() {
            System.out.println("Instance variable: " + instanceVariable);
            System.out.println("Static variable: " + staticVariable);
            System.out.println("Local variable: " + localVar);
        }
    };

    runnable.run();
}

}
```

在上面的示例中，method()方法中创建了一个匿名内部类的实例，并在该类的run()方法中访问了外部的实例变量、静态变量和本地变量localVar。这些变量都被匿名内部类捕获并访问。

需要注意的是，如果在匿名内部类中捕获非final或非最终的变量，或者在Java 8之前的版本中捕获非final的变量，编译器将报错。从Java 8开始，可以捕获对final变量的隐式引用，无需显式地将其声明为final。

3.2 Lambda表达式的变量捕获
在Lambda表达式中，同样可以捕获外部作用域的变量。Lambda表达式可以捕获以下类型的变量：

实例变量（Instance Variables）：Lambda表达式可以捕获并访问包含它的实例的实例变量。

静态变量（Static Variables）：Lambda表达式可以捕获并访问包含它的类的静态变量。

方法参数（Method Parameters）：Lambda表达式可以捕获并访问包含它的方法的参数。

本地变量（Local Variables）：Lambda表达式可以捕获并访问声明为final的本地变量。从Java 8开始，final关键字可以省略，但该变量实际上必须是最终的（即不可修改）。

与匿名内部类不同，Lambda表达式不会创建对变量的副本，而是直接访问变量本身。这意味着在Lambda表达式中捕获的变量在外部作用域中发生的改变也会在Lambda表达式中反映出来。

以下是一个示例，展示了Lambda表达式捕获外部变量的用法：

```
public class LambdaVariableCapture {
    private int instanceVariable = 10;
    private static int staticVariable = 20;

public void method() {
    int localVar = 30;

    // Lambda表达式捕获外部变量
    Runnable runnable = () -> {
        System.out.println("Instance variable: " + instanceVariable);
        System.out.println("Static variable: " + staticVariable);
        System.out.println("Local variable: " + localVar);
    };

    runnable.run();
}

}
```

在上面的示例中，method()方法中创建了一个Lambda表达式，捕获了外部的实例变量、静态变量和本地变量localVar。Lambda表达式中直接使用这些变量，无需声明。

需要注意的是，与匿名内部类一样，如果在Lambda表达式中捕获非final或非最终的变量，或者在Java 8之前的版本中捕获非final的变量，编译器将报错。从Java 8开始，可以隐式捕获对final变量的引用，无需显式地将其声明为final。

另外，Lambda表达式还有一个特点是它们可以访问外部作用域的变量，但不能修改它们的值。这是因为Lambda表达式中的变量是相当于隐式声明的final变量，一旦捕获，就不允许修改。

四、Lambda表达式在集合中的使用
为了能够让Lambda和Java的集合类集更好的一起使用，集合当中也新增了部分接口，以便与Lambda表达式对接。

4.1 Collection接口
Lambda表达式在Collection接口中的使用主要涉及对集合进行迭代、筛选和转换等操作。在Java 8及以上的版本中，Collection接口增加了一些默认方法，例如forEach()、removeIf()和stream()等，使得使用Lambda表达式更加方便。

下面是Lambda表达式在Collection接口中的常见应用示例：

迭代集合元素：

```
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

names.forEach(name -> System.out.println(name));
```

筛选集合元素：

```
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);

numbers.removeIf(n -> n % 2 == 0);
```


转换集合元素：

```
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

List<Integer> nameLengths = names.stream()
                                 .map(name -> name.length())
                                 .collect(Collectors.toList());
```


获取集合流：

```
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

Stream<Integer> stream = numbers.stream();
```


使用filter()筛选集合元素：

使用filter()筛选集合元素：

```
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

List<String> filteredNames = names.stream()
                                  .filter(name -> name.startsWith("A"))
                                  .collect(Collectors.toList());
```

以上示例展示了Lambda表达式在Collection接口中的常见应用场景。通过Lambda表达式，我们可以以更简洁、更灵活的方式对集合进行迭代、筛选和转换等操作。

4.2 List接口
Lambda表达式在List接口中的使用主要涉及到对集合元素的遍历、过滤、映射和归约等操作。下面是一些常见的Lambda表达式在List接口中的应用示例：

遍历列表元素：

```
List<String> fruits = Arrays.asList("Apple", "Banana", "Orange");

fruits.forEach(fruit -> System.out.println(fruit));

```

过滤列表元素：

过滤列表元素：

```
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

List<Integer> evenNumbers = numbers.stream()
                                   .filter(number -> number % 2 == 0)
                                   .collect(Collectors.toList());
```


映射列表元素：

映射列表元素：

```
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

List<Integer> nameLengths = names.stream()
                                 .map(name -> name.length())
                                 .collect(Collectors.toList());
```

查找列表元素：

```
List<String> fruits = Arrays.asList("Apple", "Banana", "Orange");

Optional<String> foundFruit = fruits.stream()
                                   .filter(fruit -> fruit.startsWith("B"))
                                   .findFirst();
```

排序列表元素：

```
List<Integer> numbers = Arrays.asList(5, 2, 8, 1, 6, 3, 9, 4, 7, 10);

List<Integer> sortedNumbers = numbers.stream()
                                     .sorted()
                                     .collect(Collectors.toList());
```


以上示例展示了Lambda表达式在List接口中的一些常见应用场景。通过Lambda表达式和Stream API，我们可以以更简洁、更直观的方式对列表进行操作和处理。

以上示例展示了Lambda表达式在List接口中的一些常见应用场景。通过Lambda表达式和Stream API，我们可以以更简洁、更直观的方式对列表进行操作和处理。

4.3 Map接口
Lambda表达式在Map接口中的使用可以通过Java 8引入的Stream API和Map的相关方法来实现。下面是一些Lambda表达式在Map接口中的常见应用示例：

迭代Map的键值对：

```
Map<String, Integer> map = new HashMap<>();
map.put("Alice", 25);
map.put("Bob", 30);
map.put("Charlie", 35);

map.forEach((key, value) -> System.out.println(key + ": " + value));
```

遍历Map的键或值：

```
Map<String, Integer> map = new HashMap<>();
map.put("Alice", 25);
map.put("Bob", 30);
map.put("Charlie", 35);

map.keySet().forEach(key -> System.out.println(key));
map.values().forEach(value -> System.out.println(value));
```


使用Stream过滤Map的键值对：

```
Map<String, Integer> map = new HashMap<>();
map.put("Alice", 25);
map.put("Bob", 30);
map.put("Charlie", 35);

Map<String, Integer> filteredMap = map.entrySet()
                                      .stream()
                                      .filter(entry -> entry.getValue() > 30)
                                      .collect(Collectors.toMap(Map.Entry::getKey, Map.Entry::getValue));
```


对Map的值进行映射：

对Map的值进行映射：

```
Map<String, Integer> map = new HashMap<>();
map.put("Alice", 25);
map.put("Bob", 30);
map.put("Charlie", 35);

Map<String, String> mappedMap = map.entrySet()
                                   .stream()
                                   .collect(Collectors.toMap(Map.Entry::getKey, entry -> "Age: " + entry.getValue()));
```


对Map的键或值进行归约操作：

对Map的键或值进行归约操作：

```
Map<String, Integer> map = new HashMap<>();
map.put("Alice", 25);
map.put("Bob", 30);
map.put("Charlie", 35);

int sumOfValues = map.values().stream().reduce(0, (a, b) -> a + b);
String concatenatedKeys = map.keySet().stream().reduce("", (a, b) -> a + b);
```


以上示例展示了Lambda表达式在Map接口中的常见应用场景。通过使用Lambda表达式和Stream API，可以以简洁、灵活的方式操作和处理Map的键值对数据。

五、Lambda表达式的优缺点
Lambda表达式在Java中引入了函数式编程的概念，具有许多优点和一些限制。下面是Lambda表达式的主要优点和缺点：

优点：

简洁性：Lambda表达式提供了一种更简洁、更紧凑的语法，可以减少冗余的代码和样板代码，使代码更易于理解和维护。

代码可读性：Lambda表达式使得代码更加自解释和易读，可以直接将逻辑集中在一起，提高代码的可读性和可维护性。

便于并行处理：Lambda表达式与Java 8引入的Stream API结合使用，可以方便地进行集合的并行处理，充分发挥多核处理器的优势，提高代码的执行效率。

避免匿名内部类的繁琐语法：相比于使用匿名内部类，Lambda表达式的语法更为简洁，减少了冗余的代码，提高了编码效率。

缺点：

只能用于函数式接口：Lambda表达式只能用于函数式接口（只有一个抽象方法的接口），这限制了它的使用范围。如果需要使用非函数式接口，仍然需要使用传统的方式，如匿名内部类。

可读性的折衷：尽管Lambda表达式可以提高代码的可读性，但在某些复杂的情况下，Lambda表达式可能变得难以理解和阅读，特别是当表达式变得过于复杂时。

变量捕获的限制：Lambda表达式对捕获的变量有一些限制。它们只能引用final或实际上的最终变量，这可能对某些情况下的代码编写和调试带来一些困扰。

学习曲线：对于习惯于传统Java编程风格的开发者来说，Lambda表达式是一项新的概念，需要一定的学习和适应过程。



原文链接：https://blog.csdn.net/qq_61635026/article/details/131621150