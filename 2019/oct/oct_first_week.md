第六篇
## Algorithm 罗马数字转转整数

个人建议，在leetcode提交的代码不要写注释
```go
/*
   罗马数字包含以下七种字符: I， V， X， L，C，D 和 M。
	字符          数值
	I             1
	V             5
	X             10
	L             50
	C             100
	D             500
	M             1000
 给定一个罗马数字，将其转换成整数。输入确保在 1 到 3999 的范围内。
 romaToInt1 成功

 */
package main

import (
	"fmt"
)

func main() {
	fmt.Println(romaToInt1("IV")) // 4
	fmt.Println(romaToInt1("IX"))   // 9
	fmt.Println()
}

var romaMapper map[string]int= map[string]int{
	"I" : 1,
	"V" : 5,
	"X" : 10,
	"L" : 50,
	"C" : 100,
	"D" : 500,
	"M" : 1000,
}

func romanToInt(s string) int {
    var count int = 0
	strLen := len(s)
	for i:=0;i < strLen; i++ {
		countNum := romaMapper[string(s[i])]
		if i < strLen-1 {
            // 判断
			dumpNext := romaMapper[string(s[i+1])]
			if dumpNext > countNum {
				count=count+(dumpNext-countNum)
				i++
				continue
			}
		}
		count=count+countNum
	}
	return count
}

```


## Review:同步和Java内存模型
http://gee.cs.oswego.edu/dl/cpj/jmm.html

文章介绍了 原子性，可见性，原子性，Volatile

从原子性，可见性和有序性的角度分析，声明为volatile字段的作用相当于一个类通过 get/set 同步方法保护字段
```java
final class VFloat {

    private float value;

    final synchronized void set(float f) { value = f; }
    final synchronized float get()       { return value; }
}
```

## Tip
[解决部署Spring Boot 通用项目时报错找不到入口类(不需要入口类的情况)](https://blog.csdn.net/Fyf_010316/article/details/102605838)


## Share
下面是我这一周的乱叨叨，希望阅读后，能有所感悟

https://juejin.im/post/5d922a38f265da5b9a0d9fd7