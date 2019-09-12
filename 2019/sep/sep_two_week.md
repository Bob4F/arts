
9.9-9.15
## Algorithm 两数相加
[算法链接](https://leetcode-cn.com/problems/add-two-numbers/)
因为本人最近正在学go，所以用go去写算法
```golang
func twonumcount(l1 *ListNode,l2 *ListNode)*ListNode{
	var result = &ListNode{0,nil}
	dump := result
	var carry = 0
	for l1 != nil || l2!=nil {
			x:=0
			y := 0
			if l1!=nil {
				x = l1.Val
			}
			if l2!=nil {
				y = l2.Val
			}

			sum := x + y + carry

			fmt.Println("init sum:",sum)
			carry = sum / 10
			fmt.Println("carry:",carry)
			sum %= 10
			fmt.Println("sum&:",sum)

			dump.Next = &ListNode{sum,nil}

			dump =dump.Next
			if l1 != nil{
				l1 = l1.Next
			}
			if(l2 != nil){
				l2 = l2.Next
			}
	}
	if carry==1 {
		dump.Next = &ListNode{1,nil}
	}
	return result.Next;
}
```
![](https://user-gold-cdn.xitu.io/2019/9/10/16d1a8270b0b1104?w=934&h=350&f=png&s=39347)
这么慢，我们来优化下
```golang
func addTwoNumbers(l1 *ListNode, l2 *ListNode) *ListNode {
    if l1 ==nil {
		return l2
	}else if l2== nil{
		return l1
	}
    var result = &ListNode{0,nil}
	dump := result
	var carry = 0
	for l1 != nil || l2!=nil {
           var sum int
			if l1==nil {
				sum =  l2.Val + carry
			}else if l2==nil{
				sum = l1.Val +  carry
			}else {
				sum = l1.Val + l2.Val + carry
			}
		
			carry = sum / 10
			sum %= 10
			dump.Next = &ListNode{sum,nil}
			dump =dump.Next
			if l1 != nil{
				l1 = l1.Next
			}
			if(l2 != nil){
				l2 = l2.Next
			}
	}
	if carry==1 {
		dump.Next = &ListNode{1,nil}
	}
	return result.Next;
}
```
相比之前打败了百分之九十，还不错
![](https://user-gold-cdn.xitu.io/2019/9/10/16d1a9172b1bdd20?w=940&h=468&f=png&s=49406)
## Review

## Tip: JVM GC / 垃圾回收 - 引用计数算法
我们了解下在 JVM 中如何定义垃圾的
### 引用计数算法
早期 JVM 通过该算法定义可回收的垃圾，现在已经弃用了,不过我们可以了解一下  
那么该算法是如何定义垃圾的呢？  
在每个堆对象中，分配一个计数器(RC),记录该对象的被引用次数，当 RC 为0时说明是可回收垃圾

`String str = new String("Lucas");`

![](https://user-gold-cdn.xitu.io/2019/9/12/16d23a92220a5c59?w=508&h=286&f=png&s=10675)

如果执行 `String str2 = str;` 那么计数器(RC)+1 

![](https://user-gold-cdn.xitu.io/2019/9/12/16d240474e737497?w=454&h=256&f=png&s=12693)

`str = null;`  // 删除一个引用，RC-1

![](https://user-gold-cdn.xitu.io/2019/9/12/16d2405d88e2262b?w=468&h=272&f=png&s=15027)
从上面的图，我们可以发现这样的确对可回收对象的效率判断是蛮快的，但引用计数算法有个很致命的缺点，会导致 **OOM**,下面我们通过链表来说明
```java
class ListNode{
	int val;
	ListNode next;

	public ListNode(int val){
		this.val = val;
	}
}

public static void main(String[] args){
	ListNode l1 = new ListNode(3);
	ListNode l2 = new ListNode(5);
	l1.next = l2;
	l2.next = l1;
	/*
	   操作链表
	*/ 
	l1 = null;
	l2 = null
}
```
最后进行了销毁操作，算是告诉 JVM 这是一个可回收的垃圾了，问题是我们在l1.next,l2.next又引用了他们，按上面的算法，此时他们 RC = 2，销毁后，RC 也等于1！JVM不会去销毁这个对象，那么这个对象就无法销毁了，导致了内存的溢出
## Share
