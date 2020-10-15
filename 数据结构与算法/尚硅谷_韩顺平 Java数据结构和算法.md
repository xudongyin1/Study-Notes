##  数据结构和算法的关系 

1) 数据 data 结构(structure)是一门**研究组织数据方式**的学科，有了编程语言也就有了数据结构.学好数据结构可以编写出更加漂亮,更加有效率的代码。 

2) 要学习好数据结构就要多多考虑如何将生活中遇到的问题,用程序去实现解决. 

**3)** **程序** **=** **数据结构** **+** **算法** 

4) **数据结构是算法的基础**, 换言之，想要学好算法，需要把数据结构学到位。 



数据结构包括：**线性结构**和**非线性结构**。 

> **线性结构** 

1) 线性结构作为最常用的数据结构，其特点是**数据元素之间存在一对一**的线性关系 

2) 线性结构有两种不同的存储结构，即**顺序存储结构数组)和链式存储结构(链表)**。顺序存储的线性表称为顺序 

表，顺序表中的**存储元素是连续**的 

3) 链式存储的线性表称为链表，链表中的**存储元素不一定是连续的**，元素节点中存放数据元素以及相邻元素的地 址信息 

4) 线性结构常见的有：**数组、队列、链表和栈**，后面我们会详细讲解

> **非线性结构** 

非线性结构包括：二维数组，多维数组，广义表，**树结构，图结构**



## 稀疏 sparsearray 数组

> 基本介绍 

当一个数组中大部分元素为０，或者为同一个值的数组时，可以使用稀疏数组来保存该数组。 

稀疏数组的处理方法是: 

1) 记录数组**一共有几行几列，有多少个不同**的值 

2) 把具有不同值的元素的行列及值记录在一个小规模的数组中，从而**缩小程序**的规模

![image-20200729100344782](https://gitee.com/xudongyin/img/raw/master/img/20200729100346.png)

**第一行数组表示整个二维数组有6行7列，共有8个数据**

> 应用实例 

1) 使用稀疏数组，来保留类似前面的二维数组(棋盘、地图等等) 

2) 把稀疏数组存盘，并且可以从新恢复原来的二维数组数 

3) 整体思路分析

![image-20200729101454852](https://gitee.com/xudongyin/img/raw/master/img/20200729101456.png)

```java
package sparseArray;

public class SparseArray {
    public static void main(String[] args) {
        //创建一个二维数组
        // 0: 表示没有棋子， 1 表示 黑子 2 表蓝子
        int[][] ChessArray1 = new int[11][11];
        ChessArray1[2][1] = 1;
        ChessArray1[3][2] = 2;

        //输出原数组
        System.out.println("原始的二维数组~~~~");
        for (int[] row : ChessArray1) {
            for (int data : row) {
                System.out.printf("%d\t",data);
            }
            System.out.println();
        }

        //将二维数组 转为 稀疏数组
        //先遍历二维数组
        int sum =0; //统计原数组里面的数据个数
        for (int i = 0; i < 11; i++) {
            for (int j = 0; j < 11; j++) {
                if (ChessArray1[i][j] != 0) {
                    sum++;
                }
            }
        }
        //创建稀疏数组
        int[][] sparseArray = new int[sum + 1][3];
        //给稀疏数组赋值,第一行存放原二维数组的数据
        sparseArray[0][0] = 11;
        sparseArray[0][1] = 11;
        sparseArray[0][2] = sum;
        //遍历二维数组，将非0 的数赋值给稀疏数组
        int count = 0; //用于记录第几个非0 数据
        for (int i = 0; i < 11; i++) {
            for (int j = 0; j < 11; j++) {
                if (ChessArray1[i][j] != 0) {
                    count++;
                    sparseArray[count][0] = i; //存放行索引
                    sparseArray[count][1] = j;  //存放列索引
                    sparseArray[count][2] = ChessArray1[i][j]; //存放数值
                }
            }
        }

        //输出稀疏数组
        System.out.println("稀疏数组为~~~~");
        for (int[] row : sparseArray) {
            for (int data : row) {
                System.out.printf("%d\t",data);
            }
            System.out.println();
        }

        //将稀疏数组 转为 原二维数组
        //1. 先读取稀疏数组的第一行，根据第一行的数据，创建原始的二维数组
        int[][] ChessArray2 = new int[sparseArray[0][0]][sparseArray[0][1]];
        //2. 在读取稀疏数组后几行的数据(从第二行开始)，并赋给 原始的二维数组 即可
        for (int i = 1; i < sparseArray.length; i++) {
            ChessArray2[sparseArray[i][0]][sparseArray[i][1]] = sparseArray[i][2];
        }

        //输出转换后的原二维数组
        System.out.println("转换后的原二维数组为~~~~");
        for (int[] row : ChessArray2) {
            for (int data : row) {
                System.out.printf("%d\t",data);
            }
            System.out.println();
        }
    }
}
```



## 队列

1) 队列是一个**有序列表**，可以用**数组**或是**链表**来实现。 

2) 遵循**先入先出**的原则。即：**先存入队列的数据，要先取出。后存入的要后取出 **

3) 示意图：(使用数组模拟队列示意图)

![image-20200729162844075](https://gitee.com/xudongyin/img/raw/master/img/20200729162847.png)

### 数组模拟队列

 队列本身是有序列表，若使用数组的结构来存储队列的数据，则队列数组的声明如下图, 其中 maxSize 是该队列的最大容量。 

 因为队列的输出、输入是分别从前后端来处理，因此需要两个变量 front 及 rear 分别记录队列前后端的下标， front 会随着数据输出而改变，而 rear 则是随着数据输入而改变，如图所示: 

![image-20200729162919969](https://gitee.com/xudongyin/img/raw/master/img/20200729162921.png)

 当我们将数据存入队列时称为”addQueue”，addQueue 的处理需要有两个步骤：思路分析 

1) 将尾指针往后移：rear+1 , 当 front == rear 【空】 
2) 若尾指针 rear 小于队列的最大下标 maxSize-1，则将数据存入 rear 所指的数组元素中，否则无法存入数据。 rear == maxSize - 1[队列满] 

```java
package Array.ArrayQueue;

import java.util.Scanner;

public class arrayQueuedemo {
    public static void main(String[] args) {
        ArrayQueue queue = new ArrayQueue(3);
        boolean loop = true;
        char key;//接收用户输入
        Scanner scanner = new Scanner(System.in);
        //输出一个菜单
        while (loop) {
            System.out.println("s(show): 显示队列");
            System.out.println("e(exit): 退出程序");
            System.out.println("a(add): 添加数据到队列");
            System.out.println("g(get): 从队列取出数据");
            System.out.println("h(head): 查看队列头的数据");

            key = scanner.next().charAt(0);
            switch (key) {
                case 's':
                    queue.showQueue();
                    break;
                case 'e':
                    scanner.close();
                    loop = false;
                    break;
                case 'a':
                    try {
                        queue.addQueue(scanner.nextInt());
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'g'://取出数据
                    try {
                        int head = queue.getQueue();
                        System.out.println("出列的数据为" + head);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'h'://查看队列头的数据
                    try {
                        int head = queue.headQueue();
                        System.out.printf("队列的头部数据为%d\n", head);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
            }
        }
        System.out.println("程序退出");
    }
}

// 使用数组模拟队列-编写一个 ArrayQueue 类
class ArrayQueue {
    private int maxSize; //队列的最大容量
    private int head;//队列头
    private int tail;//队列尾
    private int[] arr;//存放队列数据

    ArrayQueue(int maxSize) {
        this.maxSize = maxSize;
        arr = new int[maxSize];
        head = -1;//指向队列头部的前一个位置
        tail = -1;//指向队列最后一个数据
    }

    //判断队列是否满
    public boolean isFull() {
        if (tail == maxSize - 1) {
            return true;
        }
        return false;
    }

    //判断队列是否为空
    public boolean isEmpty() {
        return tail == head;
    }

    //输出队列的所有数据
    public void showQueue() {
        if (isEmpty()) {
            System.out.println("队列为空");
            return;
        }
        for (int i = head + 1; i < arr.length; i++) {
            //因为头部指针指向头部前一个位置所以要加一
            System.out.printf("arr[%d]=%d\n", i, arr[i]);
        }
    }

    //添加数据,进队列
    public void addQueue(int data) {
        if (isFull()) {
            throw new RuntimeException("队列已经满了");
        }
        tail++;  //尾部指针向后移
        arr[tail] = data;
    }

    //出队列，获取头部数据
    public int getQueue() {
        if (isEmpty()) {
            throw new RuntimeException("队列为空，不能获取头部");
        }
        head++; //头部指针向后移
        return arr[head];
    }

    // 显示队列的头数据， 注意不是取出数据
    public int headQueue() {
        if (isEmpty()) {
            throw new RuntimeException("队列空的，没有数据~~");
        }
        return arr[head + 1];
    }
}
```

> **问题分析并优化** 

1) 目前数组使用一次就不能用， 没有达到复用的效果 

2) 将这个数组使用算法，改进成一个**环形的队列** 取模：% 

### 数组模拟环形队列 

对前面的数组模拟队列的优化，充分利用数组. 因此将数组看做是一个环形的。(通过**取模的方式来实现**即可) 

 分析说明： 

1) 尾索引的下一个为头索引时表示队列满，即将队列容量空出一个作为约定,这个在做判断队列满的 

时候需要注意 (rear + 1) % maxSize == front 满] 

2) rear == front [空] 

3) 分析示意图![image-20200729181348072](https://gitee.com/xudongyin/img/raw/master/img/20200729181350.png)

```JAVA
package Array.ArrayQueue;

import java.util.Scanner;

public class CircleArrayQueueDemo {
    public static void main(String[] args) {
        CircleArrayQueue queue = new CircleArrayQueue(4);//实际容量只有3，因为有一个空间作为约定
        boolean loop = true;
        char key;//接收用户输入
        Scanner scanner = new Scanner(System.in);
        //输出一个菜单
        while (loop) {
            System.out.println("s(show): 显示队列");
            System.out.println("e(exit): 退出程序");
            System.out.println("a(add): 添加数据到队列");
            System.out.println("g(get): 从队列取出数据");
            System.out.println("h(head): 查看队列头的数据");

            key = scanner.next().charAt(0);
            switch (key) {
                case 's':
                    queue.showQueue();
                    break;
                case 'e':
                    scanner.close();
                    loop = false;
                    break;
                case 'a':
                    try {
                        queue.addQueue(scanner.nextInt());
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'g':
                    try {
                        int head = queue.getQueue();
                        System.out.println("出列的数据为" + head);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
                    break;
                case 'h':
                    try {
                        int head = queue.headQueue();
                        System.out.printf("队列的头部数据为%d\n", head);
                    } catch (Exception e) {
                        System.out.println(e.getMessage());
                    }
            }
        }
        System.out.println("程序退出");
    }

}

class CircleArrayQueue {
    private int maxSize; //队列的最大容量
    private int head;//队列头
    private int tail;//队列尾
    private int[] arr;//存放队列数据

    CircleArrayQueue(int maxSize) {
        this.maxSize = maxSize; //环形数组队列的实际容量是参数-1，因为要留一个空间做约定
        arr = new int[maxSize];
        //front 变量的含义做一个调整： front 就指向队列的第一个元素, 也就是说 arr[front] 就是队列的第一个元素
        head = 0;
        //rear 变量的含义做一个调整：rear 指向队列的最后一个元素的后一个位置. 因为希望空出一个空间做为约定
        tail = 0;
    }
    //判断队列是否满
    public boolean isFull() {
        return (tail + 1) % maxSize == head;
    }

    //判断队列是否为空
    public boolean isEmpty() {
        return tail == head;
    }

    //输出队列的所有数据
    public void showQueue() {
        if (isEmpty()) {
            System.out.println("队列为空");
            return;
        }
        for (int i = head ; i < head+getsize(); i++) {
            System.out.printf("arr[%d]=%d\n", i%maxSize, arr[i%maxSize]);
        }
    }

    //添加数据,进队列
    public void addQueue(int data) {
        if (isFull()) {
            throw new RuntimeException("队列已经满了");
        }
        arr[tail] = data;
        tail=(tail+1)%maxSize;  //尾部指针向后移
    }

    //出队列，获取头部数据
    public int getQueue() {
        if (isEmpty()) {
            throw new RuntimeException("队列为空，不能获取头部");
        }
        int val = arr[head];
        head=(head+1)%maxSize; //头部指针向后移
        return val;
    }

    // 显示队列的头数据， 注意不是取出数据
    public int headQueue() {
        if (isEmpty()) {
            throw new RuntimeException("队列空的，没有数据~~");
        }
        return arr[head];
    }
    //获得队列的长度
    public int getsize() {
        return (tail + maxSize - head) % maxSize;
    }
}
```



## 链表

链表是有序的列表，但是它在内存中是存储如下 

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200730110213.png" alt="image-20200730110202445" style="zoom: 50%;" />

小结上图: 

1) 链表是以节点的方式来存储,**是链式存储** 

2) 每个节点包含 data 域， next 域：指向下一个节点. 

3) 如图：发现**链表的各个节点不一定是连续存储**. 

4) 链表分**带头节点的链表**和**没有头节点的链表**，根据实际的需求来确定 

### 单链表

 单链表(带头结点) 逻辑结构示意图如下

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200730110250.png" alt="image-20200730110247556" style="zoom: 50%;" />

> **单链表的应用实例** 

使用带 head 头的单向链表实现 –水浒英雄排行榜管理完成对英雄人物的增删改查操作， 注: 删除和修改,查找 

可以考虑学员独立完成，也可带学员完成 

​		1) 第一种方法在添加英雄时，直接添加到链表的尾部 

​		思路分析示意图: 

![image-20200730110450533](https://gitee.com/xudongyin/img/raw/master/img/20200730110453.png)

​		2) 第二种方式在添加英雄时，**根据排名**将英雄插入到指定位置(如果有这个排名，则添加失败，并给出提示) 

​		思路的分析示意图:

![image-20200730110524682](https://gitee.com/xudongyin/img/raw/master/img/20200730110525.png)

​		3) 修改节点功能 

​		思路	(1) 先找到该节点，通过遍历，

​					(2) temp.name = newHeroNode.name ; temp.nickname= newHeroNode.nickname 

​		4) 删除节点 

​		思路分析的示意图:

![image-20200730110601278](https://gitee.com/xudongyin/img/raw/master/img/20200730110602.png)

```java
package LinkedList;

public class SingleLinkedListDemo {
    public static void main(String[] args) {
        SingleLinkedList singleLinkedList = new SingleLinkedList();
//        //添加节点
//        singleLinkedList.addHero(new HeroNode(1, "宋江", "及时雨"));
//        singleLinkedList.addHero(new HeroNode(2, "吴用", "智多星"));
//        singleLinkedList.addHero(new HeroNode(3, "林冲", "豹子头"));

        //按顺序排序
        singleLinkedList.addHeroByOrder(new HeroNode(2, "吴用", "智多星"));
        singleLinkedList.addHeroByOrder(new HeroNode(1, "宋江", "及时雨"));
        singleLinkedList.addHeroByOrder(new HeroNode(3, "林冲", "豹子头"));
        singleLinkedList.addHeroByOrder(new HeroNode(1, "宋江", "及时雨"));
        //遍历输出
        System.out.println("修改前");
        singleLinkedList.list();
        //修改节点信息
        singleLinkedList.update(new HeroNode(1, "宋江1号", "及时雨来了"));
        //遍历输出
        System.out.println("修改后");
        singleLinkedList.list();
        //删除3号节点
        singleLinkedList.delete(3);
        singleLinkedList.delete(2);
        System.out.println("删除后");
        singleLinkedList.list();
    }
}

//单向链表用来管理英雄
class SingleLinkedList {
    //先初始化一个头节点, 头节点不要动, 不存放具体的数据
     private HeroNode headNode = new HeroNode(0, "", "");

    //添加节点到单向链表
    // 思路，当不考虑编号顺序时
    // 1. 找到当前链表的最后节点
    // 2. 将最后这个节点的 next 指向 新的节点
    public void addHero(HeroNode heroNode) {
        //temp辅助指针
        HeroNode temp = headNode; //头结点，不动，没有数据
        while (temp.next != null) {
            temp = temp.next; //直到最后一个节点
        }
        temp.next = heroNode;

    }
    //按编号排序添加
    public void addHeroByOrder(HeroNode heroNode) {
        //因为头节点不能动，因此我们仍然通过一个辅助指针(变量)来帮助找到添加的位置
        // 因为单链表，因为我们找的 temp 是位于 添加位置的前一个节点，否则插入不了
        HeroNode temp = headNode;
        boolean flag = false;  //标志添加的节点编号是否已经存在
        while (true) {
            if (temp.next == null) {//说明 temp 已经在链表的最后
                break;
            }
            if (temp.next.No > heroNode.No) {//位置找到，就在 temp 的后面插入
                break;
            }
            if (temp.next.No == heroNode.No) {//说明希望添加的 heroNode 的编号已然存在
                flag = true;
                break;
            }
            temp = temp.next; //后移，遍历当前链表
        }
        if (flag) {//为真则不能添加，说明编号存在
            System.out.println("要添加的编号" + heroNode.No + "已经存在");
            return;
        } else {
            //插入到链表中, temp 的后面
            heroNode.next = temp.next;
            temp.next = heroNode;
        }
    }

    //修改节点的信息, 根据 no 编号来修改，即 no 编号不能改.
    // 说明
    // 1. 根据 newHeroNode 的 no 来修改即可
    public void update(HeroNode heroNode) {
        if (headNode.next == null) {
            System.out.println("链表为空");
            return;
        }
        //找到需要修改的节点, 根据 no 编号
        // 定义一个辅助变量，指向第一个节点，不是头结点
        HeroNode temp = headNode.next;
        while (true) {
            if (temp == null) {//已经遍历完链表
                System.out.println("没有相对应的节点");
                break;
            }
            if (temp.No == heroNode.No) {
                temp.Name = heroNode.Name;
                temp.nickName = heroNode.nickName;
                System.out.println("修改节点成功");
                break;
            }
            temp = temp.next;//temp 后移，遍历
        }
    }

    //删除节点
    // 1. head 不能动，因此我们需要一个 temp 辅助节点找到待删除节点的前一个节点
    // 2. 说明我们在比较时，是 temp.next.no 和 需要删除的节点的 no 比较
    public void delete(int no) {
        if (headNode.next == null) {
            System.out.println("链表为空");
            return;
        }
        HeroNode temp = headNode;
        while (true) {
            if (temp.next == null) {//已经到链表的最后
                System.out.println("没有找到相对应的节点");
                break;
            }
            if (temp.next.No == no) {//找到的待删除节点的前一个节点 temp
                temp.next = temp.next.next;
                System.out.println("节点删除成功");
                break;
            }
            temp = temp.next;//temp 后移，遍历
        }

    }
    //显示链表[遍历]
    public void list() {
        HeroNode temp = headNode;//头结点，不动，没有数据
        while (temp.next != null) {
            temp = temp.next; //头结点没有数据，所以要先指向下一个
            System.out.println(temp);
        }
        return;
    }
}
//定义HeroNode节点，每个HeroNode节点就是一个节点
class HeroNode {
    public int No; //英雄排名
    public String Name; //英雄名字
    public String nickName; //别名
    public HeroNode next;  //指向下一个节点

    HeroNode(int no, String name, String nickName) {
        this.No = no;
        this.Name=name;
        this.nickName = nickName;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "No=" + No +
                ", Name='" + Name + '\'' +
                ", nickName='" + nickName + '\'' +
                '}';
    }
}
```

#### 单链表面试题(新浪、百度、腾讯) 

单链表的常见面试题有如下: 

1) 求单链表中有效节点的个数 

```java
/**
 *获取到单链表的节点的个数(如果是带头结点的链表，需求不统计头节点)
 * @param headNode 链表头结点
 * @return 返回链表节点个数
 */
public static int getLength(HeroNode headNode) {
    if (headNode.next == null) { //链表为空
        return 0;
    }
    HeroNode temp = headNode.next; //辅助指针，指向第一个节点
    int length = 0; //记录链表节点个数
    while (temp != null) {
       length++;
        temp = temp.next; //向后遍历
    }
    return length;
}
```

2) 查找单链表中的倒数第 k 个结点 【新浪面试题】

```java
//查找单链表中的倒数第 k 个结点 【新浪面试题】
// 思路
// 1. 编写一个方法，接收 head 节点，同时接收一个 index
// 2. index 表示是倒数第 index 个节点
// 3. 先把链表从头到尾遍历，得到链表的总的长度 getLength
// 4. 得到 size 后，我们从链表的第一个开始遍历 (size-index)个，就可以得到
// 5. 如果找到了，则返回该节点，否则返回 null
public static HeroNode findNode(HeroNode headNode, int index) {
    if (headNode.next == null) {
        System.out.println("链表为空");
        return null;
    }
    int size = getLength(headNode);//第一次遍历,获取链表长度
    //第二次遍历 size-index 位置，就是我们倒数的第 K 个节点
    // 先做一个 index 的校验
    if (index <= 0 || index > size) {
        return null;
    }
    //定义给辅助变量， for 循环定位到倒数的 index
    HeroNode temp  = headNode.next;
    for (int i = 0; i < size - index; i++) {//3 // 3 - 1 = 2
        temp = temp.next;
    }
    return temp;
}
```

3) 单链表的反转【腾讯面试题，有点难度】 

 思路分析图解

![image-20200730153232224](https://gitee.com/xudongyin/img/raw/master/img/20200730153235.png)

```java
//将单链表反转
public static void reverseList(HeroNode headNode) {
    //如果链表为空或者只有一个节点
    if (headNode.next == null || headNode.next.next == null) {
        return;
    }
    HeroNode cur = headNode.next;//指向第一个节点
    HeroNode next = null; //记录位置，把节点转移到反转链表后，可以找到下一个节点
    HeroNode reverseNode = new HeroNode(0, "", "");//反转链表的头结点
    while (cur != null) {
        next = cur.next; //指向下一个节点
        cur.next = reverseNode.next; //把要转移的节点的下一个节点指针指向反转链表的第一个节点
        reverseNode.next = cur; //把节点连接到反转链表的第一个节点位置
        cur = next;//轮到下一个节点
    }
    //全部节点移动到反转链表后，把原链表的头结点替换反转链表的头结点
    headNode.next = reverseNode.next;
}
```

4) 从尾到头打印单链表 【百度，要求方式 1：反向遍历 。 方式 2：Stack 栈】 

 思路分析图解 

![image-20200730161634465](https://gitee.com/xudongyin/img/raw/master/img/20200730161640.png)

```java
//方式 2：
// 可以利用栈这个数据结构，将各个节点压入到栈中，然后利用栈的先进后出的特点，就实现了逆序打印的效果
public static void reversePrint(HeroNode headNode) {
    if (headNode.next == null) {
        return;//链表为空
    }
    HeroNode temp = headNode.next;//指向第一个节点
    Stack<HeroNode> stack = new Stack<>();//创建一个栈
    //将链表的所有节点压入栈
    while (temp != null) {
        stack.push(temp); //入栈
        temp = temp.next;//指针后移
    }
    //将栈中的节点进行打印
    while (stack.size() > 0) {
        System.out.println(stack.pop());//逐个出栈
    }
}
```



### 双向链表

使用带 head 头的双向链表实现 –水浒英雄排行榜 

 管理单向链表的缺点分析: 

1) 单向链表，查找的方向只能是一个方向，而双向链表可以向前或者向后查找。 

2) 单向链表不能自我删除，需要靠辅助节点 ，而双向链表，则可以自我删除，所以前面我们单链表删除时节点，总是找到 temp,temp 是待删除节点的前一个节点(认真体会). 

3) 分析了双向链表如何完成遍历，添加，修改和删除的思路

![image-20200730162855564](https://gitee.com/xudongyin/img/raw/master/img/20200730162858.png)

**对上图的说明**: 

分析 双向链表的遍历，添加，修改，删除的操作思路===》代码实现 

1) **遍历** 和 单链表一样，只是可以向前，也可以向后查找 

2) **添加** (默认添加到双向链表的最后) 

​		(1) 先找到双向链表的最后这个节点 

​		(2) temp.next = newHeroNode 

​		(3) newHeroNode.pre = temp; 

3) **修改** 思路和 原来的单向链表一样. 

4) **删除** 

​		(1) 因为是双向链表，因此，我们可以实现自我删除某个节点 

​		(2) 直接找到要删除的这个节点，比如 temp 

​		(3) temp.pre.next = temp.next 

​		(4) temp.next.pre = temp.pre; 

~~~java
package LinkedList;

public class DoubleLinkedListDemo {
    public static void main(String[] args) {
        DoubleLinkedList doubleLinkedList = new DoubleLinkedList();
        //添加节点
        doubleLinkedList.addHeroByOrder(new HeroNode2(2, "吴用", "智多星"));
        doubleLinkedList.addHeroByOrder(new HeroNode2(1, "宋江", "及时雨"));
        doubleLinkedList.addHeroByOrder(new HeroNode2(3, "林冲", "豹子头"));
        doubleLinkedList.addHeroByOrder(new HeroNode2(1, "宋江", "及时雨"));
        doubleLinkedList.addHeroByOrder(new HeroNode2(4, "武松", "捕头"));
        System.out.println("原链表为");
        doubleLinkedList.list();
        //修改节点
        doubleLinkedList.update(new HeroNode2(1, "小宋", "及时雨来哦"));
        System.out.println("修改后");
        doubleLinkedList.list();

        //删除
        doubleLinkedList.delete(1);
        System.out.println("删除后");
        doubleLinkedList.list();
    }
}

//双向链表用来管理英雄
class DoubleLinkedList {
    //先初始化一个头节点, 头节点不要动, 不存放具体的数据
    private HeroNode2 headNode = new HeroNode2(0, "", "");

    public HeroNode2 getHeadNode() {
        return headNode;
    }

    // 添加一个节点到双向链表的最后.
    public void addHero(HeroNode2 heroNode) {
        //temp辅助指针
        HeroNode2 temp = headNode; //头结点，不动，没有数据
        while (temp.next != null) {
            temp = temp.next; //直到最后一个节点
        }
        // 当退出 while 循环时，temp 就指向了链表的最后
        // 形成一个双向链表
        temp.next = heroNode;
        heroNode.pre = temp;

    }
    //按编号排序添加
    public void addHeroByOrder(HeroNode2 heroNode) {
        //因为头节点不能动，因此我们仍然通过一个辅助指针(变量)来帮助找到添加的位置
        // 因为单链表，因为我们找的 temp 是位于 添加位置的前一个节点，否则插入不了
        HeroNode2 temp = headNode;
        boolean flag = false;  //标志添加的节点编号是否已经存在
        while (true) {
            if (temp.next == null) {//说明 temp 已经在链表的最后
                break;
            }
            if (temp.next.No > heroNode.No) {//位置找到，就在 temp 的后面插入
                break;
            }
            if (temp.next.No == heroNode.No) {//说明希望添加的 heroNode 的编号已然存在
                flag = true;
                break;
            }
            temp = temp.next; //后移，遍历当前链表
        }
        if (flag) {//为真则不能添加，说明编号存在
            System.out.println("要添加的编号" + heroNode.No + "已经存在");
            return;
        } else {
            //插入到链表中, temp 的后面
            if (temp.next != null) {
                // 如果是最后一个节点，就不需要执行下面这句话，否则出现空指针
                temp.next.pre = heroNode;
            }
            heroNode.next = temp.next;
            temp.next = heroNode;
            heroNode.pre = temp;
        }
    }

    //修改节点的信息, 根据 no 编号来修改，即 no 编号不能改.
    // 说明
    // 1. 根据 newHeroNode 的 no 来修改即可
    public void update(HeroNode2 heroNode) {
        if (headNode.next == null) {
            System.out.println("链表为空");
            return;
        }
        //找到需要修改的节点, 根据 no 编号
        // 定义一个辅助变量，指向第一个节点，不是头结点
        HeroNode2 temp = headNode.next;
        while (true) {
            if (temp == null) {//已经遍历完链表
                System.out.println("没有相对应的节点");
                break;
            }
            if (temp.No == heroNode.No) {
                temp.Name = heroNode.Name;
                temp.nickName = heroNode.nickName;
                System.out.println("修改节点成功");
                break;
            }
            temp = temp.next;
        }
    }

    // 从双向链表中删除一个节点,
    // 说明
    // 1 对于双向链表，我们可以直接找到要删除的这个节点
    // 2 找到后，自我删除即可
    public void delete(int no) {
        if (headNode.next == null) {
            System.out.println("链表为空");
            return;
        }
        HeroNode2 temp = headNode.next;
        while (true) {
            if (temp == null) {//已经到链表的最后
                System.out.println("没有找到相对应的节点");
                break;
            }
            if (temp.No == no) {//找到的待删除节点的前一个节点 temp
                // temp.next = temp.next.next;[单向链表]
                temp.pre.next = temp.next;
                // 这里我们的代码有问题?
                // 如果是最后一个节点，就不需要执行下面这句话，否则出现空指针
                if (temp.next != null) {
                    temp.next.pre = temp.pre;
                }
                System.out.println("节点删除成功");
                break;
            }
            temp = temp.next;//temp 后移，遍历
        }

    }

    //显示链表[遍历]
    public void list() {
        HeroNode2 temp = headNode;//头结点，不动，没有数据
        while (temp.next != null) {
            temp = temp.next; //头结点没有数据，所以要先指向下一个
            System.out.println(temp);
        }
        return;
    }
}

//定义HeroNode节点，每个HeroNode节点就是一个节点
class HeroNode2 {
    public int No; //英雄排名
    public String Name; //英雄名字
    public String nickName; //别名
    public HeroNode2 next;  //指向下一个节点

    public HeroNode2 pre; //指向上一个节点

    HeroNode2(int no, String name, String nickName) {
        this.No = no;
        this.Name = name;
        this.nickName = nickName;
    }

    @Override
    public String toString() {
        return "HeroNode{" +
                "No=" + No +
                ", Name='" + Name + '\'' +
                ", nickName='" + nickName + '\'' +
                '}';
    }
}
~~~



### 单向环形链表

Josephu(约瑟夫、约瑟夫环) 问题 

Josephu 问题为：设编号为 1，2，… n 的 n 个人围坐一圈，约定编号为 k（1<=k<=n）的人从 1 开始报数，数到 m 的那个人出列，它的下一位又从 1 开始报数，数到 m 的那个人又出列，依次类推，直到所有人出列为止，由此产生一个出队编号的序列。 

提示：用一个不带头结点的循环链表来处理 Josephu 问题：先构成一个有 n 个结点的单循环链表，然后由 k 结点起从 1 开始计数，计到 m 时，对应结点从链表中删除，然后再从被删除结点的下一个结点又从 1 开始计数，直到最后一个结点从链表中删除算法结束。

 约瑟夫问题的示意图

![image-20200730173258042](https://gitee.com/xudongyin/img/raw/master/img/20200730173300.png)

 约瑟夫问题-创建环形链表的思路图解 

![image-20200730173641030](https://gitee.com/xudongyin/img/raw/master/img/20200730173642.png)

 约瑟夫问题-小孩出圈的思路分析图 

![image-20200730173659110](https://gitee.com/xudongyin/img/raw/master/img/20200730173700.png)

```java
package LinkedList;

public class JosephuDemo {
    public static void main(String[] args) {
        CircleSingleLinkedList list = new CircleSingleLinkedList(5);
        list.showBoy();
        list.countBoy(1, 2, 5);
    }

}
//环形单向链表
class CircleSingleLinkedList {
    private Boy first = null; //指向第一个节点的辅助指针

     CircleSingleLinkedList(int num) {
         Boy curBoy = null; //辅助指针，用于指向要操作的当前节点
         if (num < 1) { //节点数量少于1
             System.out.println("无法构成环形单向链表");
             return;
         }
         for (int i = 1; i <= num; i++) {
             if (i == 1) {
                 first = new Boy(1); //首节点不能动
                 curBoy = first;  //当前节点，为首节点
                 first.setNext(first); // 构成环
             } else {
                 Boy boy = new Boy(i);
                 curBoy.setNext(boy); //当前节点连接加入节点
                 boy.setNext(first); //加入节点下个节点指向首节点
                 curBoy = boy; //当前节点辅助指针指向加入节点
             }
         }
    }

    //遍历环形单向链表
    public void showBoy() {
        if (first == null) {
            System.out.println("连表为空");
            return;
        }
   // 因为 first 不能动，因此我们仍然使用一个辅助指针完成遍历
        Boy curBoy = first;
        while (true) {
            System.out.println("小孩的编号"+curBoy.getNo());
            if (curBoy.getNext() == first) {// 说明已经遍历完毕
                break;
            }
            curBoy = curBoy.getNext(); //指针后移

        }
    }

    //根据用户的输入，计算出小孩出圈的顺序
    /**
     *
     * @param startNo 开始报数的节点编号
     * @param countNum 表示数几下
     * @param num  表示圈内节点个数
     */
    public void countBoy(int startNo, int countNum, int num) {
        if (startNo > num || countNum < 1 || num < 1) {
            System.out.println("参数输入有误");
            return;
        }
        //创建一个辅助指针helper，指向要删除节点的前一个节点
        //要删除的节点用first指针指定
        Boy helper = first;
        // 辅助指针(变量) helper , 事先应该指向环形链表的最后这个节点
        while (true) {
            if (helper.getNext() == first) {// 说明 helper 指向最后小孩节点
                break;
            }
            helper = helper.getNext();//后移
        }
        //将first指针指向开始数数的位置，helper指针也跟着移动
        for (int i = 0; i < startNo-1; i++) {
            first = first.getNext();
            helper = helper.getNext();
        }
        //进行报数出圈
        while (true) {
            if (helper == first) {//只剩最后一个节点
                break;
            }
            //进行报数移动,循环结束，first指向应该移除的节点
            for (int i = 0; i < countNum - 1; i++) {
                first = first.getNext();
                helper = helper.getNext();
            }
            System.out.println("节点"+first.getNo()+"出圈");
            first = first.getNext(); //first指向下一个节点
            helper.setNext(first);
        }
        System.out.println("剩余的节点为" + helper.getNo());
    }

}
//节点
class Boy {
    private int no; //节点编号
    private Boy next;  //指向下一个节点

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



## 栈

栈是一个先入后出(FILO-First In Last Out)的有序列表。

栈(stack)是限制线性表中元素的插入和删除只能在线性表的同一端进行的一种特殊线性表。允许插入和删除的一端，为变化的一端，称为栈顶(Top)，另一端为固定的一端，称为栈底(Bottom)。 

根据栈的定义可知，最先放入栈中元素在栈底，最后放入的元素在栈顶，而删除元素刚好相反，最后放入的元素最先删除，最先放入的元素最后删除

图解方式说明出栈(pop)和入栈(push)的概念

![image-20200731115046186](https://gitee.com/xudongyin/img/raw/master/img/20200731115047.png)

> 栈的应用场景 

1) 子程序的调用：在跳往子程序前，会先将下个指令的地址存到堆栈中，直到子程序执行完后再将地址取出，以回到原来的程序中。 

2) 处理递归调用：和子程序的调用类似，只是除了储存下一个指令的地址外，也将参数、区域变量等数据存入堆栈中。 

3) 表达式的转换[中缀表达式转后缀表达式]与求值(实际解决)。 

4) 二叉树的遍历。 

5) 图形的深度优先(depth 一 first)搜索法。 

### 栈的快速入门 

1) 用数组模拟栈的使用，由于栈是一种有序列表，当然可以使用数组的结构来储存栈的数据内容， 

下面我们就用数组模拟栈的出栈，入栈等操作。 

2) 实现思路分析,并画出示意图

![image-20200731115244334](https://gitee.com/xudongyin/img/raw/master/img/20200731115245.png)

```java
package Stack;

import java.util.Scanner;

public class ArrayStackDemo{
    public static void main(String[] args) {
        ArrayStack stack = new ArrayStack(6);
        String key;
        Scanner scanner = new Scanner(System.in);
        boolean loops = true;
        while (loops) {
            System.out.println("show: 表示显示栈");
            System.out.println("exit: 退出程序");
            System.out.println("push: 表示添加数据到栈(入栈)");
            System.out.println("pop: 表示从栈取出数据(出栈)");
            System.out.println("请输入你的选择");
            key = scanner.next();
            switch (key) {
                case "show":
                    stack.showStack();
                    break;
                case "exit":
                    scanner.close();
                    loops = false;
                    break;
                case "push":
                    System.out.println("请输入一个值");
                    int val = scanner.nextInt();
                    stack.push(val);
                    break;
                case "pop":
                    try {
                        int i = stack.pop();
                        System.out.println("出栈的值为"+i);
                    } catch (Exception e) {
                        e.getMessage();
                    }
                    break;
                default:
                    break;
            }
        }
        System.out.println("程序退出");
    }
}
//栈的数组实现
class ArrayStack {
    private int top ; //栈顶指针
    private int[] arr; //栈
    private int maxSize; //栈的最大容量

    ArrayStack(int num) {
        this.maxSize = num;
        this.arr = new int[maxSize];
        top = -1;
    }

    //判断栈是否满
    public boolean isFull() {
        return top == maxSize - 1;
    }
    //判断栈是否空
    public boolean isEmpty() {
        return top == - 1;
    }

    //入栈
    public void push(int val) {
        //先判断是否栈满
        if (isFull()) {
            System.out.println("栈满");
            return;
        }
        top++;
        arr[top] = val;
    }

    //出栈
    public int pop() {
        //先判断栈是否为空
        if (isEmpty()) {
            throw new RuntimeException("栈空");
        }
        int val = arr[top];
        top--;
        return val;
    }

    //遍历栈
    public void showStack() {
        if (isEmpty()) {
            System.out.println("栈为空");
            return;
        }
        for (int i = top; i >= 0; i--) {
            System.out.printf("stack[%d]=%d\n", i, arr[i]);
        }
    }
}
```

### 栈实现综合计算器(中缀表达式) 

 使用栈来实现综合计算器- 

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200731161748.png" alt="image-20200731161746835" style="zoom:50%;" />

 思路分析(图解)

![image-20200731161812639](https://gitee.com/xudongyin/img/raw/master/img/20200731161814.png)

代码实现[1. 先实现一位数的运算， 2. 扩展到多位数的运算]

```java
package Stack;

public class Calculator {
    public static void main(String[] args) {
        String expression = "70*200-4"; // 15//如何处理多位数的问题？
        //创建两个栈，一个数栈，一个符号栈
        ArrayStack2 numStack = new ArrayStack2(10);
        ArrayStack2 operStack = new ArrayStack2(10);
        //定义需要的相关变量
        int index = 0; //指针，用于扫描
        int num1 ;
        int num2 ;
        int oper ;
        int res ;
        char ch ;//用于存放扫描到的字符
        String SplicingNum = "";//用于拼接数字
        //开始 while 循环的扫描 expression
        while (true) {
            //依次得到 expression 的每一个字符
            ch = expression.substring(index, index + 1).charAt(0);
            //判断获取的字符是什么
            if (operStack.isOper(ch)) {//如果是运算符
                if (!operStack.isEmpty()) {
                    //如果符号栈有操作符，就进行比较,如果当前的操作符的优先级小于或者等于栈中的操作符, 就需要从数栈中 pop 出两个数
                    //在从符号栈中 pop 出一个符号，进行运算，将得到结果入数栈，然后将当前的操作符入符号栈
                    if (operStack.priority(ch) <= operStack.priority(operStack.peek())) {
                        num1 = numStack.pop();
                        num2 = numStack.pop();
                        oper = operStack.pop();
                        res = numStack.cal(num1, num2, oper);
                        //把运算的结果如数栈
                        numStack.push(res);
                        //然后将当前的操作符入符号栈
                        operStack.push(ch);
                    } else {
                        //如果当前的操作符的优先级大于栈中的操作符， 就直接入符号栈.
                        operStack.push(ch);
                    }
                } else {
                    //如果为空直接入符号栈
                    operStack.push(ch);
                }
            } else {//如果是数，则直接入数栈
//                numStack.push(ch-48); //ch 为字符，要转为int
                //分析思路
                // 1. 当处理多位数时，不能发现是一个数就立即入栈，因为他可能是多位数
                // 2. 在处理数，需要向 expression 的表达式的 index 后再看一位,如果是数就进行扫描，如果是符号才入栈
                // 3. 因此我们需要定义一个变量 字符串，用于拼接
                SplicingNum += ch;
                //如果 ch 已经是 expression 的最后一位，就直接入栈
                if (index == expression.length() - 1) {
                    numStack.push(Integer.parseInt(SplicingNum));
                } else {
                    //判断下一个字符是不是数字，如果是数字，就继续扫描，如果是运算符，则入栈
                    // 注意是看后一位，不是 index++,index 没有后移
                    if (operStack.isOper(expression.substring(index + 1, index + 2).charAt(0))) {
                        //如果后一位是运算符，则入栈 keepNum = "1" 或者 "123"
                        numStack.push(Integer.parseInt(SplicingNum));
                        //重要的!!!!!!, keepNum 清空
                        SplicingNum = "";
                    }
                }

            }
            index++;//后移一位字符
            if (index >= expression.length()) {//扫描完
                break;
            }
        }
        //当表达式扫描完毕，就顺序的从数栈和符号栈中 pop 出相应的数和符号，并计算
        while (true) {
            //如果符号栈为空，则计算到最后的结果, 数栈中只有一个数字【结果】
            if (operStack.isEmpty()) {
                break;
            }
            num1 = numStack.pop();
            num2 = numStack.pop();
            oper = operStack.pop();
            res = numStack.cal(num1, num2, oper);
            //把运算的结果如数栈
            numStack.push(res);
        }
        System.out.println("表达式："+expression+"计算结果为" + numStack.pop());
    }

}
//栈的数组实现
class ArrayStack2 {
    private int top ; //栈顶指针
    private int[] arr; //栈
    private int maxSize; //栈的最大容量

    ArrayStack2(int num) {
        this.maxSize = num;
        this.arr = new int[maxSize];
        top = -1;
    }

    //判断栈是否满
    public boolean isFull() {
        return top == maxSize - 1;
    }
    //判断栈是否空
    public boolean isEmpty() {
        return top == - 1;
    }

    //获取栈顶的值
    public int peek() {
        return arr[top];
    }
    //入栈
    public void push(int val) {
        //先判断是否栈满
        if (isFull()) {
            System.out.println("栈满");
            return;
        }
        top++;
        arr[top] = val;
    }

    //出栈
    public int pop() {
        //先判断栈是否为空
        if (isEmpty()) {
            throw new RuntimeException("栈空");
        }
        int val = arr[top];
        top--;
        return val;
    }

    //遍历栈
    public void showStack() {
        if (isEmpty()) {
            System.out.println("栈为空");
            return;
        }
        for (int i = top; i >= 0; i--) {
            System.out.printf("stack[%d]=%d\n", i, arr[i]);
        }
    }

    //返回运算符的优先级，优先级是程序员来确定, 优先级使用数字表示
    // 数字越大，则优先级就越高
    public int priority(int oper) {
        if (oper == '*' || oper == '/') {
            return 1;
        } else if (oper == '+' || oper == '-') {
            return 0;
        } else {
            return -1;// 假定目前的表达式只有 +, - , * , /
        }
    }

    //判断是否是运算符
    public boolean isOper(char val) {
        return val == '+' || val == '-' || val == '*' || val == '/';
    }

    //进行运算
    public int cal(int num1, int num2, int oper) {
        int res = 0; //用于存放计算结果
        switch (oper) {
            case '+':
                res = num1 + num2;
                break;
            case '-':
                res = num2 - num1; //注意参数顺序，不能反
                break;
            case '*':
                res = num1 * num2;
                break;
            case '/':
                res = num2 / num1; //注意参数顺序，不能反
                break;
            default:
                break;
        }
        return res;
    }
}
```

### 逆波兰计算器 

我们完成一个逆波兰计算器，要求完成如下任务: 

1) 输入一个逆波兰表达式(后缀表达式)，使用栈(Stack), 计算其结果 

2) 支持小括号和多位数整数，因为这里我们主要讲的是数据结构，因此计算器进行简化，只支持**对整数**的计算。 

3) 思路分析 

![image-20200731162022296](https://gitee.com/xudongyin/img/raw/master/img/20200731162024.png)

```java
package Stack;

import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class NepolandNotation {
    public static void main(String[] args) {
        //说明为了方便，逆波兰表达式 的数字和符号使用空格隔开
        //String suffixExpression = "30 4 + 5 * 6 -";
        String suffixExpression = "4 5 * 8 - 60 + 8 2 / +"; // 76
        //思路
        // 1. 先将 "3 4 + 5 × 6 - " => 放到 ArrayList 中
        // 2. 将 ArrayList 传递给一个方法，遍历 ArrayList 配合栈 完成计算
        List<String> list = getList(suffixExpression);
        int res = calculate(list);
        System.out.println("运算结果为"+res);
    }

    //将一个逆波兰表达式， 依次将数据和运算符 放入到 ArrayList中
    public static List<String> getList(String Expression) {
        String[] strings = Expression.split(" ");
        ArrayList<String> list = new ArrayList<>();
        for (String item : strings) {
            list.add(item);
        }
        return list;
    }
    //完成对逆波兰表达式的运算
    /**
     * 1)从左至右扫描，将 3 和 4 压入堆栈；
     * 2)遇到+运算符，因此弹出 4 和 3（4 为栈顶元素，3 为次顶元素），计算出 3+4 的值，得 7，再将 7 入栈；
     * 3)将 5 入栈；
     * 4)接下来是×运算符，因此弹出 5 和 7，计算出 7×5=35，将 35 入栈；
     * 5)将 6 入栈；
     * 6)最后是-运算符，计算出 35-6 的值，即 29，由此得出最终结果
     * */
    public static int calculate(List<String> list) {
        // 创建一个栈, 只需要一个栈即可
        Stack<String> stack = new Stack<>();
        int num1;
        int num2;
        int res; //用于存放计算结果
        for (String item : list) {
            if (item.matches("\\d+")) { // 匹配的是多位数
                stack.push(item);
            }else {// pop 出两个数，并运算， 再入栈
                num1 = Integer.parseInt(stack.pop());
                num2 = Integer.parseInt(stack.pop());
                if (item.equals("+")) {
                    res = num1 + num2;
                }else if (item.equals("-")) {
                    res = num2 - num1;
                }else if (item.equals("*")) {
                    res = num1 * num2;
                } else if (item.equals("/")) {
                    res = num2 / num1;
                } else {
                    throw new RuntimeException("运算符错误");
                }
                //把 res 入栈
                stack.push("" + res);
            }
        }
        //最后留在 stack 中的数据是运算结果
        return Integer.parseInt(stack.pop());
    }
}
```

### 中缀表达式转换为后缀表达式 

大家看到，后缀表达式适合计算式进行运算，但是人却不太容易写出来，尤其是表达式很长的情况下，因此在开发 中，我们需要将 中缀表达式转成后缀表达式。

具体步骤如下: 

1) 初始化两个栈：运算符栈 s1 和储存中间结果的栈 s2； 

2) 从左至右扫描中缀表达式； 

3) 遇到操作数时，将其压 s2； 

4) 遇到运算符时，比较其与 s1 栈顶运算符的优先级： 

​	1.如果 s1 为空，或栈顶运算符为左括号“(”，则直接将此运算符入栈； 

​	2.否则，若优先级比栈顶运算符的高，也将运算符压入 s1； 

​	3.否则，将 s1 栈顶的运算符弹出并压入到 s2 中，再次转到(4-1)与 s1 中新的栈顶运算符相比较； 

5) 遇到括号时： 

​	(1) 如果是左括号“(”，则直接压入 s1 

​	(2) 如果是右括号“)”，则依次弹出 s1 栈顶的运算符，并压入 s2，直到遇到左括号为止，此时将这一对括号丢弃 

6) 重复步骤 2 至 5，直到表达式的最右边 

7) 将 s1 中剩余的运算符依次弹出并压入 s2 

8) 依次弹出 s2 中的元素并输出，结果的逆序即为中缀表达式对应的后缀表达式

> 举例说明

将中缀表达式“1+((2+3)×4)-5”转换为后缀表达式的过程如下 

![image-20200731210744122](https://gitee.com/xudongyin/img/raw/master/img/20200731210750.png)

因此结果为 :"1 2 3 + 4 × + 5 –"

```java
package Stack;

import java.util.ArrayList;
import java.util.List;
import java.util.Stack;

public class NepolandNotation {
    public static void main(String[] args) {
        String expression = "1+((2+3)*4)-5+(6*6)";//注意表达式
        List<String> list = toInfixExpressionList(expression);
        System.out.println("中缀表达式为:"+list);
        List<String> suffixExpreesionList = parseSuffixExpreesionList(list);
        System.out.println("后缀表达式为:" + suffixExpreesionList);
        int result = calculate(suffixExpreesionList);
        System.out.println("计算结果为" + result);
        /*
        //说明为了方便，逆波兰表达式 的数字和符号使用空格隔开
        //String suffixExpression = "30 4 + 5 * 6 -";
        String suffixExpression = "4 5 * 8 - 60 + 8 2 / +"; // 76
        //思路
        // 1. 先将 "3 4 + 5 × 6 - " => 放到 ArrayList 中
        // 2. 将 ArrayList 传递给一个方法，遍历 ArrayList 配合栈 完成计算
        List<String> list = getList(suffixExpression);
        int res = calculate(list);
        System.out.println("运算结果为" + res);

         */
    }

    //将一个中缀表达式封装成list
    public static List<String> toInfixExpressionList(String s) {
        List<String> list = new ArrayList<>();
        int index = 0;//这是一个指针，用于遍历 中缀表达式字符串
        String temp;// 对多位数的拼接
        char c;// 每遍历到一个字符，就放入到 c
        do {
            //如果 c 是一个非数字，我需要加入到 ls
            if ((c = s.charAt(index)) < 48 || (c = s.charAt(index)) > 57) {
                list.add("" + c);
                index++;//后移
            } else {//如果是一个数，需要考虑多位数
                temp = ""; //先将 temp 置成空 '0'[48]->'9'[57]
                while (index < s.length() && (c = s.charAt(index)) >= 48 && (c = s.charAt(index)) <= 57) {
                    temp += c;//拼接
                    index++;//后移
                }//直到index指向的字符不是数字
                list.add(temp);
            }
        } while (index < s.length());
        return list;
    }
    
///////////////////////////////////////////////////////////////////////////////
    //将中缀表达式转化为后缀表达式
    //即 ArrayList [1,+,(,(,2,+,3,),*,4,),-,5] =》 ArrayList [1,2,3,+,4,*,+,5,–]
    // 方法：将得到的中缀表达式对应的 List => 后缀表达式对应的 List
    public static List<String> parseSuffixExpreesionList(List<String> list) {
        //定义两个栈
        Stack<String> s1 = new Stack<String>();
        // 符号栈
        // 说明：因为 s2 这个栈，在整个转换过程中，没有 pop 操作，而且后面我们还需要逆序输出
        // 因此比较麻烦，这里我们就不用 Stack<String> 直接使用 List<String> s2
        // Stack<String> s2 = new Stack<String>();// 储存中间结果的栈 s2
        List<String> s2 = new ArrayList<String>();// 储存中间结果的 Lists2
        for (String item : list) {
            if (item.matches("\\d+")) {
                s2.add(item); //遇到操作数时，将其压 s2
            } else if (s1.isEmpty() || s1.peek().equals("(") || item.equals("(")) {
                s1.push(item);//如果 s1 为空，或栈顶运算符为左括号“(”，则直接将此运算符入栈,如果是左括号“(”，则直接压入 s1
            } else if (item.equals(")")) {
                do {//如果是右括号“)”，则依次弹出 s1 栈顶的运算符，并压入 s2，直到遇到左括号为止，此时将这一对括号丢弃
                    s2.add(s1.pop());
                } while (!s1.peek().equals("("));
                s1.pop();//!!! 将 ( 弹出 s1 栈， 消除小括号
            } else {
                //当 item 的优先级小于等于 s1 栈顶运算符, 将 s1 栈顶的运算符弹出并加入到 s2 中，再次转到(4.1) 与 s1 中新的栈顶运算符相比较
                // 问题：我们缺少一个比较优先级高低的方法
                while (s1.size() != 0 && Operation.getValue(item) <= Operation.getValue(s1.peek())) {
                    s2.add(s1.pop());
                }
                //直到item比栈顶运算符优先级高
                s1.push(item);
            }

        }
        //将 s1 中剩余的运算符依次弹出并加入 s2
        while (s1.size() != 0) {
            s2.add(s1.pop());
        }
        return s2; //注意因为是存放到 List, 因此按顺序输出就是对应的后缀表达式对应的 List
    }
///////////////////////////////////////////////////////////////////////////////
    
    //将一个逆波兰表达式， 依次将数据和运算符 放入到 ArrayList中
    public static List<String> getList(String Expression) {
        String[] strings = Expression.split(" ");
        ArrayList<String> list = new ArrayList<>();
        for (String item : strings) {
            list.add(item);
        }
        return list;
    }
    //完成对逆波兰表达式的运算

    /**
     * 1)从左至右扫描，将 3 和 4 压入堆栈；
     * 2)遇到+运算符，因此弹出 4 和 3（4 为栈顶元素，3 为次顶元素），计算出 3+4 的值，得 7，再将 7 入栈；
     * 3)将 5 入栈；
     * 4)接下来是×运算符，因此弹出 5 和 7，计算出 7×5=35，将 35 入栈；
     * 5)将 6 入栈；
     * 6)最后是-运算符，计算出 35-6 的值，即 29，由此得出最终结果
     */
    public static int calculate(List<String> list) {
        // 创建一个栈, 只需要一个栈即可
        Stack<String> stack = new Stack<>();
        int num1;
        int num2;
        int res; //用于存放计算结果
        for (String item : list) {
            if (item.matches("\\d+")) { // 匹配的是多位数
                stack.push(item);
            } else {// pop 出两个数，并运算， 再入栈
                num1 = Integer.parseInt(stack.pop());
                num2 = Integer.parseInt(stack.pop());
                if (item.equals("+")) {
                    res = num1 + num2;
                } else if (item.equals("-")) {
                    res = num2 - num1;
                } else if (item.equals("*")) {
                    res = num1 * num2;
                } else if (item.equals("/")) {
                    res = num2 / num1;
                } else {
                    throw new RuntimeException("运算符错误");
                }
                //把 res 入栈
                stack.push("" + res);
            }
        }
        //最后留在 stack 中的数据是运算结果
        return Integer.parseInt(stack.pop());
    }
}
///////////////////////////////////////////////////////////////////////////////
//编写一个类 Operation 可以返回一个运算符 对应的优先级
class Operation {
    private static int ADD = 1;
    private static int SUB = 1;
    private static int MUL = 2;
    private static int DIV = 2;

    //写一个方法，返回对应的优先级数字
    public static int getValue(String operation) {
        int val;
        switch (operation) {
            case "+":
                val = ADD;
                break;
            case "-":
                val = SUB;
                break;
            case "*":
                val = MUL;
                break;
            case "/":
                val = DIV;
                break;
            default:
                throw new RuntimeException("运算符有误");
        }
        return val;
    }
}
```



## 递归 

简单的说: **递归就是方法自己调用自己**,每次调用时**传入不同的变量**.递归有助于编程者解决复杂的问题,同时可以让代码变得简洁

> 递归调用机制 

1) 打印问题 

2) 阶乘问题 

3) 使用图解方式说明了递归的调用机制

![image-20200801093906974](https://gitee.com/xudongyin/img/raw/master/img/20200801093908.png)

> 递归用于解决什么样的问题 

1) 各种数学问题如: 8 皇后问题 , 汉诺塔, 阶乘问题, 迷宫问题, 球和篮子的问题(google 编程大赛) 

2) 各种算法中也会使用到递归，比如快排，归并排序，二分查找，分治算法等. 

3) 将用栈解决的问题-->递归代码比较简洁

> 递归需要遵守的重要规则 

1) 执行一个方法时，就创建一个新的受保护的独立空间(栈空间) 

2) 方法的局部变量是独立的，不会相互影响, 比如 n 变量 

3) 如果方法中使用的是引用类型变量(比如数组)，就会共享该引用类型的数据. 

4) 递归**必须向退出递归的条件逼近**，否则就是无限递归,出现 StackOverflowError，死龟了:) 

5) 当一个方法执行完毕，或者遇到 return，就会返回，**遵守谁调用，就将结果返回给谁**，同时当方法执行完毕或者返回时，该方法也就执行完毕 

### 迷宫问题

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200801133507.png" alt="image-20200801133506177" style="zoom:67%;" />

```java
package Recursion;

public class MiGong {
    public static void main(String[] args) {
        // 先创建一个二维数组，模拟迷宫
        // 地图
        int[][] map = new int[8][7];
        // 使用 1 表示墙
        for (int i = 0; i < 7; i++) {
            map[0][i] = 1;
            map[7][i] = 1;
        }
        // 左右全部置为 1
        for (int i = 0; i < 8; i++) {
            map[i][0] = 1;
            map[i][6] = 1;
        }
        //设置挡板, 1 表示
        map[3][1] = 1;
        map[3][2] = 1;
//         map[1][2] = 1;
//         map[2][2] = 1;

        //输出地图
        for (int[] item : map) {
            for (int i : item) {
                System.out.print(i + " ");
            }
            System.out.println();
        }

//        setWay(map, 1, 1);  //策略1
        setWay2(map, 1, 1); //策略2

        System.out.println("找到通路的路径：");
        for (int[] item : map) {
            for (int i : item) {
                System.out.print(i + " ");
            }
            System.out.println();
        }

    }

    //使用递归回溯来给小球找路
    // 说明
    // 1. map 表示地图
    // 2. i,j 表示从地图的哪个位置开始出发 (1,1)
    // 3. 如果小球能到 map[6][5] 位置，则说明通路找到.
    // 4. 约定： 当 map[i][j] 为 0 表示该点没有走过 当为 1 表示墙 ；
    //          2 表示通路可以走 ； 3 表示该点已经 走过，但是走不通
    //5. 在走迷宫时，需要确定一个策略(方法) 下->右->上->左 , 如果该点走不通，再回溯
    public static boolean setWay(int[][] map, int i, int j) {
        if (map[6][5] == 2) {
            return true; //走得到map[6][5]表示通路找到
        } else {
            if (map[i][j] == 0) { //点为0，没有走过
                map[i][j] = 2; //假设当前点可以走通
                if (setWay(map, i + 1, j)) { //向下走
                    return true; //走得通
                } else if (setWay(map, i, j + 1)) { //向右走
                    return true;
                } else if (setWay(map, i - 1, j)) { //向上走
                    return true;
                } else if (setWay(map, i, j - 1)) { //向左走
                    return true;
                } else {
                    map[i][j] = 3; //走不通
                    return false;
                }
            } else {//map[i][j] != 0 ,点不为0，可能为1,2,3
                return false;
            }
        }
    }

    //修改找路的策略，改成 上->右->下->左
    public static boolean setWay2(int[][] map, int i, int j) {
        if (map[6][5] == 2) {
            return true; //走得到map[6][5]表示通路找到
        } else {
            if (map[i][j] == 0) { //点为0，没有走过
                map[i][j] = 2; //假设当前点可以走通
                if (setWay2(map, i - 1, j)) { //向上走
                    return true; //走得通
                } else if (setWay2(map, i, j + 1)) { //向右走
                    return true;
                } else if (setWay2(map, i + 1, j)) { //向下走
                    return true;
                } else if (setWay2(map, i, j - 1)) { //向左走
                    return true;
                } else {
                    map[i][j] = 3; //走不通
                    return false;
                }
            } else {//map[i][j] != 0 ,点不为0，可能为1,2,3
                return false;
            }
        }
    }
}
```

> 对迷宫问题的讨论 

1) 小球得到的路径，和程序员设置的**找路策略**有关即：找路的上下左右的顺序相关 

2) 再得到小球路径时，可以先使用(下右上左)，再改成(**上右下左**)，看看路径是不是有变化 

3) 测试回溯现象 

4) **思考**: 如何求出最短路径? 思路-》代码实现(通过通路路径地图获取节点为2的个数，然后比较即可)

### 八皇后问题

八皇后问题，是一个古老而著名的问题，是回溯算法的典型案例。该问题是国际西洋棋棋手马克斯·贝瑟尔于 1848 年提出：在 8×8 格的国际象棋上摆放八个皇后，使其不能互相攻击，即：**任意两个皇后都不能处于同一行、 同一列或同一斜线上，问有多少种摆法**。

![image-20200801155903694](https://gitee.com/xudongyin/img/raw/master/img/20200801155905.png)

**说明：** 

理论上应该创建一个二维数组来表示棋盘，但是实际上可以通过算法，用一个一维数组即可解决问题. arr[8] = {0 , 4, 7, 5, 2, 6, 1, 3} //对应 arr 下标 表示第几行，即第几个皇后，arr[i] = val , val 表示第 i+1 个皇后，放在第 i+1 行的第 val+1 列 

```java
package Recursion;

public class Queen8 {
    //定义一个 max 表示共有多少个皇后
    int max = 8;
    //定义数组 array, 保存皇后放置位置的结果,比如 arr = {0 , 4, 7, 5, 2, 6, 1, 3}
    int[] array = new int[max]; //下标表示行，值表示列

    int num = 0;//记录方案数
    int count = 0;//记录判断次数

    public static void main(String[] args) {
        Queen8 queen8 = new Queen8();
        queen8.check(0);
        System.out.println("方案次数为" + queen8.num);
        System.out.println("判断次数为" + queen8.count);
    }

    //编写一个方法，放置第 n 个皇后
    //特别注意： check 是每一次递归时，进入到 check 中都有 for(int i = 0; i < max; i++)，因此会有回溯
    private void check(int n) {
        if (n == max) {//n = 8 ,就是第九个皇后， 其实 8 个皇后就已然放好
            num++;
            print();
            return;
        }
        //依次放入皇后，并判断是否冲突
        for (int i = 0; i < max; i++) {
            //先把当前这个皇后 n , 放到该行的第 1 列
            array[n] = i;
            //判断当放置第 n 个皇后到 i 列时，是否冲突
            if (judge(n)) { // 不冲突
                //接着放 n+1 个皇后,即开始递归
                check(n+1);
            }
            //如果冲突，就继续执行 array[n] = i; 即将第 n 个皇后，放置在本行的后一个列位置
        }
    }

    //查看当我们放置第 n 个皇后, 就去检测该皇后是否和前面已经摆放的皇后冲突
    private boolean judge(int n) {
        count++;
        for (int i = 0; i < n; i++) {
            // 说明
            // 1. array[i] == array[n] 表示判断 第 n 个皇后是否和前面的 n-1 个皇后在同一列
            // 2. Math.abs(n-i) == Math.abs(array[n] - array[i]) 表示判断第 n 个皇后是否和第 i 皇后是否在同一斜线
            // Math.abs(1-0) == 1 Math.abs(array[n] - array[i]) = Math.abs(1-0) = 1
            //3. 判断是否在同一行, 没有必要，n 每次都在递增
            if (array[i] == array[n] || Math.abs(n - i) == Math.abs(array[n] - array[i])) {
                return false;
            }
        }
        return true;
    }

    //输出皇后的摆放方案
    private void print() {
        for (int i : array) {
            System.out.print(i+" ");
        }
        System.out.println();
    }
}
```



## 排序算法

排序也称排序算法(Sort Algorithm)，排序是将**一组数据**，依**指定的顺序**进行**排列的过程**。 

**排序的分类：** 

1) 内部排序: 指将需要处理的所有数据都加载到**内部存储器(内存)**中进行排序。 

2) 外部排序法： **数据量过大**，无法全部加载到内存中，需要借助**外部存储(文件等)**进行排序。 

3) 常见的排序算法分类(见右图)

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200801200011.png" alt="image-20200801195958073" style="zoom: 50%;" />

> **算法的时间复杂度**

**时间频度：**一个算法花费的时间与算法中语句的执行次数成正比例，哪个算法中语句执行次数多，它花费时间就多。**一个算法中的语句执行次数称为语句频度或时间频度**。记为 T(n)。

**时间复杂度** 

1) 一般情况下，**算法中的基本操作语句的重复执行次数是问题规模** **n** **的某个函数**，用 T(n)表示，若有某个辅助函数 f(n)，使得当 n 趋近于无穷大时，T(n) / f(n) 的极限值为不等于零的常数，则称 f(n)是 T(n)的同数量级函数。 记作 **T(n)=Ｏ( f(n) )**，称Ｏ( f(n) ) 为算法的渐进时间复杂度，简称时间复杂度。 如：T(n)=n+1,T(n)的同数量级函数 f(n)=n，时间复杂度为O( f(n) )=O(n)。

2) T(n) 不同，但时间复杂度可能相同。 如：T(n)=n²+7n+6 与 T(n)=3n²+2n+2 它们的 T(n) 不同，但时间复杂度相同，都为 **O(n²)**。 

3) 计算时间复杂度的方法： 

 用常数 1 代替运行时间中的所有加法常数 T(n)=n²+7n+6 => T(n)=n²+7n+1 

 修改后的运行次数函数中，只保留最高阶项 T(n)=n²+7n+1 => T(n) = n² 

 去除最高阶项的系数 T(n) = n² => T(n) = n² => O(n²) 

> 常见的时间复杂度 

1) 常数阶 O(1) 

![image-20200801205600818](https://gitee.com/xudongyin/img/raw/master/img/20200801205603.png)

2) 对数阶 O(log2n) 

![image-20200801205840796](https://gitee.com/xudongyin/img/raw/master/img/20200801205846.png)

3) 线性阶 O(n) 

![image-20200801205618915](https://gitee.com/xudongyin/img/raw/master/img/20200801205620.png)

4) 线性对数阶 O(nlog2n)

![image-20200801205627502](https://gitee.com/xudongyin/img/raw/master/img/20200801205631.png)

5) 平方阶 O(n^2) 

![image-20200801205637115](https://gitee.com/xudongyin/img/raw/master/img/20200801205638.png)

6) 立方阶 O(n^3) 

参考上面的 O(n²) 去理解就好了，O(n³)相当于三层 n 循环，其它的类似

7) k 次方阶 O(n^k) 

8) 指数阶 O(2^n) 

**常见的时间复杂度对应的图**:

![image-20200801205524771](https://gitee.com/xudongyin/img/raw/master/img/20200801205527.png)

**说明**：

1) 常见的算法时间复杂度由小到大依次为：Ο(1)＜Ο(log2n)＜Ο(n)＜Ο(nlog2n)＜Ο(n2)＜Ο(n3)＜ Ο(nk) ＜ Ο(2n) ，随着问题规模 n 的不断增大，上述时间复杂度不断增大，算法的执行效率越低 

2) 从图中可见，我们应该尽可能避免使用指数阶的算法

> **平均时间复杂度和最坏时间复杂度** 

1) 平均时间复杂度是指所有可能的输入实例均以等概率出现的情况下，该算法的运行时间。 

2) 最坏情况下的时间复杂度称最坏时间复杂度。**一般讨论的时间复杂度均是最坏情况下的时间复杂度**。这样做的原因是：最坏情况下的时间复杂度是算法在任何输入实例上运行时间的界限，这就保证了算法的运行时间不会比最坏情况更长。 

3) 平均时间复杂度和最坏时间复杂度是否一致，和算法有关(如图:)。

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200801205752.png" alt="image-20200801205745041" style="zoom:67%;" />

> 算法的空间复杂度简介 

1) 类似于时间复杂度的讨论，一个算法的空间复杂度(Space Complexity)定义为该算法所耗费的存储空间，它也是问题规模 n 的函数。 

2) 空间复杂度(Space Complexity)是对一个算法在运行过程中临时占用存储空间大小的量度。有的算法需要占用的临时工作单元数与解决问题的规模 n 有关，它随着 n 的增大而增大，当 n 较大时，将占用较多的存储单元，例 如快速排序和**归并排序算法, 基数排序**就属于这种情况 

3) 在做算法分析时，主要讨论的是时间复杂度。**从用户使用体验上看，更看重的程序执行的速度**。一些缓存产品 (redis, memcache) 和算法(基数排序)**本质就是用空间换时间**。

### 冒泡排序 

冒泡排序（Bubble Sorting）的基本思想是：通过对待排序序列从前向后（从下标较小的元素开始）,**依次比较相邻元素的值，若发现逆序则交换**，使值较大的元素逐渐从前移向后部，就象水底下的气泡一样逐渐向上冒。

**优化：** 

因为排序的过程中，各元素不断接近自己的位置，**如果一趟比较下来没有进行过交换，就说明序列有序**，因此要在排序过程中设置一个标志 flag 判断元素是否进行过交换。从而减少不必要的比较。(这里说的优化，可以在冒泡排序写好后，再进行) 

![image-20200801213437083](https://gitee.com/xudongyin/img/raw/master/img/20200801213439.png)

小结上面的图解过程: 

(1) 一共进行 数组的大小-1 次 大的循环 

(2)每一趟排序的次数在逐渐的减少 

(3) 如果我们发现在某趟排序中，没有发生一次交换， 可以提前结束冒泡排序。这个就是优化

```java
package Sort;

import java.util.Arrays;

public class BubbleSort {
    public static void main(String[] args) {
        int arr[] = {3, 9, -1, 10, 20};
        boolean flag = true; //用于标识数组是否已经提前排好序
        int temp = 0; //临时变量
        for (int i = 0; i < arr.length-1; i++) { //进行数组长度-1次排序
            for (int j = 0; j < arr.length - 1 - i; j++){ //每次排序的比较次数都在-1
                if (arr[j] > arr[j + 1]) {
                    temp = arr[j + 1];
                    arr[j + 1] = arr[j];
                    arr[j] = temp;
                    flag = false;
                }
            }
            System.out.print("第"+(i+1)+"遍排序后的数组为：");
            System.out.println(Arrays.toString(arr));
            if (flag) { //没有进行交换排序,也就是整个数组已经是排好序的状态
                break;
            } else {
                flag = true; //排好序后重置标识
            }
        }
    }
}

//排80000个数据用了20秒左右
```

### 选择排序 

选择式排序也属于内部排序法，是从欲排序的数据中，按指定的规则选出某一元素，再依规定交换位置后达到排序的目的

> 选择排序思想

选择排序（select sorting）也是一种简单的排序方法。它的基本思想是：第一次从 arr[0]~arr[n-1]中选取最小值，与 arr[0]交换，第二次从 arr[1]~arr[n-1]中选取最小值，与 arr[1]交换，第三次从 arr[2] ~ arr[n-1]中选取最小值，与 arr[2] 交换，…，第 i 次从 arr[i-1]~arr[n-1]中选取最小值，与 arr[i-1]交换，…, 第 n-1 次从 arr[n-2]~arr[n-1]中选取最小值，与 arr[n-2]交换，总共通过 n-1 次，得到一个按排序码从小到大排列的有序序列。

![image-20200801230242269](https://gitee.com/xudongyin/img/raw/master/img/20200801230243.png)

对一个数组的选择排序再进行讲解

![image-20200801230347311](https://gitee.com/xudongyin/img/raw/master/img/20200801230349.png)

```java
package Sort;

import java.util.Arrays;

public class SelectSort {
    public static void main(String[] args) {
        int [] arr = {101, 34, 119, 1, -1, 90, 123};
        selectSort(arr);

    }
    //选择排序
     public static void selectSort(int[] arr) {
         for (int i = 0; i < arr.length - 1; i++) {//进行数组长度-1次找最小值
             int minIndex = i; //用于存储最小值的下标
             int min = arr[i]; //用于存储最小值
             for (int j = i; j < arr.length; j++) {//从第i个值比较起，最小值都排在最前面，每进行一次外循环，需要比较的个数就-1
                 if (min > arr[j]) { //找到更小的值
                     minIndex = j;
                     min = arr[j];
                 }
             }//循环结束找到最小的值
             if (minIndex != i) { //最小值的下标改变，也就是找到了更小的值
                 arr[minIndex] = arr[i];  //进行交换，把最小值换到前面
                 arr[i] = min;
             }
             System.out.println("第" + (i + 1) + "遍排序：");
             System.out.println(Arrays.toString(arr));
         }
    }
}

//排80000个数据用了2-3秒左右
```

### 插入排序

插入式排序属于内部排序法，是对于欲排序的元素以插入的方式找寻该元素的适当位置，以达到排序的目的。

插入排序（Insertion Sorting）的基本思想是：**把** **n** **个待排序的元素看成为一个有序表和一个无序表**，开始时**有序表中只包含一个元素**，无序表中包含有 **n-1** **个元素**，排序过程中每次从无序表中取出第一个元素，把它的排序码依次与有序表元素的排序码进行比较，将它插入到有序表中的适当位置，使之成为新的有序表。

![image-20200802102258052](https://gitee.com/xudongyin/img/raw/master/img/20200802102259.png)

```java
package Sort;

import java.util.Arrays;

public class InsertSort {
    public static void main(String[] args) {
        int[] arr = {101, 34, 119, 1, -1, 89};
        insertSort(arr);
    }
    //插入排序
    public static void insertSort(int[] arr) {
        for (int i = 1; i < arr.length; i++) {
            int insertIndex = i - 1; //要进行排序的值的前一个下标位置
            int insertVal = arr[i]; //插入的值
            //要插入的值在前面排好序的数组寻找插入位置
            //说明
            // 1. insertIndex >= 0 保证在给 insertVal 找插入位置，不越界
            // 2. insertVal < arr[insertIndex] 待插入的数，还没有找到插入位置
            // 3. 就需要将 arr[insertIndex] 后移
            while (insertIndex >= 0 && insertVal < arr[insertIndex]) {
                //待插入的数比排好序的数组的数据小,即排好序的数组中大的数据往后移
                arr[insertIndex + 1] = arr[insertIndex];
                insertIndex--; //后移
            }//循环结束时就找到插入位置，插入位置为 insertIndex+1  可以使用insertIndex = 0 时验证
            if (insertIndex + 1 != i) { //要插入的位置不是自己原来的位置，所以赋值
                arr[insertIndex+1] = insertVal;
            }

            System.out.println("第"+i+"轮插入");
            System.out.println(Arrays.toString(arr));
        }


        /*第一遍排序
        第 1 轮 {101, 34, 119, 1}; => {34, 101, 119, 1}

        int insertIndex = 1 - 1; //要进行排序的值的前一个下标位置
        int insertVal = arr[1]; //插入的值
        //要插入的值在前面排好序的数组寻找插入位置
        //说明
        // 1. insertIndex >= 0 保证在给 insertVal 找插入位置，不越界
        // 2. insertVal < arr[insertIndex] 待插入的数，还没有找到插入位置
        // 3. 就需要将 arr[insertIndex] 后移
        while (insertIndex >= 0 && insertVal < arr[insertIndex]) {
            //待插入的数比排好序的数组的数据小,即排好序的数组中大的数据往后移
            arr[insertIndex + 1] = arr[insertIndex];
            insertIndex--; //后移
        }//循环结束时就找到插入位置，插入位置为 insertIndex+1
        arr[insertIndex+1] = insertVal;
        System.out.println("第 1 轮插入");
        System.out.println(Arrays.toString(arr));

         */

    }
}
```

### 希尔排序

> 简单插入排序存在的问题

数组 arr = {2,3,4,5,6,1} 这时需要插入的数 1**(****最小**), 这样的过程是： 

{2,3,4,5,6,6}   {2,3,4,5,5,6}   {2,3,4,4,5,6}   {2,3,3,4,5,6}   {2,2,3,4,5,6}   {1,2,3,4,5,6} 

**结论:**当**需要插入的数是较小的数时**，**后移的次数明显增多**，对**效率**有影响.

希尔排序是希尔（Donald Shell）于 1959 年提出的一种排序算法。希尔排序也是一种**插入排序**，它是简单插入排序经过改进之后的一个**更高效的版本**，也称为**缩小增量排序**。

希尔排序法基本思想 ：希尔排序是把记录按下标的一定增量分组，对每组使用直接插入排序算法排序；随着增量逐渐减少，每组包含的关键词越来越多，**当增量减至** **1** **时**，整个文件恰被分成一组，算法便终止

![image-20200802153620010](https://gitee.com/xudongyin/img/raw/master/img/20200802153622.png)

> 希尔排序法应用实例

有一群小牛, 考试成绩分别是 {8,9,1,7,2,3,5,4,6,0} 请从小到大排序. 请分别使用 

1) 希尔排序时， 对有序序列在插入时采用**交换法**, 并测试排序速度.  

```java
//80000数据 16秒左右
public class ShellSort {
    public static void main(String[] args) {
        int[] arr = {8, 9, 1, 7, 2, 3, 5, 4, 6, 0};
        shellSort(arr);

    }

    //希尔排序
    // 希尔排序时， 对有序序列在插入时采用交换法
    public static void shellSort(int[] arr) {

        int temp = 0;//中间变量
        int count = 0;//记录排序次数
        // 因为第 1 轮排序，是将 10 个数据分成了 5 组
        for (int gap = arr.length / 2; gap >= 1; gap /= 2) {
            for (int i = gap; i < arr.length; i++) {
                // 遍历各组中所有的元素(共 gap 组，每组有 arr.length/gap 个元素), 步长 gap
                for (int j = i-gap; j >= 0; j -= gap) {
                    // 如果当前元素大于加上步长后的那个元素，说明交换
                    if (arr[j] > arr[j + gap]) {
                        temp = arr[j + gap];
                        arr[j + gap] = arr[j];
                        arr[j] = temp;
                    }
                }
            }
            System.out.println("第"+(count++)+"次希尔排序"+ Arrays.toString(arr));
        }

        /*
        // 因为第 1 轮排序，是将 10 个数据分成了 5 组
        for (int i = 5; i < arr.length; i++) {
            // 遍历各组中所有的元素(共 5 组，每组有 2 个元素), 步长 5
            for (int j = i-5; j >= 0; j -= 5) {
                // 如果当前元素大于加上步长后的那个元素，说明交换
                if (arr[j] > arr[j + 5]) {
                    temp = arr[j + 5];
                    arr[j + 5] = arr[j];
                    arr[j] = temp;
                }
            }
        }
        System.out.println("第一轮希尔排序后"+ Arrays.toString(arr));

        // 因为第 2 轮排序，是将 10 个数据分成了 5/2 = 2 组
        for (int i = 2; i < arr.length; i++) {
            // 遍历各组中所有的元素(共 2 组，每组有 5 个元素), 步长 2
            for (int j = i-2; j >= 0; j -= 2) {
                // 如果当前元素大于加上步长后的那个元素，说明交换
                if (arr[j] > arr[j + 2]) {
                    temp = arr[j + 2];
                    arr[j + 2] = arr[j];
                    arr[j] = temp;
                }
            }
        }
        System.out.println("第二轮希尔排序后"+ Arrays.toString(arr));

         */
    }
}
```

2) 希尔排序时， 对有序序列在插入时采用**移动法**, 并测试排序速度.

```java
//80000数据 1秒左右
public class ShellSort {
    public static void main(String[] args) {
        int[] arr = {8, 9, 1, 7, 2, 3, 5, 4, 6, 0};
        shellSort(arr);
        System.out.println(Arrays.toString(arr));
    }    
    //对交换式的希尔排序进行优化->移位法
    public static void shellSort(int[] arr) {
        // 增量 gap, 并逐步的缩小增量
        for (int gap = arr.length / 2; gap > 0; gap /= 2) {
            // 从第 gap 个元素，逐个对其所在的组进行直接插入排序
            for (int i = gap; i < arr.length; i++) {
                int j = i;
                int temp = arr[j];
                if (arr[j] < arr[j - gap]) {//只要插入的值比前一个步长距离的值要小就得进行排序
                    while (j - gap >= 0 && temp < arr[j - gap]) { //只要值比要插入的值大，就往后移
                        //移动
                        arr[j] = arr[j - gap];
                        j -= gap;
                    }//当退出 while 后，就给 temp 找到插入的位置 可以用 gap=1 来验证
                    arr[j] = temp;

                }
            }
        }
    }
}
```

### 快速排序法

快速排序（Quicksort）是对**冒泡排序**的一种改进。基本思想是：通过一趟排序将要排序的数据分割成独立的两部分，其中一部分的所有数据都比另外一部分的所有数据都要小，然后再按此方法对这两部分数据分别进行快速排序，**整个排序过程可以递归进行**，以此达到整个数据变成有序序列 

![image-20200802181127257](https://gitee.com/xudongyin/img/raw/master/img/20200802181128.png)

![image-20200802181153999](https://gitee.com/xudongyin/img/raw/master/img/20200802181155.png)

要求: 对 [-9,78,0,23,-567,70] 进行从小到大的排序，要求使用快速排序法。【测试 8w 和 800w】 

说明[验证分析]: 

1) 如果取消左右递归，结果是 -9 -567 0 23 78 70

2) 如果取消右递归,结果是 -567 -9 0 23 78 70 

3) 如果取消左递归,结果是 -9 -567 0 23 70 78

```java
//效率比希尔快一点
package Sort;

import java.util.Arrays;

public class QuickSort {
    public static void main(String[] args) {
        int[] arr = {-9,78,0,23,-567,70};
        System.out.println(Arrays.toString(arr));
        quickSort(arr, 0, arr.length-1);

    }

    public static void quickSort(int[] arr, int left, int right) {
        int l = left; //左下标
        int r = right; //右下标
        int pivot = arr[(l + r) / 2];//中轴值
        int temp = 0;//临时变量
        //while循环的目的是让比pivot 值小放到左边
        //比pivot 值大放到右边
        while (l < r) { //中轴值也可能会被进行数据位置交换
            //在pivot的左边一直找,找到大于等于pivot值,才退出循环
            while (arr[l] < pivot) {
                l += 1;
            }
            //在pivot的右边一直找,找到小于等于pivot值,才退出循环
            while (arr[r] > pivot) {
                r -= 1;
            }
            //如果l >= r说明pivot 的左右两边的值，已经按照左边全部是
            //小于等于pivot值，右边全部是大于等于pivot值排好
            if( l >= r) {
                break;
            }
            //交换
            temp = arr[l];
            arr[l] = arr[r];
            arr[r] = temp;
            //如果交换完后，发现这个arr[l] == pivot值 相等（也就是中轴值进行了位置交换） r--， 前移
            if(arr[l] == pivot) {
                r -= 1;
            }
            //如果交换完后，发现这个arr[r] == pivot值 相等（也就是中轴值进行了位置交换） l++， 后移
            if(arr[r] == pivot) {
                l += 1;
            }
            System.out.println(Arrays.toString(arr));
        }
        // 如果 l == r, 必须l++, r--, 否则为出现栈溢出
        if (l == r) {
            l += 1;
            r -= 1;
        }
        //向左递归
        if (left < r) {
            System.out.println("向左递归,末点："+r);
            quickSort(arr, left, r);
        }
        //向右递归
        if (right > l) {
            System.out.println("向右递归,起点："+l);
            quickSort(arr, l, right);
        }

    }
}
```

### 归并排序 

归并排序（MERGE-SORT）是利用归并的思想实现的排序方法，该算法采用经典的**分治（divide-and-conquer）策略**（分治法将问题分(divide)成一些**小的问题然后递归求解**，而治(conquer)的阶段则将分的阶段得到的各答案"修补"在一起，即分而治之)。

![image-20200803160702590](https://gitee.com/xudongyin/img/raw/master/img/20200803160705.png)

再来看看治阶段，我们需要将两个已经有序的子序列合并成一个有序序列，比如上图中的**最后一次合并**，要将 [4,5,7,8]和[1,2,3,6]两个已经有序的子序列，合并为最终序列[1,2,3,4,5,6,7,8]，来看下实现步骤 

![image-20200803160743227](https://gitee.com/xudongyin/img/raw/master/img/20200803160744.png)

```java
//效率跟快速排序差不多
package Sort;

import java.util.Arrays;

public class MergeSort {
    public static void main(String[] args) {
        int[]  arr= { 8, 4, 5, 7, 1, 3, 6, 2 };
        int[] temp = new int[arr.length + 1];
        mergeSort(arr, 0, arr.length - 1, temp);
        System.out.println(Arrays.toString(arr));
    }

    //分+合
    public static void mergeSort(int[] arr, int left, int right, int[] temp) {
        if (left < right) {
            int mid = (right + left) / 2;//中间索引
            mergeSort(arr, left, mid, temp);//向左递归进行分解
            mergeSort(arr, mid+1, right, temp);//向右递归进行分解
            merge(arr, left, mid, right, temp);//合并
        }
    }

    //合并方法
    /**
     *
     * @param arr 排序的原始数组
     * @param left 左边有序序列的初始索引
     * @param mid 中间索引
     * @param right 右边索引
     * @param temp 做中转的数组
     */
    public static void merge(int[] arr, int left, int mid, int right, int[] temp) {
        int i = left; //左边有序序列的初始下标
        int j = mid + 1;//右边有序序列的初始下标
        int t = 0;//指向temp数组的当前索引

        //(一)
        //先把左右两边(有序)的数据按照规则填充到temp数组
        //直到左右两边的有序序列，有一边处理完毕为止
        while (i <= mid && j <= right) {
            //如果左边的有序序列的当前元素，小于等于右边有序序列的当前元素
            //即将左边的当前元素，填充到 temp数组
            //然后 t++, i++
            if (arr[i] < arr[j]) {
                temp[t] = arr[i];
                i += 1;
                t += 1;
            } else { //右边的元素小于等于左边的元素
                temp[t] = arr[j];
                j += 1;
                t += 1;
            }
        }
        //(二)
        //把有剩余数据的一边的数据依次全部填充到temp
        while (i <= mid) { //左边的有序序列还有剩余的元素，就全部填充到temp
            temp[t] = arr[i];
            t += 1;
            i += 1;
        }
        while (j <= right) { //右边的有序序列还有剩余的元素，就全部填充到temp
            temp[t] = arr[j];
            t += 1;
            j += 1;
        }

        //(三)
        //将temp数组的元素拷贝到arr
        //注意，并不是每次都拷贝所有
        t = 0; //重新指向temp数组的第一个元素
        int tempLeft = left;
        System.out.println("tempLeft = "+tempLeft+"  right ="+right);
        //第一次合并 tempLeft = 0 , right = 1 //  tempLeft = 2  right = 3 // tempLeft=0 right=3
        //最后一次 tempLeft = 0  right = 7
        while (tempLeft <= right) {
            arr[tempLeft] = temp[t];
            t += 1;
            tempLeft += 1;
        }

    }

}
```

### 基数排序 

> 基数排序(桶排序)介绍: 

1) 基数排序（radix sort）属于“分配式排序”（distribution sort），又称“桶子法”（bucket sort）或 bin sort，顾名思义，它是通过键值的各个位的值，将要排序的元素分配至某些“桶”中，达到排序的作用 

2) 基数排序法是属于稳定性的排序，基数排序法的是效率高的**稳定性**排序法 

3) 基数排序(Radix Sort)是桶排序的扩展 

4) 基数排序是 1887 年赫尔曼·何乐礼发明的。它是这样实现的：将整数按位数切割成不同的数字，然后按每个位数分别比较。 

> 基数排序基本思想 

1) 将所有待比较数值统一为同样的数位长度，数位较短的数前面补零。然后，从最低位开始，依次进行一次排序。这样从最低位排序一直到最高位排序完成以后, 数列就变成一个有序序列。

2) 这样说明，比较难理解，下面我们看一个图文解释，理解基数排序的步骤 

![image-20200803172927814](https://gitee.com/xudongyin/img/raw/master/img/20200803172929.png)
![image-20200803172939443](https://gitee.com/xudongyin/img/raw/master/img/20200803173107.png)
![image-20200803172953931](https://gitee.com/xudongyin/img/raw/master/img/20200803173121.png)

~~~java
package Sort;

import java.util.Arrays;

public class RadixSort {
    public static void main(String[] args) {
        int arr[] = {53, 3, 542, 748, 14, 214};
        radixSort(arr);
    }

    //基数排序
    public static void radixSort(int[] arr) {
        //得到数组中最大的数据的位数
        int max = arr[0];//假设第一数就是最大数
        for (int i = 0; i < arr.length; i++) {
            if (arr[i] > max) {
                max = arr[i];
            }
        }
        //得到最大数据位数
        int maxLength = (max + "").length();

        //定义一个二维数组，表示10个桶, 每个桶就是一个一维数组
        //说明
        //1. 二维数组包含10个一维数组
        //2. 为了防止在放入数的时候，数据溢出，则每个一维数组(桶)，大小定为arr.length
        //3. 很明确，基数排序是使用空间换时间的经典算法
        int[][] bucket = new int[10][arr.length];
        //为了记录每个桶中，实际存放了多少个数据,我们定义一个一维数组来记录各个桶的每次放入的数据个数
        //可以这里理解
        //比如：bucketElementCounts[0] , 记录的就是  bucket[0] 桶的放入数据个数
        int[] bucketElementCounts = new int[10];

        for (int j = 0, n = 1; j < maxLength; j++, n *= 10) { //根据最大数的位数进行相对应次数的处理
            for (int i = 0; i < arr.length; i++) {
                //取出每个元素的个位值
                int digitOfElement = arr[i] / n % 10;
                //放入到对应的桶中,第 digitOfElement 个桶，第 bucketElementCounts[digitOfElement] 个位置
                bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[i];
                bucketElementCounts[digitOfElement]++; //对应桶数据个数加一，也就是桶下标后移
            }
            //按照这个桶的顺序(一维数组的下标依次取出数据，放入原来数组)
            int index = 0;
            //遍历每一桶，并将桶中是数据，放入到原数组
            for (int i = 0; i < bucket.length; i++) { //遍历每个桶
                if (bucketElementCounts[i] != 0) { //桶有数据
                    //循环该桶即第k个桶(即第k个一维数组), 放入
                    for (int k = 0; k < bucketElementCounts[i]; k++) {
                        //取出元素放入到arr
                        arr[index++] = bucket[i][k];
                    }
                }
                //第1轮处理后，需要将每个 bucketElementCounts[i] = 0 ！！！！
                //也就是把对应的桶所记录的长度为0，这样在后面的排序中可以把之前桶里的数据进行覆盖,相当于把指针前置
                // bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[i];
                bucketElementCounts[i] = 0;
            }
            System.out.println("第"+(j+1)+"轮，对个位的排序处理 arr =" + Arrays.toString(arr));
        }


        /*
        //第一轮
        for (int i = 0; i < arr.length; i++) {
            //取出每个元素的个位值
            int digitOfElement = arr[i]/1 % 10;
            //放入到对应的桶中,第 digitOfElement 个桶，第 bucketElementCounts[digitOfElement] 个位置
            bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[i];
            bucketElementCounts[digitOfElement]++; //对应桶数据个数加一，也就是桶下标后移
        }
        //按照这个桶的顺序(一维数组的下标依次取出数据，放入原来数组)
        int index = 0;
        //遍历每一桶，并将桶中是数据，放入到原数组
        for (int i = 0; i < bucket.length; i++) { //遍历每个桶
            if (bucketElementCounts[i] != 0) { //桶有数据
                //循环该桶即第k个桶(即第k个一维数组), 放入
                for (int k = 0; k < bucketElementCounts[i]; k++) {
                    //取出元素放入到arr
                    arr[index++] = bucket[i][k];
                }
            }
            //第1轮处理后，需要将每个 bucketElementCounts[i] = 0 ！！！！
            //也就是把对应的桶所记录的长度为0，这样在后面的排序中可以把之前桶里的数据进行覆盖,相当于把指针前置
            // bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[i];
            bucketElementCounts[i] = 0;
        }
        System.out.println("第1轮，对个位的排序处理 arr =" + Arrays.toString(arr));

        //第二轮
        for (int i = 0; i < arr.length; i++) {
            //取出每个元素的十位值
            int digitOfElement = arr[i]/10% 10;
            //放入到对应的桶中,第 digitOfElement 个桶，第 bucketElementCounts[digitOfElement] 个位置
            bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[i];
            bucketElementCounts[digitOfElement]++; //对应桶数据个数加一，也就是桶下标后移
        }
        //按照这个桶的顺序(一维数组的下标依次取出数据，放入原来数组)
        index = 0;
        //遍历每一桶，并将桶中是数据，放入到原数组
        for (int i = 0; i < bucket.length; i++) { //遍历每个桶
            if (bucketElementCounts[i] != 0) { //桶有数据
                //循环该桶即第k个桶(即第k个一维数组), 放入
                for (int k = 0; k < bucketElementCounts[i]; k++) {
                    //取出元素放入到arr
                    arr[index++] = bucket[i][k];
                }
            }
            bucketElementCounts[i] = 0;
        }
        System.out.println("第2轮，对十位的排序处理 arr =" + Arrays.toString(arr));

        //第三轮
        for (int i = 0; i < arr.length; i++) {
            //取出每个元素的百位值
            int digitOfElement = arr[i]/100% 10;
            //放入到对应的桶中,第 digitOfElement 个桶，第 bucketElementCounts[digitOfElement] 个位置
            bucket[digitOfElement][bucketElementCounts[digitOfElement]] = arr[i];
            bucketElementCounts[digitOfElement]++; //对应桶数据个数加一，也就是桶下标后移
        }
        //按照这个桶的顺序(一维数组的下标依次取出数据，放入原来数组)
        index = 0;
        //遍历每一桶，并将桶中是数据，放入到原数组
        for (int i = 0; i < bucket.length; i++) { //遍历每个桶
            if (bucketElementCounts[i] != 0) { //桶有数据
                //循环该桶即第k个桶(即第k个一维数组), 放入
                for (int k = 0; k < bucketElementCounts[i]; k++) {
                    //取出元素放入到arr
                    arr[index++] = bucket[i][k];
                }
            }
            bucketElementCounts[i] = 0;
        }
        System.out.println("第3轮，对百位的排序处理 arr =" + Arrays.toString(arr));

         */
    }
}
~~~

**基数排序的说明:** 

1) 基数排序是对传统桶排序的扩展，速度很快. 

2) 基数排序是经典的空间换时间的方式，占用内存很大, 当对海量数据排序时，容易造成 OutOfMemoryError 。 

3) 基数排序时稳定的。[注:假定在待排序的记录序列中，存在多个具有相同的关键字的记录，若经过排序，这些记录的相对次序保持不变，即在原序列中，r[i]=r[j]，且 r[i]在 r[j]之前，而在排序后的序列中，r[i]仍在 r[j]之前， 则称这种排序算法是稳定的；否则称为不稳定的] 

4) 有负数的数组，我们不用基数排序来进行排序, 如果要支持负数，参考: https://code.i-harness.com/zh-CN/q/e98fa9

### 常用排序算法总结和对比 

![image-20200803231523345](https://gitee.com/xudongyin/img/raw/master/img/20200803231525.png)

**相关术语解释：** 

1) 稳定：如果 a 原本在 b 前面，而 a=b，排序之后 a 仍然在 b 的前面； 

2) 不稳定：如果 a 原本在 b 的前面，而 a=b，排序之后 a 可能会出现在 b 的后面； 

3) 内排序：所有排序操作都在内存中完成； 

4) 外排序：由于数据太大，因此把数据放在磁盘中，而排序通过磁盘和内存的数据传输才能进行； 

5) 时间复杂度： 一个算法执行所耗费的时间。 

6) 空间复杂度：运行完一个程序所需内存的大小。 

7) n: 数据规模 

8) k: “桶”的个数 

9) In-place: 不占用额外内存 

10) Out-place: 占用额外内存

## 查找算法 

在 java 中，我们常用的查找有四种: 

-  顺序(线性)查找 
-  二分查找/折半查找
-  插值查找 
-  斐波那契查找

### 线性查找

有一个数列： {1,8, 10, 89, 1000, 1234} ，判断数列中是否包含此名称【顺序查找】 要求: 如果找到了，就提示找到，并给出下标值

~~~java
public class SeqSearch {

	public static void main(String[] args) {
		int arr[] = { 1, 9, 11, -1, 34, 89 };// 没有顺序的数组
		int index = seqSearch(arr, -11);
		if(index == -1) {
			System.out.println("没有找到");
		} else {
			System.out.println("找到，下标为=" + index);
		}
	}
	
	 //这里我们实现的线性查找是找到一个满足条件的值，就返回
	public static int seqSearch(int[] arr, int value) {
		// 线性查找是逐一比对，发现有相同值，就返回下标
		for (int i = 0; i < arr.length; i++) {
			if(arr[i] == value) {
				return i;
			}
		}
		return -1;
	}
}
~~~

### 二分查找 

请对一个**有序数组**进行二分查找 {1,8, 10, 89, 1000, 1234} ，输入一个数看看该数组是否存在此数，并且求出下标，如果没有就提示"没有这个数"。

![image-20200803232214003](https://gitee.com/xudongyin/img/raw/master/img/20200803232215.png)

```java
package Search;

import java.util.ArrayList;
import java.util.List;

public class BinarySearch {
    public static void main(String[] args) {
        int arr[] = { 1, 8, 10, 89,1000,1000,1000,1234};
    //    int index = binarySearch(arr, 0, arr.length, -1);
     //   if (index != -1) {
     //       System.out.println("在数组的第"+(index+1)+"位");
      //  }else System.out.println("找不到");

        List<Integer> list = binarySearch2(arr, 0, arr.length - 1, 1000);
        System.out.println(list);

    }

    //二分查找，前提是有序数组
    /**
     *
     * @param arr  要查找的数组
     * @param left  左边索引
     * @param right  右边索引
     * @param findVal 要查找的值
     * @return  返回索引，找到返回对应的下标，找不到返回-1
     */
    public static int binarySearch(int[] arr, int left,int right,int findVal) {
        if (left > right||findVal<arr[0]||findVal>arr[arr.length-1]) { // 当 left > right 时，说明递归整个数组，但是没有找到
            return -1;
        }
        int mid = (left + right) / 2; //中间索引
        if (findVal > arr[mid]) {
            return binarySearch(arr, mid + 1, right, findVal);//右递归
        } else if (findVal < arr[mid]) {
            return binarySearch(arr, left, mid - 1, findVal);//左递归
        } else { //找到值,findVal = arr[mid]
            return mid;
        }
    }
    //完成一个课后思考题:
    /*
     * 课后思考题： {1,8, 10, 89, 1000, 1000，1234} 当一个有序数组中，
     * 有多个相同的数值时，如何将所有的数值都查找到，比如这里的 1000
     *
     * 思路分析
     * 1. 在找到mid 索引值，不要马上返回
     * 2. 向mid 索引值的左边扫描，将所有满足 1000， 的元素的下标，加入到集合ArrayList
     * 3. 向mid 索引值的右边扫描，将所有满足 1000， 的元素的下标，加入到集合ArrayList
     * 4. 将Arraylist返回
     */
    public static List<Integer> binarySearch2(int[] arr, int left, int right, int findVal) {
        if (left > right||findVal<arr[0]||findVal>arr[arr.length-1]) { // 当 left > right 时，说明递归整个数组，但是没有找到
            return new ArrayList<>(); //下标集合为空
        }
        int mid = (left + right) / 2; //中间索引
        if (findVal > arr[mid]) {
            return binarySearch2(arr, mid + 1, right, findVal);//右递归
        } else if (findVal < arr[mid]) {
            return binarySearch2(arr, left, mid - 1, findVal);//左递归
        } else { //找到值,findVal = arr[mid]
            List<Integer> resIndexList = new ArrayList<>(); //用于存放下标
            int temp = mid - 1;
            while (true) {  //扫描左边的
                if (temp < left || arr[temp] != findVal) {
                    break;//扫描完左边或者值不相等，进入下次循环
                }
                resIndexList.add(temp);
                temp--; //左移
            }
            resIndexList.add(mid);
            temp = mid +1;
            while (true) {  //扫描数组右边
                if (temp > right || arr[temp] != findVal) {
                    break;//扫描完右边或者值不相等，进入下次循环
                }
                resIndexList.add(temp);
                temp++; //右移
            }
            return resIndexList;
        }
    }
}
```

### 插值查找

1) 插值查找原理介绍: 

插值查找算法类似于二分查找，不同的是插值查找每次从自适应 mid 处开始查找。 

2) 将折半查找中的求 mid 索引的公式 , low 表示左边索引 left, high 表示右边索引 right。

key 就是前面我们讲的 findVal 

![image-20200804111716679](https://gitee.com/xudongyin/img/raw/master/img/20200804111718.png)

3) int mid = low + (high - low) * (key - arr[low]) / (arr[high] - arr[low]) ;     //插值索引

对应前面的代码公式： 

int mid = left + (right – left) * (findVal – arr[left]) / (arr[right] – arr[left]) 

4) **举例说明插值查找算法** 1-100 的数组 

![image-20200804111809025](https://gitee.com/xudongyin/img/raw/master/img/20200804111810.png)

```java
package Search;

public class InsertValueSearch {
    public static void main(String[] args) {

//    int [] arr = new int[100];
//    for(int i = 0; i < 100; i++) {
//       arr[i] = i + 1;
//    }

        int arr[] = { 1, 8, 10, 89,1000,1000, 1234 };

      //  int index = insertValueSearch(arr, 0, arr.length - 1, 1);
        int index = binarySearch(arr, 0, arr.length-1, 1);
        System.out.println("index = " + index);

    }
     //二分查找
    public static int binarySearch(int[] arr, int left,int right,int findVal) {
        System.out.println("二分查找算法~~~");
        if (left > right||findVal<arr[0]||findVal>arr[arr.length-1]) { // 当 left > right 时，说明递归整个数组，但是没有找到
            return -1;
        }
        int mid = (left + right) / 2; //中间索引
        if (findVal > arr[mid]) {
            return binarySearch(arr, mid + 1, right, findVal);//右递归
        } else if (findVal < arr[mid]) {
            return binarySearch(arr, left, mid - 1, findVal);//左递归
        } else { //找到值,findVal = arr[mid]
            return mid;
        }
    }

    //插值查找，前提有序数组
    public static int insertValueSearch(int[] arr, int left, int right, int findVal) {
        System.out.println("插值查找算法~~~");
        //注意：findVal < arr[0]  和  findVal > arr[arr.length - 1] 必须需要
        //否则我们得到的 mid 可能越界
        if (left > right || findVal > arr[arr.length - 1] || findVal < arr[0]) {
            return -1;
        }
        // 求出mid, 自适应
        int mid = left + (right - left) * (findVal - arr[left]) / (arr[right] - arr[left]);
        int midVal = arr[mid];
        if (findVal > midVal) { // 说明应该向右边递归
            return insertValueSearch(arr, mid + 1, right, findVal);
        } else if (findVal < midVal) { // 说明向左递归查找
            return insertValueSearch(arr, left, mid - 1, findVal);
        } else {
            return mid;
        }
    }
}
```

**插值查找注意事项：** 

1) 对于数据量较大，关键字分布比较均匀的查找表来说，采用插值查找, 速度较快. 

2) 关键字分布不均匀的情况下，该方法不一定比折半查找要好 

### 斐波那契(黄金分割法)查找

> 斐波那契(黄金分割法)查找基本介绍

1) 黄金分割点是指把一条线段分割为两部分，使其中一部分与全长之比等于另一部分与这部分之比。取其前三位数字的近似值是 0.618。由于按此比例设计的造型十分美丽，因此称为黄金分割，也称为中外比。这是一个神奇的数字，会带来意向不大的效果。 

2) 斐波那契数列 {1, 1, 2, 3, 5, 8, 13, 21, 34, 55 } 发现斐波那契数列的两个相邻数的比例，无限接近 黄金分割值 0.618 

> 斐波那契(黄金分割法)原理

斐波那契查找原理与前两种相似，仅仅改变了中间结点（mid）的位置，mid 不再是中间或插值得到，而是位于黄金分割点附近，即 mid=low+F(k-1)-1（F 代表斐波那契数列），如下图所示

![image-20200804121035526](https://gitee.com/xudongyin/img/raw/master/img/20200804121036.png)

 **对** **F(k-1)-1** **的理解**： 

1) 由斐波那契数列 F[k]=F[k-1]+F[k-2] 的性质，可以得到 （F[k]-1）=（F[k-1]-1）+（F[k-2]-1）+1 。该式说明： 只要顺序表的长度为 F[k]-1，则可以将该表分成长度为 F[k-1]-1 和 F[k-2]-1 的两段，即如上图所示。从而中间位置为 mid=low+F(k-1)-1 

2) 类似的，每一子段也可以用相同的方式分割 

3) 但顺序表长度 n 不一定刚好等于 F[k]-1，所以需要将原来的顺序表长度 n 增加至 F[k]-1。这里的 k 值只要能使得 F[k]-1 恰好大于或等于 n 即可，由以下代码得到,顺序表长度增加后，新增的位置（从 n+1 到 F[k]-1 位置）， 都赋为 n 位置的值即可。 

~~~java
while(n>fib(k)-1) 
k++; 
~~~

```java
package Search;

import java.util.Arrays;

public class FibonacciSearch {
    public static int maxSize = 20;
    public static void main(String[] args) {
        int [] arr = {1,2,8, 10, 89, 1000, 1234,};
        System.out.println("index=" + fibonacciSearch(arr, 2));
    }

    //因为后面我们mid=low+F(k-1)-1，需要使用到斐波那契数列，
    // 因此我们需要先获取到一个斐波那契数列
    //非递归方法得到一个斐波那契数列
    public static int[] fib() {
        int[] f = new int[maxSize];
        f[0] = 1;
        f[1] = 1;
        for (int i = 2; i < f.length; i++) {
            f[i] = f[i - 1] + f[i - 2];
        }
        return f;
    }
    //编写斐波那契查找算法
    //使用非递归的方式编写算法

    /**
     *
     * @param arr 被查找的数组
     * @param findVal 要查找的值
     * @return 找到就返回下标，没有就返回-1
     */
    public static int fibonacciSearch(int[] arr,int findVal) {
        int low = 0;
        int high = arr.length - 1;
        int k = 0; //表示斐波那契分割数值的下标
        int mid = 0; //存放mid值
        int f[] = fib(); //获取到斐波那契数列
        //获取到斐波那契分割数值的下标,因为是下标所以-1
        while (high > f[k] - 1) {
            k++;
        }//在斐波那契数组中找到第一个比数组长度大的值的下标
        //因为 f[k] 值 可能大于 a 的 长度，因此我们需要使用Arrays类，构造一个新的数组，并指向temp[]
        //不足的部分会使用0填充
        int[] temp = Arrays.copyOf(arr, f[k]);
        //实际上需求使用a数组最后的数填充 temp
        //举例:
        //temp = {1,8, 10, 89, 1000, 1234, 0, 0}  => {1,8, 10, 89, 1000, 1234, 1234, 1234,}
        for (int i = high + 1; i < temp.length; i++) {
            temp[i] = arr[high];
        }
        // 使用while来循环处理，找到我们的数 key
        while (low <= high) { // 只要这个条件满足，就可以找
            mid = low + f[k - 1] - 1;
            if (findVal < temp[mid]) {
                high = mid - 1;
                //为什么是 k--
                //说明
                //1. 全部元素 = 前面的元素 + 后边元素
                //2. f[k] = f[k-1] + f[k-2]
                //因为 前面有 f[k-1]个元素,所以可以继续拆分 f[k-1] = f[k-2] + f[k-3]
                //即 在 f[k-1] 的前面继续查找 k--
                //即下次循环 mid = f[k-1-1]-1
                k--;
            } else if (findVal > temp[mid]) {
                low = mid + 1;
                //为什么是k -=2
                //说明
                //1. 全部元素 = 前面的元素 + 后边元素
                //2. f[k] = f[k-1] + f[k-2]
                //3. 因为后面我们有f[k-2] 所以可以继续拆分 f[k-2] = f[k-3] + f[k-4]
                //4. 即在f[k-2] 的前面进行查找 k -=2
                //5. 即下次循环 mid = f[k - 1 - 2] - 1
                k -= 2;
            } else {//找到
                //需要确定，返回的是哪个下标
                if (mid <= high) {
                    return mid;
                } else {
                    return high;
                }
            }
        }
        return -1;
    }
}
```



## 哈希表 

散列表（Hash table，也叫哈希表），是根据关键码值(Key value)而直接进行访问的数据结构。也就是说，它通过把关键码值映射到表中一个位置来访问记录，以加快查找的速度。这个映射函数叫做散列函数，存放记录的数组叫做散列表。

![image-20200806150911344](https://gitee.com/xudongyin/img/raw/master/img/20200806150913.png)
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200806150925.png" alt="image-20200806150921912" style="zoom:50%;" />

> google 公司的一个上机题

有一个公司,当有新的员工来报道时,要求将该员工的信息加入(id,性别,年龄,名字,住址..),当输入该员工的 id 时, 要求查找到该员工的 所有信息. 

**要求**: 

1) 不使用数据库,,速度越快越好=>哈希表(散列) 

2) 添加时，保证按照 id 从低到高插入 [课后思考：**如果** **id** **不是从低到高插入**，但要求各条链表仍是从低到高，怎么解决?] 

3) 使用链表来实现哈希表, 该链表不带表头[即: 链表的第一个结点就存放雇员信息] 

4) 思路分析并画出示意图 

![image-20200806151030680](https://gitee.com/xudongyin/img/raw/master/img/20200806151032.png)

```java
package Hash;

import java.util.Scanner;

public class HashTableDemo {
    public static void main(String[] args) {
        //创建哈希表
        HashTab hashTab = new HashTab(7);
        String key;
        Scanner scanner = new Scanner(System.in);
        while (true) {
            System.out.println("add:  添加雇员");
            System.out.println("list: 显示雇员");
            System.out.println("find: 查找雇员");
            System.out.println("exit: 退出系统");
            key = scanner.next();
            switch (key) {
                case "add":
                    System.out.println("请输入雇员id");
                    int id = scanner.nextInt();
                    System.out.println("请输入雇员名字");
                    String name = scanner.next();
                    //创建 雇员
                    Emp emp = new Emp(id, name);
                    hashTab.add(emp);
                    break;
                case "list":
                    hashTab.list();
                    break;
                case "find":
                    System.out.println("请输入要查找的id");
                    id = scanner.nextInt();
                    hashTab.findEmpById(id);
                    break;
                case "exit":
                    scanner.close();
                    System.exit(0);
                default:
                    break;
            }
        }
    }
}

class Emp {
    public int id;
    public String name;
    public Emp next; //next 默认为 null
    public Emp(int id,String name) {
        this.id = id;
        this.name = name;
    }
}
//创建EmpLinkedList ,表示链表
class EmpLinkedList {
    //头指针，执行第一个Emp,因此我们这个链表的head 是直接指向第一个Emp
    private Emp head;

    //添加雇员到链表
    //说明
    //1. 假定，当添加雇员时，id 是自增长，即id的分配总是从小到大
    //   因此我们将该雇员直接加入到本链表的最后即可
    public void add(Emp emp) {
        //如果是第一个雇员
        if (head == null) {
            head = emp;
            return;
        }
        //如果不是第一个雇员，则使用一个辅助的指针，帮助定位到最后
        Emp curEmp = head;
        while (true) {
            if (curEmp.next == null) {//说明到链表最后
                break;
            }
            curEmp = curEmp.next;
        }
        curEmp.next = emp;
    }

    //遍历表的雇员信息
    public void list(int no) {
        if (head == null) {
            System.out.println("第"+(no+1)+"条链表为空");
            return;
        }
        Emp curEmp = head;
        //辅助指针
        System.out.print("第"+(no+1)+"条链表信息：");
        while(true) {
            System.out.printf(" => id=%d name=%s\t", curEmp.id, curEmp.name);
            if(curEmp.next == null) {//说明curEmp已经是最后结点
                break;
            }
            curEmp = curEmp.next; //后移，遍历
        }
        System.out.println();
    }
    //根据id查找雇员
    //如果查找到，就返回Emp, 如果没有找到，就返回null
    public Emp findEmpById(int id) {
        if (head == null) {
            System.out.println("链表为空");
            return null;
        }
        Emp curEmp = head;
        while (true) {
            if (curEmp.id == id) {//找到
               break;//这时curEmp就指向要查找的雇员
            }
            if (curEmp.next == null) {//说明遍历当前链表没有找到该雇员
                curEmp = null;
                break;
            }
            curEmp = curEmp.next;//后移
        }
        return curEmp;
    }
}
//创建HashTab 管理多条链表
class HashTab {
    private EmpLinkedList[] empLinkedListArray;
    private int size;

    public HashTab(int size) {
        this.size = size;
        //初始化empLinkedListArray
        empLinkedListArray = new EmpLinkedList[size];
        //？留一个坑, 这时要不要分别初始化每个链表
        for (int i = 0; i < size; i++) {
            empLinkedListArray[i] = new EmpLinkedList();
        }
    }
    //添加雇员
    public void add(Emp emp) {
        //根据员工的id ,得到该员工应当添加到哪条链表
        int hashFun = hashFun(emp.id);
        //将emp 添加到对应的链表中
        empLinkedListArray[hashFun].add(emp);
    }
    //遍历所有的链表,遍历hashtab
    public void list() {
        for(int i = 0; i < size; i++) {
            empLinkedListArray[i].list(i);
        }
    }
    //编写散列函数, 使用一个简单取模法
    public int hashFun(int id) {
        return id % size;
    }

    public void findEmpById(int id) {
        //使用散列函数确定到哪条链表查找
        int hashFun = hashFun(id);
        Emp emp = empLinkedListArray[hashFun].findEmpById(id);
        if (emp != null) {
            System.out.printf("在第%d条链表找到雇员 id=%d\n",hashFun+1,id);
        }else{
            System.out.println("在哈希表中，没有找到该雇员~");
        }
    }
}
```



## 树

> 为什么需要树这种数据结构 

1) 数组存储方式的分析 

**优点**：通过**下标方式访问元素**，速度快。对于有序数组，还可使用**二分查找**提高检索速度。 

**缺点**：如果要检索具体某个值，或者**插入值(按一定顺序)会整体移动**，效率较低 [示意图] 

**画出操作示意图**： 
![image-20200806162519115](https://gitee.com/xudongyin/img/raw/master/img/20200806162521.png)

2) 链式存储方式的分析 

**优点**：在一定程度上对数组存储方式有优化(比如：**插入**一个数值节点，只需要将插入节点，链接到链表中即可， **删除**效率也很好)。 

**缺点**：在进行**检索时**，效率仍然较低，比如(检索某个值，需要从头节点开始遍历) 【示意图】 

**操作示意图**：
![image-20200806162552252](https://gitee.com/xudongyin/img/raw/master/img/20200806162553.png)

3) **树存储**方式的分析 

能提高数据**存储，读取**的效率, 比如利用 **二叉排序树**(Binary Sort Tree)，既可以保证数据的检索速度，同时也 

可以保证数据的**插入，删除，修改**的速度。【示意图,后面详讲】 

**案例:** [7, 3, 10, 1, 5, 9, 12]
![image-20200806162630228](https://gitee.com/xudongyin/img/raw/master/img/20200806162632.png)

### 二叉树

> 二叉树的概念

1) 树有很多种，每个节点**最多只能有两个子节点**的一种形式称为二叉树。 

2) 二叉树的子节点分为左节点和右节点 

3) 示意图 
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200806171105.png" alt="image-20200806171103923" style="zoom:67%;" />

4) 如果该二叉树的所有**叶子节点**都在**最后一层**，并且结点总数= **2^n -1** , n 为层数，则我们称为满二叉树。
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200806171114.png" alt="image-20200806171113154" style="zoom:67%;" /> 

5) 如果该二叉树的所有**叶子节点**都在**最后一层**或者**倒数第二层**，而且最后一层的叶子节点在左边连续，倒数第二 层的叶子节点在右边连续，我们称为完全二叉树
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200806171130.png" alt="image-20200806171128985" style="zoom:67%;" />

二叉树遍历的说明 

使用**前序，中序和后序**对下面的二叉树进行遍历. 

1) 前序遍历: 先输出父节点，再遍历左子树和右子树 

2) 中序遍历: 先遍历左子树，再输出父节点，再遍历右子树

3) 后序遍历: 先遍历左子树，再遍历右子树，最后输出父节点 

4) **小结:** 看输出父节点的顺序，就确定是前序，中序还是后序

![image-20200806171244035](https://gitee.com/xudongyin/img/raw/master/img/20200806171249.png)

二叉树-查找指定节点 

**要求** 

1) 请编写前序查找，中序查找和后序查找的方法。 

2) 并分别使用三种查找方式，查找 heroNO = 5 的节点 

3) 并分析各种查找方式，分别比较了多少次 

4) 思路分析图解 
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200806180101.png" alt="image-20200806180100005"  />

二叉树-删除节点 

 要求 

1) 如果删除的节点是叶子节点，则删除该节点 

2) 如果删除的节点是非叶子节点，则删除该子树.

3) 测试，删除掉 5 号叶子节点 和 3 号子树. 

4) 完成删除思路分析
![image-20200806180155785](https://gitee.com/xudongyin/img/raw/master/img/20200806180159.png)

```java
package Tree;

public class BinaryTreeDemo {
    public static void main(String[] args) {
        //先需要创建一颗二叉树
        BinaryTree binaryTree = new BinaryTree();
        //创建需要的结点
        HeroNode root = new HeroNode(1, "宋江");
        HeroNode node2 = new HeroNode(2, "吴用");
        HeroNode node3 = new HeroNode(3, "卢俊义");
        HeroNode node4 = new HeroNode(4, "林冲");
        HeroNode node5 = new HeroNode(5, "关胜");
        //说明，我们先手动创建该二叉树，后面我们学习递归的方式创建二叉树
        root.setLeft(node2);
        root.setRight(node3);
        node3.setRight(node4);
        node3.setLeft(node5);
        binaryTree.setRoot(root);
        //测试
//    System.out.println("前序遍历"); // 1,2,3,5,4
//    binaryTree.preOrder();

        //测试
//    System.out.println("中序遍历");
//    binaryTree.infixOrder(); // 2,1,5,3,4

//    System.out.println("后序遍历");
//    binaryTree.postOrder(); // 2,5,4,3,1

//        HeroNode node = binaryTree.postOrderSearch(5);
//        if (node != null) {
//            System.out.printf("编号为%d，名字为%s\n", node.getNo(), node.getName());
//        } else {
//            System.out.println("编号为"+5+"的英雄不存在");
//        }
        //测试删除
        System.out.println("删除前,前序遍历");
        binaryTree.preOrder(); //  1,2,3,5,4
        binaryTree.delNode(6);
        //binaryTree.delNode(3);
        System.out.println("删除后，前序遍历");
        binaryTree.preOrder(); // 1,2,3,4
    }
}

//二叉树
class BinaryTree {
    private HeroNode root;

    public void setRoot(HeroNode root) {
        this.root = root;
    }

    //前序遍历
    public void preOrder() {
        if (root != null) {
            root.preOrder();
        } else {
            System.out.println("树为空");
        }
    }
    //中序遍历
    public void infixOrder() {
        if (root != null) {
            root.infixOrder();
        } else {
            System.out.println("树为空");
        }
    }
    //后序遍历
    public void postOrder() {
        if (root != null) {
            root.postOrder();
        } else {
            System.out.println("树为空");
        }
    }

    //前序遍历查找
    public HeroNode preOrderSearch(int no) {
        if (root != null) {
            return root.preOrderSearch(no);
        } else {
            return null;
        }
    }
    //中序遍历查找
    public HeroNode infixOrderSearch(int no) {
        if (root != null) {
            return root.infixOrderSearch(no);
        } else {
            return null;
        }
    }
    //后序遍历查找
    public HeroNode postOrderSearch(int no) {
        if (root != null) {
            return root.postOrderSearch(no);
        } else {
            return null;
        }
    }
    //删除结点
    public void delNode(int no) {
        if (root != null) {
            if (root.getNo() == no) {
                root = null;
            } else {
                root.delNode(no);
            }
        } else {
            System.out.println("空树，不能删除~");
        }
    }
}
//节点
class HeroNode {
    private int  no;
    private String name;
    private HeroNode left;
    private HeroNode right;

    public HeroNode(int no, String name) {
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

    public int getNo() {
        return no;
    }

    public void setNo(int no) {
        this.no = no;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public HeroNode getLeft() {
        return left;
    }

    public void setLeft(HeroNode left) {
        this.left = left;
    }

    public HeroNode getRight() {
        return right;
    }

    public void setRight(HeroNode right) {
        this.right = right;
    }

    //前序遍历
    public void preOrder() {
        System.out.println(this);//先输出父结点
        //递归向左子树前序遍历
        if (left != null) {
            left.preOrder();
        }
        //递归向右子树前序遍历
        if (right != null) {
            right.preOrder();
        }
    }

    //中序遍历
    public void infixOrder() {
        //递归向左子树中序遍历
        if (left != null) {
            left.infixOrder();
        }
        //输出父结点
        System.out.println(this);
        //递归向右子树中序遍历
        if (right != null) {
            right.infixOrder();
        }
    }

    //后序遍历
    public void postOrder() {
        if(left != null) {
            left.postOrder();
        }
        if(right != null) {
            right.postOrder();
        }
        System.out.println(this);
    }

    //前序遍历查找
    public HeroNode preOrderSearch(int no) {
        System.out.println("进入前序遍历查找");//输出次数就是节点查找次数
        //比较当前结点是不是
        if (this.no == no) {
            return this;
        }
        //1.则判断当前结点的左子节点是否为空，如果不为空，则递归前序查找
        //2.如果左递归前序查找，找到结点，则返回
        HeroNode resNode = null;
        if (left != null) {
            resNode = left.preOrderSearch(no);
        }
        if (resNode != null) {//说明我们左子树找到
            return resNode;
        }
        //1.左递归前序查找，找到结点，则返回，否继续判断，
        //2.当前的结点的右子节点是否为空，如果不空，则继续向右递归前序查找
        if (right != null) {
            resNode = right.preOrderSearch(no);
        }
        return resNode;
    }
    //中序遍历查找
    public HeroNode infixOrderSearch(int no) {
        HeroNode resNode = null;
        //判断当前结点的左子节点是否为空，如果不为空，则递归中序查找
        if (left != null) {
            resNode = left.infixOrderSearch(no);
        }
        if (resNode != null) {
            return resNode;
        }
        System.out.println("进入中序遍历查找");//输出次数就是节点查找次数
        //如果找到，则返回，如果没有找到，就和当前结点比较，如果是则返回当前结点
        if(this.no == no) {
            return this;
        }
        if (right != null) {
            resNode = right.infixOrderSearch(no);
        }
        return resNode;
    }
    //后序遍历查找
    public HeroNode postOrderSearch(int no) {
        //判断当前结点的左子节点是否为空，如果不为空，则递归后序查找
        HeroNode resNode = null;
        if(this.left != null) {
            resNode = this.left.postOrderSearch(no);
        }
        if(resNode != null) {//说明在左子树找到
            return resNode;
        }
        //如果左子树没有找到，则向右子树递归进行后序遍历查找
        if(this.right != null) {
            resNode = this.right.postOrderSearch(no);
        }
        if(resNode != null) {
            return resNode;
        }
        System.out.println("进入后序查找");//输出次数就是节点查找次数
        //如果左右子树都没有找到，就比较当前结点是不是
        if(this.no == no) {
            return this;
        }
        return resNode;
    }

    //递归删除结点
    //1.如果删除的节点是叶子节点，则删除该节点
    //2.如果删除的节点是非叶子节点，则删除该子树
    public void delNode(int no) {
        //思路
      /*
       *     1. 因为我们的二叉树是单向的，所以我们是判断当前结点的子结点是否需要删除结点，而不能去判断当前这个结点是不是需要删除结点.
         2. 如果当前结点的左子结点不为空，并且左子结点 就是要删除结点，就将this.left = null; 并且就返回(结束递归删除)
         3. 如果当前结点的右子结点不为空，并且右子结点 就是要删除结点，就将this.right= null ;并且就返回(结束递归删除)
         4. 如果第2和第3步没有删除结点，那么我们就需要向左子树进行递归删除
         5. 如果第4步也没有删除结点，则应当向右子树进行递归删除.
       */
        //2. 如果当前结点的左子结点不为空，并且左子结点 就是要删除结点，就将this.left = null; 并且就返回(结束递归删除)
        if (left != null && left.no == no) {
            left = null;
            return;
        }
        //3. 如果当前结点的右子结点不为空，并且右子结点 就是要删除结点，就将this.right= null ;并且就返回(结束递归删除)
        if (right != null && right.no == no) {
            right = null;
            return;
        }
        //4.那么我们就需要向左子树进行递归删除
        if (left != null) {
            left.delNode(no);
        }
        //5. 如果第4步也没有删除结点，则应当向右子树进行递归删除.
        if (right != null) {
            right.delNode(no);
        }
    }
}
```

### 三种遍历非递归实现

```java
package com.xu;

import java.util.Stack;

public class Test {
    public static void main(String[] args) {
        Node node1 = new Node(1);
        Node node2 = new Node(2);
        Node node3 = new Node(3);
        Node node4 = new Node(4);
        Node node5 = new Node(5);
        Node node6 = new Node(6);
        Node node7 = new Node(7);
        Node node8 = new Node(8);
        Node node9 = new Node(9);
        Node node10 = new Node(10);
        node1.setLeft(node2);
        node1.setRight(node3);
        node2.setLeft(node4);
        node2.setRight(node5);
        node3.setRight(node7);
        node3.setLeft(node6);
        node4.setLeft(node8);
        node8.setLeft(node9);
        node5.setRight(node10);
//        node5.setLeft(node10);

        preOrder(node1);
        midOrder(node1);
        postOrder(node1);

    }
    public static void preOrder(Node root){
        if (root == null) {
            System.out.println("树为空");
        }
        Stack<Node> stack = new Stack<>();
        Node temp = root;
        while (temp != null || !stack.isEmpty()) {//只要指针不为空或者栈不为空，遍历就没结束
            while (temp!=null){
                System.out.print(temp.data+" "); //先输出根结点
                stack.push(temp);//把根结点压入栈
                temp = temp.left;//遍历左子树
            }
            temp = stack.pop();//弹出上一个根结点
            temp = temp.right;//进行右子树遍历
        }
        System.out.println();
    }

    public static void midOrder(Node root) {
        if (root == null) {
            System.out.println("树为空");
        }
        Stack<Node> stack = new Stack<>();
        Node temp = root;
        while (temp != null || !stack.isEmpty()) {//只要指针不为空或者栈不为空，遍历就没结束
            while (temp!=null){
                stack.push(temp);//把根结点压入栈
                temp = temp.left;//遍历左子树
            }
            temp = stack.pop();//弹出上一个根结点
            System.out.print(temp.data+" ");//输出结点
            temp = temp.right;//遍历右子树
        }
        System.out.println();
    }

    public static void postOrder(Node root) {
        if (root == null) {
            System.out.println("树为空");
        }
        Stack<Node> stack = new Stack<>();
        Node temp = root;
        while (temp != null || !stack.isEmpty()) {//只要指针不为空或者栈不为空，遍历就没结束
            if (temp != null) {
                stack.push(temp);//把根结点压入栈
                temp.flag = 1;//标记第一次入栈
                temp = temp.left;//遍历左子树
            } else {
                temp = stack.pop();//弹出上一个根结点
                if (temp.flag == 1) {//如果是只入栈一次
                    temp.flag = 2;
                    stack.push(temp);//标记第二次入栈
                    temp = temp.right;//遍历右子树
                } else { //入栈过两次的就输出
                    System.out.print(temp.data+" ");//输出结点
                    temp = null;//访问后，赋为空，确保下次循环时执行弹栈操作
                }
            }
            
        }
    }
}
class Node{
    public Node left;
    public Node right;
    public int data;

    public int flag;
    
    public Node(int data) {
        this.data = data;
    }
    

    public void setLeft(Node left) {
        this.left = left;
    }
    
    public void setRight(Node right) {
        this.right = right;
    }
}
```

### 顺序存储二叉树 

 基本说明 

从数据存储来看，数组存储方式和树的存储方式可以相互转换，即**数组可以转换成树**，**树也可以转换成数组**， 看下面的示意图。
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200806214752.png" alt="image-20200806214738779" style="zoom:67%;" />

 要求: 

1) 右图的二叉树的结点，要求以数组的方式来存放 arr : [1, 2, 3, 4, 5, 6, 6] 

2) 要求在遍历数组 arr 时，仍然可以以前序遍历，中序遍历和后序遍历的方式完成结点的遍历 

> **顺序存储二叉树的特点** 

1) 顺序二叉树通常只考虑完全二叉树 

2) 第 n 个元素的左子节点为 2 * n + 1 

3) 第 n 个元素的右子节点为 2 * n + 2 

4) 第 n 个元素的父节点为 (n-1) / 2 

5) n : 表示二叉树中的第几个元素(按 0 开始编号，为的是与数组下标符合)

```java
package Tree;

public class ArrBinaryTreeDemo {
    public static void main(String[] args) {
        int[] arr = { 1, 2, 3, 4, 5, 6, 7 };
        //创建一个 ArrBinaryTree
        ArrBinaryTree arrBinaryTree = new ArrBinaryTree(arr);
        arrBinaryTree.preOrder(); // 1,2,4,5,3,6,7
//        arrBinaryTree.infixOrder(0);
    }
}

class ArrBinaryTree {
    private int[] arr;//存储数据结点的数组

    public ArrBinaryTree(int[] arr) {
        this.arr = arr;
    }

    //重载
    public void preOrder() {
        preOrder(0);
    }
    //前序遍历
    public void preOrder(int index) {
        if (arr == null || arr.length == 0) {
            System.out.println("数组为空，不能按照二叉树的前序遍历");
        }
        //输出当前这个元素
        System.out.println(arr[index]);
        //向左递归遍历
        if ((index * 2 + 1) < arr.length) {
            preOrder(index * 2 + 1);
        }
        //向右递归遍历
        if((index * 2 + 2) < arr.length) {
            preOrder(2 * index + 2);
        }
    }

    //中序遍历
    public void infixOrder(int index) {
        if (arr.length == 0 || arr == null) {
            System.out.println("数组为空，不能按照二叉树的前序遍历");
        }
        //向左遍历
        if ((index * 2 + 1) < arr.length) {
            infixOrder(index * 2 + 1);
        }
        //输出当前元素
        System.out.println(arr[index]);
        //向右遍历
        if ((index * 2 + 2) < arr.length) {
            infixOrder(index*2+2);
        }
    }
}
```

### 线索化二叉树 

> 先看一个问题 

将数列 {1, 3, 6, 8, 10, 14 } 构建成一颗二叉树. n+1=7 

问题分析: 

![image-20200806224942630](https://gitee.com/xudongyin/img/raw/master/img/20200806224945.png)

1) 当我们对上面的二叉树进行中序遍历时，数列为 {8, 3, 10, 1, 6, 14 } 

2) 但是 6, 8, 10, 14 这几个节点的 左右指针，并没有完全的利用上. 

3) 如果我们希望充分的利用 各个节点的左右指针， 让各个节点可以指向自己的前后节点,怎么办? 

4) 解决方案-**线索二叉**树

> 线索二叉树基本介绍

1) n 个结点的二叉链表中含有 n+1 【公式 2n-(n-1)=n+1】 个空指针域。利用二叉链表中的空指针域，存放指向**该结点**在某种遍历次序下的前驱和后继结点的指针（这种附加的指针称为"线索"） 

2) 这种加上了线索的二叉链表称为线索链表，相应的二叉树称为线索二叉树(Threaded BinaryTree)。根据线索性质的不同，线索二叉树可分为**前序线索二叉树**、**中序线索二叉树**和**后序线索二叉树**三种 

3) 一个结点的前一个结点，称为**前驱**结点 

4) 一个结点的后一个结点，称为**后继**结点 

> 线索二叉树应用案例

应用案例说明：将下面的二叉树，进行中序线索二叉树。中序遍历的数列为 {8, 3, 10, 1, 14, 6} 
![image-20200806225250766](https://gitee.com/xudongyin/img/raw/master/img/20200806225252.png)

**思路分析:** 中序遍历的结果：{8, 3, 10, 1, 14, 6}

![image-20200806225311426](https://gitee.com/xudongyin/img/raw/master/img/20200806225313.png)

 说明: 当线索化二叉树后，Node 节点的 属性 left 和 right ，有如下情况: 

1) left 指向的是左子树，也可能是指向的前驱节点. 比如 ① 节点 left 指向的左子树, 而 ⑩ 节点的 left 指向的就是前驱节点. 

2) right 指向的是右子树，也可能是指向后继节点，比如 ① 节点 right 指向的是右子树，而⑩ 节点的 right 指向的是后继节点

```JAVA
package Tree.ThreadedBinaryTree;

public class ThreadedBinaryTreeDemo {
    public static void main(String[] args) {
        //测试一把中序线索二叉树的功能
        HeroNode root = new HeroNode(1, "tom");
        HeroNode node2 = new HeroNode(3, "jack");
        HeroNode node3 = new HeroNode(6, "smith");
        HeroNode node4 = new HeroNode(8, "mary");
        HeroNode node5 = new HeroNode(10, "king");
        HeroNode node6 = new HeroNode(14, "dim");

        //二叉树，后面我们要递归创建, 现在简单处理使用手动创建
        root.setLeft(node2);
        root.setRight(node3);
        node2.setLeft(node4);
        node2.setRight(node5);
        node3.setLeft(node6);

        //测试中序线索化
        ThreadedBinaryTree threadedBinaryTree = new ThreadedBinaryTree();
        threadedBinaryTree.setRoot(root);
        threadedBinaryTree.threadedNodes(root);
        //测试: 以10号节点测试
        HeroNode leftNode = node5.getLeft();
        HeroNode rightNode = node5.getRight();
        System.out.println("10号结点的前驱结点是 =" + leftNode); //3
        System.out.println("10号结点的后继结点是=" + rightNode); //1

        threadedBinaryTree.threadedList();

    }
}

//定义ThreadedBinaryTree 实现了线索化功能的二叉树
class ThreadedBinaryTree {
    private HeroNode root;
    //为了实现线索化，需要创建指向当前结点的前驱结点的指针
    //在递归进行线索化时，pre 总是保留前一个结点
    private HeroNode pre = null;

    public void setRoot(HeroNode root) {
        this.root = root;
    }
    //编写对二叉树进行中序线索化的方法
    public void threadedNodes(HeroNode node) {
        //如果node==null, 不能线索化
        if (node == null) {
            return;
        }
        //(一)先线索化左子树
        threadedNodes(node.getLeft());

        //(二)线索化当前结点[有难度]
        //处理当前结点的前驱结点
        //以8结点来理解
        //8结点的.left = null , 8结点的.leftType = 1
        if (node.getLeft() == null) {
            //让当前结点的左指针指向前驱结点
            node.setLeft(pre);
            //修改当前结点的左指针的类型,指向前驱结点
            node.setLeftType(1);
        }
        //处理后继结点
        if (pre != null && pre.getRight() == null) {
            //让前驱结点的右指针指向当前结点
            pre.setRight(node);
            //修改前驱结点的右指针类型
            pre.setRightType(1);
        }
        //!!! 每处理一个结点后，让当前结点是下一个结点的前驱结点
        pre = node;

        //(三)在线索化右子树
        threadedNodes(node.getRight());
    }
    //遍历线索化二叉树的方法
    public void threadedList() {
        //定义一个变量，存储当前遍历的结点，从root开始
        HeroNode node = root;
        while (node != null) {
            //循环的找到leftType == 1的结点，第一个找到就是8结点
            //后面随着遍历而变化,因为当leftType==1时，说明该结点是按照线索化
            //处理后的有效结点
            while (node.getLeftType() == 0) {
                node = node.getLeft();
            }
            //打印当前这个结点
            System.out.println(node);
            //如果当前结点的右指针指向的是后继结点,就一直输出
            while (node.getRightType() == 1) {
                //获取到当前结点的后继结点
                node = node.getRight();
                System.out.println(node);
            }
            //替换这个遍历的结点
            node = node.getRight();
        }

    }
}

//先创建HeroNode 结点
class HeroNode {
    private int no;
    private String name;
    private HeroNode left; //默认null
    private HeroNode right; //默认null
    //说明
    //1. 如果leftType == 0 表示指向的是左子树, 如果 1 则表示指向前驱结点
    //2. 如果rightType == 0 表示指向是右子树, 如果 1表示指向后继结点
    private int leftType;
    private int rightType;

    public int getLeftType() {
        return leftType;
    }

    public void setLeftType(int leftType) {
        this.leftType = leftType;
    }

    public int getRightType() {
        return rightType;
    }

    public void setRightType(int rightType) {
        this.rightType = rightType;
    }

    public HeroNode(int no, String name) {
        this.no = no;
        this.name = name;
    }

    public int getNo() {
        return no;
    }

    public void setNo(int no) {
        this.no = no;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public HeroNode getLeft() {
        return left;
    }

    public void setLeft(HeroNode left) {
        this.left = left;
    }

    public HeroNode getRight() {
        return right;
    }

    public void setRight(HeroNode right) {
        this.right = right;
    }

    @Override
    public String toString() {
        return "HeroNode [no=" + no + ", name=" + name + "]";
    }

}
```

### 树结构实际应用

#### 堆排序 

> 堆排序基本介绍 

1) 堆排序是利用堆这种数据结构而设计的一种排序算法，堆排序是一种选择排序，它的最坏，最好，平均时间复杂度均为 **O(nlogn)**，它也是不稳定排序。 

2) 堆是具有以下性质的完全二叉树：每个结点的值都大于或等于其左右孩子结点的值，称为大顶堆, 注意 : 没有要求结点的左孩子的值和右孩子的值的大小关系。 

3) 每个结点的值都小于或等于其左右孩子结点的值，称为小顶堆 

4) 大顶堆举例说明 
![image-20200807171541488](https://gitee.com/xudongyin/img/raw/master/img/20200807171543.png)

5) 小顶堆举例说明 
![image-20200807171601538](https://gitee.com/xudongyin/img/raw/master/img/20200807171603.png)

6) 一般升序采用大顶堆，降序采用小顶堆

堆排序的基本思想是： 

1) 将待排序序列构造成一个大顶堆 

2) 此时，整个序列的最大值就是堆顶的根节点。 

3) 将其与末尾元素进行交换，此时末尾就为最大值。 

4) 然后将剩余 n-1 个元素重新构造成一个堆，这样会得到 n 个元素的次小值。如此反复执行，便能得到一个有序序列了。 

可以看到在构建大顶堆的过程中，元素的个数逐渐减少，最后就得到一个有序序列了.

> 堆排序步骤图解说明 

**步骤一 构造初始堆。将给定无序序列构造成一个大顶堆（一般升序采用大顶堆，降序采用小顶堆)。** 

**原始的数组 [4, 6, 8, 5, 9]** 

1) .假设给定无序序列结构如下
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200807171811.png" alt="image-20200807171809452" style="zoom: 33%;" />

2) .此时我们从最后一个非叶子结点开始（叶结点自然不用调整，第一个非叶子结点 
arr.length/2-1=5/2-1=1，也就是下面的 6 结点），从左至右，从下至上进行调整。
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200807171918.png" alt="image-20200807171915938" style="zoom: 33%;" /> 

3) .找到第二个非叶节点 4，由于[4,9,8]中 9 元素最大，4 和 9 交换。
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200807172000.png" alt="image-20200807171958470" style="zoom:33%;" />

4) 这时，交换导致了子根[4,5,6]结构混乱，继续调整，[4,5,6]中 6 最大，交换 4 和 6。 
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200807172030.png" alt="image-20200807172028122" style="zoom:33%;" />

此时，我们就将一个无序序列构造成了一个大顶堆。 

**步骤二 将堆顶元素与末尾元素进行交换，使末尾元素最大。然后继续调整堆，再将堆顶元素与末尾元素交换，得到第二大元素。如此反复进行交换、重建、交换。** 

1) .将堆顶元素 9 和末尾元素 4 进行交换 
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200807172117.png" alt="image-20200807172115570" style="zoom:33%;" />

2) .重新调整结构，使其继续满足堆定义 
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200807172134.png" alt="image-20200807172132250" style="zoom:33%;" />

3) .再将堆顶元素 8 与末尾元素 5 进行交换，得到第二大元素 8.
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200807172158.png" alt="image-20200807172156513" style="zoom:33%;" />

4) 后续过程，继续进行调整，交换，如此反复进行，最终使得整个序列有序
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200807172224.png" alt="image-20200807172222789" style="zoom:33%;" />

**再简单总结下堆排序的基本思路：** 

**1).将无序序列构建成一个堆，根据升序降序需求选择大顶堆或小顶堆;** 

**2).将堆顶元素与末尾元素交换，将最大元素"沉"到数组末端;** 

**3).重新调整结构，使其满足堆定义，然后继续交换堆顶元素与当前末尾元素，反复执行调整+交换步骤，直到整个序列有序。** 

```java
package Tree;

import java.util.Arrays;

public class HeapSort {
    public static void main(String[] args) {
        int arr[] = {4, 6, 8, 5, 9,10,-1,-999,89,2};
        heapSort(arr);
    }
    //编写一个堆排序的方法
    public static void heapSort(int[] arr) {
        int temp = 0;
        System.out.println("堆排序!!");

//    //分步完成
//    adjustHeap(arr, 1, arr.length);
//    System.out.println("第一次" + Arrays.toString(arr)); // 4, 9, 8, 5, 6
//
//    adjustHeap(arr, 0, arr.length);
//    System.out.println("第2次" + Arrays.toString(arr)); // 9,6,8,5,4

        //完成我们最终代码
        //将无序序列构建成一个堆，根据升序降序需求选择大顶堆或小顶堆
        for (int i = arr.length / 2 - 1; i >= 0; i--) {
            adjustHeap(arr, i, arr.length);
        }
        /*
       * 2).将堆顶元素与末尾元素交换，将最大元素"沉"到数组末端;
　　     3).重新调整结构，使其满足堆定义，然后继续交换堆顶元素与当前末尾元素，反复执行调整+交换步骤，直到整个序列有序。
       */
        for (int j = arr.length - 1; j>0; j--) {
            //交换
            temp = arr[j];
            arr[j] = arr[0];
            arr[0] = temp;
            adjustHeap(arr, 0, j);
        }
        System.out.println("数组=" +Arrays.toString(arr));
    }

    //将一个数组(二叉树), 调整成一个大顶堆

    /**
     * 功能： 完成 将 以 i 对应的非叶子结点的树调整成大顶堆
     * 举例  int arr[] = {4, 6, 8, 5, 9}; => i = 1 => adjustHeap => 得到 {4, 9, 8, 5, 6}
     * 如果我们再次调用  adjustHeap 传入的是 i = 0 => 得到 {4, 9, 8, 5, 6} => {9,6,8,5, 4}
     *
     * @param arr    待调整的数组
     * @param i      表示非叶子结点在数组中的索引
     * @param lenght 表示对多少个元素继续调整， length 是在逐渐的减少
     */
    public static void adjustHeap(int arr[], int i, int lenght) {
        int temp = arr[i];//先取出当前元素的值，保存在临时变量
        //开始调整
        //说明
        //1. k = i * 2 + 1 k 是 i结点的左子结点
        for (int k = i * 2 + 1; k < lenght; k = k * 2 + 1) {
            if (k + 1 < lenght && arr[k] < arr[k + 1]) {//说明左子结点的值小于右子结点的值
                k++;// k 指向右子结点
            }
            if (arr[k] > temp) {//如果子结点大于父结点
                arr[i] = arr[k];//把较大的值赋给当前结点
                i = k;//!!! i 指向 k,继续循环比较
            } else {
                break;
            }
        }
        //当for 循环结束后，我们已经将以i 为父结点的树的最大值，放在了 最顶(局部)
        arr[i] = temp;//将temp值放到调整后的位置
    }
}
```

#### 赫夫曼树 

> 基本介绍 

1) 给定 n 个权值作为 n 个叶子结点，构造一棵二叉树，**若该树的带权路径长度(wpl)达到最小**，称这样的二叉树为最优二叉树，也称为哈夫曼树(Huffman Tree), 还有的书翻译为霍夫曼树。 

2) 赫夫曼树是带权路径长度最短的树，权值较大的结点离根较近 

> 赫夫曼树几个重要概念和举例说明 

1) **路径和路径长度**：在一棵树中，从一个结点往下可以达到的孩子或孙子结点之间的通路，称为路径。通**路中分支的数目称为路径长度**。若规定根结点的层数为 1，则从根结点到第 L 层结点的路径长度为 L-1 

2) **结点的权及带权路径长度**：若将树中结点赋给一个有着某种含义的数值，则这个数值称为该结点的权。**结点的带权路径长度为**：从根结点到该结点之间的路径长度与该结点的权的乘积 

3) 树的带权路径长度：树的带权路径长度规定为**所有叶子结点的带权路径长度之和**，记为 WPL(weighted path length) ,权值越大的结点离根结点越近的二叉树才是最优二叉树。 

4) WPL 最小的就是赫夫曼树 
![image-20200807215054697](https://gitee.com/xudongyin/img/raw/master/img/20200807215056.png)

> 赫夫曼树创建思路图解 

给你一个数列 {13, 7, 8, 3, 29, 6, 1}，要求转成一颗赫夫曼树. 

**构成赫夫曼树的步骤**： 

1) 从小到大进行排序, 将每一个数据，每个数据都是一个节点 ， 每个节点可以看成是一颗最简单的二叉树 

2) 取出根节点权值最小的两颗二叉树 

3) 组成一颗新的二叉树, 该新的二叉树的根节点的权值是前面两颗二叉树根节点权值的和

4) 再将这颗新的二叉树，以根节点的权值大小 再次排序， 不断重复 1-2-3-4 的步骤，直到数列中，所有的数据都被处理，就得到一颗赫夫曼树 

5) 图解:
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200807215217.png" alt="image-20200807215215753" style="zoom:67%;" />

```java
package Tree.Huffman;

import java.util.ArrayList;
import java.util.Collections;

public class HuffmanTree {
    public static void main(String[] args) {
        int arr[] = { 13, 7, 8, 3, 29, 6, 1 };
        Node root = createHuffmanTree(arr);

        //测试一把
        preOrder(root); //
    }

    //前序遍历哈夫曼树
    public static void preOrder(Node node) {
        if (node != null) {
            node.preOrder();
        } else {
            System.out.println("树为空");
        }
    }

    // 创建赫夫曼树的方法
    /**
     *
     * @param arr 需要创建成哈夫曼树的数组
     * @return 创建好后的赫夫曼树的root结点
     */
    public static Node createHuffmanTree(int[] arr) {
        // 第一步为了操作方便
        // 1. 遍历 arr 数组
        // 2. 将arr的每个元素构成成一个Node
        // 3. 将Node 放入到ArrayList中
        ArrayList<Node> nodes = new ArrayList<>();
        for (int i = 0; i < arr.length; i++) {
            nodes.add(new Node(arr[i]));
        }
        while (nodes.size() > 1) {
            //排序 从小到大
            Collections.sort(nodes);
            //取出根节点权值最小的两颗二叉树
            //(1) 取出权值最小的结点（二叉树）
            Node leftNode = nodes.get(0);
            //(2) 取出权值第二小的结点（二叉树）
            Node rightNode = nodes.get(1);
            //(3)构建一颗新的二叉树
            Node parent = new Node(leftNode.value + rightNode.value);
            parent.left = leftNode;
            parent.right = rightNode;

            //(4)从ArrayList删除处理过的二叉树
            nodes.remove(leftNode);
            nodes.remove(rightNode);
            //(5)将parent加入到nodes
            nodes.add(parent);
        }
        //返回哈夫曼树的root结点
        return nodes.get(0);
    }
}

// 创建结点类
// 为了让Node 对象持续排序Collections集合排序
// 让Node 实现Comparable接口
class Node implements Comparable<Node>{
    public int value;// 结点权值
    public Node left;// 指向左子结点
    public Node right;// 指向右子结点

    public Node(int value) {
        this.value = value;
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }

    @Override
    public int compareTo(Node o) {
        // 表示从小到大排序
        return this.value-o.value;
    }

    //前序遍历
    public void preOrder() {
        System.out.println(this);
        if (left != null) {
            left.preOrder();
        }
        if (right != null) {
            right.preOrder();
        }
    }
}
```

#### 赫夫曼编码 

> 基本介绍 

1) 赫夫曼编码也翻译为哈夫曼编码(Huffman Coding)，又称霍夫曼编码，是一种编码方式, 属于一种程序算法 

2) 赫夫曼编码是赫哈夫曼树在电讯通信中的经典的应用之一。 

3) 赫夫曼编码广泛地用于数据文件压缩。其压缩率通常在 20%～90%之间 

4) 赫夫曼码是可变字长编码(VLC)的一种。Huffman 于 1952 年提出一种编码方法，称之为最佳编码 

11.3.2 原理剖析 

 通信领域中信息的处理方式 1-定长编码 
![image-20200807230535335](https://gitee.com/xudongyin/img/raw/master/img/20200807230537.png)

 通信领域中信息的处理方式 2-变长编码
![image-20200807230612852](https://gitee.com/xudongyin/img/raw/master/img/20200807230615.png)

 通信领域中信息的处理方式 3-赫夫曼编码

传输的字符串 

1) i like like like java do you like a java 

2) d:1 y:1 u:1 j:2 v:2 o:2 l:4 k:4 e:4 i:5 a:5  :9 // 各个字符对应的个数 

3) 按照上面字符出现的次数构建一颗赫夫曼树, 次数作为权值 

构成赫夫曼树的步骤： 

​		1) 从小到大进行排序, 将每一个数据，每个数据都是一个节点 ， 每个节点可以看成是一颗最简单			的二叉树 

​		2) 取出根节点权值最小的两颗二叉树 

​		3) 组成一颗新的二叉树, 该新的二叉树的根节点的权值是前面两颗二叉树根节点权值的和 

​		4) 再将这颗新的二叉树，以根节点的权值大小 再次排序， 不断重复 1-2-3-4 的步骤，直到数列			中，所有的数据都被处理， 就得到一颗赫夫曼树
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200807231444.png" alt="image-20200807231442198" style="zoom:80%;" />

4) 根据赫夫曼树，给各个字符,规定编码 (前缀编码)， 向左的路径为 0 向右的路径为 1 ， 编码 

如下: 

o: 1000  u: 10010 d: 100110 y: 100111 i: 101  a : 110  k: 1110  e: 1111  j: 0000  v: 0001  l: 001  : 01 

5) 按照上面的赫夫曼编码，我们的"i like like like java do you like a java" 

字符串对应的编码为 (注意这里我们使用的无损压缩) 

**101**01**001**101111011110100110111101111010011011110111101000011000011100110011110000110 

01111000100100100110111101111011100100001100001110 通过赫夫曼编码处理 长度为 133 

6） 长度为 ： 133 

说明: 原来长度是 359 , 压缩了 (359-133) / 359 = 62.9%

此编码满足前缀编码, 即字符的编码都不能是其他字符编码的前缀。不会造成匹配的多义性 

 注意事项 

注意, 这个赫夫曼树根据**排序方法不同**，也可能不太一样，这样对应的**赫夫曼编码也不完全一样**，但是 **wpl** 是一样的，都是最小的, 最后生成的赫夫曼编码的长度是一样，比如: 如果我们让每次生成的新的二叉树总是排在权值相同的二叉树的最后一个，则生成的二叉树为
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200807231554.png" alt="image-20200807231552488" style="zoom:80%;" />

```java
package Tree.Huffman.HuffmanCode;

import java.io.*;
import java.util.*;

public class HuffmanCodeDemo {
    public static void main(String[] args) {
//        //测试压缩
//        zipFile("c://1.txt", "c://1.zip");
//        System.out.println("压缩成功");
//
//        //测试解压
//        unZipFile("c://1.zip","c://2.txt");
//        System.out.println("解压成功");

        
        String content = "i like like like java do you like a java";
        byte[] contentBytes = content.getBytes();
        System.out.println(contentBytes.length); //40

        //测试一把，创建的赫夫曼树
//        List<Node> nodes = getNodes(contentBytes);
//        System.out.println(nodes);
//        Node root = createHuffmanTree(nodes);
//        preOrder(root);
        //测试一把是否生成了对应的赫夫曼编码
//        Map<Byte, String> huffmanCodes = getHuffmanCodes(root);
//        System.out.println(huffmanCodes);
//        byte[] bytes = zip(contentBytes, huffmanCodes);
//        System.out.println(Arrays.toString(bytes));

        //测试编码封装方法
        byte[] huffmanZip = huffmanZip(contentBytes);
        System.out.println(Arrays.toString(huffmanZip));
        //测试解码
        byte[] bytes = decode(huffmanCodes, huffmanZip);
        System.out.println("原来的字符串="+new String(bytes));


    }
    //编写一个方法，完成对压缩文件的解压
    /**
     *
     * @param zipFile 准备解压的文件
     * @param dstFile 将文件解压到哪个路径
     */
    public static void unZipFile(String zipFile, String dstFile) {
        InputStream is = null;//定义文件输入流
        ObjectInputStream ois = null;//定义一个对象输入流
        OutputStream os = null;//定义文件的输出流
        try {
            //创建文件输入流
            is = new FileInputStream(zipFile);
            //创建一个和  is关联的对象输入流
            ois = new ObjectInputStream(is);
            //读取byte数组  huffmanBytes
            byte[] huffmanBytes = (byte[]) ois.readObject();
            //读取哈夫曼编码表
            Map<Byte, String> huffmanCodes = (Map<Byte, String>) ois.readObject();
            //解码
            byte[] bytes = decode(huffmanCodes, huffmanBytes);
            //将bytes 数组写入到目标文件
            os = new FileOutputStream(dstFile);
            os.write(bytes);

        } catch (Exception e) {
            System.out.println(e.getMessage());
        } finally {
            try {
                os.close();
                ois.close();
                is.close();
            } catch (IOException e) {
                System.out.println(e.getMessage());
            }
        }

    }
    //编写方法，将一个文件进行压缩
    /**
     * @param srcFile 你传入的希望压缩的文件的全路径
     * @param dstFile 我们压缩后将压缩文件放到哪个目录
     */
    public static void zipFile(String srcFile, String dstFile) {
        //文件的输入流
        InputStream is = null;
        //文件输出流
        OutputStream os = null;
        ObjectOutputStream oos = null;
        try {
            is = new FileInputStream(srcFile);
            //创建一个和源文件大小一样的byte[]
            byte[] bytes = new byte[is.available()];
            //读取文件
            is.read(bytes);
            //直接对源文件压缩
            byte[] huffmanBytes = huffmanZip(bytes);
            //创建文件的输出流, 存放压缩文件
            os = new FileOutputStream(dstFile);
            //创建一个和文件输出流关联的ObjectOutputStream
            oos = new ObjectOutputStream(os);
            //把 赫夫曼编码后的字节数组写入压缩文件
            oos.writeObject(huffmanBytes);
            //这里我们以对象流的方式写入 赫夫曼编码，是为了以后我们恢复源文件时使用
            //注意一定要把赫夫曼编码 写入压缩文件
            oos.writeObject(huffmanCodes);

        } catch (IOException e) {
            System.out.println(e.getMessage());
        } finally {
            try {
                oos.close();
                os.close();
                is.close();
            } catch (IOException e) {
                System.out.println(e.getMessage());
            }
        }
    }
    //完成数据的解压
    //思路
    //1. 将huffmanCodeBytes [-88, -65, -56, -65, -56, -65, -55, 77, -57, 6, -24, -14, -117, -4, -60, -90, 28]
    //   首先先转成 赫夫曼编码对应的二进制的字符串 "1010100010111..."
    //2.  赫夫曼编码对应的二进制的字符串 "1010100010111..." =》 对照 赫夫曼编码  =》 "i like like like java do you like a java"

    //编写一个方法，完成对压缩数据的解码

    /**
     * @param huffmanCodes 赫夫曼编码表 map
     * @param huffmanBytes 赫夫曼编码得到的字节数组
     * @return 就是原来的字符串对应的数组
     */
    private static byte[] decode(Map<Byte, String> huffmanCodes, byte[] huffmanBytes) {
        StringBuilder stringBuilder = new StringBuilder();
        //将byte数组转成二进制的字符串
        for (int i = 0; i < huffmanBytes.length; i++) {
            //判断是不是最后一个字节
            boolean flag = (i == huffmanBytes.length - 1);
            byte b = huffmanBytes[i];
            String s = byteToBitString(!flag, b);//最后一位不用补高位，无论是否为正数,因为最后一位的编码可能不够8位
            stringBuilder.append(s);
        }
        //把字符串按照指定的赫夫曼编码进行解码
        //把赫夫曼编码表进行调换，因为反向查询 a->100 100->a
        Map<String, Byte> map = new HashMap<>();
        for (Map.Entry<Byte, String> entry : huffmanCodes.entrySet()) {
            map.put(entry.getValue(), entry.getKey());
        }
        //创建集合，存放byte
        ArrayList<Byte> list = new ArrayList<>();
        for (int i = 0; i < stringBuilder.length(); ) {
            int count = 1;
            boolean flag = true;
            Byte b = null;
            while (flag) {
                //1010100010111...
                //递增的取出 key
                String key = stringBuilder.substring(i, i + count);//i 不动，让count移动，指定匹配到一个字符
                b = map.get(key);
                if (b == null) {//说明没有匹配到
                    count++;
                } else {
                    //匹配到
                    flag = false;
                }
            }
            list.add(b);
            i += count;//i 直接移动到 count
        }
        //当for循环结束后，我们list中就存放了所有的字符  "i like like like java do you like a java"
        //把list 中的数据放入到byte[] 并返回
        byte[] bytes = new byte[list.size()];
        for (int i = 0; i < list.size(); i++) {
            bytes[i] = list.get(i);
        }
        return bytes;
    }

    /**
     * 将一个byte 转成一个二进制的字符串, 如果看不懂，可以参考我讲的Java基础 二进制的原码，反码，补码
     *
     * @param b    传入的 byte
     * @param flag 标志是否需要补高位如果是true ，表示需要补高位，如果是false表示不补, 如果是最后一个字节，无需补高位
     * @return 是该 b 对应的二进制的字符串，（注意是按补码返回）
     */
    private static String byteToBitString(boolean flag, byte b) {
        int temp = (int) b;//将 b 转成 int
        //如果是正数我们还存在补高位
        if (flag) {
            temp |= 256; //按位或 256  1 0000 0000  | 0000 0001 => 1 0000 0001
        }
        String binaryString = Integer.toBinaryString(temp);//返回的是temp对应的二进制的补码
        if (flag) {
            return binaryString.substring(binaryString.length() - 8);
        } else {
            return binaryString;
        }

    }
    
    //使用一个方法，将前面的方法封装起来，便于我们的调用.
    /**
     * @param bytes 原始要处理的字节数组
     * @return 哈夫曼编码后的字节数组
     */
    public static byte[] huffmanZip(byte[] bytes) {
        List<Node> nodes = getNodes(bytes);
        Node huffmanTreeRoot = createHuffmanTree(nodes);
        Map<Byte, String> huffmanCodes = getHuffmanCodes(huffmanTreeRoot);
        byte[] zip = zip(bytes, huffmanCodes);
        return zip;
    }

    //编写一个方法，将字符串对应的byte[] 数组，通过生成的赫夫曼编码表，返回一个赫夫曼编码 压缩后的byte[]

    /**
     * @param bytes        这时原始的字符串对应的 byte[]
     * @param huffmanCodes 生成的赫夫曼编码map
     * @return 返回赫夫曼编码处理后的 byte[]
     * 举例： String content = "i like like like java do you like a java"; =》 byte[] contentBytes = content.getBytes();
     * 返回的是 字符串 "1010100010111111110010001011111111001000101111111100100101001101110001110000011011101000111100101000101111111100110001001010011011100"
     * => 对应的 byte[] huffmanCodeBytes  ，即 8位对应一个 byte,放入到 huffmanCodeBytes
     * huffmanCodeBytes[0] =  10101000(补码) => byte  [推导  10101000=> 10101000 - 1 => 10100111(反码)=> 11011000= -88 ]
     * huffmanCodeBytes[1] = -88
     */
    public static byte[] zip(byte[] bytes, Map<Byte, String> huffmanCodes) {
        //1.利用 huffmanCodes 将  bytes 转成  赫夫曼编码对应的字符串
        StringBuilder stringBuilder = new StringBuilder();
        for (byte b : bytes) {
            stringBuilder.append(huffmanCodes.get(b));//将对应的字符的编码取出来
        }
        //将 "1010100010111111110..." 转成 byte[]

        //统计返回  byte[] huffmanCodeBytes 长度
        //一句话 int len = (stringBuilder.length() + 7) / 8;
        int len;
        if (stringBuilder.length() % 8 == 0) {
            len = stringBuilder.length() / 8;
        } else {
            len = stringBuilder.length() / 8 + 1;
        }
        //创建 存储压缩后的 byte数组
        byte[] huffmanCodeBytes = new byte[len];
        int index = 0;//记录是第几个byte
        //将哈夫曼编码转换为byte数组
        for (int i = 0; i < stringBuilder.length(); i += 8) {
            String strbyte;
            if (i + 8 > stringBuilder.length()) {//如果最后一段编码不足8个
                strbyte = stringBuilder.substring(i);
            } else {
                strbyte = stringBuilder.substring(i, i + 8);
            }
            //将strByte 转成一个byte,放入到 huffmanCodeBytes
            huffmanCodeBytes[index] = (byte) Integer.parseInt(strbyte, 2);
            index++;
        }
        return huffmanCodeBytes;
    }

    //生成赫夫曼树对应的赫夫曼编码
    //思路:
    //1. 将赫夫曼编码表存放在 Map<Byte,String> 形式
    //   生成的赫夫曼编码表{32=01, 97=100, 100=11000, 117=11001, 101=1110, 118=11011, 105=101, 121=11010, 106=0010, 107=1111, 108=000, 111=0011}
    static Map<Byte, String> huffmanCodes = new HashMap<>();
    //2. 在生成赫夫曼编码表示，需要去拼接路径, 定义一个StringBuilder 存储某个叶子结点的路径
    static StringBuilder stringBuilder = new StringBuilder();

    //为了调用方便，我们重载 getCodes
    public static Map<Byte, String> getHuffmanCodes(Node root) {
        if (root == null) {
            return null;
        }
        //处理root的左子树
        getCodes(root.left, "0", stringBuilder);
        //处理root的右子树
        getCodes(root.right, "1", stringBuilder);
        return huffmanCodes;
    }

    /**
     * 功能：将传入的node结点的所有叶子结点的赫夫曼编码得到，并放入到huffmanCodes集合
     *
     * @param node          传入结点
     * @param code          路径： 左子结点是 0, 右子结点 1
     * @param stringBuilder 用于拼接路径
     */
    public static void getCodes(Node node, String code, StringBuilder stringBuilder) {
        StringBuilder stringBuilder2 = new StringBuilder(stringBuilder);
        //将code 加入到 stringBuilder2
        stringBuilder2.append(code);
        if (node != null) {//如果node == null不处理
            //判断当前node 是叶子结点还是非叶子结点
            if (node.data == null) {//非叶子结点
                //递归处理
                //向左递归
                getCodes(node.left, "0", stringBuilder2);
                //向右递归
                getCodes(node.right, "1", stringBuilder2);
            } else { //叶子结点
                huffmanCodes.put(node.data, stringBuilder2.toString());
            }
        }
    }

    //前序遍历的方法
    public static void preOrder(Node root) {
        if (root == null) {
            System.out.println("树为空");
        } else {
            root.preOrder();
        }
    }

    //可以通过List 创建对应的赫夫曼树
    public static Node createHuffmanTree(List<Node> nodes) {
        while (nodes.size() > 1) {
            //排序, 从小到大
            Collections.sort(nodes);
            //取出第一颗最小的二叉树
            Node leftNode = nodes.get(0);
            //取出第二颗最小的二叉树
            Node rightNode = nodes.get(1);
            //创建一颗新的二叉树,它的根节点 没有data, 只有权值
            Node parent = new Node(leftNode.weight + rightNode.weight, null);
            parent.left = leftNode;
            parent.right = rightNode;

            //将已经处理的两颗二叉树从nodes删除
            nodes.remove(leftNode);
            nodes.remove(rightNode);
            //将新的二叉树，加入到nodes
            nodes.add(parent);
        }
        //nodes 最后的结点，就是赫夫曼树的根结点
        return nodes.get(0);
    }

    /**
     * @param bytes 接收字节数组
     * @return 返回的就是 List 形式   [Node[date=97 ,weight = 5], Node[date=32,weight = 9]......],
     */
    public static List<Node> getNodes(byte[] bytes) {
        //1创建一个ArrayList
        ArrayList<Node> nodes = new ArrayList<Node>();
        //遍历 bytes , 统计 每一个byte出现的次数->map[key,value]
        HashMap<Byte, Integer> map = new HashMap<Byte, Integer>();
        for (byte b : bytes) {
            Integer count = map.get(b);
            if (count == null) {// Map还没有这个字符数据,第一次
                map.put(b, 1);
            } else {
                map.put(b, count + 1);
            }
        }
        for (Map.Entry<Byte, Integer> entry : map.entrySet()) {
            nodes.add(new Node(entry.getValue(), entry.getKey()));
        }
        return nodes;
    }
}

class Node implements Comparable<Node> {
    public int weight; //权值, 表示字符出现的次数
    public Byte data;// 存放数据(字符)本身，比如'a' => 97 ' ' => 32
    public Node left;
    public Node right;

    public Node(int weight, Byte data) {
        this.weight = weight;
        this.data = data;
    }

    @Override
    public String toString() {
        return "Node{" +
                "weight=" + weight +
                ", data=" + data +
                '}';
    }

    @Override
    public int compareTo(Node o) {
        return this.weight - o.weight;//从小到大排序
    }

    //前序遍历
    public void preOrder() {
        System.out.println(this);
        if (left != null) {
            left.preOrder();
        }
        if (right != null) {
            right.preOrder();
        }
    }
}
```

#### 二叉排序树 

> 先看一个需求 

给你一个数列 (7, 3, 10, 12, 5, 1, 9)，要求能够高效的完成对数据的查询和添加 

> 解决方案分析

 使用数组 

数组未排序， 优点：直接在数组尾添加，速度快。 缺点：查找速度慢. 

数组排序，优点：可以使用二分查找，查找速度快，缺点：为了保证数组有序，在添加新数据时，找到插入位置后，后面的数据需整体移动，速度慢。

 使用链式存储-链表 

不管链表是否有序，查找速度都慢，添加数据速度比数组快，不需要数据整体移动。 

 使用二叉排序树 

> 二叉排序树介绍 

**二叉排序树**：BST: (Binary Sort(Search) Tree), 对于二叉排序树的**任何一个非叶子节点**，要求**左子节点的值比当前节点的值小**，**右子节点的值比当前节点的值大**。 

**特别说明**：如果有相同的值，可以将该节点放在左子节点或右子节点 

比如针对前面的数据 (7, 3, 10, 12, 5, 1, 9) ，对应的二叉排序树为：
![image-20200808194004065](https://gitee.com/xudongyin/img/raw/master/img/20200808194006.png)

**二叉排序树创建和遍历** 

一个数组创建成对应的二叉排序树，并使用中序遍历二叉排序树，比如: 数组为 Array(7, 3, 10, 12, 5, 1, 9) ， 创建成对应的二叉排序树为 :
![image-20200808194148881](https://gitee.com/xudongyin/img/raw/master/img/20200808194151.png)

**二叉排序树的删除** 

二叉排序树的删除情况比较复杂，有下面三种情况需要考虑 

1) **删除叶子节点** (比如：2, 5, 9, 12) 

2) 删除**只有一颗子树的节点** (比如：1) 

3) 删除**有两颗子树的节点**. (比如：7, 3，10 ) 

4) 操作的思路分析
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200808224503.png" alt="image-20200808224501750" style="zoom: 67%;" />

~~~java
//对删除结点的各种情况的思路分析: 
第一种情况: 删除叶子节点 (比如：2, 5, 9, 12) 
    思路(1) 需求先去找到要删除的结点 targetNode 
        (2) 找到 targetNode 的 父结点 parent 
        (3) 确定 targetNode 是 parent 的左子结点 还是右子结点 
        (4) 根据前面的情况来对应删除 
        左子结点 parent.left = null 
        右子结点 parent.right = null; 

第二种情况: 删除只有一颗子树的节点 比如 1 
    思路(1) 需求先去找到要删除的结点 targetNode 
        (2) 找到 targetNode 的 父结点 parent 
        (3) 确定 targetNode 的子结点是左子结点还是右子结点 
        (4) targetNode 是 parent 的左子结点还是右子结点 
        (5) 如果 targetNode 有左子结点 
            5. 1 如果 targetNode 是 parent 的左子结点
                 parent.left = targetNode.left;
            5.2 如果 targetNode 是 parent 的右子结点 
                 parent.right = targetNode.left;
	   (6) 如果 targetNode 有右子结点 
            6.1 如果 targetNode 是 parent 的左子结点 
            parent.left = targetNode.right; 
            6.2 如果 targetNode 是 parent 的右子结点 
            parent.right = targetNode.right
                
情况三 ： 删除有两颗子树的节点. (比如：7, 3，10 ) 
	思路
        (1) 需求先去找到要删除的结点 targetNode 
        (2) 找到 targetNode 的 父结点 parent 
        (3) 从 targetNode 的右子树找到最小的结点 
        (4) 用一个临时变量，将 最小结点的值保存 temp = 11 
        (5) 删除该最小结点 
        (6) targetNode.value = temp 
~~~

```java
package Tree.BinarySortTree;

public class BinarySortTreeDemo {
    public static void main(String[] args) {
        int[] arr = {7, 3, 10, 12, 5, 1, 9, 2};
        BinarySortTree binarySortTree = new BinarySortTree();
        //循环的添加结点到二叉排序树
        for (int i = 0; i < arr.length; i++) {
            binarySortTree.add(new Node(arr[i]));
        }
        //中序遍历二叉排序树
        System.out.println("中序遍历二叉排序树~");
        binarySortTree.infixOrder(); // 1,2, 3, 5, 7, 9, 10, 12

        binarySortTree.delNode(1);
        binarySortTree.delNode(7);
        binarySortTree.delNode(2);
        System.out.println("删除结点后");
        binarySortTree.infixOrder();
    }
}

//创建二叉排序树
class BinarySortTree {
    private Node root;

    //添加结点的方法
    public void add(Node node) {
        if (root == null) {
            root = node;//如果root为空则直接让root指向node
        } else {
            root.add(node);
        }
    }

    //中序遍历
    public void infixOrder() {
        if (root == null) {
            System.out.println("树为空");
        } else {
            root.infixOrder();
        }
    }

    //找到要删除的结点
    public Node search(int value) {
        if (root == null) {
            return null;
        } else {
            return root.search(value);
        }
    }

    //找到要删除的结点的父结点
    public Node searchParent(int value) {
        if (root == null) {
            return null;
        } else {
            return root.searchParent(value);
        }
    }

    //编写方法:
    //1. 返回的 以node 为根结点的二叉排序树的最小结点的值
    //2. 删除node 为根结点的二叉排序树的最小结点

    /**
     * @param node 传入的结点(当做二叉排序树的根结点)
     * @return 返回的 以node 为根结点的二叉排序树的最小结点的值
     */
    public int delRightTreeMin(Node node) {
        Node target = node;
        //循环的查找左子节点，就会找到最小值
        while (target.left != null) {
            target = target.left;
        }
        //这时 target就指向了最小结点
        //删除最小结点
        delNode(target.value);
        return target.value;
    }

    //删除结点
    public void delNode(int value) {
        if (root == null) {
            return;
        }
        //1.需求先去找到要删除的结点  targetNode
        Node targetNode = search(value);
        if (targetNode == null) {
            return;
        }
        //如果我们发现当前这颗二叉排序树只有一个结点
        if (root.left == null && root.right == null) {
            root = null;
            return;
        }
        //去找到targetNode的父结点
        Node parent = searchParent(value);
        //如果要删除的结点是叶子结点
        if (targetNode.right == null && targetNode.left == null) {
            //判断targetNode 是父结点的左子结点，还是右子结点
            if (parent.left != null && parent.left.value == value) {//是左子结点
                parent.left = null;
            } else if (parent.right != null && value == parent.right.value) {//是右子节点
                parent.right = null;
            }
        } else if (targetNode.left != null && targetNode.right != null) {//删除有两颗子树的节点
            int minVal = delRightTreeMin(targetNode.right);
            targetNode.value = minVal;//删除右子树最小结点，然后把最小结点值赋给targetNode
        } else {// 删除只有一颗子树的结点
            if (targetNode.left != null) {//只有左子树
                if (parent != null) {
                    //如果 targetNode 是 parent 的左子结点，parent.left != null避免空指针异常
                    if (parent.left != null && parent.left.value == targetNode.value) {
                        parent.left = targetNode.left;
                    } else {//  targetNode 是 parent 的右子结点
                        parent.right = targetNode.left;
                    }
                } else {//没有父结点,也就是树只有root结点和一个子节点
                    root = targetNode.left;
                }
            } else {//只有右子树
                if (parent != null) {
                    //如果 targetNode 是 parent 的左子结点
                    if (parent.left != null && parent.left.value == targetNode.value) {
                        parent.left = targetNode.right;
                    } else {//如果 targetNode 是 parent 的右子结点
                        parent.right = targetNode.right;
                    }
                } else {//没有父结点,也就是树只有root结点和一个子节点
                    root = targetNode.right;
                }
            }

        }

    }
}

class Node {
    int value;
    Node left;
    Node right;

    public Node(int value) {
        this.value = value;
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }
    //查找要删除的结点

    /**
     * @param value 希望删除的结点的值
     * @return 如果找到返回该结点，否则返回null
     */
    public Node search(int value) {
        if (value == this.value) {//刚好是要找的结点
            return this;
        } else if (value < this.value) {//如果查找的值小于当前结点，向左子树递归查找
            if (this.left == null) {//如果左子结点为空
                return null;
            }
            return this.left.search(value);
        } else {//如果查找的值不小于当前结点，向右子树递归查找
            if (this.right == null) {
                return null;
            }
            return this.right.search(value);
        }
    }
    //查找要删除结点的父结点

    /**
     * @param value 要找到的结点的值
     * @return 返回的是要删除的结点的父结点，如果没有就返回null
     */
    public Node searchParent(int value) {
        //如果当前结点就是要删除的结点的父结点，就返回
        if ((this.left != null && this.left.value == value) ||
                (this.right != null && this.right.value == value)) {
            return this;
        } else {
            //如果查找的值小于当前结点的值, 并且当前结点的左子结点不为空
            if (this.left != null && value < this.value) {
                return left.searchParent(value);//向左子树递归查找
            } else if (this.right != null && value >= this.value) {
                return right.searchParent(value);//向右子树递归查找
            } else {//当前结点不是要找的结点，并且左右子结点都为空
                return null;// 没有找到父结点
            }
        }
    }

    //添加节点
    public void add(Node node) {
        if (node == null) {
            return;
        }
        //判断传入的结点的值，和当前子树的根结点的值关系
        if (node.value < this.value) {
            if (left == null) {//如果当前结点左子结点为null
                left = node;
            } else {
                //递归的向左子树添加
                left.add(node);
            }
        } else {//添加的结点的值大于等于 当前结点的值
            if (right == null) {
                right = node;
            } else {
                //递归的向右子树添加
                right.add(node);
            }
        }
    }

    //中序遍历
    public void infixOrder() {
        if (left != null) {
            left.infixOrder();
        }
        System.out.println(this);
        if (right != null) {
            right.infixOrder();
        }
    }
}
```

#### 平衡二叉树(AVL 树) 

> 看一个案例(说明二叉排序树可能的问题) 

给你一个数列{1,2,3,4,5,6}，要求创建一颗二叉排序树(BST), 并分析问题所在. 

 左边 BST 存在的问题分析: 
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200809111158.png" alt="image-20200809111156587" style="zoom:80%;" />

1) 左子树全部为空，从形式上看，更像一个单链表. 

2) 插入速度没有影响 

3) 查询速度明显降低(因为需要依次比较), 不能发挥 BST的优势，因为每次还需要比较左子树，其查询速度比单链表还慢 

4) 解决方案-平衡二叉树(AVL)

> 基本介绍 

1) 平衡二叉树也叫平衡**二叉搜索树**（Self-balancing binary search tree）又被称为 AVL 树， 可以保证查询效率较高。 

2) 具有以下特点：它是**一 棵空树**或**它的左右两个子树的高度差的绝对值不超过** **1**，并且**左右两个子树都是一棵平衡二叉树**。平衡二叉树的常用实现方法有红黑树、AVL、替罪羊树、Treap、伸展树等。 

3) 举例说明, 看看下面哪些 AVL 树, 为什么? 
![image-20200809145714739](https://gitee.com/xudongyin/img/raw/master/img/20200809145716.png)

> 应用案例-单旋转(左旋转) 

1) 要求: 给你一个数列，创建出对应的平衡二叉树.数列 {4,3,6,5,7,8} 

2) 思路分析(示意图)
![image-20200809145741272](https://gitee.com/xudongyin/img/raw/master/img/20200809145742.png)

```java
//左旋转方法
public void leftRotate() {
    //创建新的结点，以当前根结点的值
    Node newNode = new Node(this.value);
    //把新的结点的左子树设置成当前结点的左子树
    newNode.left = this.left;
    //把新的结点的右子树设置成根结点的右子树的左子树
    newNode.right = this.right.left;
    //把当前结点的值替换成右子结点的值
    this.value = right.value;
    //把当前结点的右子树设置成当前结点右子树的右子树
    this.right = this.right.right;
    //把当前结点的左子树(左子结点)设置成新的结点
    this.left = newNode;
}
```

> 应用案例-单旋转(右旋转) 

1) 要求: 给你一个数列，创建出对应的平衡二叉树.数列 {10,12, 8, 9, 7, 6} 

2) 思路分析(示意图) 
![image-20200809145849376](https://gitee.com/xudongyin/img/raw/master/img/20200809145851.png)

```java
//右旋转
public void rightRotate() {
    //创建新的结点，以当前根结点的值
    Node newNode = new Node(value);
    //把新的结点的右子树设置成当前结点的右子树
    newNode.right = right;
    //把新的结点的左子树设置成根结点的左子树的右子树
    newNode.left = left.right;
    //把当前结点的值设置成左子结点的值
    value = left.value;
    //把当前结点的的左子树设置成当前结点的左子树的左子树
    left = left.left;
    //把当前结点的右子树(右子结点)设置成新的结点
    right = newNode;
}
```

> 应用案例-双旋转 

前面的两个数列，进行单旋转(即一次旋转)就可以将非平衡二叉树转成平衡二叉树,但是在某些情况下，单旋转 不能完成平衡二叉树的转换。比如数列 
int[] arr = { 10, 11, 7, 6, 8, 9 }; 运行原来的代码可以看到，并没有转成 AVL 树. 
int[] arr = {2,1,6,5,7,3}; // 运行原来的代码可以看到，并没有转成 AVL 树 

1) 问题分析 
![image-20200809153602628](https://gitee.com/xudongyin/img/raw/master/img/20200809153623.png)

![image-20200809151911287](https://gitee.com/xudongyin/img/raw/master/img/20200809151912.png)

2) 解决思路分析 

1. 当符号右旋转的条件时 
2. 如果根结点的左子树的右子树高度大于它的左子树的左子树的高度
3. 先对当前这个根结点的左节点进行左旋转 
4. 再对当前根结点进行右旋转的操作即可 

```java
package Tree.AVL_Tree;

public class AVL_TreeDemo {
    public static void main(String[] args) {
//        int[] arr = {4,3,6,5,7,8};
//        int[] arr = { 10, 12, 8, 9, 7, 6 };
        int[] arr = {10, 11, 7, 6, 8, 9, 22, 3, 2};
        AVLTree avlTree = new AVLTree();
        for (int i = 0; i < arr.length; i++) {
            avlTree.add(new Node(arr[i]));
        }
        System.out.println("树的高度为" + avlTree.getRoot().getHeight());
        System.out.println("左子树的高度为" + avlTree.getRoot().getLeftHeight());
        System.out.println("右子树的高度为" + avlTree.getRoot().getRightHeight());
        avlTree.delNode(9);
        avlTree.delNode(11);
        avlTree.delNode(22); //这里树会变成不平衡
        avlTree.add(new Node(4)); //加上一个节点，树就会双旋转自己平衡
        System.out.println("树的高度为" + avlTree.getRoot().getHeight());
        System.out.println("左子树的高度为" + avlTree.getRoot().getLeftHeight());
        System.out.println("右子树的高度为" + avlTree.getRoot().getRightHeight());

    }
}

//创建二叉排序树
class AVLTree {
    private Node root;

    public Node getRoot() {
        return root;
    }

    //添加结点的方法
    public void add(Node node) {
        if (root == null) {
            root = node;//如果root为空则直接让root指向node
        } else {
            root.add(node);
        }
    }

    //中序遍历
    public void infixOrder() {
        if (root == null) {
            System.out.println("树为空");
        } else {
            root.infixOrder();
        }
    }

    //找到要删除的结点
    public Node search(int value) {
        if (root == null) {
            return null;
        } else {
            return root.search(value);
        }
    }

    //找到要删除的结点的父结点
    public Node searchParent(int value) {
        if (root == null) {
            return null;
        } else {
            return root.searchParent(value);
        }
    }

    //编写方法:
    //1. 返回的 以node 为根结点的二叉排序树的最小结点的值
    //2. 删除node 为根结点的二叉排序树的最小结点

    /**
     * @param node 传入的结点(当做二叉排序树的根结点)
     * @return 返回的 以node 为根结点的二叉排序树的最小结点的值
     */
    public int delRightTreeMin(Node node) {
        Node target = node;
        //循环的查找左子节点，就会找到最小值
        while (target.left != null) {
            target = target.left;
        }
        //这时 target就指向了最小结点
        //删除最小结点
        delNode(target.value);
        return target.value;
    }

    //删除结点
    public void delNode(int value) {
        if (root == null) {
            return;
        }
        //1.需求先去找到要删除的结点  targetNode
        Node targetNode = search(value);
        if (targetNode == null) {
            return;
        }
        //如果我们发现当前这颗二叉排序树只有一个结点
        if (root.left == null && root.right == null) {
            root = null;
            return;
        }
        //去找到targetNode的父结点
        Node parent = searchParent(value);
        //如果要删除的结点是叶子结点
        if (targetNode.right == null && targetNode.left == null) {
            //判断targetNode 是父结点的左子结点，还是右子结点
            if (parent.left != null && parent.left.value == value) {//是左子结点
                parent.left = null;
            } else if (parent.right != null && value == parent.right.value) {//是右子节点
                parent.right = null;
            }
        } else if (targetNode.left != null && targetNode.right != null) {//删除有两颗子树的节点
            int minVal = delRightTreeMin(targetNode.right);
            targetNode.value = minVal;//删除右子树最小结点，然后把最小结点值赋给targetNode
        } else {// 删除只有一颗子树的结点
            if (targetNode.left != null) {//只有左子树
                if (parent != null) {
                    //如果 targetNode 是 parent 的左子结点
                    if (parent.left != null && parent.left.value == targetNode.value) {
                        parent.left = targetNode.left;
                    } else {//  targetNode 是 parent 的右子结点
                        parent.right = targetNode.left;
                    }
                } else {//没有父结点,也就是树只有root结点和一个子节点
                    root = targetNode.left;
                }
            } else {//只有右子树
                if (parent != null) {
                    //如果 targetNode 是 parent 的左子结点
                    if (parent.left != null && parent.left.value == targetNode.value) {
                        parent.left = targetNode.right;
                    } else {//如果 targetNode 是 parent 的右子结点
                        parent.right = targetNode.right;
                    }
                } else {
                    root = targetNode.right;
                }
            }

        }

    }
}

class Node {
    int value;
    Node left;
    Node right;

    public Node(int value) {
        this.value = value;
    }

    @Override
    public String toString() {
        return "Node{" +
                "value=" + value +
                '}';
    }

    //获取左子树的高度
    public int getLeftHeight() {
        if (left == null) {
            return 0;
        }
        return left.getHeight();
    }

    //获取右子树的高度
    public int getRightHeight() {
        if (right == null) {
            return 0;
        }
        return right.getHeight();
    }

    //获取当前结点为根结点的树的高度
    public int getHeight() {//每一层加一
        return Math.max(left == null ? 0 : left.getHeight(), right == null ? 0 : right.getHeight()) + 1;
    }

    //左旋转方法
    public void leftRotate() {
        //创建新的结点，以当前根结点的值
        Node newNode = new Node(this.value);
        //把新的结点的左子树设置成当前结点的左子树
        newNode.left = this.left;
        //把新的结点的右子树设置成根结点的右子树的左子树
        newNode.right = this.right.left;
        //把当前结点的值替换成右子结点的值
        this.value = right.value;
        //把当前结点的右子树设置成当前结点右子树的右子树
        this.right = this.right.right;
        //把当前结点的左子树(左子结点)设置成新的结点
        this.left = newNode;
    }

    //右旋转
    public void rightRotate() {
        //创建新的结点，以当前根结点的值
        Node newNode = new Node(value);
        //把新的结点的右子树设置成当前结点的右子树
        newNode.right = right;
        //把新的结点的左子树设置成根结点的右子树的左子树
        newNode.left = left.right;
        //把当前结点的值设置成左子结点的值
        value = left.value;
        //把当前结点的的左子树设置成当前结点的左子树的左子树
        left = left.left;
        //把当前结点的右子树(右子结点)设置成新的结点
        right = newNode;
    }

    //查找要删除的结点

    /**
     * @param value 希望删除的结点的值
     * @return 如果找到返回该结点，否则返回null
     */
    public Node search(int value) {
        if (value == this.value) {//刚好是要找的结点
            return this;
        } else if (value < this.value) {//如果查找的值小于当前结点，向左子树递归查找
            if (this.left == null) {//如果左子结点为空
                return null;
            }
            return this.left.search(value);
        } else {//如果查找的值不小于当前结点，向右子树递归查找
            if (this.right == null) {
                return null;
            }
            return this.right.search(value);
        }
    }
    //查找要删除结点的父结点

    /**
     * @param value 要找到的结点的值
     * @return 返回的是要删除的结点的父结点，如果没有就返回null
     */
    public Node searchParent(int value) {
        //如果当前结点就是要删除的结点的父结点，就返回
        if ((this.left != null && this.left.value == value) ||
                (this.right != null && this.right.value == value)) {
            return this;
        } else {
            //如果查找的值小于当前结点的值, 并且当前结点的左子结点不为空
            if (this.left != null && value < this.value) {
                return left.searchParent(value);//向左子树递归查找
            } else if (this.right != null && value >= this.value) {
                return right.searchParent(value);//向右子树递归查找
            } else {//当前结点不是要找的结点，并且左右子结点都为空
                return null;// 没有找到父结点
            }
        }
    }

    //添加节点
    public void add(Node node) {
        if (node == null) {
            return;
        }
        //判断传入的结点的值，和当前子树的根结点的值关系
        if (node.value < this.value) {
            if (left == null) {//如果当前结点左子结点为null
                left = node;
            } else {
                //递归的向左子树添加
                left.add(node);
            }
        } else {//添加的结点的值大于等于 当前结点的值
            if (right == null) {
                right = node;
            } else {
                //递归的向右子树添加
                right.add(node);
            }
        }
        //当添加完一个结点后，如果: (右子树的高度-左子树的高度) > 1 , 左旋转
        //这里的 this 是根结点 root
        if (this.getRightHeight() - this.getLeftHeight() > 1) {
            //如果它的右子树的左子树的高度大于它的右子树的右子树的高度
            if (right != null && right.getLeftHeight() > right.getRightHeight()) {
                //先对右子结点进行右旋转
                right.rightRotate();
                //再对根结点进行左旋转
                this.leftRotate();
            } else {//直接进行左旋转即可
                this.leftRotate();
            }
            return; //必须要！！！，可以省略下面代码的运行，节约资源
        }
        //当添加完一个结点后，如果: (左子树的高度-右子树的高度) > 1 ,右旋转
        if (getLeftHeight() - getRightHeight() > 1) {
            //如果根结点的左子树的右子树高度大于它的左子树的左子树的高度
            if (left != null && left.getRightHeight() > left.getLeftHeight()) {
                //先对左子结点进行左旋转
                left.leftRotate();
                //再对根结点进行右旋转
                this.rightRotate();
            } else {
                //直接进行右旋转
                rightRotate();
            }
        }
    }

    //中序遍历
    public void infixOrder() {
        if (left != null) {
            left.infixOrder();
        }
        System.out.println(this);
        if (right != null) {
            right.infixOrder();
        }
    }
}
```

#### 多路查找树

二叉树与 B 树 

> 二叉树的问题分析 

二叉树的操作效率较高，但是也存在问题, 请看下面的二叉树 
![image-20200809210028976](https://gitee.com/xudongyin/img/raw/master/img/20200809210031.png)

1) 二叉树需要加载到内存的，如果二叉树的节点少，没有什么问题，但是如果二叉树的节点很多(比如 1 亿)， 就存在如下问题: 

2) 问题 1：在构建二叉树时，需要多次进行 i/o 操作(海量数据存在数据库或文件中)，节点海量，构建二叉树时， 速度有影响 

3) 问题 2：节点海量，也会造成二叉树的高度很大，会降低操作速度

##### 多叉树 

1) 在二叉树中，每个节点有数据项，最多有两个子节点。如果允许每个节点可以有更多的数据项和更多的子节点，就是多叉树（multiway tree） 

2) 后面我们讲解的 2-3 树，2-3-4 树就是多叉树，多叉树通过重新组织节点，减少树的高度，能对二叉树进行优化。 

3) 举例说明(下面 2-3 树就是一颗多叉树)
![image-20200809210141645](https://gitee.com/xudongyin/img/raw/master/img/20200809210144.png)

> B 树的基本介绍 

B 树通过重新组织节点，降低树的高度，并且减少 i/o 读写次数来提升效率。 
![image-20200809210330465](https://gitee.com/xudongyin/img/raw/master/img/20200809210331.png)

1) 如图 B 树通过重新组织节点， 降低了树的高度. 

2) 文件系统及数据库系统的设计者利用了磁盘预读原理，将一个节点的大小设为等于一个页(页得大小通常为 4k)， 这样每个节点只需要一次 I/O 就可以完全载入 

3) 将树的度 M 设置为 1024，在 600 亿个元素中最多只需要 4 次 I/O 操作就可以读取到想要的元素, B 树(B+)广泛应用于文件存储系统以及数据库系统中 

##### 2-3 树 

 2-3 树是最简单的 B 树结构, 具有如下特点: 

1) **2-3 树的所有叶子节点都在同一层.(只要是 B 树都满足这个条件)** 

2) 有两个子节点的节点叫二节点，二节点要么没有子节点，要么有两个子节点.

3) 有三个子节点的节点叫三节点，三节点要么没有子节点，要么有三个子节点. 

4) 2-3 树是由二节点和三节点构成的树。 

> 2-3 树应用案例 

将数列{16, 24, 12, 32, 14, 26, 34, 10, 8, 28, 38, 20} 构建成 2-3 树，并保证数据插入的大小顺序。(演示一下构建 2-3树的过程.) 
![image-20200809210602565](https://gitee.com/xudongyin/img/raw/master/img/20200809210604.png)

插入规则: 

1) 2-3 树的所有叶子节点都在同一层.(只要是 B 树都满足这个条件) 

2) 有两个子节点的节点叫二节点，二节点要么没有子节点，要么有两个子节点. 

3) 有三个子节点的节点叫三节点，三节点要么没有子节点，要么有三个子节点 

4) 当按照规则插入一个数到某个节点时，不能满足上面三个要求，就需要拆，先向上拆，如果上层满，则拆本层，拆后仍然需要满足上面 3 个条件。 

5) 对于三节点的子树的值大小仍然遵守(BST 二叉排序树)的规则 

> 其它说明 

除了 23 树，还有 234 树等，概念和 23 树类似，也是一种 B 树。 如图:
![image-20200809210645734](https://gitee.com/xudongyin/img/raw/master/img/20200809210647.png)

##### B 树、B+树和 B*树 

> B 树的介绍 

B-tree 树即 B 树，B 即 Balanced，平衡的意思。有人把 B-tree 翻译成 B-树，容易让人产生误解。会以为 B-树是一种树，而 B 树又是另一种树。实际上，B-tree 就是指的 B 树。 

前面已经介绍了 2-3 树和 2-3-4 树，他们就是 B 树(英语：B-tree 也写成 B-树)，这里我们再做一个说明，我们在学习 Mysql 时，经常听到说某种类型的索引是基于 B 树或者 B+树的，如图: 
![image-20200809210809731](https://gitee.com/xudongyin/img/raw/master/img/20200809210811.png)

对上图的说明: 

1) B 树的阶：节点的最多子节点个数。比如 2-3 树的阶是 3，2-3-4 树的阶是 4

2) B-树的搜索，从根结点开始，对结点内的关键字（有序）序列进行二分查找，如果命中则结束，否则进入查询关键字所属范围的儿子结点；重复，直到所对应的儿子指针为空，或已经是叶子结点 

3) 关键字集合分布在整颗树中, 即叶子节点和非叶子节点都存放数据. 

4) 搜索有可能在非叶子结点结束 

5) 其搜索性能等价于在关键字全集内做一次二分查找

> B+树的介绍 

B+树是 B 树的变体，也是一种多路搜索树。 
![image-20200809211000567](https://gitee.com/xudongyin/img/raw/master/img/20200809211002.png)

对上图的说明: 

1) B+树的搜索与 B 树也基本相同，区别是 B+树只有达到叶子结点才命中（B 树可以在非叶子结点命中），其性能也等价于在关键字全集做一次二分查找 

2) 所有关键字都出现在叶子结点的链表中（即数据只能在叶子节点【也叫稠密索引】），且链表中的关键字(数据)恰好是有序的。 

3) 不可能在非叶子结点命中 

4) 非叶子结点相当于是叶子结点的索引（稀疏索引），叶子结点相当于是存储（关键字）数据的数据层

5) 更适合文件索引系统 

6) B 树和 B+树各有自己的应用场景，不能说 B+树完全比 B 树好，反之亦然. 

> B*树的介绍 

B*树是 B+树的变体，在 B+树的**非根和非叶子结点**再增**加指向兄弟的指针**。 
![image-20200809211112602](https://gitee.com/xudongyin/img/raw/master/img/20200809211114.png)

 B*树的说明: 

1) B*树定义了非叶子结点关键字个数至少为(2/3)*M，即块的最低使用率为 2/3，而 B+树的块的最低使用率为的 1/2。 

2) 从第 1 个特点我们可以看出，B*树分配新结点的概率比 B+树要低，空间使用率更高



## 图

> 为什么要有图 

1) 前面我们学了线性表和树 

2) 线性表局限于一个直接前驱和一个直接后继的关系 

3) 树也只能有一个直接前驱也就是父节点 

4) 当我们需要**表示多对多**的关系时， 这里我们就用到了**图**。 

图是一种**数据结构**，其中结点可以具有零个或多个相邻元素。两个结点之间的连接称为边。 结点也可以称为 顶点。如图： 
![image-20200809211346473](https://gitee.com/xudongyin/img/raw/master/img/20200809211349.png)

> 图的常用概念

1) 顶点(vertex) 

2) 边(edge) 

3) 路径 

4) 无向图(右图)
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200809211520.png" alt="image-20200809211517578"  />

5) 有向图 

6) 带权图
![image-20200809211657882](https://gitee.com/xudongyin/img/raw/master/img/20200809211659.png)

> 图的表示方式 

图的表示方式有两种：二维数组表示（邻接矩阵）；链表表示（邻接表）。

**邻接矩阵** 

邻接矩阵是表示图形中顶点之间相邻关系的矩阵，对于 n 个顶点的图而言，矩阵是的 row 和 col 表示的是 1....n 个点。
![image-20200809212032257](https://gitee.com/xudongyin/img/raw/master/img/20200809212034.png)

**邻接表** 

1) 邻接矩阵需要为每个顶点都分配 n 个边的空间，其实有很多边都是不存在,会造成空间的一定损失. 

2) 邻接表的实现只关心存在的边，不关心不存在的边。因此没有空间浪费，邻接表由数组+链表组成 

3) 举例说明
![image-20200809212044930](https://gitee.com/xudongyin/img/raw/master/img/20200809212046.png)

> 图的快速入门案例 

1) 要求: 代码实现如下图结构
![image-20200810174732326](https://gitee.com/xudongyin/img/raw/master/img/20200810174734.png)

2) 思路分析 (1) 存储顶点 String 使用 ArrayList (2) 保存矩阵 int[][] edges 

```java
//添加节点
public void insertVertex(String vertex) {
    vertexList.add(vertex);
}
//添加边
/**
 * @param v1     表示点的下标即使第几个顶点  "A"-"B" "A"->0 "B"->1
 * @param v2     第二个顶点对应的下标
 * @param weight 表示
 */
public void insertEdge(int v1, int v2, int weight) {
    edges[v1][v2] = weight;
    edges[v2][v1] = weight;
    numOfEdges++;
}
```

### 深度优先遍历

> 图遍历介绍 

所谓图的遍历，即是对结点的访问。一个图有那么多个结点，如何遍历这些结点，需要特定策略，一般有两种访问策略: (1)深度优先遍历 (2)广度优先遍历 

> 深度优先遍历基本思想 

图的深度优先搜索(**Depth First Search**) 。 

1) 深度优先遍历，从初始访问结点出发，初始访问结点可能有多个邻接结点，深度优先遍历的策略就是首先访问第一个邻接结点，然后再以这个被访问的邻接结点作为初始结点，访问它的第一个邻接结点， 可以这样理解： 每次都在访问完当前结点后首先访问当前结点的第一个邻接结点。 

2) 我们可以看到，这样的访问策略是优先往纵向挖掘深入，而不是对一个结点的所有邻接结点进行横向访问。 

3) 显然，深度优先搜索是一个递归的过程 

> **深度优先遍历算法步骤** 

1) 访问初始结点 v，并标记结点 v 为已访问。 

2) 查找结点 v 的第一个邻接结点 w。 

3) 若 w 存在，则继续执行 4，如果 w 不存在，则回到第 1 步，将从 v 的下一个结点继续。 

4) 若 w 未被访问，对 w 进行深度优先遍历递归（即把 w 当做另一个 v，然后进行步骤 123）。 

5) 查找结点 v 的 w 邻接结点的下一个邻接结点，转到步骤 3。 

6) 分析图
![image-20200810175121913](https://gitee.com/xudongyin/img/raw/master/img/20200810175123.png)

```java
//深度优先遍历算法
//i 第一次就是 0
public void dfs(boolean[] isVisited, int i) {
    //首先我们访问该结点,输出
    System.out.print(getValueByIndex(i) + "->");
    //将结点设置为已经访问
    isVisited[i] = true;
    //查找结点i的第一个邻接结点w
    int w = getFirstNeighbor(i);
    while (w != -1) {//说明有
        if (!isVisited[w]) {//如果w结点没有被访问过
            dfs(isVisited, w);
        }
        //如果w结点已经被访问过，返回前一个结点查找下一个邻接点
        w = getNextNeighbor(i, w);
    }
}
//对dfs 进行一个重载, 遍历我们所有的结点，并进行 dfs
public void dfs() {
    isVisited = new boolean[vertexList.size()];
    //遍历所有的结点，进行dfs[回溯]
    for(int i = 0; i < getNumOfVertex(); i++) {
        if(!isVisited[i]) {
            dfs(isVisited, i);
        }
    }
}
```

### 广度优先遍历 

> 广度优先遍历基本思想 

1) 图的广度优先搜索(Broad First Search) 。 

2) 类似于一个**分层搜索**的过程，广度优先遍历需要使用一个队列以保持访问过的结点的顺序，以便按这个顺序来访问这些结点的邻接结点 

> **广度优先遍历算法步骤** 

1) 访问初始结点 v 并标记结点 v 为已访问。 

2) 结点 v 入队列

3) 当队列非空时，继续执行，否则算法结束。 

4) 出队列，取得队头结点 u。 

5) 查找结点 u 的第一个邻接结点 w。 

6) 若结点 u 的邻接结点 w 不存在，则转到步骤 3；否则循环执行以下三个步骤： 

​		6.1 若结点 w 尚未被访问，则访问结点 w 并标记为已访问。 

​		6.2 结点 w 入队列 

​		6.3 查找结点 u 的继 w 邻接结点后的下一个邻接结点 w，转到步骤 6。
![image-20200810175533034](https://gitee.com/xudongyin/img/raw/master/img/20200810175534.png) 

```java
//广度优先遍历算法
public void bfs(boolean[] isVisited, int i) {
    int u;// 表示队列的头结点对应下标
    int w;// 邻接结点w下标
    //队列，记录结点访问的顺序
    LinkedList queue = new LinkedList();
    //访问结点，输出结点信息
    System.out.print(getValueByIndex(i) + "=>");
    //标记为已访问
    isVisited[i] = true;
    //将结点加入队列
    queue.addLast(i);
    while (!queue.isEmpty()) {
        //取出队列的头结点下标
        u = (int) queue.removeFirst();
        //得到第一个邻接结点的下标 w
        w = getFirstNeighbor(u);
        while (w != -1) {//找到
            //是否访问过
            if (!isVisited[w]) {
                System.out.print(getValueByIndex(w) + "=>");
                //标记已经访问
                isVisited[w] = true;
                //入队
                queue.addLast(w);
            }
            //以u为前驱点，找w后面的下一个邻结点
            w = getNextNeighbor(u, w);//体现出我们的广度优先
        }
    }

}
//遍历所有的结点，都进行广度优先搜索
public void bfs() {
    isVisited = new boolean[vertexList.size()];
    for(int i = 0; i < getNumOfVertex(); i++) {
        if(!isVisited[i]) {
            bfs(isVisited, i);
        }
    }
}
```

> 图的深度优先 VS 广度优先

![image-20200810175944608](https://gitee.com/xudongyin/img/raw/master/img/20200810175946.png)

### 图的代码汇总

```java
package Graph;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.LinkedList;

public class Graph{
    private ArrayList<String> vertexList;//存储顶点集合
    private int[][] edges;//存储图对应的邻结矩阵
    private int numOfEdges; //表示边的数目
    //定义给数组boolean[], 记录某个结点是否被访问
    private boolean[] isVisited;
    public static void main(String[] args) {
        int n = 8;  //结点的个数
//        String Vertexs[] = {"A", "B", "C", "D", "E"};
        String Vertexs[] = {"1", "2", "3", "4", "5", "6", "7", "8"};
        //创建图对象
        Graph graph = new Graph(n);
        //循环的添加顶点
        for(String vertex: Vertexs) {
            graph.insertVertex(vertex);
        }

        //添加边
        //A-B A-C B-C B-D B-E
//    graph.insertEdge(0, 1, 1); // A-B
//    graph.insertEdge(0, 2, 1); //
//    graph.insertEdge(1, 2, 1); //
//    graph.insertEdge(1, 3, 1); //
//    graph.insertEdge(1, 4, 1); //

        //更新边的关系
        graph.insertEdge(0, 1, 1);
        graph.insertEdge(0, 2, 1);
        graph.insertEdge(1, 3, 1);
        graph.insertEdge(1, 4, 1);
        graph.insertEdge(3, 7, 1);
        graph.insertEdge(4, 7, 1);
        graph.insertEdge(2, 5, 1);
        graph.insertEdge(2, 6, 1);
        graph.insertEdge(5, 6, 1);

        //显示一把邻结矩阵
        graph.showGraph();

        System.out.println("进行深度优先遍历");
        graph.dfs();
        System.out.println();
        System.out.println("进行广度优先遍历");
        graph.bfs();
    }

    public Graph(int n) {
        //初始化矩阵和vertexList
        edges = new int[n][n];
        vertexList = new ArrayList<>(n);
        numOfEdges = 0;
    }

    //得到第一个邻接结点的下标 w
    public int getFirstNeighbor(int index) {
        for (int j = 0; j < vertexList.size(); j++) {
            if (edges[index][j] > 0) {
                return j;
            }
        }
        return -1;
    }

    //根据前一个邻接结点的下标来获取下一个邻接结点
    public int getNextNeighbor(int v1, int v2) {
        for (int j = v2 + 1; j < vertexList.size(); j++) {
            if (edges[v1][j] > 0) {
                return j;
            }
        }
        return -1;
    }

    //广度优先遍历算法
    public void bfs(boolean[] isVisited, int i) {
        int u;// 表示队列的头结点对应下标
        int w;// 邻接结点w下标
        //队列，记录结点访问的顺序
        LinkedList queue = new LinkedList();
        //访问结点，输出结点信息
        System.out.print(getValueByIndex(i) + "=>");
        //标记为已访问
        isVisited[i] = true;
        //将结点加入队列
        queue.addLast(i);
        while (!queue.isEmpty()) {
            //取出队列的头结点下标
            u = (int) queue.removeFirst();
            //得到第一个邻接结点的下标 w
            w = getFirstNeighbor(u);
            while (w != -1) {//找到
                //是否访问过
                if (!isVisited[w]) {
                    System.out.print(getValueByIndex(w) + "=>");
                    //标记已经访问
                    isVisited[w] = true;
                    //入队
                    queue.addLast(w);
                }
                //以u为前驱点，找w后面的下一个邻结点
                w = getNextNeighbor(u, w);//体现出我们的广度优先
            }
        }

    }
    //遍历所有的结点，都进行广度优先搜索
    public void bfs() {
        isVisited = new boolean[vertexList.size()];
        for(int i = 0; i < getNumOfVertex(); i++) {
            if(!isVisited[i]) {
                bfs(isVisited, i);
            }
        }
    }

    //深度优先遍历算法
    //i 第一次就是 0
    public void dfs(boolean[] isVisited, int i) {
        //首先我们访问该结点,输出
        System.out.print(getValueByIndex(i) + "->");
        //将结点设置为已经访问
        isVisited[i] = true;
        //查找结点i的第一个邻接结点w
        int w = getFirstNeighbor(i);
        while (w != -1) {//说明有
            if (!isVisited[w]) {//如果w结点没有被访问过
                dfs(isVisited, w);
            }
            //如果w结点已经被访问过，返回前一个结点查找下一个邻接点
            w = getNextNeighbor(i, w);
        }
    }
    //对dfs 进行一个重载, 遍历我们所有的结点，并进行 dfs
    public void dfs() {
        isVisited = new boolean[vertexList.size()];
        //遍历所有的结点，进行dfs[回溯]
        for(int i = 0; i < getNumOfVertex(); i++) {
            if(!isVisited[i]) {
                dfs(isVisited, i);
            }
        }
    }
    //返回图的节点数目
    public int getNumOfVertex() {
        return vertexList.size();
    }
    //显示图对应的矩阵
    public void showGraph() {
        for(int[] link : edges) {
            System.err.println(Arrays.toString(link));
        }
    }
    //返回结点i(下标)对应的数据 0->"A" 1->"B" 2->"C"
    public String getValueByIndex(int i) {
        return vertexList.get(i);
    }
    //返回v1和v2的权值
    public int getWeight(int v1, int v2) {
        return edges[v1][v2];
    }
    //得到边的数目
    public int getNumOfEdges() {
        return numOfEdges;
    }
    //添加节点
    public void insertVertex(String vertex) {
        vertexList.add(vertex);
    }
    //添加边
    /**
     * @param v1     表示点的下标即使第几个顶点  "A"-"B" "A"->0 "B"->1
     * @param v2     第二个顶点对应的下标
     * @param weight 表示
     */
    public void insertEdge(int v1, int v2, int weight) {
        edges[v1][v2] = weight;
        edges[v2][v1] = weight;
        numOfEdges++;
    }
}
```



## 二分查找算法(非递归) 

> 二分查找算法(非递归)介绍 

1) 前面我们讲过了二分查找算法，是使用递归的方式，下面我们讲解二分查找算法的非递归方式 

2) 二分查找法只适用于从有序的数列中进行查找(比如数字和字母等)，将数列排序后再进行查找 

3) 二分查找法的运行时间为对数时间 O(㏒₂n) ，即查找到需要的目标位置最多只需要㏒₂n 步，假设从[0,99]的 队列(100 个数，即 n=100)中寻到目标数 30，则需要查找步数为㏒₂100 , 即最多需要查找 7 次( 2^6 < 100 < 2^7)

```java
package BinarySearchNoRecursion;

public class BinarySearchNoRecursion {
    public static void main(String[] args) {
        //测试
        int[] arr = {1,3, 8, 10, 11, 67, 100};
        int index = binarySearch(arr, 100);
        System.out.println("index=" + index);// 6

    }
    //二分查找的非递归实现
    /**
     *
     * @param arr  被查找的数组
     * @param target 要找的值
     * @return 找到的话返回下标，没有返回-1
     */
    public static int binarySearch(int[] arr, int target) {
        int left = 0;
        int right = arr.length - 1;
        while (left <= right) { //说明可以继续查找
            int mid = (left + right) / 2;
            if (arr[mid] == target) {
                return mid;
            } else if (arr[mid] > target) {
                right = mid - 1;//需要向左边查找
            } else {
                left = mid + 1;//需要向右边查找
            }
        }
        return -1;
    }
}
```

## 分治算法 

> 分治算法介绍 

1) 分治法是一种很重要的算法。字面上的解释是“分而治之”，就是把一个复杂的问题分成两个或更多的相同或相似的子问题，再把子问题分成更小的子问题……直到最后子问题可以简单的直接求解，原问题的解即子问题的解的合并。这个技巧是很多高效算法的基础，如排序算法(快速排序，归并排序)，傅立叶变换(快速傅立叶变换)…… 

2) 分治算法可以求解的一些经典问题 

- 二分搜索 
- 大整数乘法 
- 棋盘覆盖 
- 合并排序 
- 快速排序 
- 线性时间选择 
- 最接近点对问题 
- 循环赛日程表 
- **汉诺塔**

> **分治算法的基本步骤** 

**分治法在每一层递归上都有三个步骤：** 

**1)** **分解：将原问题分解为若干个规模较小，相互独立，与原问题形式相同的子问题** 

**2)** **解决：若子问题规模较小而容易被解决则直接解，否则递归地解各个子问题**

**3)** **合并：将各个子问题的解合并为原问题的解。** 
![image-20200810210036462](https://gitee.com/xudongyin/img/raw/master/img/20200810210038.png)

> 分治算法最佳实践-汉诺塔

 汉诺塔游戏的演示和思路分析:

1) 如果是有一个盘， A->C 

如果我们有 n >= 2 情况，我们总是可以看做是两个盘 1.最下边的盘 2. 上面的盘 

2) 先把 最上面的盘 A->B 

3) 把最下边的盘 A->C 

4) 把 B 塔的所有盘 从 B->C

```JAVA
package DivideAndConquer;

public class DivideAndConquer {
    public static void main(String[] args) {
        HanoiTower(4, 'A', 'B', 'C');
    }

    //汉诺塔的移动的方法
    //使用分治算法
    public static void HanoiTower(int num, char a, char b, char c) {
        //如果只有一个盘
        if (num == 1) {
            System.out.println("第1个盘从 " + a + "->" + c);
        } else {
            //如果我们有 n >= 2 情况，我们总是可以看做是两个盘 1.最下边的一个盘 2. 上面的所有盘
            //1. 先把 最上面的所有盘 A->B， 移动过程会使用到 c
            HanoiTower(num - 1, a, c, b);
            //2. 把最下边的盘 A->C
            System.out.println("第" + num + "个盘从 " + a + "->" + c);
            //3. 把B塔的所有盘 从 B->C , 移动过程使用到 a塔
            HanoiTower(num - 1, b, a, c);
        }
    }
}
```

## 动态规划算法 

> 动态规划算法介绍 

1) 动态规划(Dynamic Programming)算法的核心思想是：将**大问题划分为小问题**进行解决，从而一步步获取最优解的处理算法 

2) 动态规划算法与分治算法类似，其基本思想也是将待求解问题分解成若干个子问题，先求解子问题，然后从这些子问题的解得到原问题的解。 

3) 与分治法不同的是，适合于用动态规划求解的问题，经分解得到**子问题往往不是互相独立的**。 ( 即下一个子阶段的求解是建立在上一个子阶段的解的基础上，进行进一步的求解 ) 

4) 动态规划可以通过填表的方式来逐步推进，得到最优解.

> 应用场景-背包问题 

背包问题：有一个背包，容量为 4 磅 ， 现有如下物品
![image-20200810210258156](https://gitee.com/xudongyin/img/raw/master/img/20200810210300.png)

1) 要求达到的目标为装入的背包的总价值最大，并且重量不超出 

2) 要求装入的物品不能重复

3) 思路分析和图解 

背包问题主要是指一个给定容量的背包、若干具有一定价值和重量的物品，如何选择物品放入背包使物品的价值最大。其中又分 **01** **背包**和**完全背包**(完全背包指的是：每种物品都有无限件可用) 

4) 这里的问题属于 **01** **背包**，即每个物品最多放一个。而无限背包可以转化为 01 背包。 

5) 算法的主要思想，利用动态规划来解决。每次遍历到的第 i 个物品，根据 w[i]和 v[i]来确定是否需要将该物品放入背包中。即对于给定的 n 个物品，设 v[i]、w[i]分别为第 i 个物品的价值和重量，C 为背包的容量。再令 v[i] [j] 表示在前 i 个物品中能够装入容量为 j 的背包中的最大价值。则我们有下面的结果： 

~~~java
(1) v[i][0]=v[0][j]=0; //表示 填入表 第一行和第一列是 0 
(2) 当 w[i]> j 时：v[i][j]=v[i-1][j] // 当准备加入新增的商品的容量大于 当前背包的容量时，就直接使用上一个 单元格的装入策略 
(3) 当 j>=w[i]时： v[i][j]=max{v[i-1][j], v[i]+v[i-1][j-w[i]]} 
// 当 准备加入的新增的商品的容量小于等于当前背包的容量, 
// 装入的方式: 
v[i-1][j]： 就是上一个单元格的装入的最大值 
v[i] : 表示当前商品的价值 
v[i-1][j-w[i]] ： 装入 i-1 商品，到剩余空间 j-w[i]的最大值 
当 j>=w[i]时： v[i][j]=max{v[i-1][j], v[i]+v[i-1][j-w[i]]} :
~~~

6) 图解的分析
![image-20200810210821599](https://gitee.com/xudongyin/img/raw/master/img/20200810210823.png)

```java
package Dynamic;

public class KnapsackProblem {
    public static void main(String[] args) {
        int[] w = {1, 4, 3};//物品的重量
        int[] val = {1500, 3000, 2000}; //物品的价值 这里val[i] 就是前面讲的v[i]
        int m = 4; //背包的容量
        int n = val.length; //物品的个数

        //创建二维数组，
        //v[i][j] 表示在前i个物品中能够装入容量为j的背包中的最大价值
        int[][] v = new int[n + 1][m + 1];
        //为了记录放入商品的情况，我们定一个二维数组
        int[][] path = new int[n + 1][m + 1];

        //初始化第一行和第一列, 这里在本程序中，可以不去处理，因为默认就是0
        for (int i = 0; i < v.length; i++) {
            v[i][0] = 0;//将第一列设置为0
        }
        for (int i = 0; i < v[0].length; i++) {
            v[0][i] = 0; //将第一行设置0
        }
        //根据前面得到公式来动态规划处理
        for (int i = 1; i < v.length; i++) {//不处理第一行 i是从1开始的
            for (int j = 1; j < v[0].length; j++) {//不处理第一列, j是从1开始的
                //公式
                if(w[i-1]> j) { // 因为我们程序i 是从0开始的，因此原来公式中的 w[i] 修改成 w[i-1]
                    v[i][j]=v[i-1][j];
                } else {
                    //说明:
                    //v[i][j]=max{v[i-1][j], v[i]+v[i-1][j-w[i]]}
                    //因为我们的i 从0开始的， 因此公式需要调整成
                    //v[i][j] = Math.max(v[i - 1][j], val[i - 1] + v[i - 1][j - w[i - 1]]);
                    //为了记录商品存放到背包的情况，我们不能直接的使用上面的公式，需要使用if-else来体现公式
                    if (v[i - 1][j] < val[i - 1] + v[i - 1][j - w[i - 1]]) {
                        v[i][j] = val[i - 1] + v[i - 1][j - w[i - 1]];
                        //把当前的情况记录到path ,对应背包容量对应商品的最优方案
                        path[i][j] = 1;
                    } else {
                        v[i][j] = v[i - 1][j];
                    }
                }
            }
        }
        //输出一下v 看看目前的情况
        for(int i =0; i < v.length;i++) {
            for(int j = 0; j < v[i].length;j++) {
                System.out.print(v[i][j] + " ");
            }
            System.out.println();
        }
        System.out.println("============================");
        //输出最后我们是放入的哪些商品
        //遍历path, 这样输出会把所有的放入情况都得到, 其实我们只需要最后的放入
//    for(int i = 0; i < path.length; i++) {
//       for(int j=0; j < path[i].length; j++) {
//          if(path[i][j] == 1) {
//             System.out.printf("第%d个商品放入到背包\n", i);
//          }
//       }
//    }

        int i = path.length - 1;//行的最大下标
        int j = path[0].length - 1; //列的最大下标
        while (i >= 0 && j >= 0) {//从path的最后开始找
            if (path[i][j] == 1) {
                System.out.printf("第%d个商品放入到背包\n", i);
                j = j - w[i - 1];//剩余的空间
            }
            i--;//向前扫描
        }
    }
}
```

## KMP 算法

> 字符串匹配问题 

 字符串匹配问题： 

1) 有一个字符串 str1= ""硅硅谷 尚硅谷你尚硅 尚硅谷你尚硅谷你尚硅你好""，和一个子串 str2="尚硅谷你尚硅你" 

2) 现在要判断 str1 是否含有 str2, 如果存在，就返回第一次出现的位置, 如果没有，则返回-1

### 暴力匹配算法 

如果用暴力匹配的思路，并假设现在 str1 匹配到 i 位置，子串 str2 匹配到 j 位置，则有: 

1) 如果当前字符匹配成功（即 str1[i] == str2[j]），则 i++，j++，继续匹配下一个字符 

2) 如果失配（即 str1[i]! = str2[j]），令 i = i - (j - 1)，j = 0。相当于每次匹配失败时，i 回溯，j 被置为 0。 

3) 用暴力方法解决的话就会有大量的回溯，每次只移动一位，若是不匹配，移动到下一位接着判断，浪费了大量的时间。(不可行!) 

4) 暴力匹配算法实现

```java
package KMP;

public class ViolenceMatch {
    public static void main(String[] args) {
        //测试暴力匹配算法
        String str1 = "硅硅谷 尚硅谷你尚硅 尚硅谷你尚硅谷你尚硅你好";
        String str2 = "尚硅谷你尚硅你";
        int index = violenceMatch(str1, str2);
        System.out.println("index=" + index);
    }

    //暴力匹配算法
    public static int violenceMatch(String str1, String str2) {
        char[] s1 = str1.toCharArray();
        char[] s2 = str2.toCharArray();

        int s1Len = s1.length;
        int s2Len = s2.length;

        int i = 0;// i索引指向s1
        int j = 0;// j索引指向s2
        while (i < s1Len && j < s2Len) {// 保证匹配时，不越界
            if (s1[i] == s2[j]) {//匹配ok,后移
                i++;
                j++;
            } else {//没有匹配成功
                //如果失配（即str1[i]! = str2[j]），令i = i - (j - 1)，j = 0。
                i = i - (j - 1);
                j = 0;
            }
        }
        //判断是否匹配成功
        if (j == s2Len) {
            return  i - j;
        } else {
            return -1;
        }
    }
}
```

> KMP 算法介绍 

1) KMP 是一个解决模式串在文本串是否出现过，如果出现过，最早出现的位置的经典算法 

2) Knuth-Morris-Pratt 字符串查找算法，简称为 “KMP 算法”，常用于在一个文本串 S 内查找一个模式串 P 的出现位置，这个算法由 Donald Knuth、Vaughan Pratt、James H. Morris 三人于 1977 年联合发表，故取这 3 人的姓氏命名此算法. 

3) KMP 方法算法就利用之前判断过信息，通过一个 next 数组，保存模式串中前后最长公共子序列的长度，每次回溯时，通过 next 数组找到，前面匹配过的位置，省去了大量的计算时间 

4) 参考资料：https://www.cnblogs.com/ZuoAndFutureGirl/p/9028287.html 

> KMP 算法最佳应用-字符串匹配问题

 字符串匹配问题：： 

1) 有一个字符串 str1= "BBC ABCDAB ABCDABCDABDE"，和一个子串 str2="ABCDABD" 

2) 现在要判断 str1 是否含有 str2, 如果存在，就返回第一次出现的位置, 如果没有，则返回-1 

3) 要求：使用 **KMP** **算法完成**判断，不能使用简单的暴力匹配算法

 思路分析图解 

举例来说，有一个字符串 Str1 = “BBC ABCDAB ABCDABCDABDE”，判断，里面是否包含另一个字符串 Str2 = “ABCDABD”？ 

​	1.首先，用 Str1 的第一个字符和 Str2 的第一个字符去比较，不符合，关键词向后移动一位 
​		<img src="https://gitee.com/xudongyin/img/raw/master/img/20200811101736.png" alt="image-20200811101735295" style="zoom:67%;" />

2. 重复第一步，还是不符合，再后移 
   <img src="https://gitee.com/xudongyin/img/raw/master/img/20200811101834.png" alt="image-20200811101825507" style="zoom:67%;" />

3. 一直重复，直到 Str1 有一个字符与 Str2 的第一个字符符合为止
   <img src="https://gitee.com/xudongyin/img/raw/master/img/20200811101914.png" alt="image-20200811101912422" style="zoom:67%;" />

4. 接着比较字符串和搜索词的下一个字符，还是符合。
   <img src="https://gitee.com/xudongyin/img/raw/master/img/20200811101936.png" alt="image-20200811101934960" style="zoom:67%;" />

5. 遇到 Str1 有一个字符与 Str2 对应的字符不符合。
   <img src="https://gitee.com/xudongyin/img/raw/master/img/20200811101956.png" alt="image-20200811101954689" style="zoom:67%;" />

6. 这时候，想到的是继续遍历 Str1 的下一个字符，重复第 1 步。(其实是很不明智的，因为此时 BCD 已经比较过了，没有必要再做重复的工作，一个基本事实是，当空格与 D 不匹配时，你其实知道前面六个字符是”ABCDAB”。 KMP 算法的想法是，设法利用这个已知信息，不要把”搜索位置”移回已经比较过的位置，继续把它向后移，这样就提高了效率。)
   <img src="https://gitee.com/xudongyin/img/raw/master/img/20200811102031.png" alt="image-20200811102029635" style="zoom:67%;" />

7. 怎么做到把刚刚重复的步骤省略掉？可以对 Str2 计算出一张《部分匹配表》，这张表的产生在后面介绍
   <img src="https://gitee.com/xudongyin/img/raw/master/img/20200811102059.png" alt="image-20200811102056903" style="zoom:67%;" />

8. .已知空格与 D 不匹配时，前面六个字符”ABCDAB”是匹配的。查表可知，最后一个匹配字符 B 对应的”部分匹配值”为 2，因此按照下面的公式算出向后移动的位数： 

   移动位数 = 已匹配的字符数 - 对应的部分匹配值 

   因为 6 - 2 等于 4，所以将搜索词向后移动 4 位。

9. 因为空格与Ｃ不匹配，搜索词还要继续往后移。这时，已匹配的字符数为 2（”AB”），对应的”部分匹配值”为 0。所以，移动位数 = 2 - 0，结果为 2，于是将搜索词向后移 2 位。
   <img src="https://gitee.com/xudongyin/img/raw/master/img/20200811102157.png" alt="image-20200811102156265" style="zoom:67%;" />

10. 因为空格与 A 不匹配，继续后移一位。 
    <img src="https://gitee.com/xudongyin/img/raw/master/img/20200811102223.png" alt="image-20200811102215538" style="zoom:67%;" />

11. 逐位比较，直到发现 C 与 D 不匹配。于是，移动位数 = 6 - 2，继续将搜索词向后移动 4 位。
    <img src="https://gitee.com/xudongyin/img/raw/master/img/20200811102246.png" alt="image-20200811102242823" style="zoom:67%;" />

12. 逐位比较，直到搜索词的最后一位，发现完全匹配，于是搜索完成。如果还要继续搜索（即找出全部匹配），移动位数 = 7 - 0，再将搜索词向后移动 7 位，这里就不再重复了。
    <img src="https://gitee.com/xudongyin/img/raw/master/img/20200811102324.png" alt="image-20200811102323111" style="zoom:67%;" />

13. .介绍《部分匹配表》怎么产生的 

    先介绍前缀，后缀是什么 
    <img src="https://gitee.com/xudongyin/img/raw/master/img/20200811102351.png" alt="image-20200811102348907" style="zoom:67%;" />

    “部分匹配值”就是”前缀”和”后缀”的最长的共有元素的长度。以”ABCDABD”为例， 

    －”A”的前缀和后缀都为空集，共有元素的长度为 0； 

    －”AB”的前缀为[A]，后缀为[B]，共有元素的长度为 0； 

    －”ABC”的前缀为[A, AB]，后缀为[BC, C]，共有元素的长度 0； 

    －”ABCD”的前缀为[A, AB, ABC]，后缀为[BCD, CD, D]，共有元素的长度为 0； 

    －”ABCDA”的前缀为[A, AB, ABC, ABCD]，后缀为[BCDA, CDA, DA, A]，共有元素为”A”，长度为 1； 

    －”ABCDAB”的前缀为[A, AB, ABC, ABCD, ABCDA]，后缀为[BCDAB, CDAB, DAB, AB, B]，共有元素为”AB”，长度为 2； 

    －”ABCDABD”的前缀为[A, AB, ABC, ABCD, ABCDA, ABCDAB]，后缀为[BCDABD, CDABD, DABD, ABD, BD, D]，共有元素的长度为 0。

14. ”部分匹配”的实质是，有时候，字符串头部和尾部会有重复。比如，”ABCDAB”之中有两个”AB”，那么它的”部分匹配值”就是 2（”AB”的长度）。搜索词移动的时候，第一个”AB”向后移动 4 位（字符串长度- 部分匹配值），就可以来到第二个”AB”的位置。 
    <img src="https://gitee.com/xudongyin/img/raw/master/img/20200811102458.png" alt="image-20200811102456801" style="zoom:67%;" />
    到此 KMP 算法思想分析完毕! 

```java
package KMP;

import java.util.Arrays;

public class KMPAlgorithm {
    public static void main(String[] args) {
        String str1 = "BBC ABCDAB ABCDABCDABDE";
        String str2 = "ABCDABD";
        //String str2 = "BBC";

        int[] next = kmpNext("ABCDABD"); //[0, 0, 0, 0, 1, 2, 0]
        System.out.println("next=" + Arrays.toString(next));

        int index = kmpSearch(str1, str2, next);
        System.out.println("index=" + index); // 15了
    }
    //写出我们的kmp搜索算法
    /**
     *
     * @param str1 源字符串
     * @param str2 子串
     * @param next 部分匹配表, 是子串对应的部分匹配表
     * @return 如果是-1就是没有匹配到，否则返回第一个匹配的位置
     */
    public static int kmpSearch(String str1, String str2, int[] next) {
        //遍历
        for (int i = 0, j = 0; i < str1.length(); i++) {
            //需要处理 str1.charAt(i) ！= str2.charAt(j), 去调整j的大小
            //KMP算法核心点, 可以验证...
            while (j > 0 && str1.charAt(i) != str2.charAt(j)) {
                j = next[j - 1];
            }
            if (str1.charAt(i) == str2.charAt(j)) {
                j++;
            }
            if (j == str2.length()) {//找到了
                return i - j + 1;
            }
        }
        return -1;
    }
    //获取到一个字符串(子串) 的部分匹配值表
    public static  int[] kmpNext(String dest) {
        //创建一个next 数组保存部分匹配值
        int[] next = new int[dest.length()];
        next[0] = 0;//如果字符串是长度为1 部分匹配值就是0
        for (int i = 1, j = 0; i < next.length; i++) {
            //当dest.charAt(i) != dest.charAt(j) ，我们需要从next[j-1]获取新的j
            //直到我们发现 有  dest.charAt(i) == dest.charAt(j)成立才退出
            //这是kmp算法的核心点
            while (j > 0 && dest.charAt(i) != dest.charAt(j)) {
                j = next[j - 1];
            }
            //当dest.charAt(i) == dest.charAt(j) 满足时，部分匹配值就+1
            if (dest.charAt(i) == dest.charAt(j)) {
                j++;
            }
            next[i] = j;
        }
        return next;
    }
}
```

## 贪心算法

> 贪心算法介绍 

1) 贪婪算法(贪心算法)是指在对问题进行求解时，**在每一步选择中都采取最好或者最优(即最有利)*的选择**，从而希望能够导致结果是最好或者最优的算法

2) 贪婪算法所得到的结果不一定是最优的结果(有时候会是最优解)，但是都是相对近似(接近)最优解的结果

> 贪心算法最佳应用-集合覆盖 

1) 假设存在如下表的需要付费的广播台，以及广播台信号可以覆盖的地区。 如何选择最少的广播台，让所有的地区都可以接收到信号
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200811122606.png" alt="image-20200811122604321" style="zoom: 80%;" />

2) 思路分析: 

 如何找出覆盖所有地区的广播台的集合呢，使用穷举法实现,列出每个可能的广播台的集合，这被称为幂集。假设总的有 n 个广播台，则广播台的组合总共有 2ⁿ -1 个,假设每秒可以计算 10 个子集， 如图:
![image-20200811122637604](https://gitee.com/xudongyin/img/raw/master/img/20200811122639.png)

 使用贪婪算法，效率高: 

1) 目前并没有算法可以快速计算得到准备的值， 使用贪婪算法，则可以得到非常接近的解，并且效率高。选择策略上，因为需要覆盖全部地区的最小集合: 

2) 遍历所有的广播电台, 找到一个覆盖了最多未覆盖的地区的电台(此电台可能包含一些已覆盖的地区，但没有关系） 

3) 将这个电台加入到一个集合中(比如 ArrayList), 想办法把该电台覆盖的地区在下次比较时去掉。 

4) 重复第 1 步**直到覆盖了全部的**地区 

分析的图解: 
![image-20200811151347244](https://gitee.com/xudongyin/img/raw/master/img/20200811151350.png)

```java
package Greedy;

import java.util.ArrayList;
import java.util.HashMap;
import java.util.HashSet;

public class GreedyAlgorithm {
    public static void main(String[] args) {
        //创建广播电台,放入到Map
        HashMap<String, HashSet<String>> broadcasts = new HashMap<>();
        //将各个电台放入到broadcasts
        HashSet<String> hashSet1 = new HashSet<String>();
        hashSet1.add("北京");
        hashSet1.add("上海");
        hashSet1.add("天津");

        HashSet<String> hashSet2 = new HashSet<String>();
        hashSet2.add("广州");
        hashSet2.add("北京");
        hashSet2.add("深圳");

        HashSet<String> hashSet3 = new HashSet<String>();
        hashSet3.add("成都");
        hashSet3.add("上海");
        hashSet3.add("杭州");

        HashSet<String> hashSet4 = new HashSet<String>();
        hashSet4.add("上海");
        hashSet4.add("天津");

        HashSet<String> hashSet5 = new HashSet<String>();
        hashSet5.add("杭州");
        hashSet5.add("大连");
        //加入到map
        broadcasts.put("K1", hashSet1);
        broadcasts.put("K2", hashSet2);
        broadcasts.put("K3", hashSet3);
        broadcasts.put("K4", hashSet4);
        broadcasts.put("K5", hashSet5);
        //allAreas 存放所有的地区
        ArrayList<String> allAreas = new ArrayList<>();
        allAreas.add("北京");
        allAreas.add("上海");
        allAreas.add("天津");
        allAreas.add("广州");
        allAreas.add("深圳");
        allAreas.add("成都");
        allAreas.add("杭州");
        allAreas.add("大连");

        //创建ArrayList, 存放选择的电台集合
        ArrayList<String> selects = new ArrayList<>();
        //定义一个临时的集合， 在遍历的过程中，存放遍历过程中的电台覆盖的地区和当前还没有覆盖的地区的交集
        HashSet<String> tempSet = new HashSet<String>();
        //定义给maxKey ， 保存在一次遍历过程中，能够覆盖最大未覆盖的地区对应的电台的key
        //如果maxKey 不为null , 则会加入到 selects
        String maxKey = null;
        //遍历 broadcasts, 取出对应key
        while (allAreas.size() > 0) {// 如果allAreas 不为0, 则表示还没有覆盖到所有的地区
            //每进行一次while,需要
            maxKey = null;
            for (String key : broadcasts.keySet()) {
                //每进行一次for,需要
                tempSet.clear();
                //当前这个key能够覆盖的地区
                HashSet<String> areas = broadcasts.get(key);
                tempSet.addAll(areas);
                //求出tempSet 和   allAreas 集合的交集, 交集会赋给 tempSet
                tempSet.retainAll(allAreas);
                //如果当前这个集合包含的未覆盖地区的数量，比maxKey指向的集合地区还多
                //就需要重置maxKey
                // tempSet.size() >broadcasts.get(maxKey).size()) 体现出贪心算法的特点,每次都选择最优的
                if (tempSet.size() > 0 &&
                        (maxKey == null || tempSet.size() > broadcasts.get(maxKey).size())) {
                    maxKey = key;
                }
            }
            //maxKey != null, 就应该将maxKey 加入selects
            if (maxKey != null) {
                selects.add(maxKey);
                //将maxKey指向的广播电台覆盖的地区，从 allAreas 去掉
                allAreas.removeAll(broadcasts.get(maxKey));
            }
        }
        System.out.println("得到的选择结果是" + selects);//[K1,K2,K3,K5]
    }
}
```

## 普里姆算法 

> 应用场景-修路问题 

 **看一个应用场景和问题：**
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200811161001.png" alt="image-20200811160959887" style="zoom:67%;" /> 

**1)** **有胜利乡有** **7** **个村庄(A, B, C, D, E, F, G)，现在需要修路把 7 个村庄连通** 

**2)** **各个村庄的距离用边线表示(权) ，比如 A – B** **距离** **5** **公里** 

**3)** **问：如何修路保证各个村庄都能连通，并且总的修建公路总里程最短?** 

**思路:** **将** **10** **条边，连接即可，但是总的里程数不是最小.** 

**正确的思路，就是尽可能的选择少的路线，并且每条路线最小，保证总里程数最少.** 

> 最小生成树 

**修路问题本质就是就是最小生成树问题， 先介绍一下最小生成树(Minimum Cost Spanning Tree)，简称MST。** 

**给定一个带权的无向连通图,如何选取一棵生成树,使树上所有边上权的总和为最小,这叫最小生成树** 

**1) N** **个顶点，一定有** **N-1** **条边** 

**2)** **包含全部顶点** 

**3) N-1** **条边都在图中** 

**4)** **举例说明(如图:)**
![image-20200811161220163](https://gitee.com/xudongyin/img/raw/master/img/20200811161224.png)

**5)** **求最小生成树的算法主要是普里姆算法和克鲁斯卡尔算法** 

> 普里姆算法介绍 

普利姆(Prim)算法求最小生成树，也就是在包含 n 个顶点的连通图中，找出只有(n-1)条边包含所有 n 个顶点的连通子图，也就是所谓的极小连通子图 

普利姆的算法如下: 

1) 设 G=(V,E)是连通网，T=(U,D)是最小生成树，V,U 是顶点集合，E,D 是边的集合 

2) 若从顶点 u 开始构造最小生成树，则从集合 V 中取出顶点 u 放入集合 U 中，标记顶点 v 的 visited[u]=1 

3) 若集合 U 中顶点 ui 与集合 V-U 中的顶点 vj 之间存在边，则寻找这些边中权值最小的边，但不能构成回路，将顶点 vj 加入集合 U 中，将边（ui,vj）加入集合 D 中，标记 visited[vj]=1 

4) 重复步骤②，直到 U 与 V 相等，即所有顶点都被标记为访问过，此时 D 中有 n-1 条边 

5) 提示: 单独看步骤很难理解，我们通过代码来讲解，比较好理解. 

6) 图解普利姆算法
![image-20200811161338562](https://gitee.com/xudongyin/img/raw/master/img/20200811161340.png)

```java
package Prim;

import java.util.Arrays;

public class Prim {
    public static void main(String[] args) {
        //测试看看图是否创建ok
        char[] data = new char[]{'A','B','C','D','E','F','G'};
        int verxs = data.length;
        //邻接矩阵的关系使用二维数组表示,10000这个大数，表示两个点不联通
        int [][]weight=new int[][]{
                {10000,5,7,10000,10000,10000,2},
                {5,10000,10000,9,10000,10000,3},
                {7,10000,10000,10000,8,10000,10000},
                {10000,9,10000,10000,10000,4,10000},
                {10000,10000,8,10000,10000,5,4},
                {10000,10000,10000,4,5,10000,6},
                {2,3,10000,10000,4,6,10000}};

        //创建MGraph对象
        Graph graph = new Graph(verxs);
        //创建一个MinTree对象
        MinTree minTree = new MinTree();
        minTree.createGraph(graph, verxs, data, weight);
        //输出
        minTree.showGraph(graph);
        //测试普利姆算法
        minTree.prim(graph, 0);
    }
}
//创建最小生成树->村庄的图
class MinTree {
    //创建图的邻接矩阵
    /**
     * @param graph 图对象
     * @param vertex 图的顶点数
     * @param data 图的各个顶点的值
     * @param weight 图的邻接矩阵
     */
    public void createGraph(Graph graph, int vertex, char[] data, int[][] weight) {
        for (int i = 0; i < vertex; i++) {
            graph.data[i] = data[i];
            for (int j = 0; j < vertex; j++) {
                graph.weight[i][j] = weight[i][j];
            }
        }
    }

    //显示图的邻接矩阵
    public void showGraph(Graph graph) {
        for (int[] link : graph.weight) {
            System.out.println(Arrays.toString(link));
        }
    }

    //普利姆算法,得到最小生成树
    /**
     * @param graph 图
     * @param v 表示从图的第几个顶点开始生成'A'->0 'B'->1...
     */
    public void prim(Graph graph, int v) {
        //visited[] 标记结点(顶点)是否被访问过
        int[] visited = new int[graph.vertex];
        //visited[] 默认元素的值都是0, 表示没有访问过
//    for(int i =0; i <graph.verxs; i++) {
//       visited[i] = 0;
//    }
        //把当前这个结点标记为已访问
        visited[v] = 1;
        //h1 和 h2 记录两个顶点的下标
        int h1 = -1;
        int h2 = -1;
        int minWeight = 10000;//将 minWeight 初始成一个大数，后面在遍历过程中，会被替换
        for (int k = 1; k < graph.vertex; k++) {//因为有 graph.vertex 顶点，普利姆算法结束后，有 graph.vertex-1边
            //这个是确定每一次生成的子图 ，和哪个结点的距离最近
            for (int i = 0; i < graph.vertex; i++) {// i结点表示被访问过的结点
                for (int j = 0; j < graph.vertex; j++) {//j结点表示还没有访问过的结点
                    //无论是否访问过的结点都会被遍历
                    if (visited[i] == 1 && visited[j] == 0 && graph.weight[i][j] < minWeight) {
                        //替换minWeight(寻找已经访问过的结点和未访问过的结点间的权值最小的边)
                        minWeight = graph.weight[i][j];
                        h1 = i;
                        h2 = j;
                    }
                }
            }
            //找到一条边是最小
            System.out.println("边<" + graph.data[h1] + "," + graph.data[h2] + "> 权值:" + minWeight);
            //将当前这个结点标记为已经访问
            visited[h2] = 1;
            //minWeight 重新设置为最大值 10000
            minWeight = 10000;
        }
    }

}
//图
class Graph {
    int vertex;//表示图的节点个数
    char[] data;//存放结点数据
    int[][] weight;//存放边，就是我们的邻接矩阵

    public Graph(int num) {
        this.vertex = num;
        data = new char[num];
        weight = new int[num][num];
    }
}
```

## 克鲁斯卡尔算法

> 克鲁斯卡尔算法介绍

1) 克鲁斯卡尔(Kruskal)算法，是用来求加权连通图的最小生成树的算法。 

2) 基本思想：按照权值从小到大的顺序选择 n-1 条边，并保证这 n-1 条边不构成回路 

3) 具体做法：首先构造一个只含 n 个顶点的森林，然后依权值从小到大从连通网中选择边加入到森林中，并使森林中不产生回路，直至森林变成一棵树为止 

> 应用场景-公交站问题 

看一个应用场景和问题： 
![image-20200811173252324](https://gitee.com/xudongyin/img/raw/master/img/20200811173254.png)

1) 某城市新增 7 个站点(A, B, C, D, E, F, G) ，现在需要修路把 7 个站点连通 

2) 各个站点的距离用边线表示(权) ，比如 A – B 距离 12 公里 

3) 问：如何修路保证各个站点都能连通，并且总的修建公路总里程最短?

> **克鲁斯卡尔算法图解说明** 

以城市公交站问题来图解说明 克鲁斯卡尔算法的原理和步骤： 

在含有 n 个顶点的连通图中选择 n-1 条边，构成一棵极小连通子图，并使该连通子图中 n-1 条边上权值之和达到最小，则称其为连通网的最小生成树。
![image-20200811173354307](https://gitee.com/xudongyin/img/raw/master/img/20200811173356.png) 

例如，对于如上图 G4 所示的连通网可以有多棵权值总和不相同的生成树。
![image-20200811173405806](https://gitee.com/xudongyin/img/raw/master/img/20200811173407.png)

以上图 G4 为例，来对克鲁斯卡尔进行演示(假设，用数组 R 保存最小生成树结果)。
![image-20200811173527644](https://gitee.com/xudongyin/img/raw/master/img/20200811173529.png)

**第** **1** **步**：将边<E,F>加入 R 中。 边<E,F>的权值最小，因此将它加入到最小生成树结果 R 中。 

**第** **2** **步**：将边<C,D>加入 R 中。 上一步操作之后，边<C,D>的权值最小，因此将它加入到最小生成树结果 R 中。 

**第** **3** **步**：将边<D,E>加入 R 中。 上一步操作之后，边<D,E>的权值最小，因此将它加入到最小生成树结果 R 中。 

**第** **4** **步**：将边<B,F>加入 R 中。 上一步操作之后，边<C,E>的权值最小，但<C,E>会和已有的边构成回路；因此，跳过边<C,E>。同理，跳过边<C,F>。将边<B,F>加入到最小生成树结果 R 中。 

**第** **5** **步**：将边<E,G>加入 R 中。 上一步操作之后，边<E,G>的权值最小，因此将它加入到最小生成树结果 R 中。 

**第** **6** **步**：将边<A,B>加入 R 中。 上一步操作之后，边<F,G>的权值最小，但<F,G>会和已有的边构成回路；因此，跳过边<F,G>。同理，跳过边<B,C>。将边<A,B>加入到最小生成树结果 R 中。 

此时，最小生成树构造完成！它包括的边依次是：**<E,F>，<C,D>，<D,E>，<B,F>，<E,G>，<A,B>。**

> **克 鲁 斯 卡 尔 算 法 分 析** 

根据前面介绍的克鲁斯卡尔算法的基本思想和做法，我们能够了解到，克鲁斯卡尔算法重点需要解决的以下两个问题：

**问题一** 对图的所有边按照权值大小进行排序。 

**问题二** 将边添加到最小生成树中时，怎么样判断是否形成了回路。 

问题一很好解决，采用排序算法进行排序即可。 

问题二，处理方式是：记录顶点在"最小生成树"中的终点，顶点的终点是"在最小生成树中与它连通的最大顶点"。然后每次需要将一条边添加到最小生存树时，判断该边的两个顶点的终点是否重合，重合的话则会构成回路。

> **如 何 判 断 是 否 构 成 回 路**

![image-20200811174045453](https://gitee.com/xudongyin/img/raw/master/img/20200811174047.png)

在将<E,F> <C,D> <D,E>加入到最小生成树 R 中之后，这几条边的顶点就都有了终点： 

**(01)** C 的终点是 F。 
**(02)** D 的终点是 F。 
**(03)** E 的终点是 F。 
**(04)** F 的终点是 F。 

关于终点的说明：（**加入最小生成树后才有终点**） 

1) 就是将所有顶点按照从小到大的顺序排列好之后；某个顶点的终点就是"**与它连通的最大顶点**"。 

2) 因此，接下来，虽然<C,E>是权值最小的边。但是 C 和 E 的终点都是 F，即它们的终点相同，因此，将<C,E>加入最小生成树的话，会形成回路。这就是判断回路的方式。也就是说，**我们加入的边的两个顶点不能都指向同一个终点，否则将构成回路**。【后面有代码说明】

```java
package Kruskal;

import java.util.Arrays;

public class KruskalCase {
    private int edgeNum;//边的个数
    private char[] vertexs;//顶点数组
    private int[][] matrix;//邻接矩阵
    //使用 INF 表示两个顶点不能连通
    private static final int INF = Integer.MAX_VALUE;
    public static void main(String[] args) {
        char[] vertexs = {'A', 'B', 'C', 'D', 'E', 'F', 'G'};
        //克鲁斯卡尔算法的邻接矩阵
        int matrix[][] = {
                /*A*//*B*//*C*//*D*//*E*//*F*//*G*/
                /*A*/ {   0,  12, INF, INF, INF,  16,  14},
                /*B*/ {  12,   0,  10, INF, INF,   7, INF},
                /*C*/ { INF,  10,   0,   3,   5,   6, INF},
                /*D*/ { INF, INF,   3,   0,   4, INF, INF},
                /*E*/ { INF, INF,   5,   4,   0,   2,   8},
                /*F*/ {  16,   7,   6, INF,   2,   0,   9},
                /*G*/ {  14, INF, INF, INF,   8,   9,   0}};
        KruskalCase kruskalCase = new KruskalCase(vertexs, matrix);
        kruskalCase.print();
        kruskalCase.kruskal();
    }

    public KruskalCase(char[] vertexs, int[][] matrix) {
        //初始化顶点数和边的个数
        int vlen = vertexs.length;
        //初始化顶点, 复制拷贝的方式
        this.vertexs = new char[vlen];
        for (int i = 0; i < vlen; i++) {
            this.vertexs[i] = vertexs[i];
        }
        //也可以直接赋值
//        this.vertexs = vertexs;

        //初始化边, 使用的是复制拷贝的方式
        this.matrix = new int[vlen][vlen];
        for(int i = 0; i < vlen; i++) {
            for(int j= 0; j < vlen; j++) {
                this.matrix[i][j] = matrix[i][j];
            }
        }
        //统计边数
        for (int i = 0; i < vlen; i++) {
            for (int j = 1 + i; j < vlen; j++) {
                if (this.matrix[i][j] != INF) {
                    edgeNum++;
                }
            }
        }
    }
    public void kruskal() {
        int index = 0;//表示最后结果数组的索引
        int[] ends = new int[edgeNum];//用于保存"已有最小生成树" 中的每个顶点在最小生成树中的终点
        //创建结果数组, 保存最后的最小生成树
        EData[] rets = new EData[edgeNum];
        //获取图中 所有的边的集合 ， 一共有12边
        EData[] edges = getEdges();
        System.out.println("图的边的集合=" + Arrays.toString(edges) + " 共"+ edges.length); //12
        //按照边的权值大小进行排序(从小到大)
        sortEdges(edges);
        //遍历edges 数组，将边添加到最小生成树中时，判断是准备加入的边否形成了回路，如果没有，就加入 rets, 否则不能加入
        for (int i = 0; i < edges.length; i++) {
            //获取到第i条边的第一个顶点(起点)
            int p1 = getPosition(edges[i].start);//p1=4
            //获取到第i条边的第2个顶点
            int p2 = getPosition(edges[i].end);//p2 = 5
            //获取p1这个顶点在已有最小生成树中的终点
            int m = getEnd(ends, p1); //m = 4
            //获取p2这个顶点在已有最小生成树中的终点
            int n = getEnd(ends, p2); // n = 5
            //是否构成回路
            if (m != n) { //没有构成回路
                ends[m] = n; // 设置m 在"已有最小生成树"中的终点 <E,F> [0,0,0,0,5,0,0,0,0,0,0,0]
                rets[index++] = edges[i];//有一条边加入到rets数组
            }
        }
        //<E,F> <C,D> <D,E> <B,F> <E,G> <A,B>。
        //统计并打印 "最小生成树", 输出  rets
        System.out.println("最小生成树为");
        for(int i = 0; i < index; i++) {
            System.out.println(rets[i]);
        }

    }

    //打印邻接矩阵
    public void print() {
        for (int i = 0; i < vertexs.length; i++) {
            for (int j = 0; j < vertexs.length; j++) {
                System.out.printf("%12d", matrix[i][j]);
            }
            System.out.println();
        }
    }
    /**
     * 功能：对边进行排序处理, 冒泡排序
     * @param edges 边的集合
     */
    private void sortEdges(EData[] edges) {
        for (int i = 0; i < edges.length-1; i++) {
            for (int j = 0; j < edges.length - 1 - i; j++) {
                if (edges[j].weight > edges[j + 1].weight) {
                    EData temp = edges[j];
                    edges[j] = edges[j + 1];
                    edges[j + 1] = temp;
                }
             }
        }
    }
    /**
     *
     * @param ch 顶点的值，比如'A','B'
     * @return 返回ch顶点对应的下标，如果找不到，返回-1
     */
    private int getPosition(char ch) {
        for (int i = 0; i < vertexs.length; i++) {
            if (vertexs[i] == ch) {
                return i;
            }
        }
        //找不到,返回-1
        return -1;
    }
    /**
     * 功能: 获取图中边，放到EData[] 数组中，后面我们需要遍历该数组
     * 是通过matrix 邻接矩阵来获取
     * EData[] 形式 [['A','B', 12], ['B','F',7], .....]
     * @return EData[] 数组
     */
    public EData[] getEdges() {
        int index = 0;
        EData[] edges = new EData[edgeNum];
        for (int i = 0; i < vertexs.length; i++) {
            for (int j = i + 1; j < vertexs.length; j++) {
                if (matrix[i][j] != INF) {
                    edges[index++] = new EData(vertexs[i], vertexs[j], matrix[i][j]);
                }
            }
        }
        return edges;
    }
    /**
     * 功能: 获取下标为i的顶点的终点(), 用于后面判断两个顶点的终点是否相同
     * @param ends ： 数组就是记录了各个顶点对应的终点是哪个,ends 数组是在遍历过程中，逐步形成
     * @param i : 表示传入的顶点对应的下标
     * @return 返回的就是 下标为i 的这个顶点对应的终点的下标, 一会回头还有来理解
     */
    private int getEnd(int[] ends, int i) { // i = 4 [0,0,0,0,5,0,0,0,0,0,0,0]
        while (ends[i] != 0) {//顶点有终点
            i = ends[i]; 
        }
        return i;
    }
}
//创建一个类EData ，它的对象实例就表示一条边
class EData {
    char start; //边的一个点
    char end; //边的另外一个点
    int weight; //边的权值

    public EData(char start, char end, int weight) {
        this.start = start;
        this.end = end;
        this.weight = weight;
    }

    @Override
    public String toString() {
        return "EData [<" + start + ", " + end + ">= " + weight + "]";
    }
}
```

##  迪杰斯特拉算法

> 迪杰斯特拉(Dijkstra)算法介绍 

迪杰斯特拉(Dijkstra)算法是**典型最短路径算法**，用于计算一个结点到其他结点的最短路径。它的主要特点是以起始点为中心向外层层扩展(**广度优先搜索思想**)，直到扩展到终点为止。 

> **迪杰斯特拉(Dijkstra)算法过程** 

1) 设置出发顶点为 v，顶点集合 V{v1,v2,vi...}，v 到 V 中各顶点的距离构成距离集合 Dis，Dis{d1,d2,di...}，Dis 集合记录着 v 到图中各顶点的距离(到自身可以看作 0，v 到 vi 距离对应为 di) 

2) 从 Dis 中选择值最小的 di 并移出 Dis 集合，同时移出 V 集合中对应的顶点 vi，此时的 v 到 vi 即为最短路径

3) 更新 Dis 集合，更新规则为：比较 v 到 V 集合中顶点的距离值，与 v 通过 vi 到 V 集合中顶点的距离值，保留值较小的一个(同时也应该更新顶点的前驱节点为 vi，表明是通过 vi 到达的) 

4) 重复执行两步骤，直到最短路径顶点为目标顶点即可结束 

> 迪杰斯特拉(Dijkstra)算法最佳应用-最短路径 

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200811225450.png" alt="image-20200811225448678" style="zoom:67%;" />

1) 战争时期，胜利乡有 7 个村庄(A, B, C, D, E, F, G) ，现在有六个邮差，从 G 点出发，需要分别把邮件分别送到 A, B, C , D, E, F 六个村庄 

2) 各个村庄的距离用边线表示(权) ，比如 A – B 距离 5 公里 

3) 问：如何计算出 G 村庄到 其它各个村庄的最短距离? 

4) 如果从其它点出发到各个点的最短距离又是多少? 

5) 使用图解的方式分析了迪杰斯特拉(Dijkstra)算法 思路
![image-20200812102551742](https://gitee.com/xudongyin/img/raw/master/img/20200812102652.png)
![image-20200812102641980](https://gitee.com/xudongyin/img/raw/master/img/20200812102645.png)

```java
package Dijkstra;

import java.util.Arrays;

public class Dijkstra {
    public static void main(String[] args) {
        char[] vertex = { 'A', 'B', 'C', 'D', 'E', 'F', 'G' };
        //邻接矩阵
        int[][] matrix = new int[vertex.length][vertex.length];
        final int N = 65535;// 表示不可以连接
        matrix[0]=new int[]{N,5,7,N,N,N,2};
        matrix[1]=new int[]{5,N,N,9,N,N,3};
        matrix[2]=new int[]{7,N,N,N,8,N,N};
        matrix[3]=new int[]{N,9,N,N,N,4,N};
        matrix[4]=new int[]{N,N,8,N,N,5,4};
        matrix[5]=new int[]{N,N,N,4,5,N,6};
        matrix[6]=new int[]{2,3,N,N,4,6,N};
        //创建 Graph对象
        Graph graph = new Graph(vertex, matrix);
        //测试, 看看图的邻接矩阵是否ok
        graph.showGraph();
        graph.dsj(6);
        graph.showDijkstra();
    }

}

class Graph {
    private int[][] matrix; // 邻接矩阵
    private char[] vertex; // 顶点数组
    private VisitedVertex vv; //已经访问的顶点的集合

    public Graph(char[] vertex, int[][] matrix) {
        this.vertex = vertex;
        this.matrix = matrix;
    }

    public void showGraph() {
        for (int[] link : matrix) {
            System.out.println(Arrays.toString(link));
        }
    }

    public void showDijkstra() {
        vv.show();
    }
    //迪杰斯特拉算法实现
    /**
     *
     * @param index 表示出发顶点对应的下标
     */
    public void dsj(int index) {
        vv = new VisitedVertex(vertex.length, index);
        update(index);
        for (int i = 1; i < vertex.length; i++) {
            index = vv.updateArr(); // 选择并返回新的访问顶点
            update(index);//更新index下标顶点到周围顶点的距离（从开始点的距离）和周围顶点的前驱顶点,
        }
    }
    //更新index下标顶点到周围顶点的距离和周围顶点的前驱顶点,
    public void update(int index) {
        int len = 0;
        //根据遍历我们的邻接矩阵的  matrix[index]行
        for (int i = 0; i < matrix[index].length; i++) {
            // len 含义是 : 出发顶点到index顶点的距离 + 从index顶点到i顶点的距离的和
            len = vv.getDis(index) + matrix[index][i];
            // 如果j顶点没有被访问过，并且 len 小于出发顶点到i顶点的距离，就需要更新
            if (!vv.in(i) && len < vv.getDis(i)) {
                vv.updateDis(i, len);//更新出发顶点到 i 顶点的距离
                vv.updatePre(i, index);//更新 i 顶点的前驱为index顶点
            }
        }
    }
}
// 已访问顶点集合
class VisitedVertex {
    // 记录各个顶点是否访问过 1表示访问过,0未访问,会动态更新
    public int[] already_arr;
    // 每个下标对应的值为前一个顶点下标, 会动态更新
    public int[] pre_visited;
    // 记录出发顶点到其他所有顶点的距离,比如G为出发顶点，就会记录G到其它顶点的距离，会动态更新，求的最短距离就会存放到dis
    public int[] dis;
    //构造器
    /**
     * @param length :表示顶点的个数
     * @param index: 出发顶点对应的下标, 比如G顶点，下标就是6
     */
    public VisitedVertex(int length, int index) {
        this.already_arr = new int[length];
        this.dis = new int[length];
        this.pre_visited = new int[length];
        //初始化 dis数组
        Arrays.fill(dis, 65535);
        this.already_arr[index] = 1; //设置出发顶点被访问过
        this.dis[index] = 0; //设置出发顶点的访问距离为0
    }

    /**
     * 功能: 判断index顶点是否被访问过
     *
     * @param index
     * @return 如果访问过，就返回true, 否则访问false
     */
    public boolean in(int index) {
        return already_arr[index] == 1;
    }

    /**
     * 功能: 更新出发顶点到index顶点的距离
     * @param index
     * @param len
     */
    public void updateDis(int index, int len) {
        dis[index] = len;
    }
    /**
     * 功能: 更新pre这个顶点的前驱顶点为index顶点
     * @param pre
     * @param index
     */
    public void updatePre(int pre, int index) {
        pre_visited[pre] = index;
    }
    /**
     * 功能:返回出发顶点到index顶点的距离
     * @param index
     */
    public int getDis(int index) {
        return dis[index];
    }
    /**
     * 继续选择并返回新的访问顶点， 比如这里的G 完后，就是 A点作为新的访问顶点(注意不是出发顶点)
     * @return
     */
    public int updateArr() {
        int min = 65535, index = 0;
        //找到可以到达的还未被访问的距离最近的顶点 i
        for (int i = 0; i < already_arr.length; i++) {
            if (already_arr[i] == 0 && dis[i] < min) {
                min = dis[i];
                index = i;
            }
        }
        //更新 index 顶点被访问过
        already_arr[index] = 1;
        return index;
    }

    public void show() {
        for (int link : already_arr) {
            System.out.print(link+" ");
        }
        System.out.println();
        for (int link : pre_visited) {
            System.out.print(link+" ");
        }
        System.out.println();
        for (int link : dis) {
            System.out.print(link+" ");
        }
        System.out.println();
        //为了好看最后的最短距离，我们处理
        char[] vertex = { 'A', 'B', 'C', 'D', 'E', 'F', 'G' };
        int count = 0;
        for (int i : dis) {
            if (i != 65535) {
                System.out.print(vertex[count] + "("+i+") ");
            } else {
                System.out.println("N ");
            }
            count++;
        }
    }
}
```

## 弗洛伊德算法 

> 弗洛伊德(Floyd)算法介绍 

1) 和 Dijkstra 算法一样，弗洛伊德(Floyd)算法也是一种用于寻找给定的加权图中顶点间最短路径的算法。该算法名称以创始人之一、1978 年图灵奖获得者、斯坦福大学计算机科学系教授罗伯特·弗洛伊德命名 

2) 弗洛伊德算法(Floyd)计算图中各个顶点之间的最短路径 

3) 迪杰斯特拉算法用于计算图中某一个顶点到其他顶点的最短路径。 

4) 弗洛伊德算法 VS 迪杰斯特拉算法：**迪杰斯特拉算法通过选定的被访问顶点，求出从出发访问顶点到其他顶点的最短路径；弗洛伊德算法中每一个顶点都是出发访问点，所以需要将每一个顶点看做被访问顶点，求出从每一个顶点到其他顶点的最短路径。** 

> 弗洛伊德(Floyd)算法图解分析 

1) 设置顶点 vi 到顶点 vk 的最短路径已知为 Lik，顶点 vk 到 vj 的最短路径已知为 Lkj，顶点 vi 到 vj 的路径为 Lij， 则 vi 到 vj 的最短路径为：min((Lik+Lkj),Lij)，vk 的取值为图中所有顶点，则可获得 vi 到 vj 的最短路径 

2) 至于 vi 到 vk 的最短路径 Lik 或者 vk 到 vj 的最短路径 Lkj，是以同样的方式获得

3) 弗洛伊德(Floyd)算法图解分析-举例说明

示例：求最短路径为例说明 
![image-20200812165941267](https://gitee.com/xudongyin/img/raw/master/img/20200812165943.png)
![image-20200812165955896](https://gitee.com/xudongyin/img/raw/master/img/20200812165957.png)

弗洛伊德算法的步骤： 

第一轮循环中，以 A(下标为：0)作为中间顶点【即把 A 作为中间顶点的所有情况都进行遍历, 就会得到更新距离表 和 前驱关系】， 距离表和前驱关系更新为：
![image-20200812170019712](https://gitee.com/xudongyin/img/raw/master/img/20200812170021.png)

分析如下： 

1) 以 A 顶点作为中间顶点是，B->A->C 的距离由 N->9，同理 C 到 B；C->A->G 的距离由 N->12，同理 G 到 C 

2) 更换中间顶点，循环执行操作，直到所有顶点都作为中间顶点更新后，计算结束
![image-20200812170043628](https://gitee.com/xudongyin/img/raw/master/img/20200812170045.png)
![image-20200812170107831](https://gitee.com/xudongyin/img/raw/master/img/20200812170112.png)

> 弗洛伊德(Floyd)算法最佳应用-最短路径 

<img src="https://gitee.com/xudongyin/img/raw/master/img/20200812170230.png" alt="image-20200812170228365" style="zoom:67%;" />

1) 胜利乡有 7 个村庄(A, B, C, D, E, F, G) 

2) 各个村庄的距离用边线表示(权) ，比如 A – B 距离 5 公里 

3) 问：如何计算出各村庄到 其它各村庄的最短距离? 

4) 代码实现 

```java
package Floyd;

import java.util.Arrays;

public class Floyd {
    public static void main(String[] args) {
// 测试看看图是否创建成功
        char[] vertex = { 'A', 'B', 'C', 'D', 'E', 'F', 'G' };
        //创建邻接矩阵
        int[][] matrix = new int[vertex.length][vertex.length];
        final int N = 65535;
        matrix[0] = new int[] { 0, 5, 7, N, N, N, 2 };
        matrix[1] = new int[] { 5, 0, N, 9, N, N, 3 };
        matrix[2] = new int[] { 7, N, 0, N, 8, N, N };
        matrix[3] = new int[] { N, 9, N, 0, N, 4, N };
        matrix[4] = new int[] { N, N, 8, N, 0, 5, 4 };
        matrix[5] = new int[] { N, N, N, 4, 5, 0, 6 };
        matrix[6] = new int[] { 2, 3, N, N, 4, 6, 0 };

        //创建 Graph 对象
        Graph graph = new Graph(vertex.length, matrix, vertex);
        graph.floyd();
        graph.show();
    }
}

class Graph {
    private char[] vertex;// 存放顶点的数组
    private int[][] dis;// 保存，从各个顶点出发到其它顶点的距离，最后的结果，也是保留在该数组
    private int[][] pre;// 保存到达目标顶点的前驱顶点
    // 构造器
    /**
     * @param length  大小
     * @param matrix  邻接矩阵
     * @param vertex  顶点数组
     */
    public Graph(int length,int[][] matrix,char[] vertex) {
        this.vertex = vertex;
        this.pre = new int[length][length];
        this.dis = matrix;
        // 对pre数组初始化, 注意存放的是前驱顶点的下标
        for (int i = 0; i < length; i++) {
            Arrays.fill(pre[i], i);
        }
    }
    // 显示pre数组和dis数组
    public void show() {
        //为了显示便于阅读，我们优化一下输出
        char[] vertex = { 'A', 'B', 'C', 'D', 'E', 'F', 'G' };
        for (int k = 0; k < dis.length; k++) {
            for (int i = 0; i < pre[k].length; i++) {
                System.out.print(vertex[pre[k][i]]);
            }
            System.out.println();
            for (int i = 0; i < dis[k].length; i++) {
                System.out.print("(" + vertex[k] + "到" + vertex[i] + "的最短路径是" + dis[k][i] + ")");
            }
            System.out.println();
            System.out.println();
        }
    }
    //弗洛伊德算法, 比较容易理解，而且容易实现
    public void floyd() {
        int len = 0;//变量保存距离
        //对中间顶点遍历， k 就是中间顶点的下标 [A, B, C, D, E, F, G]
        for (int k = 0; k < dis.length; k++) {
            //从i顶点开始出发 [A, B, C, D, E, F, G]
            for (int i = 0; i < dis.length; i++) {
                //到达j顶点 // [A, B, C, D, E, F, G]
                for (int j = 0; j < dis.length; j++) {
                    len = dis[i][k] + dis[k][j];// => 求出从i 顶点出发，经过 k中间顶点，到达 j 顶点距离
                    if (len < dis[i][j]) {//如果len小于 dis[i][j]
                        dis[i][j] = len;//更新距离
                        pre[i][j] = pre[k][j];//更新前驱顶点
                    }
                }
            }
        }
    }
}
```

马踏棋盘算法 

> 马踏棋盘算法介绍和游戏演示 

1) 马踏棋盘算法也被称为骑士周游问题 

2) 将马随机放在国际象棋的 8×8 棋盘 Board[0～7][0～7]的某个方格中，马按走棋规则(马走日字)进行移动。要求每个方格只进入一次，走遍棋盘上全部 64 个方格 

3) 游戏演示: http://www.4399.com/flash/146267_2.htm
<img src="https://gitee.com/xudongyin/img/raw/master/img/20200812223736.png" alt="image-20200812223734351" style="zoom: 50%;" />

> 马踏棋盘游戏代码实现 

1) 马踏棋盘问题(骑士周游问题)实际上是图的深度优先搜索(DFS)的应用。 

2) 如果使用回溯（就是深度优先搜索）来解决，假如马儿踏了 53 个点，如图：走到了第 53 个，坐标（1,0），发现已经走到尽头，没办法，那就只能回退了，查看其他的路径，就在棋盘上不停的回溯…… ，**思路分析**+代码 实现 

 对第一种实现方式的思路图解
![image-20200812223838554](https://gitee.com/xudongyin/img/raw/master/img/20200812223840.png)

3) 分析第一种方式的问题，并使用贪心算法（greedyalgorithm）进行优化。解决马踏棋盘问题. 
![image-20200812223912052](https://gitee.com/xudongyin/img/raw/master/img/20200812223913.png)

4) 使用前面的游戏来验证算法是否正确。 

5) 代码实现 

```java
package HorseChessboard;

import java.awt.*;
import java.util.ArrayList;
import java.util.Comparator;

public class HorseChessboard {
    public static int X;// 棋盘的列数
    public static int Y;// 棋盘的行数
    //创建一个数组，标记棋盘的各个位置是否被访问过
    public static boolean[] visited;
    //使用一个属性，标记是否棋盘的所有位置都被访问
    private static boolean finished; // 如果为true,表示成功

    public static void main(String[] args) {
        System.out.println("骑士周游算法，开始运行~~");
        //测试骑士周游算法是否正确
        X = 8;
        Y = 8;
        int row = 1; //马儿初始位置的行，从1开始编号
        int column = 1; //马儿初始位置的列，从1开始编号
        //创建棋盘
        int[][] chessboard = new int[X][Y];
        visited = new boolean[X * Y];//初始值都是false
        //测试一下耗时
        long start = System.currentTimeMillis();
        traversalChessboard(chessboard, row - 1, column - 1, 1);
        long end = System.currentTimeMillis();
        System.out.println("共耗时: " + (end - start) + " 毫秒");

        //输出棋盘的最后情况
        for(int[] rows : chessboard) {
            for(int step: rows) {
                System.out.print(step + "\t");
            }
            System.out.println();
        }
    }
    /**
     * 完成骑士周游问题的算法
     * @param chessboard 棋盘
     * @param row 马儿当前的位置的行 从0开始
     * @param column 马儿当前的位置的列  从0开始
     * @param step 是第几步 ,初始位置就是第1步
     */
    public static void traversalChessboard(int[][] chessboard, int row, int column, int step) {
        chessboard[row][column] = step;//标记步数
        //row = 4 X = 8 column = 4 = 4 * 8 + 4 = 36
        visited[row * X + column] = true;//标记该位置已经访问
        //获取当前位置可以走的下一个位置的集合
        ArrayList<Point> ps = next(new Point(column, row));
        //对ps进行排序,排序的规则就是对ps的所有的Point对象的下一步的位置的数目，进行非递减排序
        sort(ps);
        while (!ps.isEmpty()) {
            Point p = ps.remove(0);//取出下一个可以走的位置
            //判断该点是否已经访问过
            if (!visited[p.y * X + p.x]) {//说明还没有访问过
                traversalChessboard(chessboard, p.y, p.x, step + 1);
            }
        }
        //判断马儿是否完成了任务，使用   step 和应该走的步数比较 ，
        //如果没有达到数量，则表示没有完成任务，将整个棋盘置0
        //说明: step < X * Y  成立的情况有两种
        //1. 棋盘到目前位置,仍然没有走完
        //2. 棋盘处于一个回溯过程
        if (step < X * Y && !finished) {
            chessboard[row][column] = 0;
            visited[row * X + column] = false;
        } else {
            finished = true;
        }
    }

    /**
     * 功能： 根据当前位置(Point对象)，计算马儿还能走哪些位置(Point)，并放入到一个集合中(ArrayList), 最多有8个位置
     *
     * @param curPoint 当前点
     * @return 马儿在当前位置能走的所有点的集合
     */
    public static ArrayList<Point> next(Point curPoint) {
        //创建一个ArrayList
        ArrayList<Point> ps = new ArrayList<Point>();
        //创建一个Point
        Point p1 = new Point();
        //表示马儿可以走5这个位置
        if ((p1.x = curPoint.x - 2) >= 0 && (p1.y = curPoint.y - 1) >= 0) {
            ps.add(new Point(p1));
        }
        //判断马儿可以走6这个位置
        if ((p1.x = curPoint.x - 1) >= 0 && (p1.y = curPoint.y - 2) >= 0) {
            ps.add(new Point(p1));
        }
        //判断马儿可以走7这个位置
        if ((p1.x = curPoint.x + 1) < X && (p1.y = curPoint.y - 2) >= 0) {
            ps.add(new Point(p1));
        }
        //判断马儿可以走0这个位置
        if ((p1.x = curPoint.x + 2) < X && (p1.y = curPoint.y - 1) >= 0) {
            ps.add(new Point(p1));
        }
        //判断马儿可以走1这个位置
        if ((p1.x = curPoint.x + 2) < X && (p1.y = curPoint.y + 1) < Y) {
            ps.add(new Point(p1));
        }
        //判断马儿可以走2这个位置
        if ((p1.x = curPoint.x + 1) < X && (p1.y = curPoint.y + 2) < Y) {
            ps.add(new Point(p1));
        }
        //判断马儿可以走3这个位置
        if ((p1.x = curPoint.x - 1) >= 0 && (p1.y = curPoint.y + 2) < Y) {
            ps.add(new Point(p1));
        }
        //判断马儿可以走4这个位置
        if ((p1.x = curPoint.x - 2) >= 0 && (p1.y = curPoint.y + 1) < Y) {
            ps.add(new Point(p1));
        }
        return ps;
    }
    //根据当前这个一步的所有的下一步的选择位置，进行非递减排序, 减少回溯的次数
    public static void sort(ArrayList<Point> ps) {
        ps.sort(new Comparator<Point>() {
            @Override
            public int compare(Point o1, Point o2) {
                //获取到o1的下一步的所有位置个数
                int count1 = next(o1).size();
                //获取到o2的下一步的所有位置个数
                int count2 = next(o2).size();
                if (count1 < count2) {
                    return -1;
                } else if (count1 == count2) {
                    return 0;
                } else {
                    return 1;
                }
            }
        });
    }
}
```