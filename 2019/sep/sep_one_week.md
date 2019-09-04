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



### 
