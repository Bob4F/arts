9.2-9.8
## Algorithm :
### 回文数 
> 判断一个整数是否是回文数。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。  
> 进阶：你能不将整数转为字符串来解决这个问题吗？  

较为简单，思路可以根据[上一次](https://github.com/lucasIsfyf/arts/blob/master/2019/auguest/last-arts.md)做的整数反转做进阶
```java
// 12ms / 37.9MB
public boolean isPalindrome(int x) {
        // 保存 x
        int x1 =x;
        // 要转换的变量
        int temp = 0;
        // 转换
        while(x>0){
            temp = temp * 10 + x%10;
            x = x / 10;
        }
        // 比较
        if(x1==temp){
            return true;
        }
        return false;
    }
```
最近在学go，用go试试
```golang
// 36 ms / 5 mb
func isPalindrome(x int) bool {
    cache_x := x
    temp := 0 
    for ; x > 0 ; x = x / 10 {
        temp = temp*10 + x%10
    }
    if temp == cache_x {
        return true
    }
    return false 
}

// 优化下看看
func isPalindrome(x int) bool {
    cache_x := x
    temp := 0 
    for x > 0 {
        temp = temp*10 + x%10
        x /= 10
    }
    if temp == cache_x {
        return true
    }else {
          return false
    }
}
```
![](https://user-gold-cdn.xitu.io/2019/9/4/16cfa5eddc501f3d?w=904&h=286&f=png&s=28119)


###  移除元素
> 给定一个数组 nums 和一个值 val，你需要原地移除所有数值等于 val 的元素，返回移除后数组的新长度  
> 不要使用额外的数组空间，你必须在原地修改输入数组并在使用 O(1) 额外空间的条件下完成。

#### Java
```java
public int removeElement(int[] nums, int val) {
    int size =0;// 记录不等于val的数
    for (int index=0;nums.length>index;index++){
        if(nums[index] != val){
            // 重新赋值 将当前啊index索引下的值修改，重新排列数组
            nums[size] = nums[index];
            size++;
        }
    }
    return size;
}
```
![](https://user-gold-cdn.xitu.io/2019/9/5/16cff46fe54b9509?w=888&h=372&f=png&s=37460)
#### Go
```go
func removeElement(nums []int, val int) int {
    size:=0
    for index:=0;index<len(nums);index++ {
        if(val!=nums[index]){
            nums[size]=nums[index]
            size++
        }
    }
    return size
}
```
![](https://user-gold-cdn.xitu.io/2019/9/5/16cff4dff03ef2d1?w=904&h=272&f=png&s=28342)

## Review

## Tip: Redis 缓存对象淘汰机制
### 什么时候 Redis 淘汰机制
Redis 淘汰机制是指创建的一些 内存对象被 Redis 主动淘汰(从内存中移除),导致部分缓存失效
### 为什么要淘汰机制
假设我们有一个 Redis 服务器，服务器物理内存大小为 1G ，刚开始我们需要缓存的数据很少，似乎足够用了，随着业务量增长，我们放在 Redis 里面的数据越来越多了，数据量大小似乎超过了 1G，但是应用还可以正常运行，这是虚拟内存的功劳，这里就不多做解释了；问题在于一台服务器不可能全部内存都用来存储信息，所以实现者为了防止内存过于饱和，采取了一些措施来管控内存

**Redis 淘汰机制的初衷是为了更好地使用内存，用一部分的缓存失效来换取内存的使用效率**

### 如何使用淘汰机制
```conf
# maxmemory <bytes>
```
我们可以通过配置 redis.conf 中的 maxmemory 这个值来开启内存淘汰功能
maxmemory 为 0 的时候表示我们对 Redis 的内存使用没有限制
#### 相关淘汰策略
内存淘汰只是 Redis 提供的一个功能，为了更好地实现这个功能，必须为不同的应用场景提供不同的策略，内存淘汰策略讲的是为实现内存淘汰我们具体怎么做，要解决的问题包括淘汰键空间如何选择？在键空间中淘汰键如何选择？

Redis 提供了下面几种淘汰策略供用户选择，其中默认的策略为 noeviction 策略：

+ noeviction：当内存使用达到阈值的时候，所有引起申请内存的命令会报错
+ allkeys-lru：在主键空间中，优先移除最近未使用的key
+ volatile-lru：在设置了过期时间的键空间中，优先移除最近未使用的 key
+ allkeys-random：在主键空间中，随机移除某个 key
+ volatile-random：在设置了过期时间的键空间中，随机移除某个 key
+ volatile-ttl：在设置了过期时间的键空间中，具有更早过期时间的 key 优先移除


## Share
[Spring Boot + Spring MVC 拦截器实现接口令牌验证](https://blog.csdn.net/Fyf_010316/article/details/100669876)