### `==`和`equals`方法、hashcode方法的区别

`==`比较的是变量的（栈）内存中存放的对象的（堆）内存地址，用来判断两个对象的地址是否相同，即是否是指同一个对象。比较的是真正意义上的指针操作。

`equals`默认是用来比较两个变量的引用地址是否相等，可以覆盖对象的equals方法来改变其意义。

`hashcode`

Java对于eqauls方法和hashCode方法是这样规定的： 

(1)同一对象上多次调用hashCode()方法，总是返回相同的整型值。

(2)如果a.equals(b)，则一定有a.hashCode() 一定等于 b.hashCode()。 

(3)如果!a.equals(b)，则a.hashCode() 不一定等于 b.hashCode()。此时如果a.hashCode() 总是不等b.hashCode()，会提高hashtables的性能。

(4)a.hashCode()==b.hashCode() 则 a.equals(b)可真可假。

(5)a.hashCode()！= b.hashCode() 则 a.equals(b)为假。 

关于这两个方法的重要规范： 

规范1：若重写equals(Object obj)方法，有必要重写hashcode()方法，确保通过equals(Object obj)方法判断结果为true的两个对象具备相等的hashcode()返回值。说得简单点就是：“如果两个对象相同，那么他们的hashcode应该相等”。不过请注意：这个只是规范，如果你非要写一个类让equals(Object obj)返回true而hashcode()返回两个不相等的值，编译和运行都是不会报错的。不过这样违反了Java规范，程序也就埋下了BUG。 

规范2：如果equals(Object obj)返回false，即两个对象“不相同”，并不要求对这两个对象调用hashcode()方法得到两个不相同的数。说的简单点就是：“如果两个对象不相同，他们的hashcode可能相同”。 



**总结：**
1、equals方法用于比较对象的内容是否相等（覆盖以后）

2、hashcode方法只有在集合中用到

3、当覆盖了equals方法时，比较对象是否相等将通过覆盖后的equals方法进行比较（判断对象的内容是否相等）。

4、将对象放入到集合中时，首先判断要放入对象的hashcode值与集合中的任意一个元素的hashcode值是否相等，如果不相等直接将该对象放入集合中。如果hashcode值相等，然后再通过equals方法判断要放入对象与集合中的任意一个对象是否相等，如果equals判断不相等，直接将该元素放入到集合中，否则不放入。