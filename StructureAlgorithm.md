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

   







​       



