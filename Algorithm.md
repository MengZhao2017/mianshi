
----------------------------------------------------------------------------------------------------------------------------------------

寻找两个整数的GCD：

	public class GCD {
		public static void main(String[] args) {
			// TODO Auto-generated method stub
			Scanner s=new Scanner(System.in);
			System.out.println("请输入第一个数：");
			int a=s.nextInt();
			System.out.println("请输入第二个数：");
			int b=s.nextInt();
			System.out.println("最大公约数是："+G(a,b));
		}
		public static int G(int a,int b)
		{
			int gcd=1;
			if(a%b==0)
			{
				return b;
			}
			for(int k=b/2;k>=1;k--)
			{
				if(a%k==0 && b%k==0)
				{
					gcd=k;
					break;
				}
			}
			return gcd;
		}

	}

	运行结果：
	请输入第一个数：
	64
	请输入第二个数：
	42
	最大公约数是：2

----------------------------------------------------------------------------------------------------------------------------------------

链表的翻转

     public class lianbiao_reverse {
	 class node{
		
	    node next=null;
		int data;
		 public node(int data)
		 {
			 this.data=data;
		 }
		public node getNext() {
			return next;
		}
		public void setNext(node next) {
			this.next = next;
		}
		public int getData() {
			return data;
		}
		public void setData(int data) {
			this.data = data;
		}
	}
	

	public static void main(String[] args) {
		lianbiao_reverse a=new lianbiao_reverse();
		node head=a.new node(4);//内部类引用外部类的效果
		node no1=a.new node(3);
		node no2=a.new node(1);
		node no3=a.new node(2);
		head.setNext(no1);
		no1.setNext(no2);
		no2.setNext(no3);
		//打印翻转前的链表
		node h=head;
		while(h!=null)
		{
			System.out.println(h.getData()+' ');
			h=h.getNext();
		}
		//打印翻转后的链表
		head=Reverse(head);
		System.out.println("\n****************************");
		while(head!=null)
		{
			System.out.println(head.getData()+' ');
			head=head.getNext();
		}
	}
	

	/** 
	 * 
	 * 链表翻转
	 * 
	 * **/
	public static node Reverse(node head)
	{
		if(head==null || head.next==null)
			return head;
		node pReversehead=head;  //这个是翻转后的头结点
		node pNode=head;       //这个是原来链表指示头结点的
		node prev=null;       //这个是新的链表的临时结点
		node pnext=null;   //这个是旧表的临时结点，用来存储头结点的下一个结点
		while(pNode!=null)
		{
			pnext=pNode.next;
			if(pnext==null)
				pReversehead=pNode;
			pNode.next=prev;
			prev=pNode;
			pNode=pnext;
		
		}
		return pReversehead;
	}
     }

	运行结果：
	36
	35
	33
	34

	****************************
	34
	33
	35
	36

----------------------------------------------------------------------------------------------------------------------------------------

求链表中倒数第k个结点

    public class find_daoshu_k {
	public static void main(String[] args) {
		find_daoshu_k a=new find_daoshu_k();
		node head=a.new node(4);
		node no1=a.new node(3);
		node no2=a.new node(1);
		node no3=a.new node(2);
		node no4=a.new node(5);
		head.setNext(no1);
		no1.setNext(no2);
		no2.setNext(no3);
		no3.setNext(no4);
		node h=head;
		while(h!=null)
		{
			System.out.println(h.getData()+' ');
			h=h.getNext();
		}
		node s=find_daoshu_elment(head,2);
		System.out.println("\n****************************");
	    System.out.println(s.getData()+' ');

	}
	
	
    class node{
		
	    node next=null;
		int data;
		 public node(int data)
		 {
			 this.data=data;
		 }
		public node getNext() {
			return next;
		}
		public void setNext(node next) {
			this.next = next;
		}
		public int getData() {
			return data;
		}
		public void setData(int data) {
			this.data = data;
		} 
	}
	
	
	public static node find_daoshu_elment(node head,int k)
	{
		if(k<1)
		{
			return null;
		}
		
		node p1=head;
		node p2=head;
		for (int i =0;i<k;i++)
		{
			p1=p1.next;
		}
		while (p1!=null)
		{
			p1=p1.next;
			p2=p2.next;
		}
		
		return p2;
	}

     }
	实验结果：
	36
	35
	33
	34
	37

	****************************
	34


----------------------------------------------------------------------------------------------------------------------------------------

查找链表的中间结点

思想：使用指针追赶的方法，fast每次走两步，slow每次走一步，当fast到链表尾部的时候，slow指向中间的链表结点。

	public class find_mid_lianbiao {
	public static void main(String[] args) {
		
		find_mid_lianbiao a=new find_mid_lianbiao();
		node head=a.new node(4);
		node no1=a.new node(3);
		node no2=a.new node(1);
		node no3=a.new node(2);
		node no4=a.new node(5);
		head.setNext(no1);
		no1.setNext(no2);
		no2.setNext(no3);
		no3.setNext(no4);
		node h=head;
		while(h!=null)
		{
			System.out.println(h.getData()+' ');
			h=h.getNext();
		}
		node s=findMid(head);
		System.out.println("\n****************************");
		System.out.println(s.getData()+' ');
	}

 	 class node{
	    node next=null;
		int data;
		 public node(int data)
		 {
			 this.data=data;
		 }
		public node getNext() {
			return next;
		}
		public void setNext(node next) {
			this.next = next;
		}
		public int getData() {
			return data;
		}
		public void setData(int data) {
			this.data = data;
		} 
	   }
  
  	//查找链表中间节点
  	public static node findMid(node head)
  	{
	  if(head==null || head.next==null)
	  {
		  return head;
	  }
	  node p=head;
	  node q=head;
	  while(p.next!=null && q.next!=null)
	  {
		  p=p.next;
		  q=q.next.next;
	  }
	  return p;
 	 }
	}

	实验结果；
	36
	35
	33
	34
	37

	****************************
	33

----------------------------------------------------------------------------------------------------------------------------------------

删除链表中重复的结点：

		public static void  delet_chongfu_element(node head)
	{
		
		node p=head;
		while(p.next!=null)
		{
			node q=p;
			while(q.next!=null)
			{
				if(p.data==q.next.data)
				{
					q.next=q.next.next;
				}else {
					q=q.next;
				}
			}
			p=p.next;
			System.out.println(p.getData()+' ');
		}
		
	}


----------------------------------------------------------------------------------------------------------------------------------------

计算随机字符串中每个字符出现的次数：

	public class sum_String_count {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Scanner as=new Scanner(System.in);
		System.out.println("请输入一个字符串:");
		String Str=as.next();
	
		Map map=new HashMap();
		for(int i=0;i<Str.length();i++)
		{
			char ch=Str.charAt(i);
			boolean b=map.containsKey(ch);
			if(b)
			{
				int count=(int) map.get(ch);
				count=count+1;
				map.put(ch, count);
			}else
			{
				map.put(ch, 1);
			}
		}
		//我们通常说，keySet()返回所有的键，values()返回所有的值
		for(Object ch:map.keySet())
		{
			Object value=map.get(ch);
			System.out.println("字符："+ch+"出现的次数为："+value);
		}
	}
	}
	运行结果：

	请输入一个字符串:
	gyuljkhkghfl
	字符：u出现的次数为：1
	字符：f出现的次数为：1
	字符：g出现的次数为：2
	字符：h出现的次数为：2
	字符：y出现的次数为：1
	字符：j出现的次数为：1
	字符：k出现的次数为：2
	字符：l出现的次数为：2

----------------------------------------------------------------------------------------------------------------------------------------

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。

![](https://github.com/MengZhao2017/mianshi/raw/master/res/zidianxu.png)
    
    public class zidianxu_string {
	public static void main(String[] args) {
		zidianxu_string p=new zidianxu_string();
		Scanner sc=new Scanner(System.in);
		System.out.println("请输入字符串：");
		String S=sc.next();
		System.out.println("字符串字典序如下："+Permutation(S));
	}
	
	
	public static ArrayList<String> Permutation(String str)
	{
		List<String> res=new ArrayList<>();
		if(str!=null && str.length()>0)
		{
			PermutationHelper(str.toCharArray(),0,res);
			Collections.sort(res);
		}
		
		return (ArrayList)res;
	}
	
	public static void PermutationHelper(char[] cs,int i,List<String> list)
	{
		if(i==cs.length-1)
		{
			String val=String.valueOf(cs);
			if(!list.contains(val))
			{
				list.add(val);
			}
		}
		else
		{
			for(int j=i;j<cs.length;j++)
			{
				swap(cs,i,j);
				PermutationHelper(cs,i+1,list);
				swap(cs,i,j);
			}
		}
	}
	
	public static void swap(char[] cs,int i,int j)
	{
		char temp=cs[i];
		cs[i]=cs[j];
		cs[j]=temp;
	}
     }


结果：
请输入字符串：
abcd
字符串字典序如下：[abcd, abdc, acbd, acdb, adbc, adcb, bacd, badc, bcad, bcda, bdac, bdca, cabd, cadb, cbad, cbda, cdab, cdba, dabc, dacb, dbac, dbca, dcab, dcba]

----------------------------------------------------------------------------------------------------------------------------------------

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

方法1：

		    public class GetLeastNumbers {
			public static void main(String[] args) {
				// TODO Auto-generated method stub
				Scanner sc=new Scanner(System.in);
				int array[]=new int[8];
				for(int i=0;i<=7;i++)
				{
					System.out.println("请输入一个整数：");
					array[i]=sc.nextInt();
				}
				System.out.println(GetLeastNumbers_Solution(array,4));
			}

			public static ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
				Arrays.sort(input);
				List list=new ArrayList<>();
				for(int i=0;i<k;i++)
				{
					list.add(input[i]);
				}
				return (ArrayList<Integer>) list;
		    }
		}

解法2.

冒泡排序的思想，只不过最外层循环K次就可以了，也就是说不用全部排序，只挑出符合提议的K个就可以。

	public ArrayList<Integer> GetLeastNumbers_Solution(int [] input, int k) {
		ArrayList<Integer> al = new ArrayList<Integer>();
		if (k > input.length) {
		    return al;
		}
		for (int i = 0; i < k; i++) {
		    for (int j = 0; j < input.length - i - 1; j++) {
			if (input[j] < input[j + 1]) {  //这里是把最小值替换到最后
			    int temp = input[j];
			    input[j] = input[j + 1];
			    input[j + 1] = temp;
			}
		    }
		    al.add(input[input.length - i - 1]);
		}
		return al;
	    }
	    
 结果：
 
 请输入一个整数：
4
请输入一个整数：
5
请输入一个整数：
1
请输入一个整数：
6
请输入一个整数：
2
请输入一个整数：
7
请输入一个整数：
3
请输入一个整数：
8
[1, 2, 3, 4]

----------------------------------------------------------------------------------------------------------------------------------------

判断一个单链表是不是有环？

思想：使用指针追赶的方法，slow指针每次走一步，fast指针每次走两步，如果存在环，则两者相遇。

     public class ListIsLoop {

	class node{
	    node next=null;
		int data;
		 public node(int data)
		 {
			 this.data=data;
		 }
		public node getNext() {
			return next;
		}
		public void setNext(node next) {
			this.next = next;
		}
		public int getData() {
			return data;
		}
		public void setData(int data) {
			this.data = data;
		}
	}
	
	
	
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		ListIsLoop a=new ListIsLoop();
		node n1 =a. new node(1);  
		node n2 =a. new node(2);  
		node n3 = a.new node(3);  
		node n4 = a.new node(4);  
		node n5 = a.new node(5);  
	
        n1.next = n2;  
        n2.next = n3;  
        n3.next = n4;  
        n4.next = n5;  
        n5.next = n1;  
        
      System.out.println(IsLoop(n1)); 
	

	}
	
	public static boolean IsLoop(node head)
	{
		node fast=head;
		node slow=head;
		if(fast==null)
		{
			return  false;
		}
		while(fast!=null && fast.next!=null)
		{
			fast=fast.next.next;
			slow=slow.next;
			if(fast==slow)
			{
				System.out.println("这个链表有环");
				return true;
			}
		}
		return false;
	}	
    }

结果：

这个链表有环

true

----------------------------------------------------------------------------------------------------------------------------------------

寻找单链表中的环？

思想：分别从碰撞的点，头指针开始走，相遇的那个点就是连接点。

    public class ListIsLoop {
	class node{	
	    node next=null;
		int data;
		 public node(int data)
		 {
			 this.data=data;
		 }
		public node getNext() {
			return next;
		}
		public void setNext(node next) {
			this.next = next;
		}
		public int getData() {
			return data;
		}
		public void setData(int data) {
			this.data = data;
		}
	}
	
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		ListIsLoop a=new ListIsLoop();
		node n1 =a. new node(1);  
		node n2 =a. new node(2);  
		node n3 = a.new node(3);  
		node n4 = a.new node(4);  
		node n5 = a.new node(5);  	
        n1.next = n2;  
        n2.next = n3;  
        n3.next = n4;  
        n4.next = n5;  
        n5.next = n2;  
        
      System.out.println(FindLoopPort(n1).getData()); 
	}
	
	//找到环的入口
	
	public static node FindLoopPort(node head)
	{
		node fast=head;
		node slow=head;
		while(fast!=null && fast.next!=null)
		{
			fast=fast.next.next;
			slow=slow.next;
			if(fast==slow)
			{
				break;
			}
		}
		
		if(fast==null || fast.next==null)
		{
			return null;
		}
		
		slow=head;
		while(slow!=fast)
		{
			slow=slow.next;
			fast=fast.next;
		}
		return slow;
	      }
            }

结果：

2

----------------------------------------------------------------------------------------------------------------------------------------

判断两个链表（无环）是否相交？

思路：先遍历一个链表到其尾部，然后将尾部原本指向null的next指针指向第二个链表，这样两个链表就合并成一个链表了，问题就转化为判断一个链表是否有环的问题了，可以使用上面的方法求解。

----------------------------------------------------------------------------------------------------------------------------------------

寻找两个相交链表的第一个公共结点？

思路：最简单的方法就是先顺序访问其中的一个链表，在每访问一个结点时候，都对另外一个链表进行遍历，知道找到一个相等的结点位置。

----------------------------------------------------------------------------------------------------------------------------------------

求一维连续子向量的最大和？例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。

思想：dp动态规划。

       public class FindGreatestSumOfSubArray {
	public static void main(String[] args) {
		Scanner sc=new Scanner(System.in);
		System.out.println("请输入一串整数并在输入的时候用英文逗号隔开：");
		String inputString=sc.next().toString();
		String stringArray[]=inputString.split(",");
		int num[]=new int[stringArray.length];
		for(int i=0;i<num.length;i++)
		{
			num[i]=Integer.parseInt(stringArray[i]);
		}
                 //下面是动态规划思想
		int res=num[0];
		int max=num[0];
		for(int i=1;i<num.length;i++)
		{
			max=Math.max(max+num[i], num[i]);
			res=Math.max(max, res);
		}
		
		System.out.println(res);

	}

     }

结果：

请输入一串整数并在输入的时候用英文逗号隔开：

6,-3,-2,7,-15,1,2,2

8

----------------------------------------------------------------------------------------------------------------------------------------
统计整数内出现 1 的个数：

求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数。



     public class NumberOf_1_between1AndN {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		int count=0;
		Scanner sc=new Scanner(System.in);
		System.out.println("请输入一个正整数：");
		int n=sc.nextInt();
		StringBuffer s=new StringBuffer();
		for(int i=1;i<n;i++)
		{
			s.append(i);
		}
		
		for(int i=0;i<s.length()-1;i++)
		{
			if(s.charAt(i)=='1')
			{
				count++;
			}
		}
		
		System.out.println("一共有数字1的个数是："+count);

	}
       }


结果：
请输入一个正整数：

13

一共有数字1的个数是：5

----------------------------------------------------------------------------------------------------------------------------------------

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

	import java.util.*;

	public class printMinNumber {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		Scanner sc=new Scanner(System.in);
		System.out.println("请输入一组整数，并用英文逗号分开：");
		String inputString=sc.next().toString();
		String stringArray[]=inputString.split(",");
		int num[]=new int[stringArray.length];
		for(int i=0;i<num.length;i++)
		{
			num[i]=Integer.parseInt(stringArray[i]);
		}

	    System.out.println(printNum(num));

	}
	
	
	public static String printNum(int num[])
	{
		if(num==null ||num.length==0)
		{
			return "";
		}
		
		MyComparator myComparator=new printMinNumber().new MyComparator();
		List<Integer>  list=new ArrayList<Integer>();
		for(int i:num)
		{
			list.add(i);
		}
		
		Collections.sort(list, myComparator);
		StringBuilder sb=new StringBuilder();
		for(Integer val :list) {
			sb.append(val);
		}
		return sb.toString();
		
	}
	
	
	//内部类
	
	public class MyComparator implements Comparator<Integer>{
		
		public int compare(Integer o1,Integer o2)
		{
			String s1=String.valueOf(o1);
			String s2=String.valueOf(o2);
			String str1=s1+s2;
			String str2=s2+s1;
			return str1.compareTo(str2);
		}
		
	}

    }

结果：

请输入一组整数，并用英文逗号分开：

3,32,321

321323

注解：这里说一下重写的 public int compareTo(Student o){} 这个方法，它返回三种 int 类型的值： 负整数，零 ，正整数。

返回值	含义

负整数	当前对象的值 < 比较对象的值 ， 位置排在前

零	当前对象的值 = 比较对象的值 ， 位置不变

正整数	当前对象的值 > 比较对象的值 ， 位置排在后

关于比较器，比如例题中的{3，32，321} 数组中先放入3，而后3和32比较，因为332>323 所以3>32 数组此时为[32,3]; 再往数组中加入321，先与32比较，32132<32321 故 321<32  故321应排在32前面，再与3比较 3213<3321 故321<3 数组最终排序[321，32，3]

----------------------------------------------------------------------------------------------------------------------------------------
把只包含因子2、3和5的数称作丑数（Ugly Number）。例如6、8都是丑数，但14不是，因为它包含因子7。 习惯上我们把1当做是第一个丑数。求按从小到大的顺序的第N个丑数。


	package ex1;
	import java.text.MessageFormat;
	import java.util.*;
	public class choushu {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Scanner sc=new Scanner(System.in);
		System.out.println("请输入一个正整数：");
		int input=sc.nextInt();
		System.out.println(MessageFormat.format("按照 从小到大的第{0}个丑数为：{1}",input,GetUglyNumber(input)));

	}
	
	
	public static int GetUglyNumber(int index)
	{
		
		if(index==0)
		{
			return 0;
		}
		
	ArrayList<Integer> list=new ArrayList<Integer>();
	//add第一个丑数
	list.add(1);
	//三个小标用于记录丑数的位置
	int i2=0,i3=0,i5=0;
	
	while(list.size()<index)
	{
		//三个数都是可能的丑数，取最小的放进丑数数组里面
		int n2=list.get(i2)*2;
		int n3=list.get(i3)*3;
		int n5=list.get(i5)*5;
		int min=Math.min(n2, Math.min(n3, n5));
		list.add(min);
		if(min==n2)
			i2++;
		if(min==n3)
			i3++;
		if(min==n5)
			i5++;
	}
	
	return list.get(list.size()-1);		
	}
    }

结果：

请输入一个正整数：
10
按照 从小到大的第10个丑数为：12

----------------------------------------------------------------------------------------------------------------------------------------

在一个字符串(1<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置

    public class FirstNotRepeatingChar {
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		
		Scanner sc=new Scanner(System.in);
		System.out.println("请输入字符串序列：");
		String str=sc.next();
		System.out.println("第一个出现字符串序列位置为："+FirstChar(str));
	}
	
	public static int FirstChar(String str)
	{
		if(str==null || str.length()<1 || str.length()>10000)
		{
			return 0;
		}
		
		LinkedHashMap<Character,Integer> map=new LinkedHashMap<Character,Integer>();
		for(int i=0;i<str.length();i++)
		{
			char ch=str.charAt(i);
			boolean b=map.containsKey(ch);
			
			if(b)
			{
				int time=map.get(str.charAt(i));
				map.put(str.charAt(i), ++time);
			}else
			{
				map.put(str.charAt(i), 1);
			}
		}
		
		int pos=-1;
		int i=0;
		for(;i<str.length();i++)
		{
			char c=str.charAt(i);
			if(map.get(c)==1)
			{
				return i;
			}
		}
		
		return i;	
	}
      }

结果：

请输入字符串序列：

ababcabababab

第一个出现字符串序列位置为：4

---------------------------------------------------------------------------------------------------------------------------------------

输入两个链表，找出它们的第一个公共结点。

     public ListNode FindFirstCommonNode(ListNode pHead1,ListNode pHead2)
	{
		ListNode current1=pHead1;
		ListNode current2= pHead2;
		
		HashMap<ListNode,Integer> hashmap=new HashMap<ListNode,Integer>();
		while(current1!=null)
		{
			hashmap.put(current1,null);
			current1=current1.next;
		}
		
		while(current2!=null)
		{
			if(hashmap.containsKey(current2))
			{
				return current2;
			}
			current2=current2.next;
		}
		
		return null;
	 }


----------------------------------------------------------------------------------------------------------------------------------------
输出所有和为S的连续正数序列。序列内按照从小至大的顺序，序列间按照开始数字从小到大的顺序：

		package ex1;
		import java.util.*;
		public class FindContinuousSequence {

			public static void main(String[] args) {
				// TODO Auto-generated method stub
				Scanner sc=new Scanner(System.in);
				System.out.println("请输入一个整数：");
				System.out.println("结果序列为："+Findsum(sc.nextInt()));

			}


		 public static ArrayList<ArrayList<Integer>> Findsum(int sum)
		 {
			 ArrayList<ArrayList<Integer>> result =new ArrayList<>();
			 //两个起点，相当于动态窗口的两边
			 int plow=1,phigh=2;

			 while(phigh>plow)
			 {
				 //由于是连续的，差为1的一个序列
				 int cur=(phigh+plow) *(phigh-plow+1)/2;
				 //相等，那么就将窗口范围的所有数添加进结果集
				 if(cur==sum)
				 {
					 ArrayList<Integer> list =new ArrayList<>();
					 for(int i=plow;i<=phigh;i++)
					 {
						 list.add(i);
					 }

					 result.add(list);
					 plow++;
				 }else if(cur<sum)
				 {
					 phigh++;
				 }else {
					 plow++;
				 }
			 }

			 return result;
		 }


		}

结果：

请输入一个整数：

100

请输入一个整数：[[9, 10, 11, 12, 13, 14, 15, 16], [18, 19, 20, 21, 22]]


---------------------------------------------------------------------------------------------------------------------------------------
输入一个递增排序的数组和一个数字S，在数组中查找两个数，是的他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

	package ex1;
	import java.util.*;
	import java.util.Scanner;

	public class FindTwoSum {
		public static void main(String[] args) {
			// TODO Auto-generated method stub
			Scanner sc=new Scanner(System.in);
			System.out.println("请输入一组从小到大的整数，并用英文逗号分开：");
			String inputString=sc.next().toString();
			String stringArray[]=inputString.split(",");
			int num[]=new int[stringArray.length];
			for(int i=0;i<num.length;i++)
			{
				num[i]=Integer.parseInt(stringArray[i]);
			}
			  System.out.println(Findtwosum(num,10));
		}


		public static ArrayList<Integer> Findtwosum(int a[],int sum)
		{
			ArrayList<Integer> list=new ArrayList<Integer>();
			if(a==null || a.length<2)
			{
				return list;
			}
			int i=0,j=a.length-1;
			while(i<j)
			{
				if(a[i]+a[j]==sum)
				{
					list.add(a[i]);
					list.add(a[j]);
					return list;
				}else if(a[i]+a[j]>sum)
				{
					j--;
				}else
				{
					i++;
				}
			}
			return list;
		}
	}

结果：

请输入一组从小到大的整数，并用英文逗号分开：

1,2,3,4,5,6

[4, 6]


----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------



----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------



----------------------------------------------------------------------------------------------------------------------------------------



----------------------------------------------------------------------------------------------------------------------------------------



----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------



----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------
