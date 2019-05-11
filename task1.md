# 1 Three Sum

## 1.1 题目

给定一个包含 *n* 个整数的数组 `nums`，判断 `nums` 中是否存在三个元素 *a，b，c ，*使得 *a + b + c =* 0 ？找出所有满足条件且不重复的三元组。

## 1.2 解题思路

第一步，先对数组中的元素进行排序；
第二步，设置三个指针p,q,r，且p<q<r，其中指针p指向元素一x，指针q和r分别指向元素二和元素三；指针p和指针q的移动方向为从左向右，指针r的移动方向为从右向左；
第三步，如果指针q和指针r指向的元素之和为-x，则保存这三个元素，指针q和指针r继续移动；如果指针q和指针r指向的元素之和小于-x，则指针q向右移动一个元素；如果指针q和指针r指向的元素之和大于-x，则指针r向左移动一个元素；
第四步，注意跳过所有重复的情况。

## 1.3 代码

```C
class Solution {
public:
	vector<vector<int>> threeSum(vector<int>& nums) {
		vector<int> numSet(3);
		vector<vector<int>> r;
		//1.排序
		sort(nums.begin(), nums.end());
		int len = nums.size();
		int sum;
		//2.固定一个数，转换为两数之和
		for (int i = 0; i < len - 2; i++)
		{
			sum = -nums[i];
			numSet[0] = nums[i];
			//3. 两数之和
			for (int j = i + 1, k = len - 1; j < k;)
			{
				if (nums[j] + nums[k] == sum)
				{
					numSet[1] = nums[j++];
					numSet[2] = nums[k--];
					r.push_back(numSet);
					//跳过重复元素
					while (j < k && nums[j] == nums[j-1])
						j++;
					while (j < k && nums[k] == nums[k+1])
						k--;
				}
				else if (nums[j] + nums[k] < sum)
					j++;
				else
					k--;
			}
			while (i < len - 2 && nums[i+1] == nums[i])
				i++;
		}
		return r;
	}
};
```



## 1.4 实验结果

![1557562836170](C:\Users\Chen\AppData\Roaming\Typora\typora-user-images\1557562836170.png)

## 1.5 参考链接

<https://blog.csdn.net/u010656539/article/details/52204607>

# 2 Majority Element

## 2.1 题目

给定一个大小为 *n* 的数组，找到其中的众数。众数是指在数组中出现次数**大于** `⌊ n/2 ⌋` 的元素。
你可以假设数组是非空的，并且给定的数组总是存在众数。

## 2.2 解题思路

### 2.2.1 解法一

对数组先排序，然后返回排序后数组中间位置的值。

### 2.2.2 解法二

摩尔投票：从第一个书开始count=1,遇到相同的就加1，遇到不同的就减1，减到0时就换下一个数开始计数，最后计数的元素就是众数。

## 2.3 代码

### 2.3.1 解法一

```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        return nums[nums.size() / 2];
    }
};
```

### 2.3.2 解法二

```C++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int count = 1;
        int maj = nums[0];
        for(int i=1; i<nums.size();i++)
        {
            if (maj==nums[i])
                count++;
            else
            {
                count--;
                if(count==0 && i<nums.size()-1)
                    maj=nums[i+1];
            }        
        }
        return maj;
    }
};
```



## 2.4 实验结果

### 2.4.1 解法一

![1557566135757](C:\Users\Chen\AppData\Roaming\Typora\typora-user-images\1557566135757.png)

### 2.4.2 解法二

![1557566953763](C:\Users\Chen\AppData\Roaming\Typora\typora-user-images\1557566953763.png)

# 3 Missing Positive

## 3.1 题目

给定一个未排序的整数数组，找出其中没有出现的最小的正整数。
算法的时间复杂度应为O(*n*)，并且只能使用常数级别的空间。

## 3.2 解题思路



## 3.3 代码



## 3.4 实验结果



# 4 Linked List Cycle I

## 4.1 题目

给定一个链表，判断链表中是否有环。
为了表示给定链表中的环，我们使用整数 `pos` 来表示链表尾连接到链表中的位置（索引从 0 开始）。 如果 `pos` 是 `-1`，则在该链表中没有环。

## 4.2 解题思路

使用快慢指针的方法，设定两个指针，如果快指针追上慢指针则有环，如果指向了NULL则无环。
要注意判断快指针的next是否为NULL。

## 4.3 代码

```C++
struct ListNode{
    int val;
    listNode *next;
    ListNode(int x):val(x),next(NULL){}
}
class Solution{
public:
    bool hasCycle(ListNode *head){
        ListNode *L1, *L2;
        L1 = head;
        L2 = head;
        while(L1!=NULL && L1->next!=NULL)
        {
            L1 = L1->next->next;
            L2 = L2->next;
            if(L1==L2)
                return true;
        }
        return false;
    }
};
```



## 4.4 实验结果

![1557581109767](C:\Users\Chen\AppData\Roaming\Typora\typora-user-images\1557581109767.png)

# 5 Merge k Sorted Lists

## 5.1 题目

合并 *k* 个排序链表，返回合并后的排序链表。请分析和描述算法的复杂度。

## 5.2 解题思路

由于每个链表已经有序，故每次只需用从`k`个链表的头结点中选出最小，可考虑把k个链表的头结点放入优先级队列中（优先级队列一般是大顶堆，此处要变成小顶堆），然后每次取出top即为最小，并将top对应节点的下一个非空节点放入优先级队列中即可。

## 5.3 代码

```c++
struct ListNode{
	int val;
	ListNode* next;
	ListNode(int x) : val(x), next(NULL){};
};

class cmp{
public:
	bool operator()(const ListNode* p1, const ListNode* p2){
		return p1->val > p2->val;
	}
};

class Solution {
public:
	ListNode* mergeKLists(vector<ListNode*>& lists) {
		ListNode dummy(-1), *root = &dummy;
		priority_queue<ListNode*, vector<ListNode*>, cmp> myqueue;
		int n = lists.size();
		for (int i = 0; i < n; i++)
		{
			if (lists[i] == NULL)
				continue;
			myqueue.push(lists[i]);
		}
		while (!myqueue.empty())
		{
			ListNode* node = myqueue.top();
			myqueue.pop();
			root->next = node;
			root = root->next;
			//root->next = NULL;
			if (node->next != NULL)
				myqueue.push(node->next);
			root->next = NULL;
		}
		return dummy.next;
	}
};
```



## 5.4 实验结果

![1557587449984](C:\Users\Chen\AppData\Roaming\Typora\typora-user-images\1557587449984.png)

## 5.5 参考链接

<https://blog.csdn.net/lv1224/article/details/82821883>
<https://blog.csdn.net/keshacookie/article/details/19612355>