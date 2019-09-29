9.16-9.22
## Algorithm:最长回文子串

[Go 解答链接](https://github.com/lucasIsfyf/go_baselearn/blob/master/learn/algorithm/%E4%B8%AD%E5%BF%83%E6%89%A9%E6%95%A3%E7%AE%97%E6%B3%95/LongestPalindrome.go)


![](https://user-gold-cdn.xitu.io/2019/9/17/16d3dc189282a4f4?w=898&h=400&f=png&s=45254)
## Review
本人基础比较差，因此重新开始学习英语
![](https://user-gold-cdn.xitu.io/2019/9/19/16d4801e790b7b1f?w=1080&h=1440&f=png&s=1257342)

![](https://user-gold-cdn.xitu.io/2019/9/19/16d4802467d3e547?w=1080&h=1440&f=png&s=961843)

## Tip:可达性分析算法
引用计数算法无法解决循环依赖的内存溢出问题，因此 JVM 后面没有再使用该算法进行实现定义垃圾，不过 Python 的垃圾回收依然使用的是引用计数算法
而垃圾回收是 JAVA 的一大特点，这个总不能弃用吧，厉害的 JAVA 工程师使用**可达性算法**代替引用计数算法

> 可达性算法: 从一个叫GC Roots的根节点出发，往链接路径深处搜索，如果一个对象不能达到GC Roots的时候，说明该对象不再被引用，可以被回收。下图中没有能通向 GC Roots 路径的对象，虽然他们内部相互调用，但是它们已经没有作用了，这样就解决了循环依赖的问题



![](https://user-gold-cdn.xitu.io/2019/9/24/16d622a9de9bda22?w=234&h=215&f=png&s=61638)

我们需要知道什么可以作为 GC Roots 对象

+ **局部变量/Local variables**   
虚拟机栈中引用的对象

```java
    // 1M
    private byte[] memory = new byte[ 1 * 1024 * 1024];

    public static String main(String[] args){
        
    }
```

+ **活动 JAVA 线程/Active Java Threads**  
处于启动,存活还没销毁的线程都是GC Roots，你可以想一下 JAVA 中每个线程都会独立开辟线程堆栈，那么以存活线程作为 GC Roots 再好不过

+  **静态变量/Static variables**   
注意:类本身可以被垃圾收集，这将删除所有引用的静态变量


+ **JNI 引用/ JNI References**     
   就记住引用Native方法的对象属于 GC Roots


## Share :Java I/O + Spring MVC + Nginx实现Base64转PNG图片并存储，并通过 Nginx 映射服务器图片地址实现更换用户头像功能

链接地址 : https://blog.csdn.net/Fyf_010316/article/details/101053478
