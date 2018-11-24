C#本質論

## 第九章 Well-Formed Types

![第九章](./img/c_9.jpg)

### 重寫Object的成員

默認實現只是返回類名

對於調試有很大的意義

![tostring的指導意見](./img/tostring_guildline.jpg)

### 重寫GetHashCode

重寫Equals的同時也需要重寫GetHashCode,特別是儅作爲一個哈希表的鍵的時候，這個方法的目的就是爲了平衡哈希表

原則：
* Required：相等的實例必須有相等的HashCode

* Required: 在實例的實力周期内HashCode必須保持相同，在很多情況下，爲了達到這個目的需要緩存這個結果

* Required: GetHashCode() 不能抛出異常; Get HashCode() 必須始終的返回一個值

* Performance: 只要可能，哈希码应该是惟一的。然而，由于哈希码只返回一个int值，对于那些可能比int值更多的对象(实际上是所有类型)，哈希码中必然存在重叠。 (一個明顯的例子就是long類型，long的值顯然就比int的值更多，必然存在重叠) 

* Performance: 哈希值應當分佈均匀 . 例如，创建一个散列的時候，如果没有考虑到字符串在基于拉丁的语言中的分布主要集中在初始128个ASCII字符上这一事实，这将导致字符串值的分布非常不均匀，不是一个强大的GetHashCode()算法.

* Performance: GetHashCode() 應該針對性地進行優化. GetHashCode()通常在Equals()实现中使用，以便在哈希码不同时短路完整的Equals比较。 因此，当类型在字典集合中用作键类型时，经常调用它。

* Performance: 两个对象之间的小差异应该导致哈希码值之间的大差异——理想情况下，对象中的1位差异应该平均导致大约16位哈希码发生变化。这有助于确保哈希表无论如何“嵌套”哈希值都保持平衡。

* Security: 攻击者应该很难创建具有特定散列代码的对象。这种攻击是向哈希表中注入大量数据，这些数据全部哈希到相同的值。哈希表实现可能变得低效，导致可能的拒绝服务攻击。

        NOTE

        Calling ReferenceEquals() on value types will always return false 

        NOTE 

        The implementation of object.Equals() , the default implementation on all objects before overloading, relies on ReferenceEquals() alone.

The steps for overriding Equals() are as follows: 

1. Check for null 
2. Check for reference equality if the type is a reference type.
3. Check for equivalent types.
5. Possibly check for equivalent hash codes to short-circuit an extensive, field-by-field comparison. (Two objects that are equal cannot have dif- ferent hash codes.) 
6. Check base.Equals() if the base class overrides Equals() .
7. Compare each identifying field for equality.
8. Override GetHashCode() .
9. Override the == and != operators (see the next section).

![](./img/equals.jpg)

### Operator Overloading

C#不支持的重載：
x.y , f(x) , new , typeof , default , checked , unchecked , delegate , is , as , = , and => 

One particularly noteworthy operator that can- not be implemented is the assignment operator; there is no way to change the behavior of the = operator

#### Comparison Operators (==, !=, <, >, <=, >=)

Once Equals() is overridden, there is a possible inconsistency. That is, two objects could return true for Equals() but false for the == operator because == performs a reference equality check by default. To correct this flaw, it is important to overload the equals ( == ) and not equals ( != ) opera- tors as well.

For the most part, the implementation for these operators can delegate the logic to Equals() , or vice versa. However, for reference types, some initial null checks are required first (see Listing 9.6)

    NOTE 
    AVOID using the equality comparison operator ( == ) from within the implementation of the == operator overload.

#### Binary Operators (+, -, *, /, %, &, |, ^, <<, >>)

#### Combining Assignment with Binary Operators

#### Conditional Logical Operators (&&, ||)

Like assignment operators, conditional logical operators cannot be over-
loaded explicitly.

However, because the logical operators & and | can be
overloaded, and the conditional operators comprise the logical opera-
tors, effectively it is possible to overload conditional operators.

#### Unary Operators (+, -, !, ~, ++, --, true, false)

#### Conversion Operators

对于可能出现异常的转换，总是把他设置成显示转换

任何会损失精度的转换都应该设置为显示转换。

转换出现异常抛出：System.InvalidCastException

隐式转换不要抛出异常

### Referencing Other Assemblies

#### Changing the Assembly Target

The compiler allows you to create four 1 different assembly types via the /target option:

1. Console executable: This is the default type of assembly, and all compila- tion thus far has been to a console executable. (Leaving off the /target option or specifying /target:exe creates a console executable.)

2. Class library: Classes that are shared across multiple executables are generally defined in a class library ( /target:library ).

3. Windows executable: Windows executables are designed to run in the Microsoft Windows family of operating systems and outside the com- mand console ( /target:winexe ).
4. Module: To facilitate use of multiple languages within the same as- sembly, code can be compiled to a module and multiple modules can be combined to form an assembly ( /target:module ).

#### Referencing an Assembly

    NOTE： Members with an accessibility modifier of protected internal will be accessible from all locations within the containing assembly and from classes that derive from the type, even if the derived class is not in the same assembly.

### Defining Namespaces

    NOTE： In the CLR there is no such thing as a “namespace.” The type’s name actually is the fully qualified type name.

命名空间支持嵌套

#### Namespace Alias Qualifier

![](./img/namespace.jpg)

### Garbage Collection

#### Weak References

### Resource Cleanup

#### Finalizers

    NOTE 
    You cannot determine at compile time exactly when the finalizer will execute.

    NOTE 
    Finalizers cannot be called explicitly; only the garbage collector can invoke a finalizer.

终结器负责对资源（文件，数据库连接等）的回收，不负责对内存的回收

终结器不要抛出异常

#### Deterministic Finalization with the using Statement

终结器是个保险，当类的使用者忘记显示释放资源的时候

#### Garbage Collection, Finalization, and IDisposable

    NOTE
    Objects with finalizers that are not explicitly disposed will end up with
    an extended object lifetime. Even after all explicit references have gone
    out of scope, the f-reachable queue will have references, keeping the
    object alive until the f-reachable queue processing is complete.

### Lazy Initialization    