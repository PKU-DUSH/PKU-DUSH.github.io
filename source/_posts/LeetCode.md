---
title: LeetCode
date: 2023-10-25 16:16:48
description: 此博客记录了Leetcode的题解
tags:
	- Array
	- LinkedList
	- HashTable
categories:
	- Leetcode
---

{% note info %}

# LeetCode

## 数组

### 二分查找

> 题目链接：https://leetcode.cn/problems/binary-search/

~~~C++
class Solution {
public:
    int search(vector<int>& nums, int target) {
            int left = 0, right = nums.size() - 1;
            while (left <= right) {
                int mid = left + (right - left)/2;
                if(nums[mid] > target)
                    right = mid - 1;
                else if(nums[mid] < target)
                    left  = mid + 1;
                else
                    return mid;
            }
            return -1;
    }
};
~~~

* 二分查找
  * **有序数组**，**无重复元素**
  * 边界条件：
    * target在[left，right]，也就是`right = num.size() - 1`
    * 于是 left<=right（等号的边界条件是最右面的数是target，也就是`right]` ）
    * 于是 `if (nums[mid] > target)    right = mid-1`
* 为什么`int mid = left + (right - left)/2`  
  * `left <= MAX_VALUE`和 `right <= MAX_VALUE`是肯定的
  * 但是`left+right <= MAX_INT` 我们无法确定，所以会造成栈溢出。

### 移除元素

> 题目链接：https://leetcode.cn/problems/remove-element/

~~~C++
//暴力解，双层for
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int size = nums.size();
        for (int i = 0; i <size; i++){
            if(nums[i] == val){
                for(int j = i + 1; j < size; j++)
                    nums[j - 1] = nums[j];
                size--;
                i--;
            }
        } 
        return size;
    }
};

//双指针法
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slowIndex = 0; 
        for(int fastIndex = 0; fastIndex < nums.size(); fastIndex++){
            if(nums[fastIndex] != val)
                nums[slowIndex++] = nums[fastIndex];
        }
        return slowIndex;
    }
};
~~~

* 双指针法（快慢指针法）： **一个快指针和慢指针可以在一个for循环下完成两个for循环的工作。**
  - 快指针：**用于判断逻辑，寻找应该放入新数组的元素** ，新数组就是不含有目标元素的数组
  - 慢指针：指向新数组下标的位置
  - 数组中的**双指针**其实就是**下标索引**

### 有序数组的平方

> 题目链接：https://leetcode.cn/problems/squares-of-a-sorted-array/

~~~C++
//暴力解
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        for(int i = 0; i < nums.size(); i++){
            nums[i]=nums[i]*nums[i];
        }
        sort(nums.begin(),nums.end());//注意sort的用法
        return nums;
    }
};

//双指针法
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int k = nums.size() - 1;
        vector<int> result(nums.size() ,0); //注意vector的初始化
        for (int i = 0, j =nums.size() - 1; i <= j;) {
            if (nums[i]*nums[i] >= nums[j]*nums[j]) {
                result[k--] = nums[i] * nums[i];
                i++;
            }
            else {
                result[k--] = nums[j] * nums[j];
                j--;
            }
        }
        return result;
    }
};
~~~

* 双指针法：

  * 由于数组有序，那么平方之后最大值肯定在两侧，所以可以两侧都设置一个指针
  * 注意`i<=j`，因为如果`i==j`就停止，那么i和j同时指向的这个元素就不会被放入新数组中
  * 这个思想有点像快排？
  
* ~~~C++
  if (nums[i]*nums[i] > nums[j]*nums[j])
                  result[k--] = nums[i++]*nums[i++];//注意不能这样，这样在第一个nums[i++]后i就会增加了		
              else																	//可以nums[i]*nums[i++]
                  result[k--] = nums[j--]*nums[j--];	
  ~~~

### 长度最小的子数组

> 题目链接：https://leetcode.cn/problems/minimum-size-subarray-sum/description/

~~~c++
//暴力解
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int length = 100000; //注意length变化的情况下要设为全局变量，并且有更新的逻辑
        int subLength = 0;       
        for (int i = 0; i < nums.size(); i++) {
            int result = 0;
            for (int j = i; j < nums.size(); j++ ) {
                result += nums[j];
                if (result >= target){
                    subLength = j - i + 1;
                    length = subLength < length ? subLength : length; //用此方式可以更简洁
                    break;
                }    
            }
        }
        return length == 100000 ? 0 : length;    
    }
    
//滑动窗口
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int length = INT_MAX; //注意int最大值用INT_MAX表示比较好
        int i = 0;
        int subLength = 0;
        int result = 0;
        for (int j = 0; j < nums.size(); j++){
            result += nums[j];
            while (result >= target) {	//！！注意这里是while而不是if，可以考虑j到头了，if的话后面几个就没法再减小了
                subLength = j - i + 1;
                length = subLength < length ? subLength : length;
                result -= nums[i++]; //关键代码，等同于另一个for
            }
        }
        return length == INT_MAX ? 0 : length;
    }
};
~~~

* 两重for循环的本质是：**第一个for循环确定起始位置，另一个for循环确定结束位置**
* 滑动窗口（也是双指针法的一种）：
  * 不像双重for一样每次for都会更新result，而是**只在一个for中计算一次result【即总在结束位置累加计算】，然后通过起始位置减少result**。
  * 在本题中实现滑动窗口，主要确定如下三点：
    - 窗口内是什么？
      - 窗口就是 **满足其和 ≥ s 的长度最小的 连续 子数组**。
    - 如何移动窗口的起始位置？
      - 窗口的起始位置如何移动：**如果当前窗口的值大于s了，窗口就要向前移动了（也就是该缩小了）**。
      - `result -= nums[i++]`  关键就是通过这句实现起始位置的移动
    - 如何移动窗口的结束位置？
      - 窗口的结束位置如何移动：**窗口的结束位置就是遍历数组的指针，也就是for循环里的索引**。

* 思路（文字翻译成代码）：
  * **判断条件是什么**：和大于等于target  ->  **result** >=target
  * 长度最小的数组：
    * 判断长度最小需要循环比较然后在更新，所以要全局变量length
    * 每个循环体要有局部变量用于比较是否最小  ->  subLength

### 螺旋矩阵

> 题目链接：https://leetcode.cn/problems/spiral-matrix-ii/description/

~~~C++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> result(n,vector<int>(n, 0)); //注意vector二维数组的创建方式
        int loop = n/2;
        int count = 1;
        int startx = 0,starty = 0;
        int offset = 1; //key:每一次循环都要比上次少一圈
        int i = 0, j = 0;
        while (loop--) {
            for (j = starty; j < n - offset; j++)
                result[startx][j] = count++;

            for (i = startx; i < n - offset;i++)
                result[i][j] = count++;

            for (; j > starty; j--)
                result[i][j] = count++;

            for (; i > startx; i--)
                result[i][j] = count++;
            
            startx++;
            starty++;
            offset++;
        }
        if (n%2 != 0)
            result[n/2][n/2] = count;
        return result;
    }
};
~~~

* 思路：
  * 要绕几圈？n/2圈，单纯的循环while更简洁
  * 螺旋的要分四个for循环来，每一条边都要单独的for循环
  * 每一圈的赋值的起始点和终止点都有变化
    * 起始点通过startx/y++ 改变
    * 终止点通过offset++改变

* 注意奇偶区别，奇数的时候中间的要特殊逻辑（loop = n/2 + 1不行，因为n为偶数比如4的时候就是循环两次） 
* 注意offset是控制循环终止边界的，因为开始边界是由while循环的startx++决定的

{% endnote %}

{% note info %}

## 链表

### 移除链表元素

> 题目链接：https://leetcode.cn/problems/remove-linked-list-elements/

~~~C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead -> next = head;
        ListNode* cur = dummyHead;
        while(cur -> next){		//注意都是cur -> next
            if (cur-> next -> val == val){		
                ListNode* tmp = cur -> next;
                cur -> next = cur -> next ->next;
                delete  tmp;
            }
            else{
                cur = cur -> next;
            }
        } 
        head = dummyHead -> next;
        delete dummyHead;
        return head;
    }
};
~~~

* `ListNode *virtualNode  = new ListNode()` //这样创建对象是创建一个指针指向开辟的内存，所以可以和NULL比较。
  * 但是记住用完要delete
  * 在堆开辟，占用空间大，适合大程序；`ListNode virtualNode`栈开辟，占用空间小，适合小程序

* 操作当前节点必须要找前一个节点才能操作，但是头结点没有前一个节点了，这就需要虚拟头节点了。
* 节点移除要设置tmp变量
* 是对`cur->next`判断的，否则定位不到前一个节点

### 设计链表

> 题目链接：https://leetcode.cn/problems/design-linked-list/description/

~~~C++
class MyLinkedList {
public:
    MyLinkedList() {
        size = 0;
        dummyHead = new ListNode(0); 
    }
    
    int get(int index) {
        if(index < 0 || index >= size)
            return -1;
        ListNode* cur = dummyHead;
        while(index--)
            cur = cur -> next;
        return cur -> val;
    }
    
    void addAtHead(int val) {
        addAtIndex(0,val);
    }
    
    void addAtTail(int val) {
        addAtIndex(size, val);
    }
    
    void addAtIndex(int index, int val) {
        if (index > size)
            return;
        index = max(0, index);
        size++;
        ListNode* pred = dummyHead;
        ListNode* Add = new ListNode(val);
        while(index--)  //注意，链表中有索引都是通过while(index--)来遍历的
            pred = pred -> next;
        Add -> next = pred -> next;
        pred -> next = Add;
    }
    
    void deleteAtIndex(int index) {
        if(index < 0 || index >= size)
            return;
        size--;
        ListNode* del = dummyHead;
        while(index--)
            del = del -> next;
        ListNode* tmp = del -> next;
        del -> next = del -> next -> next;
        delete tmp;
    }
private:
    int size;
    ListNode* dummyHead;
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
~~~



### 翻转链表

> 题目链接：https://leetcode.cn/problems/reverse-linked-list/

~~~C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) { //核心是两个指针
        ListNode* cur = head;
        ListNode* pre = NULL;
        while(cur){
            ListNode* tmp = cur -> next; //注意tmp指针，若无tmp，一旦cur->next改变，将无法找到下个节点
            cur -> next = pre;
            pre = cur;
            cur = tmp;
        }
        return pre;

    }
};
~~~



### 两两交换链表中的节点*

> 题目链接：https://leetcode.cn/problems/swap-nodes-in-pairs/description/

~~~C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dummyHead = new ListNode(0);
        dummyHead -> next = head;
        ListNode* cur = dummyHead;

        while(cur -> next && cur -> next -> next){  //注意交换两个节点不仅要判断next，还要判断next->next
            ListNode* tmp =  cur -> next;   
            ListNode* tmp1 = cur -> next -> next -> next; //保存断开指针后的值

            cur -> next = cur -> next -> next; //步骤一
            cur -> next -> next = tmp; //步骤二
            cur -> next -> next -> next = tmp1; //步骤三

            cur = cur -> next -> next;
            
        }
        return dummyHead -> next;

    }
};
~~~

* 交换结点类的题都至少要设置三个指针
  * 两个指针用于交换
  * 一个指针tmp用于存储临时节点
* 设置一个虚拟头结点更方便
  * 注意dummyHead是个结点，不是指针！！！
    * 一般返回操作后的链表就用dummyHead->next返回
  * 而cur是个指针，指向这个dummyHead
  * 所以cur可以改变dummyHead->next



### 删除链表的倒数第N个结点

> 题目链接：https://leetcode.cn/problems/remove-nth-node-from-end-of-list/

~~~C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* fast = new ListNode(0);
        ListNode* slow = new ListNode(0);
        ListNode* dummyHead = new ListNode(0);

        dummyHead -> next = head;
        fast = dummyHead;
        slow = dummyHead;
        n++;

        while (n--)
            fast = fast -> next;
        while (fast){
            fast = fast -> next;
            slow = slow -> next;
        }
        slow -> next = slow -> next -> next;

         return dummyHead -> next;
    }
};
~~~

* 典型的快慢指针问题

  ![image-20231009124214423](https://s2.loli.net/2023/10/09/Oj3dnykKAEf41xa.png)

### 链表相交*

> 题目链接：https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/description/

~~~C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
 //传统双指针方法
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* curA = headA;
        ListNode* curB = headB;
        int lengthA = 0;
        int lengthB = 0;

        while (curA){
            lengthA++;
            curA = curA -> next;
        }
        while (curB){
            lengthB++;
            curB = curB -> next;
        }

        curA = headA;
        curB = headB; //注意要将cur返回

        if (lengthB > lengthA){
            swap(lengthA, lengthB);
            swap(curA, curB);
        }

        int distance = lengthA - lengthB;
        while (distance--)
            curA = curA -> next;
        
        while (curA && curB)
        {
            if(curA == curB)
                return curA;
            curA = curA -> next;
            curB = curB -> next;
        }

        return NULL;
    }
};

//数学公式方法*
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
            ListNode* curA = headA;
            ListNode* curB = headB;
            if (!curA || !curB)
                return nullptr;
            
            while (curA != curB){
                if (!curA)
                    curA = headB;
                curA = curA -> next;  //注意if和本行的顺序，如果换过来就是见到nullptr就跳过，这样在无交点时就会死循环

                if (!curB)
                    curB = headA;
                curB = curB -> next;
                
                // curA =    curA == nullptr ? headB : curA -> next;
                // curB =    curB == nullptr ? headA : curB -> next;

            }
            return curA;

    }
};
~~~

* 传统双指针：双指针先对齐，然后同时向后搜索

  ![image-20231009163406450](https://s2.loli.net/2023/10/09/frjqdV6YMApWOBT.png)

* 数学公式双指针：两个指针都搜索a+b+c的距离，此时一定是节点的位置

  * 注意注释掉的部分解决了无交点无限循环的问题，因为如果没有交点，那么最后都要到达nullptr而跳出循环（两指针都遍历了A+B的长度）；也就是nullptr不能跳过而直接变成另一链表的头节点，这样在无交点的时候就没有终止条件了。

  ![image-20231009163005284](https://s2.loli.net/2023/10/09/UgPH4OQjkBvRroC.png)


### 环形链表*

> 题目链接：https://leetcode.cn/problems/linked-list-cycle-ii/description/

~~~C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        ListNode* cur = head;

        while (fast && fast->next){
            fast = fast->next ->next;
            slow = slow -> next;
            
            if (fast == slow){
                while (cur != fast){
                    cur = cur -> next;
                    fast = fast->next;
                }
                return cur;
            }
        }
        return nullptr;
    }
};
~~~

* 先快慢指针判断是否有环
* 有环情况下找到入口
  * 也就是**从头结点出发一个指针，从相遇节点也出发一个指针，这两个指针每次只走一个节点， 那么当这两个指针相遇的时候就是 环形入口的节点**（虽然可能从相遇节点出发的指针已经绕了好几圈）
    ![image-20231009171140021](https://s2.loli.net/2023/10/09/gV763BlhrF1pPsR.png)

* 注意一旦有`fast ->next ->next`,那么不仅要判断`fast != NULL`， 也要判断 `fast ->next != NULL`，因为空指针是不能->next的。
  * 并且要判断fast而不是slow，因为fast在slow前面

* 注意while中的if判断一般是在语句之前，因为有边界情况是不需要动就已经满足条件了

~~~C++
if (slowPtr == fastPtr) {
                while (cur) {
                    if (cur == slowPtr)
                        return cur;
                    cur = cur -> next;
                    slowPtr = slowPtr -> next;
                } 
            }
~~~



### 合并两个有序链表

> 题目链接：https://leetcode.cn/problems/merge-two-sorted-lists/description/?envType=study-plan-v2&envId=top-100-liked

~~~
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* head = new ListNode(-1);
        ListNode* pre = head;

        while (list1 && list2) {
            if (list2->val >= list1->val) {
                pre -> next = list1;
                list1 = list1 -> next;
            }
            else {
                pre -> next = list2;
                list2 = list2 -> next;
            }
            pre = pre -> next;
        }

        pre -> next = list1? list1 : list2;

        return head -> next;

    }   
};
~~~

* 想知道前一个结点，必须要在一开始就设置一个
* 返回链表头节点，也要设置
* 注意体会这道题一开始设置的两个链表节点

{% endnote %}

{% note info %}

## 哈希表

> **当需要查询一个元素是否出现过**，或者**一个元素是否在集合里的时候**，要第一时间想到哈希法

![image-20231012191805536](https://s2.loli.net/2023/10/12/6hCWI3deNvVcfzO.png)

* 哈希表要考虑：元素是否可以重复 & 元素是否有序
  * 若不需要有序：
    * 用unorder，因为此时查询和增删效率都最高
* `vector`
  * .begin()  .end()
    * 返回指针
    * `vector<int>::iterator iter=a.begin(); cout<<*iter`
  * .front() .back()
    * 返回引用
    * `vector<int>a={1,0}; cout<<a.back();`
* set是集合，map是键值对映射

### 有效的字母异位词

>  题目链接：https://leetcode.cn/problems/valid-anagram/

~~~C++
class Solution {
public:
    bool isAnagram(string s, string t) {
        int record[26] = {0};
        for (int i = 0; i < s.size(); i++ )
            record[s[i] - 'a'] ++;  //注意s[i] - 'a'就把字符转换成下标了
        for (int i = 0; i < t.size(); i++ )
            record[t[i] - 'a'] --;
        for (int i = 0; i < 26 ; i++ )
            if (record[i] != 0) 
                return false;
        return true;       
    }
};
~~~

* 注意`record[s[i] - 'a']`，这样把字符转换成下标了

### 两个数组的交集

> 题目链接：https://leetcode.cn/problems/intersection-of-two-arrays/description/

~~~C++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> result_set;
        unordered_set<int> nums(nums1.begin(), nums1.end());

        for (int num : nums2)
            if (nums.find(num) != nums.end())  //find()返回的是一个指针，如果没找到就指向最后一个元素，也就是end()
                result_set.insert(num);
        
        return vector<int>(result_set.begin(), result_set.end());  //因为要求返回值是vector
    }
};
~~~

* 注意find()的用法

### 快乐数

> 题目链接：https://leetcode.cn/problems/happy-number/description/

~~~C++
class Solution {
public:
    int sum (int n) {
        int sum = 0;
        while (n) {
            sum += (n % 10) * (n % 10);
            n = n / 10;
        }
            return sum;
    }
    bool isHappy(int n) {
        unordered_set<int> resSet;
        while (1) {
            int res = sum (n);
            n = res; 

            if (res == 1)
                return true;

            if (resSet.find(res) != resSet.end())
                return false;
            else
                resSet.insert(res);
        }

    }
};
~~~

* 自己定义的函数要放在solution函数外面
* 注意取每一位的方法
* **无限循环怎么判断?** `unordered_set`，看是否会出现重复数字

### 两数之和*

> 题目链接：https://leetcode.cn/problems/two-sum/

~~~C++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> map;
        for (int i = 0; i < nums.size(); i++ ){
            auto iter = map.find(target - nums[i]);
            if(iter != map.end())
                return {i, iter -> second};
            map.insert(pair<int, int>(nums[i], i));  
        } 
        return {};
    }
};
~~~

* unordered_map查询效率O(1)，指的是由哈希表实现，等于直接查询下标
  * 所以想查询值的下标，不能用数组的思维用下标对应值，这样还是O(n)
  * 而是把值作为下标（key），下标作为值（value）
* 注意函数的返回值
  * vector的返回值是一个数组，如果为空不能是return 0；而是`return{};`
  * 如果是数不能是 return 1，而是 `return {1};`

### 四数相加*

> 题目链接：https://leetcode.cn/problems/4sum-ii/description/

~~~C++
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int, int> map12;
        for (int a : nums1)
            for(int b : nums2)
                map12[a + b]++;
        
        int count = 0;
        for (int c : nums3){
            for (int d : nums4){
                if (map12.find(0 - (c + d)) != map12.end() )
                    count+=map12[0 - (c + d)];
            }
        }
        return count;
    }
};
~~~

* 和上一道题的想法一模一样
* 注意不要超过两层循环，你想知道a+b就要用两层循环

![image-20231026125715364](https://s2.loli.net/2023/10/26/wZtygAxW3XI7oCE.png)

### 赎金信 

> 题目链接：https://leetcode.cn/problems/ransom-note/description/

~~~C++
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int record[26] = {0};
        for (auto a : magazine)
            record[a - 'a']++;
        
        for (auto b : ransomNote){ 
            record[b - 'a']--;

            if (record[b - 'a'] < 0)
                return false;
        }
        return true;
    }
};
~~~

* 如果数组可以做就别用map了，因为map的空间消耗比较大

### 三数之和*

> 题目链接：https://leetcode.cn/problems/3sum/description/

~~~C++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> result;
        sort(nums.begin(), nums.end());    //注意sort的使用方法

        for (int i = 0; i < nums.size(); i++){
            if (nums[i] > 0)
                return result;

            if (i > 0 && nums[i] == nums[i-1])
                continue;
            
            int left = i + 1;
            int right = nums.size() - 1;

            while (left < right){
                if (nums[i] + nums[left] +nums[right] > 0)
                    right--;
                else if (nums[i] + nums[left] +nums[right] < 0)
                    left++;
                else{
                    result.push_back(vector<int>{nums[i],nums[left],nums[right]});  //注意二维vector的push_back，是{}
                    while(left < right && nums[right] == nums[right - 1])
                        right--;
                    while(left < right && nums[left] == nums[left + 1])
                        left++;
                    
                    left++;
                    right--;
                }
            } 

        } 
        return result;
    }
};
~~~

* 还是一样，双指针可以在一个for循环下完成两个for循环的工作，那么这道暴力解是三重for的就可以通过双指针变成$O(n^2)$

* 注意去重操作

* 再看看思路：https://www.programmercarl.com/0015.%E4%B8%89%E6%95%B0%E4%B9%8B%E5%92%8C.html#%E6%80%9D%E8%B7%AF

### 四数之和

> 题目链接：https://leetcode.cn/problems/4sum/description/

~~~C++
~~~





{% endnote %}



{% note info %}

## 字符串

### 反转字符串

> 题目链接：https://leetcode.cn/problems/reverse-string/description/

~~~C++
class Solution {
public:
    void reverseString(vector<char>& s) {
        for(int i = 0, j = s.size() - 1; i < (s.size())/2; i++ , j--)
            swap(s[i], s[j]);
    }
};
~~~

* 注意和反转链表的区别，因为链表不能直接通过下标索引找到，而字符串可以，所以可以直接头尾交换

{% endnote %}
