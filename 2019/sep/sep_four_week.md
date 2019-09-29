9.23-9.29
## Algorithm: 无重复字符的最长子串
给定一个字符串，请你找出其中不含有重复字符的 最长子串 的长度。

```
示例 1:
输入: "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

示例 2:
输入: "bbbbb"
输出: 1
解释: 因为无重复字符的最长子串是 "b"，所以其长度为 1。

示例 3:
输入: "pwwkew"
输出: 3
解释: 因为无重复字符的最长子串是 "wke"，所以其长度为 3。
(请注意，你的答案必须是 子串 的长度，"pwke" 是一个子序列，不是子串)   
```
下面是 Golang 代码
```go
func main() {
	fmt.Println(lengthOfLongSubstring("a"))		   // a 1
	fmt.Println(lengthOfLongSubstring("aab")) // aab 2
	fmt.Println(lengthOfLongSubstring("abcabcbb")) // abc 3
	fmt.Println(lengthOfLongSubstring("pwwkew")) // abc 3
}

// 返回 最长无重复子串 长度
func lengthOfLongSubstring(s string)int{
	lastOccurred := make(map[int32]int) //记录字符最后出现的位置
	start,maxLength := 0,0
	for i, ch := range []int32(s) {
        // 如果ch在map中 并且 该ch的索引大于等于start
        // 假设 s = "abcabcbb" i = 5  ch = b  
		// 在map中 存了上一个b的位置 abc  那么lastI = 2 那么此时 start 是 3
		if lastI, ok := lastOccurred[ch]; ok && lastI >= start {
			start = lastI + 1
        }
        // 如果没有重复的 那么 start 一直没变，i在增长，我们可以通过下面的等式得到这个无重复子串的长度
        // +1是因为索引是从0开始
		if i-start+1 > maxLength {
			maxLength = i - start + 1
		}
		// ch 其实是底层的ascii值
		lastOccurred[ch] = i
	}
	return maxLength
}
```
leetcode结果，处理时间有一些波动，最大在8ms左右
![](https://user-gold-cdn.xitu.io/2019/9/25/16d665d6466c0618?w=922&h=256&f=png&s=29162)

## Review
https://shipilev.net/jvm/anatomy-quarks/3-gc-design-and-pauses/

该文分通过100M个16字节对象列出了不同 OpenJDK GC的设计以及暂停
```java
public class AL {
    static List<Object> l;
    public static void main(String... args) {
        l = new ArrayList<>();
        for (int c = 0; c < 100_000_000; c++) {
            l.add(new Object());
        }
    }
}
```
下面的收集器是 *ConcurrentMarkSweepGC*

```vim
[GC (Allocation Failure) [PSYoungGen: 55684K->10738K(76288K)] 55684K->36340K(251392K), 0.0616123 secs] 
[Times: user=0.17 sys=0.02, real=0.07 secs] 

[GC (Allocation Failure) [PSYoungGen: 74111K->10720K(141824K)] 99713K->80915K(316928K), 0.0932121 secs] [Times: user=0.25 sys=0.02, real=0.09 secs] 

[GC (Allocation Failure) [PSYoungGen: 141792K->10752K(141824K)] 211987K->188106K(319488K), 0.2669306 secs] 
[Times: user=0.54 sys=0.07, real=0.27 secs] 

[Full GC (Ergonomics) [PSYoungGen: 10752K->0K(141824K)] [ParOldGen: 177354K->171712K(399360K)] 188106K->171712K(541184K), [Metaspace: 3152K->3152K(1056768K)], 1.9670609 secs] 
[Times: user=2.79 sys=0.02, real=1.97 secs] 

```
> user代表GC 需要的各个CPU总时间(各个CPU时间相加)，sys代表回收器自身的行为所占用CPU时间，real则代表本次GC所耗费的真正耗时(在多核CPU中并行回收，它通常小于user) 。

+ G1 (Garbage First-Garbage Collection)  
JDK9 中默认采用的GC设计，年轻代的暂停收集在500-1000ms范围内，当状态稳定后，停顿时间会减少;user时间大于real，说明GC会通过所有可用并发来收集指针
+ Parallel
使用Paraller,会开辟更大的 Eden/Survivors 区承受更多的分配，user大于real
+ Concurrent Mark Sweep  
性能相对较慢，案例代码，real时间是Paraller的5倍以上，G1的15倍  
但好处是并发扫描旧生代并清理，也不会停止应用程序

+ Shenandoah  
没有 Young 回收
+ Epsilon GC
预估 GC 开销
real几乎等于user与sys之和，说明只启动了一个线程

+ 文章的总结
不同的GC在其实现中具有不同的权衡。把GC当作“坏主意”是很困难的。通过了解您的工作量，可用的GC实现和性能要求，为您的工作量选择一个收集器。即使选择不使用GC的平台作为目标，您仍然需要知道（并选择！）您的本机内存分配器。在运行实验性工作负载时，请尝试了解它们会告诉您什么，并从中学到东西。

## Tips Docker For Mac 启动，不能在命令行使用docker命令
网上大多数配置 */etc/paths/*方法对我无效
先用这个路径 
*/Applications/Docker.app/Contents/Resources/bin*
在 */etc/profile* 中配置上面这个路径

```bash
sudo vim /etc/profile  // 超级管理员打开

// 下面是配置
export MAVEN_HOME="/Users/mac/dev/apache-maven-3.6.1"
export DOCKER_PATH="/Applications/Docker.app/Contents/Resources/bin"
export PATH=".$PATH:$MAVEN_HOME:$DOCKER_PATH"
// wq! /etc/profile 结束

source /etc/profile //使配置生效
```

![](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAxOS85LzI4LzE2ZDc3MDFkZWFmODE2NjM?x-oss-process=image/format,png)
成功



## Share：Nginx配置HTTPS
https://juejin.im/post/5d8c87cb5188252b38498b6b
