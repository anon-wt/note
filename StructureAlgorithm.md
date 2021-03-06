# 线性结构和非线性结构

## 线性结构

1.  线性结构是常用的数据结构， 其特点是**数据元素之间存在一对一的线性关系**
2. 线性结构有两种不同的存储结构： **顺序存储结构和链式存储结构**
3. 顺序存储结构的表叫做顺序表， 顺序表里的元素是连续的
4. 链式存储结构的表叫做链表，链表里的元素不一定是连续的，元素节点中存放数据元素和相邻元素的地址信息
5. 常用的线性结构：数组、队列、链表、栈

## 非线性结构

1.常见的非线性结构：二维数组、多维数组、广义表、树结构、图结构



***



# 稀疏数组和队列

##  稀疏数组

1. 当一个数组中， 大部分元素是0时， 或者为同一个值的数组时， 可以使用稀疏数组来保存该数组

2. 稀疏数组的处理方法

   2.1. 记录数组一共有几行几列，一共有多少个不同的值

   2.2. 把具有不同值的行列以及值记录在一个小规模的数组中，从而缩小程序的规模

3. 稀疏数组的转换思路

​       3.1 二维数组转换为稀疏数组的思路

​            3.1.1 遍历原始二维数组， 得到行列数，以及有效值sum

​            3.1.2 创建一个稀疏数组sparse[sum+1, 3]

​            3.1.3 在第一行记录下原始数组的行列以及sum

​            3.1.4 将二维数组的有效值填充到稀疏数组中

​        3.2 稀疏数组转为二维数组的思路

​            3.2.1 先读取稀疏数组的第一行，根据第一行的数据，创建二维数组, 例如chess[11,11]

​             3.2.2 读取稀疏数组的后几行，赋值给二维数组即可

4. 相关代码：

   ```java
    /**
        * 普通数组转稀疏数组
        * @param chessArr
        * @return
        */
       private static int[][] toSparseArr(int[][] chessArr) {
           // 1. 计算sum
           int sum = 0;
           for (int[] row : chessArr) {
               for (int data : row) {
                   if(data != 0) {
                    sum ++;
                   }
               }
           }
           // 2. 创建稀疏数组
           int[][] sparseArr = new int[sum + 1][3];
           int row = chessArr.length;
   
           sparseArr[0][0] = chessArr.length;
           sparseArr[0][1] = chessArr[0].length;
           sparseArr[0][2] = sum;
   
           int count = 1;
           for (int i = 0; i < chessArr.length; i++) {
               for (int j = 0; j < chessArr[i].length; j++) {
                   if (chessArr[i][j] != 0) {
                       sparseArr[count][0] = i;
                       sparseArr[count][1] = j;
                       sparseArr[count][2] = chessArr[i][j];
                       count ++;
                   }
               }
           }
           return sparseArr;
       }
   
       /**
        * 普通数组转稀疏数组
        * @param sparseArr
        * @return
        */
       private static int[][] toChessArr(int[][] sparseArr) {
           // 1. 读取第一行数据创建不同数组
           int[][] chessArr = new int[sparseArr[0][0]][sparseArr[0][1]];
           for (int i = 1; i < sparseArr.length; i++) {
               chessArr[sparseArr[i][0]][sparseArr[i][1]] = sparseArr[i][2];
           }
           return chessArr;
       }
   
   ```

## 队列

1.  基本概念
   1. 队列是有序列表，可以使用**数组**和**链表**来实现
   2. 队列特点：**先进先出**

2. 数组模拟队列思路

      队列是有序列表，如果要使用数组方式存储队列数据，则队列声明如下

   * maxArrSize 数组最大容量
   * front 数组的前端下标，头数据的前一位下标
   * rear 数组的后端下班，尾数据的当前下标
   * front = rear 数据为空 rear = maxArrSize -1 数据满 当插入一个数据时 front + 1, 当获取一个数据时 rear-1

3. 代码实现

   ```java
   
   /**
    * 一次性队列
    */
   class ArrQueue {
       private int maxArrSize; // 数组最大容量
       private int front;      // 指针头 头数据的前一位下标
       private int rear;       // 指针尾 尾数据的下标
       private int[] arr;
   
       public ArrQueue(int maxArrSize) {
           this.maxArrSize = maxArrSize;
           front = -1;
           rear = -1;
           arr = new int[maxArrSize];
       }
   
       public boolean isFull() {
           return rear == maxArrSize - 1;
       }
   
       public boolean isEmpty() {
           return rear == front;
       }
   
       public void addQueue(int n) {
           if(isFull()) {
               System.out.println("队列已满... 无法添加");
               return;
           }
           rear ++;
           arr[rear] = n;
       }
   
       public int getQueue() {
           if(isEmpty()) {
               System.out.println("队列已经空，无法获取数据");
               throw new RuntimeException("队列已经空，无法获取数据");
           }
           front ++;
           return arr[front];
       }
   
       public void showQueue() {
           for (int i = 0; i < arr.length; i++) {
               System.out.printf("%d数据为：%d\n", i, arr[i]);
           }
       }
   
       public void headQueue() {
           if(isEmpty()) {
               System.out.println("队列已经空，无法获取数据");
               throw new RuntimeException("队列已经空，无法获取数据");
           }
           int head = arr[front + 1];
           System.out.printf("头部数据为：%d\n",  head);
       }
   }
   
   ```

   

## 环形队列

1. 重新定义：front指向队列头数据， 默认值为0

2. 重新定义：rear 指向队列尾数据后面一位，默认值为0

3. 重新定义：maxSize 最大数组长度，队列最大容量maxSize -1， 有一个作为标志位

4. 重新定义：是否为满： (rear+1) % maxSize = front 例: rear = 3, front = 0, maxSize = 4

5. 重新定义：判断为空 front == rear

6. 重新定义： 有效容量：(rear + maxSize - front) % maxSize  例：front = 0 rear=3, maxSize = 4 

7. 代码实现

   ```java
   
   /**
    * 环形队列
    */
   class CircleArrQueueQueue {
       private int maxArrSize; // 队列最大容量maxSize -1， 有一个作为标志位
       private int front;      // 指针头 头数据的下标
       private int rear;       // 指针尾 尾数据的后一位下标
       private int[] arr;
   
       public CircleArrQueueQueue(int maxArrSize) {
           this.maxArrSize = maxArrSize;
           arr = new int[maxArrSize];
       }
   
       public boolean isFull() {
           return (rear + 1 ) % maxArrSize == front;
       }
   
       public boolean isEmpty() {
           return rear == front;
       }
   
       public void addQueue(int n) {
           if(isFull()) {
               System.out.println("队列已满... 无法添加");
               return;
           }
           arr[rear] = n;
           rear = (rear + 1) % maxArrSize;
       }
   
       public int getQueue() {
           if(isEmpty()) {
               System.out.println("队列已经空，无法获取数据");
               throw new RuntimeException("队列已经空，无法获取数据");
           }
           int value = arr[front];  // 0
           front = (front + 1) % maxArrSize;
           return value;
       }
   
       public void showQueue() {
           for (int i = front; i < front + size(); i++) {
               int index = i % maxArrSize;
               System.out.printf("arr[%d]数据为：%d\n", index, arr[index]);
           }
       }
   
       /**
        * rear = 0 maxArrSize = 4 front = 0
        * @return
        */
       public int size() {
           return (rear + maxArrSize - front) % maxArrSize;
       }
   
       public void headQueue() {
           if(isEmpty()) {
               System.out.println("队列已经空，无法获取数据");
               throw new RuntimeException("队列已经空，无法获取数据");
           }
           int head = arr[front];
           System.out.printf("头部数据为：%d\n",  head);
       }
   }
   
   ```

***

# 链表

## 单链表

1.链表是以节点的方式存储， 链表是**链式存储**

2.每个节点包含**data域**和**next域**， next 指向下一个节点

3.链表的各个节点**不一定是顺序存储**

4.链表分为**带头节点链表**和**不带头节点链表**

5.代码：

```java

class SingleHeroNode {
    public HeroNode headHeroNode = new HeroNode(-1, null, null);
    public SingleHeroNode() {
    }

    // 1.添加到后面
    public void add(HeroNode heroNode) {
        HeroNode temp =  headHeroNode;

        while (true) {
            if (temp.next == null) {
                break;
            }
            temp = temp.next;
        }
        temp.next = heroNode;
    }

    // 2.按序添加
    public void addByOrder(HeroNode heroNode) {
        HeroNode temp =  headHeroNode;
        while (true) {
            if (temp.next == null) {
                temp.next = heroNode;
                break;
            }
            if (heroNode.no < temp.next.no) {
                heroNode.next = temp.next;
                temp.next = heroNode;
                break;
            } else if (heroNode.no == temp.next.no) {
                System.out.printf("no[%d] 已经存在", heroNode.no);
                break;
            }
            temp = temp.next;
        }
    }

    // 3. 遍历
    public void list() {
        HeroNode temp =  headHeroNode.next;
        if (temp == null) {
            System.out.println("链表为空~~~");
            return;
        }
        while (true) {
            System.out.println("hero: " + temp.toString());
            if (temp.next == null) {
                break;
            }
            temp = temp.next;
        }
    }

    // 4. 修改
    public void update(HeroNode newHeroNode) {
        HeroNode temp =  headHeroNode;
        while (true) {
            if (newHeroNode.no == temp.no) {
                temp.name = newHeroNode.name;
                System.out.printf("no[%d] 已经修改\n", newHeroNode.no);
                break;
            }

            if (temp.next == null) {
                break;
            }
            temp = temp.next;
        }
    }

    // 5. 删除
    public void delete(int no) {
        HeroNode temp =  headHeroNode;
        while (true) {
            if (temp.next == null) {
                break;
            }
            if (temp.next.no == no) {
                temp.next = temp.next.next;
                System.out.printf("no[%d] 已经删除\n", no);
                break;
            }
            temp = temp.next;
        }
    }


}


class HeroNode {
    int no;
    String name;
    HeroNode next;

    public HeroNode(int no, String name) {
        this.no = no;
        this.name = name;
    }


    public HeroNode(int no, String name, HeroNode next) {
        this.no = no;
        this.name = name;
        this.next = next;
    }


    @Override
    public String toString() {
        return "HeroNode{" +
                "no=" + no +
                ", name='" + name + '\'' +
                '}';
    }
}
```

6.面试题

* 求单链表中有效节点的个数， 代码如下：

  ```java
      public int getLength() {
          HeroNode node = headHeroNode;
          int length = 0;
          while (node.next != null) {
              length ++;
              node = node.next;
          }
          return length;
      }
  ```

* 求单链表中倒数第k个节点 ， 代码如下：

  ```java
  public HeroNode getLastIndexNode(int index) {
          // 1. 获得节点个数
          int length = getLength();
  
          if (index <= 0 || index > length) {
              System.out.printf("out of index: %d\n", index);
              throw new RuntimeException("out of index");
          }
  
          HeroNode node = headHeroNode.next;
          for (int i = 0; i < length - index; i ++) {
              node = node.next;
          }
          return node;
      }
  
  ```

* 单链表的反转， 代码如下：

  ```java
      public void reserveNode() {
          if (headHeroNode.next == null || headHeroNode.next.next == null) {
              return;
          }
          HeroNode head = new HeroNode(0, "");
          HeroNode curr = headHeroNode.next;
          HeroNode next;
          while (curr != null) {
              next = curr.next;
              curr.next = head.next;
              head.next = curr;
              curr = next;
          }
          headHeroNode.next = head.next;
      }
  ```

  

* 从尾到头打印单链表， 代码如下：

  ```java
      public void print() {
          // 打印链表
          Stack<HeroNode> heroNodes = new Stack<HeroNode>();
          HeroNode curr = headHeroNode.next;
          while (curr != null) {
              heroNodes.push(curr);
              curr = curr.next;
          }
  
          while (heroNodes.size() > 0) {
              System.out.println(heroNodes.pop());
          }
      }
      // 不推荐， 破坏了原有的链表结构
      public void print2() {
          reserveNode();
          HeroNode curr = headHeroNode.next;
          while (curr != null) {
              System.out.println(curr);
              curr = curr.next;
          }
      }
  ```

  

* 合并两个有序的单链表， 合并后的单链表依然有序， 代码如下：

  ```java
  public void union(SingleHeroNode singleHeroNode) {
      HeroNode curr = singleHeroNode.headHeroNode.next;
          HeroNode next;
          while (curr != null) {
              next = curr.next;
              addByOrder(curr);
              curr = next;
          }
      }
  
  ```

## 双向链表

1. 和单向链表对比：
   * 单向链表的查找方向只能是一个方向， 双向链表可以向前向后两个方向查找
   * 单项链表不能进行自我删除，需要提供一个temp:被删除节点的前一个， 双向链表可以进行自我删除

2. 链表的增删改查

   * **遍历** 和单项链表一样， 只是可以向前也可以向后遍历
   * **添加**（默认添加到最后）
     - 先找到双向链表的最后一个节点
     - temp.next = newHeroNode; newHeroNode.pre = tmep

   * **修改**思路和单项链表一样
   * **删除**
     - 因为是双向链表，所以我们可以实现自我删除莫格节点
     - 直接找到对应节点比如temp
     - temp.pre.next = temp.next
     - temp.next.pre = temp.pre

   * **排序添加** 和单项链表略微相同， 在其之上稍微修改即可

3. 代码如下：

   ```JAVA
   
   class DoubleLinkedNode {
       private HeroNode2 headNode = new HeroNode2(-1, "");
   
       public DoubleLinkedNode() {
       }
   
       public HeroNode2 getHeadNode() {
           return headNode;
       }
   
   
       // 1. 新增
       public void add(HeroNode2 heroNode) {
           HeroNode2 tmp = headNode;
           while (tmp.next != null) {
               tmp = tmp.next;
           }
           tmp.next = heroNode;
           heroNode.pre = tmp;
       }
   
       public void addByOrder(HeroNode2 heroNode) {
           HeroNode2 tmp = headNode;
           while (true) {
               if (tmp.next == null) {
                   heroNode.pre = tmp;
                   tmp.next = heroNode;
                   break;
               }
   
               if (tmp.no == heroNode.no) {
                   System.out.printf("this no[%d] is exist!\n", heroNode.no);
                   break;
               } else if (tmp.next.no > heroNode.no ) {
                   heroNode.pre = tmp;
                   heroNode.next = tmp.next;
                   tmp.next.pre = heroNode;
                   tmp.next = heroNode;
                   break;
               }
   
               tmp = tmp.next;
           }
   
   
   
       }
   
   
       // 2. 删除
       public void delete(int no) {
           if (headNode.next == null) {
               System.out.println("this is empty list");
               return;
           }
   
           HeroNode2 tmp = headNode.next;
           boolean flag = false;
           while (tmp.next != null) {
               if (tmp.no == no) {
                   flag = true;
                   break;
               }
               tmp = tmp.next;
           }
   
           if (flag) {
               tmp.pre.next = tmp.next;
               tmp.next.pre = tmp.pre;
           }
       }
   
       // 3. 修改
       public void update(HeroNode2 heroNode2) {
           HeroNode2 tmp = headNode.next;
           boolean flag = false;
           while (tmp.next != null) {
               if (tmp.no == heroNode2.no) {
                   flag = true;
                   break;
               }
               tmp = tmp.next;
           }
   
           if (flag) {
               tmp.name = heroNode2.name;
           }
       }
   
       // 4. list
       public void list() {
           HeroNode2 tmp = headNode.next;
           while (tmp != null) {
               System.out.println("this node: " + tmp);
               tmp = tmp.next;
           }
   
   
       }
   
   
   
   }
   
   
   class HeroNode2 {
       int no;
       String name;
       HeroNode2 next;
       HeroNode2 pre;
   
       public HeroNode2(int no, String name) {
           this.no = no;
           this.name = name;
       }
   
   
   
       @Override
       public String toString() {
           return "HeroNode{" +
                   "no=" + no +
                   ", name='" + name + '\'' +
                   '}';
       }
   }
   ```

   

## 单向环形链表

1. 单向环形链表创建

   * 先创建第一个节点，让first指向该节点，并形成环形
   * 随后每创建一个节点，都将该节点加入到该环形队列中即可

2.  单项环形链表遍历：

   * 创建一个辅助变量curr 让curr 指向first ,然后 curr = curr.getNext ,直到 curr.getNext = first 结束

3. Josepfu 问题

   设编号为1，2，... n的n个人围成一圈，约定标号为k（1 <= k <= n）的人,从1开始报数，数到m的人出列，他的下一位继续从1开始报数， 数到m出列，以此类推，直到最后一个人出列位置，请输出出列标号顺序

   例如： n =5 , k =1, m =2  其出列顺序为： 2->4->1->5->3

2. Josepfu问题解题思路

   * 创建环形单向链表

   * 创建一个辅助变量helper,指向环形链表的最后一个节点
   * 在让小孩报数前，先让first和helper移动k-1次
   * 当小孩报数时， 让first和helper均移动m-1次
   * 这时就可以让first 指向的节点出队列： first = first.getNext(); helper.setNext() = first;

3. 代码:

   ```java
   
   
   class CircleSingleLink {
       private Boy first;
   
       public CircleSingleLink() {
       }
   
       public void add(int num) throws Exception {
           if (num < 1) {
               throw new Exception("CircleSingleLink num must be bigger 1");
           }
   
           Boy curr = null;
           for (int i = 1; i <= num; i++) {
               Boy boy = new Boy(i);
               if (i == 1) {
                   first = boy;
                   first.setNext(first);
                   curr = first;
               } else {
                   curr.setNext(boy);
                   boy.setNext(first);
                   curr = boy;
               }
           }
       }
   
       public void show() throws Exception {
           Boy curr = first;
           if (curr == null) {
               throw new Exception("CircleSingleLink is empty");
           }
           while (true) {
               System.out.printf("this boy no is [%d] \n", curr.getNo());
               if (curr.getNext() == first) {
                   break;
               }
               curr = curr.getNext();
           }
       }
   
       /**
        * 约瑟夫问题
        * @param start 起始位置
        * @param slip 数几个
        * @param num 总数
        */
       public void countBoy(int start, int slip, int num) {
           if (start < 1 || start > num) {
               System.out.printf("起始位置应在1-%d 之间\n", num);
           }
           // 1. 设置一个first位置， 以及一个before 位置， first 位置数据将被丢弃， before 位置在first 位置之前
           Boy helper = first;
           while (true) {
               if (helper.getNext() == first) {
                   break;
               }
               helper = helper.getNext();
           }
   
           // 2. 选择起始位置
           for (int i = 0; i < start -1 ; i++) {
               first = first.getNext();
               helper = helper.getNext();
           }
   
           // 3. 根据滑动步伐
           while (true) {
               if (helper == first) {
                   break;
               }
               for (int i = 0; i < slip -1 ; i++) {
                   first = first.getNext();
                   helper = helper.getNext();
               }
               System.out.printf("输出的节点no is [%d] \n", first.getNo());
               first = first.getNext();
               helper.setNext(first);
           }
           System.out.printf("输出的最后一个节点no is [%d] \n", first.getNo());
       }
   }
   
   class Boy {
       private int no;
       private Boy next;
   
       public Boy(int no) {
           this.no = no;
       }
   
       public int getNo() {
           return no;
       }
   
       public void setNo(int no) {
           this.no = no;
       }
   
       public Boy getNext() {
           return next;
       }
   
       public void setNext(Boy next) {
           this.next = next;
       }
   }
   
   ```

   

---

# 栈

## 基本原理以及实现

1. 栈是**先入后出**的有序列表

2. 栈是线性表中，元素的**插入和删除都在线性表的同一端**的线性表，允许元素插入和删除的一端叫做栈顶（top）另一端为栈底（bottom）

3. 由栈的定义可知，最先放入栈的元素在栈底，最后放入栈的元素在栈顶，而删除却恰恰相反，最先删除的是最后放入的，最后删除的是第一个放入的

4. 栈的应用场景：
   * 子程序的调用，在跳往子程序前，会将下一个命令的地址存放在栈中，直到子程序执行完毕，再将地址取出以回到程序中
   * 处理递归调用，和子程序调用类似，只是在存放下一个命令地址外也会将参数，区域变量等数据存放在栈中
   * 表达式的转换与求值
   * 二叉树的遍历
   * 图形的深度优先（depth-first）搜索法

5. 定义栈思路
   * 用数组定义栈
   * 定义一个变量为top 初始化为-1 push top ++; arr[top] = data; pop top --;

6. 代码

   ```java
   
   class ArrStack {
       private int maxSize;
       private int[] arr;
       private int top = -1;
   
       public ArrStack(int maxSize) {
           this.maxSize = maxSize;
           arr = new int[maxSize];
       }
   
       public boolean isEmpty() {
           return top == -1;
       }
   
       public boolean isFull() {
           return top == maxSize -1;
       }
   
       public void push(int data) {
           if(isFull()) {
               System.out.println("this stack is full ... ");
               return;
           }
   
           top ++;
           arr[top] = data;
       }
   
       public void pop() throws Exception {
           if(isEmpty()) {
               throw new Exception("this stack is empty ... ");
           }
           top --;
       }
   
       public void show() {
           if(isEmpty()) {
               System.out.println("this stack is empty...");
               return;
           }
           for(int i = top; i >= 0; i -- ) {
               System.out.printf("第%d位数为%d \n", i, arr[i]);
           }
       }
   }
   ```

   ```java
   
   class LinkedStack {
       private Node head;
   
       public LinkedStack() {
           head = new Node(-1);
       }
   
       public boolean isEmpty() {
          return head.getNext() == null;
       }
   
       public void push (int no) {
           Node node = new Node(no);
           node.setNext(head.getNext());
           head.setNext(node);
       }
   
       public void pop() throws Exception {
           if (isEmpty()) {
               throw new Exception("this stack is empty");
           }
           Node tmp = head;
           head.setNext(head.getNext().getNext());
       }
   
   
       public void show() {
           Node temp = head;
           while (true) {
               if (temp != head) {
                   System.out.printf("this node is %d \n", temp.getNo());
               }
               if (temp.getNext() == null) {
                   break;
               }
               temp = temp.getNext();
           }
       }
       
   
   }
   
   
   
   class Node {
       private int no;
       private Node next;
   
       public Node(int no) {
           this.no = no;
       }
   
       public int getNo() {
           return no;
       }
   
       public void setNo(int no) {
           this.no = no;
       }
   
       public Node getNext() {
           return next;
       }
   
       public void setNext(Node next) {
           this.next = next;
       }
   }
   
   ```

## 利用栈进行表达式计算

1. 实现步骤：

   1. 通过一个索引index来遍历表达式
   2. 如果我们发现是一个数字，就直接放入数字栈，如果我们发现是一个符号则分以下情况
      - 如果当前的符号栈为空，直接入栈
      - 如果当前的符号栈不为空，则与符号栈里的符号进行比较
        * 如果当前符号栈优先级小于等于栈内的符号，则从数栈pop出两个数字，从符号栈poo出一个符号，进行运算， 将运算的结果push数栈，并将当前的符号入符号栈
        * 如果当前的符号优先级大于栈内的符号

   3. 当扫描完毕后，再从数栈和符号栈pop出数字和符号进行运算
   4. 最后数栈中只有一个结果就是表达式的结果

2. 代码：

   ```java
   public class ExpressionDemo {
       public static void main(String[] args) throws Exception {
           String expression = "340+2*6-20";
           ArrayStack numberStack = new ArrayStack(10);
           ArrayStack operatorStack = new ArrayStack(10);
   
           int index = 0;
           int num1;
           int num2;
           char operator;
           int result;
           char ch;
           String keepNum = "";
   
           int length = expression.length();
           while (true) {
               if (index == length) {
                   break;
               }
               ch = expression.charAt(index);
               if (isOperator(ch)) {
                   if(operatorStack.isEmpty()) {
                       operatorStack.push(ch);
                   } else {
                       if (priority(ch) <= priority(operatorStack.peep())) {
                           num1 = numberStack.pop();
                           num2 = numberStack.pop();
                           operator = (char) operatorStack.pop();
                           result = (char)calculation(num1, num2, operator);
   
                           numberStack.push(result);
                           operatorStack.push(ch);
                       } else {
                           operatorStack.push(ch);
                       }
                   }
                   index ++;
               } else {
                   while (true) {
                       if (index == length) {
                           break;
                       }
                       ch = expression.charAt(index);
                       if (isOperator(ch)) {
                           break;
                       }
                       keepNum += ch;
                       index ++;
                   }
                   numberStack.push(Integer.parseInt(keepNum));
                   keepNum = "";
               }
           }
   
           while(true) {
               if (operatorStack.isEmpty()) {
                   break;
               }
               num1 = numberStack.pop();
               num2 = numberStack.pop();
               operator = (char) operatorStack.pop();
               result = calculation(num1, num2, operator);
               numberStack.push(result);
           }
   
           System.out.printf("表达式 %s 的结果为：%d", expression, numberStack.pop());
       }
   
   
       // 判断是否是符号
       public static boolean isOperator(int data) {
           return data == '+' || data == '-' || data == '*' || data == '/';
       }
   
   
       public static int priority(int data) throws Exception {
           int flag = 0;
           switch (data) {
               case '-' :
               case '+':
                   flag = 1;
                   break;
               case '*' :
               case '/':
                   flag = 2;
                   break;
               default:
                   throw new Exception("not support this operator");
           }
           return flag;
       }
   
       public static int calculation(int num1, int num2, char operator) throws Exception {
           int result = 0;
           switch (operator) {
               case '-' :
                   result = num2 - num1;
                   break;
               case '+':
                   result = num2 + num1;
                   break;
               case '*' :
                   result = num2 * num1;
                   break;
               case '/':
                   result = num2 / num1;
                   break;
               default:
                   throw new Exception("not support this operator");
           }
           return result;
       }
   
   
   }
   
   
   class ArrayStack {
       private int maxSize;
       private int[] arr;
       private int top;
   
       public ArrayStack(int maxSize) {
           this.maxSize = maxSize;
           arr = new int[maxSize];
           top = -1;
       }
   
       public boolean isEmpty() {
           return top == -1;
       }
   
       public boolean isFull() {
           return top == maxSize-1;
       }
   
       public void push(int data) {
           if (isFull()) {
               System.out.println("数组已经满了");
           } else {
               top ++;
               arr[top] = data;
           }
       }
   
       public int pop() throws Exception {
           if (isEmpty()) {
               throw new Exception("数组已经空...");
           } else {
               int data = arr[top];
               top --;
               return data;
           }
       }
   
       public int peep() {
           return arr[top];
       }
   }
   ```

   

  ## 前缀、中缀、后缀表达式

1. 前缀表达式

   * 前缀表达式： 又名波兰表达式， 前缀表达式的运算符位于操作数之前，
   * 前缀表达式的求值：从右至左扫描表达式，当遇到数字时，压入数字栈，遇到运算符时，弹出栈顶的两个数，用运算符对他们进行计算，重复以上过程， 直至表达式最左侧，最后运算符得到的值则为表达式结果

   * 例如：(3+4)*5-6的前缀表达式为：- * + 3 4 5 6 求值过程如下：
     * 从右至左扫描，将6 5 4 3 压入栈
     * 遇到+ 符号， 将栈顶两个元素弹出 3 ，4   然后进行计算 3+4 = 7  ,将7压入栈中
     * 遇到* 符号， 将栈顶两个元素弹出7，5   然后进行计算 7* 5 = 35， 将35压入栈中
     * 遇到- 符号， 将栈顶两个元素弹出 35 ，6 然后进行计算35 -6 = 29 即最终结果

2. 中缀表达式

   * 中缀表达式，常见的表达式： 如(3+4)*5-6
   * 中缀表达式是我们人所熟悉的表达式，但是对于计算机来说不好操作。因此在计算结果时，往往将表达式转化为其他表达式（一般转化为后缀表达式）

3. 后缀表达式，

   * 又称为逆波兰表达式，与前缀表达式相似，只是运算符在操作数之后。
   * 后缀表达式求值，从左向右扫描， 遇到数值时，将其压入栈中，遇到运算符时，则弹出栈中两个元素，用运算符对其进行计算，并将结果入栈， 重复以上过程，直到表达式最右端。最后运算出的值即为表达式的结果
   * 例如：(3+4)*5-6的前缀表达式为：3 4 + 5 * 6 - 求值过程如下：
        * 从左至右扫描，将3，4压入堆栈
        * 遇到+符号，弹出栈顶两个元素4 3  对其进行计算 3+4 = 7 ,将7 压入栈中
        * 将5入栈
        * 遇到* 符号， 弹出栈顶两个元素5 7 对其进行计算 7 * 5 = 35，将35 压入栈中
        * 将6入栈
        * 遇到-符号，弹出栈顶两个元素 6 35 对其进行计算  35 - 5 = 29 即为最终结果

4. 中缀表达式转为后缀表达式

   1. 初始化两个栈，运算符栈s1 和存贮中间结果栈 s2

   2. 从左至右扫描中缀表达式

   3. 遇到操作数值时，将其压入s2

   4. 遇到运算符时，比较其与栈内的运算符的比较优先级

      4.1 如果s1为空，或者s1栈顶运算符是“(”,则直接将该运算符直接插入到s1中

      4.2 否则，若优先级比栈顶元素优先级高，则直接将该运算符插入到s1中

      4.3 否则将s1中的元素弹出直接压入s2中，重复以上4比较步骤，与s1中的新栈顶元素比较

   5. 遇到括号时

      5.1 遇到“(”括号时，直接将括号压入s1中

      5.2 遇到“)”括号时，依次弹出s1中的运算符，压入s2，直到遇到"(",并将这一对括号舍弃

   6. 重复以上2-5步骤，直到表达式最右侧
   7. 将s1中的剩余符号依次压入s2中
   8. 依次弹出s2中的元素并输出， 结果的逆序则为中缀表达式的后缀表达式

  



