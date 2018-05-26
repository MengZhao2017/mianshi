
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



----------------------------------------------------------------------------------------------------------------------------------------


----------------------------------------------------------------------------------------------------------------------------------------
