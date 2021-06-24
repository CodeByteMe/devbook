### 谈谈你对JVM的认识。

### SpringMVC的理解。

### SpringBoot的优缺点。

### SpringBoot与Spring Cloud的区别。

### 什么是微服务。

### 谈谈垃圾回收机制。

### JVM 内存模型。

### JVM 如何保证线程安全。

### 谈谈对Valialite的理解。

### 谈谈synchronized 的理解。

### 讲讲AQS原理。

### java中的实现锁有哪些方式。

### MySql引擎有哪些？
    InnoDB :支持事务，新版本支持全文索引，存储数据方式是：表空间数据文件+日志文件，表的大小只受限于操作系统文件的大小 ，使用的是聚簇索引
    MyISAM：不支持事务，支持全文索引，存储数据格式：表定义文件+数据文件+索引文件。在跨平台方面要优于InnoDB。使用的是非聚簇索引
    
    

### MySql中的聚簇索引与非聚簇索引的区别。
    聚簇索引：叶子节点存放主键数据，找到了索引就等同找到了数据，一张表只可存在一个聚簇索引。
    非聚簇索引：叶子节点存放的是数据对应的行，一般myisam引擎会把索引缓存至内存，这样一来数据查询就会慢了很多。
### HashMap底层实现。

### HashMap采用红黑树的原因。
       JDK1.8以前，hashMap是基于数组+链表实现的，而1.8以后则采用数组+链表+红黑树。当数据量比较庞大的时候，为了优化map的查找性能，jdk则引入了红黑树的结构。理想状态下为了配合hashcode的性能
    当链表节点数量超过8时就会自动转换成红黑树。根据官方解释：当链表节点数为8的时候hash冲突的概率就会降低到百万分之一，据统计链表节点数超过8极其罕见，通常情况下hashmap节点不到8个就会进行自
    动扩容。只有及其不好的情况下链表节点达到8，同时hash表容量又很大的情况下，才会通过红黑树进行提升性能。
    不用avl树的原因：因为avl树的特点是追求严格的二叉平衡，当进行节点插入或删除的情况下，会损耗性能来进行平衡操作。在数据量比较大的情况下，红黑树比二叉树更优选。

### MySQL索引用B+树的原因。
    红黑树往往会在树的深度比较大的时候，造成大量的频繁磁盘IO操作，从而降低了效率。
    
### 事务隔离级别
 
       DEFAULT 默认
       Read uncommitted 未提交读 ： 解决了更新丢失，但还是可能会出现脏读。
       Read committed  提交读：解决了更新丢失和脏读问题。
       Repeatable read 可重复读：解决了更新丢失、脏读、不可重复读、但是还会出现幻读。
       Serializable  串行化：最严格的级别。
       
       脏读:事务未提交，却被其他事务读取。
       不可重复读：同一个事务再经过插入删除操作时，读到不同的数据（针对数据本身）。
       幻读：同一个事务再经过新增操作时，读取到不同数据（针对记录条数）。
### 链表反转算法
       1)迭代法(双指针法)： 时间复杂度：O(n)，空间复杂度：O(1)
        public class Solution {
            public ListNode ReverseList(ListNode head) {
                ListNode pre=null;
                ListNode curr=head;
                while(curr!=null){
                    ListNode tmp=curr.next;
                    curr.next=pre;
                    pre=curr;
                    curr=tmp;
                }
                return pre;
            }
        }
        2)递归法：时间复杂度：O(n)，空间复杂度：O(n)
        public class Solution {
           public ListNode ReverseList(ListNode head) {
               if (head == null || head.next == null) { 
                   return head;
               }
               ListNode p = ReverseList(head.next); 
               head.next.next = head.next;
               head.next = null; 
               return p; 
           }
        }

###  排序算法
    1） 冒泡排序
        a.每一次遍历将最大值放到最右边
        public int [] maopaoSort(int[] arr){
            for(int i=0;i<arr.length;i++){
                for(int j=0;j<arr.length-i-1;j++){
                    if(arr[j]>arr[j+1]){
                        int tmp=arr[j+1];
                        arr[j+1]=arr[j];
                        arr[j]=tmp;
                    }
                }
            }
            return arr;
        }
        
        2)选择排序
        a.先将第一个看做最小值
        b.每次循环，通过比较找出比最小值和下标，并交换最小值与起始下标；
        public int[] xuanzeSort(int[] arr){
            for(int i=0;i<arr.length;i++){
                int min =arr[i];
                int index=i;
                for(int j=i+1;j<arr.length;j++){
                    if(min>arr[j]){
                        min=arr[j];
                        index=j;
                    }
                }
                //将最小值进行重新赋值
                int tmp=arr[i];
                arr[i]=min;
                arr[index]=tmp;
            }
            return arr;
        }
        
        3)插入排序
         a.默认从第二个数据开始比较。
         b.先与第一个数比较，如果比第一个数小，则交换，依次再与第三个数比较，如果小则插入，否则推出循环。
          public static int[] charuSort(int[] arr){
                 for(int i=1;i<arr.length;i++){
                     for(int j=i;j>0;j--){
                         if(arr[j]<arr[j-1]){
                             int tmp=arr[j-1];
                             arr[j-1]=arr[j];
                             arr[j]=tmp;
                         }else{
                             break;
                         }
         
                     }
                 }
                 return arr;
           }


### LRU缓存算法（如何设计）
       可采用LinkedHashMap 进行设计，因为是LinkedHashMap在存储的时候有序的，这样就保证了先得到的数据先插入，这样的话，删除不常用的值很方便。
       思想：就是当设置缓存容量为3的时候，如果仍有剩余空间，则直接插入，否则删除最少使用的节点。
       特点：put时判断缓存是否存在该值，若存在则缓存值更新，并更新位置为队列头部，如果不存在，就判断容量是否足够，直接插入。如果容量不够，则删除最久未使用的，也就是队尾元素。
             get时，如果不存在咋返回-1，若存在，则先删除改元素，再将该元素插入队列头部
        
 
    

### 二叉树 先序，中序，后序 遍历

    先序遍历(并打印)： 若node 不为空，则优先遍历左树节点。然后再遍历右树节点。 中左右遍历
                public void theFirstTraversal(Node root) {  //先序遍历  
                    printNode(root);  
                    if (root.getLeftNode() != null) {  //使用递归进行遍历左孩子  
                        theFirstTraversal(root.getLeftNode());  
                    }  
                    if (root.getRightNode() != null) {  //递归遍历右孩子  
                        theFirstTraversal(root.getRightNode());  
                    }  
                } 
    中序遍历(并打印)：左中右遍历
          public void theInOrderTraversal(Node root) {  //中序遍历  
                if (root.getLeftNode() != null) {  
                    theInOrderTraversal(root.getLeftNode());  
                }  
                printNode(root);  
                if (root.getRightNode() != null) {  
                    theInOrderTraversal(root.getRightNode());  
                }  
            }
     后序遍历(并打印)： 左右中遍历
            public void thePostOrderTraversal(Node root) {  //后序遍历  
                if (root.getLeftNode() != null) {  
                    thePostOrderTraversal(root.getLeftNode());  
                }  
                if(root.getRightNode() != null) {  
                    thePostOrderTraversal(root.getRightNode());  
                }  
                printNode(root);  
            }  
           
           
      层序遍历：先计算深度，再由上至下，再左右
            /*
                 * 层序遍历
                 * 递归
                 */
                public void levelOrder(BinaryNode<AnyType> Node) {
                    if (Node == null) {
                        return;
                    }
            
                    int depth = depth(Node);
            
                    for (int i = 1; i <= depth; i++) {
                        levelOrder(Node, i);
                    }
                }
            
                private void levelOrder(BinaryNode<AnyType> Node, int level) {
                    if (Node == null || level < 1) {
                        return;
                    }
            
                    if (level == 1) {
                        System.out.print(Node.element + "  ");
                        return;
                    }
            
                    // 左子树
                    levelOrder(Node.left, level - 1);
            
                    // 右子树
                    levelOrder(Node.right, level - 1);
                }


### 根据中序遍历与后序遍历构造二叉树。

    /*
         * 构造二叉树
           inorder 中序结果
           postorder 后序结果
         * */
        public TreeNode buildTree(int[] inorder, int[] postorder) {
            return buildBinaryTreeProcess(inorder, postorder, postorder.length - 1, 0, inorder.length - 1);    
          }
    
        /*
         * 根据二叉树中序遍历和后序遍历的结果构造二叉树
            inorder 中序结果
            postorder 后序结果
            ppos 后序结果长度
            ie   中序结果长度
            is 临时变量
            
         * */
        public TreeNode buildBinaryTreeProcess(int[] inorder, int[] postorder, int ppos, int is, int ie){
            //若长后序结果长度为空，或者is>中序结果数组长度，则返回空
            if(ppos >= postorder.length || is > ie) return null;
            
            //构造一个二叉树节点
            TreeNode node = new TreeNode(postorder[ppos]);
            int pii = 0;
            for(int i = 0; i < inorder.length; i++){
              if(inorder[i] == postorder[ppos]) pii = i;  
            }
            //构造左节点
            node.leftChild = buildBinaryTreeProcess(inorder, postorder, ppos - 1 - ie + pii, is, pii - 1);
            //构造右节点
            node.rightChild = buildBinaryTreeProcess(inorder, postorder, ppos - 1 , pii + 1, ie);
            return node;
          }

    

 ####最大滑动窗口数(双端队列也可求解)
    题解：首先分析数组与窗口容纳元素大小
        1.若数组为空，或者数组长度小与窗口大小，或者窗口大小小与1时直接返回空
        2.假设从第一个元素下标从0开始，那么总共滑动次数是 arr.length-size+1次，外层循环滑动操作
        3.假设从第一个是窗口最大值，那么从第一个开始计算，窗口中的元素就是可以看成子数组，数组长度是K
        4.将数组子数组的元素拿出来进行比较,若拿出来的数组元素比默认的大，则加入列表
        public class Solution {
            public ArrayList<Integer> maxInWindows(int [] num, int size) {
                   int len=num.length;
                    ArrayList win=new ArrayList();
                    if(len==0||size<1||len<size) return win;
                    for(int i=0;i<len-size+1;i++){
                        int l=size+i;
                        int max=num[i];
                        for(int j=i;j<l;j++){
                            if(max<num[j]){
                                max=num[j];
                            }
                        }
                        win.add(max);
                        
                    }
                return win;
            }
        }
 
 
 ####最大子序和(求一个数组最大连续子数组的和，并取最大值)
    题解：动态转移方程 f(i)=max{f(i−1)+nums[i],nums[i]}
        class Solution {
            public int maxSubArray(int[] nums) {
                int pre=0;
                int maxNum=nums[0];
               for (int x : nums) {
                    pre = Math.max(pre + x, x);
                    maxNum = Math.max(maxNum, pre);
                }
                return maxNum;
            }
        }
 
 ####线程池的理解
 
 
 







