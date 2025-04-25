# Java中提供了抽象类还有接口，区别是什么？开发中如何去选择呢？
抽象类是为了抽象出“共性”，可以包含方法实现和成员变量，适合表示一种“是…的一种”（is-a）的关系，比如 BaseActivity。

接口是为了定义“能力”，只能定义方法（Java 8+ 可以有默认实现），更强调“能做什么”，比如 Runnable、OnClickListener。

开发中，如果你需要提供部分通用实现或状态，选抽象类；如果只是定义行为规范、需要多继承，选接口。简单记：抽象类是父类，接口是能力标签。

特点总结如下:
### **抽象类 vs 接口的区别**

| 特性 | 抽象类（abstract class） | 接口（interface） |
|------|-------------------------|--------------------|
| **方法实现** | 可以有方法的实现 | Java 8 之前只能有抽象方法，Java 8+ 可有 default 和 static 方法 |
| **成员变量** | 可以有成员变量（可选 final） | 只能是 public static final 常量（即常量） |
| **构造方法** | 有构造方法（但不能直接 new） | 没有构造方法 |
| **多继承支持** | 不支持（只能继承一个类） | 支持（一个类可实现多个接口） |
| **使用意图** | 是“某种类型”的基础（“is-a”关系） | 是“具有某种能力”（“can-do”关系） |
| **访问修饰符** | 可以是 public/protected/default | 方法默认就是 public，不能用 protected/private |
| **状态存储** | 可以维护内部状态 | 一般不保存状态，只定义行为 |


# 重载和重写是什么意思，区别是什么？
重载（Overload）是一个类中多个方法**名字相同**，但**参数不同**（类型或数量不同），是**编译时行为**。它让方法更灵活，常用于构造方法或工具类中。

重写（Override）是子类对父类中已有方法的**重新实现**，方法名、参数、返回值都必须一致，是**运行时行为**。它用于多态，让子类定义自己的行为。

区别是：重载发生在同一个类中，靠参数区分；重写发生在父子类之间，靠继承实现。重载是编译器做的事，重写是运行时的表现。




# 静态内部类是什么？和非静态内部类的区别是什么？
静态内部类是用 `static` 修饰的内部类，它属于外部类本身，而不是某个外部类的实例。它**不能访问外部类的非静态成员**，只能访问外部类的静态成员。

非静态内部类（也叫成员内部类）没有用 `static` 修饰，它属于外部类的实例，可以直接访问外部类的所有成员（包括私有的和非静态的）。

主要区别：
1. 静态内部类不依赖外部类对象创建，可以直接 `new 外部类.内部类()`；非静态内部类必须先创建外部类实例。
2. 静态内部类只能访问外部类的静态成员；非静态内部类可以访问所有成员。
3. 静态内部类更像一个普通类，适合逻辑上与外部类关系紧密但不依赖外部类对象的情况。



# Java中在传参数时是将值进行传递，还是传递引用？
Java 中参数传递方式是**值传递**，但对于对象，这个“值”是对象引用的副本(可以理解为一个指针)。

简单说：
- 如果传的是基本类型（如 int、double），传的是数值的拷贝，方法里怎么改都不会影响原值。
- 如果传的是对象（如数组、List），传的是引用的拷贝，方法里可以通过这个引用修改对象的内容，但不能改变原引用（或者说是原指针）。简单来说，就是通过这个引用，你可以修改对象的内容，但不能让原引用指向其他对象。

所以本质上，Java 是**统一的值传递机制**，只是当值是引用时，看起来像是“传引用”，但你不能改变调用者的原始引用。

#使用equals和==进行比较的区别 
在 Java 中，`equals()` 和 `==` 都用于比较，但它们有**不同的用途**和行为：

- `==` 比较的是**引用**是否相等，也就是两个对象是否指向同一个内存地址。
- `equals()` 比较的是**对象的内容**是否相等。

### 重点区别：
- **`==`**：比较的是**对象的引用**是否相同，即两个对象是否是同一个实例。
- **`equals()`**：默认情况下也是比较引用（就像 `==`），但如果类重写了 `equals()` 方法，就可以比较对象的**内容**是否相同。

通常，当你需要比较**对象的内容是否相等**时，应使用 `equals()` 方法。如果是比较两个对象是否是同一个实例（即同一个内存位置），则使用 `==`。



# String s = new String("xxx");创建了几个String对象?
在这行代码 `String s = new String("xxx");` 中，创建了**两个 `String` 对象**。

1. **字符串字面量** `"xxx"`：当你使用字面量创建字符串时，Java 会将 `"xxx"` 字符串存储在字符串常量池中。如果常量池中已经有 `"xxx"` 字符串，那么它不会重复创建，而是直接引用池中的对象。如果没有，它会在常量池中创建一个新的 `"xxx"` 字符串。

2. **`new String("xxx")`**：这部分代码通过 `new` 关键字显式创建了一个新的 `String` 对象，该对象存储在堆内存中。这个对象的值与 `"xxx"` 相同，但它是一个新的对象，和常量池中的 `"xxx"` 是两个不同的对象。

总结：
- 一个 `"xxx"` 字符串对象被放入字符串常量池中（如果没有的话）。
- 一个 `new String("xxx")` 创建的 `String` 对象存储在堆内存中。

所以，这行代码总共创建了**两个 `String` 对象**。



# finally中的代码一定会执行吗？try里有return，finally还执行么

在 Java 中，`finally` 块中的代码**几乎一定会执行**，即使 `try` 块中有 `return`，`finally` 依然会执行。**唯一的例外**是当 JVM 强制退出程序（如 `System.exit()`）时，`finally` 不会被执行。

### 例子：

```java
public class Test {
    public static void main(String[] args) {
        System.out.println(testMethod());
    }

    public static int testMethod() {
        try {
            System.out.println("Inside try block");
            return 1;  // 即使有 return，finally 还是会执行
        } finally {
            System.out.println("Inside finally block");
        }
    }
}
```

输出结果：

```
Inside try block
Inside finally block
1
```

### 解释：
1. `try` 块中的 `return 1;` 会将 1 返回，但在返回之前，**`finally` 块中的代码会先执行**。
2. 只有在 JVM 执行 `finally` 块后才会执行 `return` 操作，因此你会看到 `finally` 先执行，然后方法返回 1。

### 特别注意：
- 如果 `finally` 中有 `return`，它会覆盖 `try` 或 `catch` 中的 `return`，即 `finally` 中的 `return` 会成为最终的返回值。


# Java异常机制中，异常Exception与错误Error区别
在 Java 的异常机制中，`Exception` 和 `Error` 都是 `Throwable` 类的子类，用于表示程序中的不同类型的错误情况，但它们的含义和处理方式不同。

### 1. `Exception`：
- **表示程序可以处理的异常情况**。
- `Exception` 及其子类通常表示程序可以捕获和恢复的异常情况，程序有可能会做一些适当的处理或者修复，继续执行。
- 可以通过 `try-catch` 语句捕获并处理。

#### 子类：
- **`RuntimeException`**：表示运行时异常，通常是程序错误，譬如空指针异常 (`NullPointerException`)、数组越界异常 (`ArrayIndexOutOfBoundsException`) 等。它是 **未检查异常**，也就是说，编译器不会强制要求必须捕获或声明。
- **其他异常**：如 `IOException`、`SQLException` 等，通常是与 I/O、数据库连接等操作相关的异常，编译器会强制要求捕获或声明。

#### 示例：
```java
try {
    int result = 10 / 0; // 会抛出 ArithmeticException
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero");
}
```

### 2. `Error`：
- **表示程序无法处理的严重问题**。
- `Error` 用来表示 **虚拟机层面的错误**，比如内存溢出 (`OutOfMemoryError`) 或者栈溢出 (`StackOverflowError`) 等。这些错误通常是严重到无法处理或恢复的，应用程序不应该捕获它们。
- `Error` 类及其子类是 **严重错误**，通常无法通过代码处理或恢复。

#### 示例：
```java
public class Test {
    public static void main(String[] args) {
        try {
            throw new StackOverflowError(); // 模拟栈溢出错误
        } catch (Error e) {
            System.out.println("Caught an error: " + e);
        }
    }
}
```

### 总结：
- **`Exception`**：表示程序能够处理的异常情况，通常是由程序内部的错误或外部资源问题引起的。
- **`Error`**：表示程序无法处理的严重错误，通常是 JVM 层面的问题，程序不能恢复。

一般来说，程序会捕获并处理 `Exception`，而对于 `Error`，通常不应该进行捕获和处理，因为它们通常表示严重的环境问题。



# 序列Parcelable,Serializable的区别？

在 Java 和 Android 中，`Parcelable` 和 `Serializable` 都用于对象的序列化（即将对象转换为字节流，以便存储或传输），但它们有一些**显著的区别**，主要体现在性能、灵活性和使用场景上。

### 1. `Serializable`：
- **Java 原生接口**：`Serializable` 是 Java 提供的标准接口，可以将对象序列化为字节流，并能够反序列化回对象。
- **实现简单**：实现 `Serializable` 接口的类不需要重写任何方法，只需要实现该接口即可。
- **性能较低**：`Serializable` 的实现依赖反射，效率相对较低，尤其是在 Android 开发中，性能开销比较大。
- **通用性强**：`Serializable` 是 Java 标准库的一部分，适用于 Java 生态中的大部分环境和框架。

#### 示例：
```java
import java.io.Serializable;

public class Person implements Serializable {
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

### 2. `Parcelable`：
- **Android 特有接口**：`Parcelable` 是 Android 平台专门为性能优化设计的接口。它提供了一种更高效的对象序列化方式，通常用于 Android 中的 `Intent`、`Bundle` 等对象传递。
- **实现较复杂**：为了实现 `Parcelable`，需要手动实现 `writeToParcel()` 和 `CREATOR` 等方法，过程相对较繁琐，但能够优化性能。
- **性能更优**：`Parcelable` 使用的是内存映射机制，序列化和反序列化的性能比 `Serializable` 更高，适合频繁传输和存储大型对象的场景。
- **仅限于 Android**：`Parcelable` 是 Android 提供的接口，只适用于 Android 开发。

#### 示例：
```java
import android.os.Parcel;
import android.os.Parcelable;

public class Person implements Parcelable {
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    protected Person(Parcel in) {
        name = in.readString();
        age = in.readInt();
    }

    @Override
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeString(name);
        dest.writeInt(age);
    }

    public static final Creator<Person> CREATOR = new Creator<Person>() {
        @Override
        public Person createFromParcel(Parcel in) {
            return new Person(in);
        }

        @Override
        public Person[] newArray(int size) {
            return new Person[size];
        }
    };

    @Override
    public int describeContents() {
        return 0;
    }
}
```

### 主要区别：

1. **性能**：
   - `Parcelable` 的性能优于 `Serializable`，特别是在 Android 中，`Parcelable` 被广泛使用来高效地传递数据。
   - `Serializable` 依赖反射，序列化和反序列化速度较慢，适合较少使用的场景。

2. **易用性**：
   - `Serializable` 使用更简单，不需要手动实现方法。
   - `Parcelable` 需要手动实现方法，代码比较繁琐，开发者需要实现 `writeToParcel()` 和 `CREATOR`。

3. **使用场景**：
   - `Serializable` 适用于需要在 Java 中进行序列化的场景，适合非 Android 环境。
   - `Parcelable` 是 Android 的序列化机制，适合在 Android 中传递数据（如通过 `Intent`、`Bundle` 等）。

4. **灵活性**：
   - `Serializable` 适用于更复杂的对象图（例如包含多层级嵌套的对象）。
   - `Parcelable` 需要开发者手动处理嵌套对象，但提供更高的性能和优化空间。

### 总结：
- **使用 `Serializable`**：当你需要将对象序列化存储，且没有特别的性能要求，或者在非 Android 环境中使用时。
- **使用 `Parcelable`**：当你在 Android 中需要高效地传递对象（特别是在 `Intent`、`Bundle`、`ContentProvider` 等场景），并且能够接受更多的编码工作时。




# 为什么Intent传递对象为什么需要序列化？
Intent 传递对象需要序列化是因为 Android 系统在跨进程通信（IPC）或组件间数据传递时需要将对象转换为可存储或传输的格式。当通过 Intent 传递对象时，系统会将数据写入到系统服务管理的共享内存区域（Binder 驱动机制），而序列化是实现这一过程的关键步骤。  

首先，Intent 的设计目标是支持组件间通信，包括同一应用内的 Activity/Service/BroadcastReceiver 跨进程调用（例如启动另一个应用的 Activity）。如果直接传递对象引用，由于不同进程的内存空间隔离，目标进程无法访问源进程的对象地址。序列化将对象转换为字节流后，系统可以通过 Binder 驱动在不同进程间安全传输这些数据，并在目标进程中反序列化重建对象。  

其次，序列化提供了数据的持久化和一致性保证。即使不涉及跨进程通信，在某些情况下（如配置变更导致 Activity 重建），系统可能需要通过 Intent 重新传递数据，此时序列化能确保数据完整性和正确性。Android 的 Parcelable 和 Serializable 接口分别针对性能和兼容性提供了序列化方案：Parcelable 是 Android 原生高效实现，适合高性能场景（如 Binder 通信）；Serializable 是 Java 标准接口，兼容性更好但性能较低。  

此外，序列化还支持数据的版本控制。通过实现 Serializable 接口的 serialVersionUID 或 Parcelable 的 CREATOR 字段，开发者可以控制对象结构变更时的兼容性处理，避免因字段增减导致反序列化失败。因此，Intent 传递对象要求序列化本质上是 Android 系统为保证跨进程通信安全、数据完整性和性能优化而设计的必要机制。




# 泛型是什么，泛型擦除呢？
 **泛型（Generics）**是 Java 提供的一种机制，它允许在类、接口和方法中使用类型参数，从而实现**类型安全**和**代码复用**。比如你可以写一个泛型类 `Box<T>` 来存储任何类型的对象，而不需要为每种类型写一个单独的类。

示例：

```java
class Box<T> {
    private T value;

    public void set(T value) { this.value = value; }
    public T get() { return value; }
}
```

你可以使用：

```java
Box<String> box = new Box<>();
box.set("Hello");
String s = box.get();
```

---

**泛型擦除（Type Erasure）**是 Java 编译器在编译期间的一种机制。**Java 的泛型在编译之后会被“擦除”掉，变成原始类型（Raw Type）**，以保持与旧版本 JVM 的兼容。

例如，上面的 `Box<T>` 在编译后，T 会被擦除为 `Object`，所以字节码里其实是：

```java
class Box {
    private Object value;

    public void set(Object value) { this.value = value; }
    public Object get() { return value; }
}
```

因此运行时其实并不知道泛型的具体类型。这也是为什么不能用泛型做如下操作：
- `new T()`（不能创建泛型对象）
- `T.class`（不能获取泛型的 Class）
- 泛型数组 `new T[10]`（不允许）

总结：
- 泛型提供编译期的类型检查和自动类型转换。
- 泛型擦除是在编译期完成的，运行时并不保留类型信息。



# Java的泛型中super 和 extends 有什么区别？

在 Java 泛型中，`extends` 和 `super` 都是通配符（wildcard）的一部分，用于控制泛型的上下边界，它们的核心区别在于：

### `<? extends T>`：上界通配符
表示参数的类型是 `T` 或 `T` 的子类，**你可以“读取”它，但不能安全地“写入”**。

适用于**只读取，不写入**的场景（即**生产者**）。

```java
List<? extends Number> list = new ArrayList<Integer>();
Number n = list.get(0); // ✅ 可以读取为 Number 类型
list.add(1);            // ❌ 不允许添加任何元素（除了 null）
```

### `<? super T>`：下界通配符
表示参数的类型是 `T` 或 `T` 的父类，**你可以安全地“写入” T 类型或子类，但读取时只知道是 Object 或其父类**。

适用于**只写入，不读取**的场景（即**消费者**）。

```java
List<? super Integer> list = new ArrayList<Number>();
list.add(1);            // ✅ 可以添加 Integer 或其子类
Object obj = list.get(0); // ✅ 读取时只能当作 Object 处理
```

### 总结口诀：
- **`? extends T`：你只能从中读取 T 或其子类，但不能写入**
- **`? super T`：你可以写入 T 或其子类，但读取时只能作为 Object 使用**

经典记忆法：**PECS 原则（Producer Extends, Consumer Super）**。  
- 如果你只需要**生产（读）**数据，用 `extends`
- 如果你只需要**消费（写）**数据，用 `super`



# 注解是什么？有哪些使用场景？
注解（Annotation）是 Java 提供的一种**元数据机制**，用于在代码中添加额外的信息，这些信息可以在**编译期、类加载时、运行时**被读取和处理。注解本身不会直接影响代码逻辑，但可以被工具或框架用来生成代码、做校验或运行时处理。

### 常见使用场景：

1. **编译检查**
   - `@Override`：检查方法是否正确重写父类方法
   - `@Deprecated`：标记已废弃的方法或类
   - `@SuppressWarnings`：抑制编译器警告

2. **运行时反射处理**
   - 如 Java 的自定义注解配合反射，可以在运行时扫描类、方法、字段上的注解并执行特定逻辑

3. **框架配置**
   - Spring、Android、Retrofit、Room 等大量使用注解进行自动配置或注入
   - 示例：`@Autowired`、`@Entity`、`@GET("/user")`

4. **代码生成**
   - 一些库使用注解生成代码，如 ButterKnife、Dagger、ARouter、Room
   - Annotation Processor 在编译期处理注解，生成额外的 Java 文件

5. **文档生成**
   - `@param`、`@return` 等 JavaDoc 注解用于生成 API 文档

### 示例（自定义注解）：

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Log {
    String value() default "";
}
```

```java
public class Demo {
    @Log("打印日志")
    public void doSomething() {
        // 业务逻辑
    }
}
```

配合反射读取注解内容，可以实现 AOP、权限校验等功能。

### 小结：
注解是对程序的说明，不会改变程序逻辑，但能增强**可读性**、**配置灵活性**，并用于**编译器检查**、**代码生成**和**运行时行为控制**，是现代 Java 框架的重要基础设施。



# 假如只有一个cpu，单核，多线程还有用吗？
有用，哪怕只有一个 CPU 核心，多线程依然是有意义的。

### 原因如下：

#### 1. **提高程序响应性**
即使是单核 CPU，线程也可以**交替执行**，比如：
- UI 线程负责界面响应
- 后台线程处理耗时操作（如网络请求、文件IO）

这样可以避免整个程序因某个任务阻塞而“卡死”。

#### 2. **简化逻辑结构**
将不同功能划分到不同线程，使代码更清晰、更好维护。例如：
- 一个线程负责下载
- 一个线程负责解析数据
- 主线程负责展示

#### 3. **充分利用等待时间**
单核 CPU 执行时，如果一个线程在等待（比如等待磁盘、网络数据），CPU 可以切换去执行其他线程，从而提高 CPU 使用率。

#### 4. **系统级并发**
操作系统使用时间片轮转机制调度线程，所以虽然物理上是一个核心，但多个线程在宏观上“看起来”是同时执行的，实现**并发**。

### 举个例子：
你开一个线程下载文件，另一个线程实时更新进度条。虽然 CPU 只有一个核心，但系统会快速切换线程，给用户“两个任务同时在跑”的体验。

### 总结：
单核 CPU 上的多线程 **不能实现真正的并行**，但依然可以提升程序的**响应性、结构清晰性、资源利用率**，所以仍然非常有用。




# 假如只有一个cpu，单核，多线程还有用吗
有用，哪怕只有一个 CPU 核心，多线程依然是有意义的。

### 原因如下：

#### 1. **提高程序响应性**
即使是单核 CPU，线程也可以**交替执行**，比如：
- UI 线程负责界面响应
- 后台线程处理耗时操作（如网络请求、文件IO）

这样可以避免整个程序因某个任务阻塞而“卡死”。

#### 2. **简化逻辑结构**
将不同功能划分到不同线程，使代码更清晰、更好维护。例如：
- 一个线程负责下载
- 一个线程负责解析数据
- 主线程负责展示

#### 3. **充分利用等待时间**
单核 CPU 执行时，如果一个线程在等待（比如等待磁盘、网络数据），CPU 可以切换去执行其他线程，从而提高 CPU 使用率。

#### 4. **系统级并发**
操作系统使用时间片轮转机制调度线程，所以虽然物理上是一个核心，但多个线程在宏观上“看起来”是同时执行的，实现**并发**。

### 举个例子：
你开一个线程下载文件，另一个线程实时更新进度条。虽然 CPU 只有一个核心，但系统会快速切换线程，给用户“两个任务同时在跑”的体验。

### 总结：
单核 CPU 上的多线程 **不能实现真正的并行**，但依然可以提升程序的**响应性、结构清晰性、资源利用率**，所以仍然非常有用。





# sychronied修饰普通方法和静态方法的区别？什么是可见性?#

`synchronized` 修饰普通方法和静态方法的区别主要在于**加锁对象不同**：


### 1. `synchronized` 修饰普通方法
锁的是**当前实例对象（this）**。

```java
public synchronized void method() {
    // 线程访问时锁的是 this 实例
}
```
适用于多个线程访问**同一个对象实例**时的同步。


### 2. `synchronized` 修饰静态方法
锁的是**当前类的 Class 对象（ClassName.class）**。

```java
public static synchronized void staticMethod() {
    // 线程访问时锁的是 Class 对象
}
```
适用于多个线程访问**类级别的共享资源**。


### 举例：
```java
MyClass obj1 = new MyClass();
MyClass obj2 = new MyClass();
```
- `obj1.synchronizedMethod()` 和 `obj2.synchronizedMethod()` **互不干扰**
- `MyClass.staticSynchronizedMethod()` 对所有线程是**全局锁**


### 可见性（Visibility）是什么？

可见性是指**一个线程对共享变量的修改，能及时地被其他线程看到**。

在多线程中，由于 CPU 缓存机制，某个线程修改了变量，**其他线程可能一段时间内看不到最新值**。这就叫“可见性问题”。

关键字如 `volatile`、`synchronized`、`Lock` 都能保证可见性。

比如：
```java
boolean flag = false;

Thread A:
while (!flag) {
    // 一直循环看不到 flag 的更新
}

Thread B:
flag = true;
```
如果没有可见性保证，Thread A 可能永远不会跳出循环。使用 `volatile` 或 `synchronized` 能确保修改能被立即看到。






# volatile关键字干了什么？（什么叫指令重排）

`volatile` 是 Java 提供的轻量级同步机制，主要有两个作用：

1. 保证变量对所有线程的**可见性**。当一个线程修改了 `volatile` 变量的值，其他线程能立即看到最新值。
2. 禁止指令重排优化，保证代码执行的**有序性**，特别是在多线程并发场景下。

所谓**指令重排**是指，为了提高执行效率，编译器或处理器会对指令的执行顺序做优化，在不影响单线程语义的前提下改变指令执行顺序。但这种优化在多线程环境中可能会导致问题，比如变量还没初始化就被另一个线程访问到了。

使用 `volatile` 可以防止这种问题，它会在底层插入**内存屏障（memory barrier）**，确保在它前后的读写操作不会被重排。

简单说：`volatile` 让变量的更新对所有线程立即可见，并且防止乱序执行导致线程不一致问题。适用于状态标记、双重检查锁等轻量级同步场景，但不保证原子性。



# volatile 能否保证线程安全？在DCL上的作用是什么？

`volatile` 不能完全保证线程安全，它只能保证变量的**可见性**和**禁止指令重排**，不能保证操作的**原子性**。比如 `count++` 是复合操作，`volatile` 也无法防止线程冲突。

在 DCL（双重检查锁）中，`volatile` 的作用是防止对象在创建过程中发生**指令重排**。对象的创建过程包含三步：分配内存、调用构造方法、将引用赋值给变量。如果发生重排，可能会先赋值，再构造，导致其他线程拿到一个还没初始化完成的对象。使用 `volatile` 可以禁止这种重排，确保多线程环境下单例模式的安全。




# volatile和synchronize有什么区别？

`volatile` 和 `synchronized` 都是用于多线程并发控制的关键字，但它们的作用和使用场景不同：

**1. 功能区别：**

- `volatile` 保证 **可见性** 和 **禁止指令重排**，**不保证原子性**。
- `synchronized` 保证 **可见性、原子性、有序性**，是一个完整的同步机制。

**2. 是否加锁：**

- `volatile` 不加锁，性能高，不会阻塞线程。
- `synchronized` 会加锁，可能阻塞线程，性能稍低。

**3. 使用场景：**

- `volatile` 适用于状态标志、DCL 中的单例等轻量场景。
- `synchronized` 适用于临界区代码、涉及多个操作的复合逻辑，确保原子性。

**4. 底层原理：**

- `volatile` 通过内存屏障实现内存可见性和禁止重排。
- `synchronized` 通过 JVM 的 monitor（监视器锁）机制实现互斥和同步。

总结：`volatile` 更轻便，但只适用于简单变量的可见性控制；`synchronized` 更强大，可用于复杂的并发控制。

