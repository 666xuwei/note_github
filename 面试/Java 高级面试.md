## 一、集合

#### 1、List、Set、Map 是否继承自 Collection 接口？

List、Set 是，Map 不是。Map 是键值对映射容器，与 List 和 Set 有明显的区别，而 Set 存储的零散的元素且不允许有重复元素（数学中的集合也是如此），List是线性结构的容器，适用于按数值索引访问元素的情形。

#### 2、阐述 ArrayList、Vector、LinkedList 的存储性能和特性。

ArrayList 和 Vector 都是使用数组方式存储数据，此数组元素数大于实际存储的数据以便增加和插入元素，它们都允许直接按序号索引元素，但是插入元素要涉及数组元素移动等内存操作，所以索引数据快而插入数据慢，Vector 中的方法由于添加了 synchronized 修饰，因此 Vector 是线程安全的容器，但性能上较 ArrayList 差，因此已经是 Java 中的遗留容器。

LinkedList 使用双向链表实现存储（将内存中零散的内存单元通过附加的引用关联起来，形成一个可以按序号索引的线性结构，这种链式存储方式与数组的连续存储方式相比，内存的利用率更高），按序号索引数据需要进行前向或后向遍历，但是插入数据时只需要记录本项的前后项即可，所以插入速度较快。

Vector 属于遗留容器（Java 早期的版本中提供的容器，除此之外，Hashtable、Dictionary、BitSet、Stack、Properties都是遗留容器），已经不推荐使用，但是由于 ArrayList 和 LinkedListed 都是非线程安全的，如果遇到多个线程操作同一个容器的场景，则可以通过工具类 Collections  中的 synchronizedList 方法将其转换成线程安全的容器后再使用。

#### 3、Collection 和 Collections 的区别？

Collection 是一个接口，它是 Set、List 等容器的父接口；

Collections 是个一个工具类，提供了一系列的静态方法来辅助容器操作，这些方法包括对容器的搜索、排序、线程安全化等等

#### 4、List、Map、Set 三个接口存取元素时，各有什么特点？

List 以特定索引来存取元素，可以有重复元素。Set 不能存放重复元素（用对象的equals()方法来区分元素是否重复）。Map 保存键值对（key-value pair）映射，映射关系可以是一对一或一对多。

Set 和 Map 容器都有基于哈希存储和排序树的两种实现版本，基于哈希存储的版本理论存取时间复杂度为 O(1)，而基于排序树版本的实现在插入或删除元素时会按照元素或元素的键（key）构成排序树从而达到排序和去重的效果。

#### 5、TreeMap 和 TreeSet 在排序时如何比较元素？Collections 工具类中的 sort()方法如何比较元素？



## 二、IO 流

#### 如何实现对象克隆？

有两种方式：

1. 实现 Cloneable 接口并重写 Object 类中的 clone()方法；
2. 实现 Serializable 接口，通过对象的序列化和反序列化实现克隆，可以实现真正的深度克隆，代码如下。

```java
import java.io.ByteArrayInputStream;
import java.io.ByteArrayOutputStream;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;
public class MyUtil {
    private MyUtil() {
        throw new AssertionError();
    }
    @SuppressWarnings("unchecked")
    public static <T extends Serializable> T clone(T obj) throws
        Exception {
        ByteArrayOutputStream bout = new ByteArrayOutputStream();
        ObjectOutputStream oos = new ObjectOutputStream(bout);
        oos.writeObject(obj);
        ByteArrayInputStream bin = new ByteArrayInputStream(bout.toByteArray());
        ObjectInputStream ois = new ObjectInputStream(bin);
        return (T) ois.readObject();
        // 说明：调用 ByteArrayInputStream 或 ByteArrayOutputStream 对象的 close 方法没有任何意义
        // 这两个基于内存的流只要垃圾回收器清理对象就能够释放资源，这一点不同于对外部资源（如文件流）的释放
    }
}
```



## 三、反射

#### 1、Java 反射机制的作用

反射机制是指程序在运行状态中能够获取自身的信息，对于任意一个类，都能够知道这个类的所有属性和方法，对于任意一个对象，都可以获取该对象的属性和方法，这种动态获取的机制称为java的反射机制。

#### 2、通过反射获取类对象的三种方式

获取类对象有 3 种方式

1. `Class.forName()`  （常用）
2. `Hero.class`
3. `new Hero().getClass()`

三种方式中，常用第一种，第二种需要导入包，依赖性太强，不导包就抛编译错误。第三种对象都有了还要反射干什么。一般都第一种，一个字符串可以传入也可写在配置文件中等多种方法。

## 四、多线程



## 五、网络编程



## 六、JVM



## 七、设计模式

