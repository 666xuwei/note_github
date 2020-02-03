[TOC]

## 一、引言

为了解决空指针异常，Google 公司著名的 Guava 项目引入了 Optional 类，Guava 通过使用检查空值的方式来防止代码污染，它鼓励程序员写更干净的代码。受到 Google Guava 的启发，Optional 类已经成为 Java 8 类库的一部分。

Optional<T> 类(java.util.Optional) 是一个容器类，它可以保存类型 T 的值，代表这个值存在。或者仅仅保存 null，表示这个值不存在。

Optional类的 Javadoc 描述如下：这是一个可以为 null 的容器对象。如果值存在则 isPresent() 方法会返回 true，调用 get()方法会返回该对象。

## 二、常用方法

- **创建 Optional 类对象的方法**：

- - Optional.of(T t) : 创建一个 Optional 实例，t必须非空；
    - Optional.empty() : 创建一个空的 Optional 实例。
    - Optional.ofNullable(T t)：t可以为null。

- **判断 Optional 容器中是否包含对象**：

- - boolean isPresent() : 判断是否包含对象。
    - void ifPresent(Consumer<? super T> consumer) ：如果有值，就执行 Consumer 接口的实现代码，并且该值会作为参数传给它。

- **获取 Optional 容器的对象**：

- - T get(): 如果调用对象包含值，返回该值，否则抛异常
    - T orElse(T other) ：如果有值则将其返回，否则返回指定的other对象。
    - T orElseGet(Supplier<? extends T> other) ：如果有值则将其返回，否则返回由 Supplier 接口实现提供的对象。
    - T orElseThrow(Supplier<? extends X> exceptionSupplier) ：如果有值则将其返回，否则抛出由 Supplier接口实现提供的异常

**代码实现**

```java
public class Demo01 {
    public static void main(String[] args) {
        Demo01 demo01 = new Demo01();
        Integer value1 = null;
        Integer value2 = new Integer(10);

        //Optional.ofNullable - 允许传递为 null 参数
        Optional<Integer> a = Optional.ofNullable(value1);

        //Optional.of - 如果传递的参数是 null，抛出异常 NullPointerException
        Optional<Integer> b = Optional.of(value2);
        System.out.println(demo01.sum(a, b));
    }

    public Integer sum(Optional<Integer> a, Optional<Integer> b) {
        //Optional.isPresent --判断值是否存在
        System.out.println("第一个参数存在：" +a.isPresent());
        System.out.println("第二个参数存在：" +b.isPresent());

        //Optional.orElse - 如果值存在，返回它，否则返回默认值
        Integer value1 = a.orElse(new Integer(0));

        //Optional.get - 获取值，值需要存在
        Integer value2 = b.get();
        return value1 + value2;
    }
}
```