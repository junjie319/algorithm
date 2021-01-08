#### **双指针**

##### **数组区间**

整数列表如下[3，2，7，8，1，4，10，11，12]，请设计程序使连续的整数序列取前后两个数，并输出所有的列表。上面列表应该输出[7，8]，[10，12]。

```php
public function summaryRanges(){
    $nums = [3,2,7,8,1,4,10,11,12];
    $res = [];
    $begin = $nums[0];
    $len = count($nums);
    for ($i = 0; $i < $len; $i++) {
        $next = $nums[$i + 1];
        if ($next != $nums[$i] + 1) {
            if ($begin != $nums[$i]){
                $res[] = [$begin,$nums[$i]];
            }
            $begin = $next;
        }
    }
    return $res;
}

```

##### **长度最小子数组**

给定一个含有 n 个正整数的数组和一个正整数 s ，找出该数组中满足其和 ≥ s 的长度最小的连续子数组。

输入:s = 7, nums = [2,3,1,2,4,3] 输出: 2 解释: 子数组 [4,3]是该条件下的长度最小的连续子数组。

```php
function minSubArrayLen($s, $nums) {
    //滑动窗口
    //初始条件，如果全部相加都小于$s，直接返回0
    if(array_sum($nums)<$s) return 0;
    $len = count($nums);
    $low = 0;$height = 0;//滑动窗口左右边界
    $min = $len;//最长数组长度
    $sum = 0;
    //右指针要后移一位
    while ($height<=$len) {
        //满足条件则比较，且左边界右移
        if($sum >= $s) {
            $min = min($min,$height-$low);
            $sum -= $nums[$low++];
        }else{
            //没满足条件则继续相加，右边界右移
            $sum += $nums[$height];
            $height++;
        }
     }
     return $min;
}
```

##### **两数之和**

给出一个整数数组，请在数组中找出两个加起来等于目标值的数，返回这两个数字的下标

```php
class Solution
{
    /**
     * @param Integer[] $nums
     * @param Integer $target
     * @return Integer[]
     */
    public function twoSum(array $nums, $target) 
    {
        $find = [];
        $count = count($nums);
        
        for ($i = 0; $i < $count; $i++) {
            $value = $nums[$i];
            if ($a = array_keys($find, ($target - $value))) {
                return [$a[0], $i];
            }
            $find[$i] = $value;
        }
    }
}
```

##### **0移动到数组末尾**

```php
    function moveZeroes2()
    {
        $nums = [0,13,8,0,6,4,0,7];
        $j = 0;$i = 0;
        $numsSize = count($nums);
        while ($j < $numsSize) {
            if ($nums[$j] != 0) {
                $nums[$i++] = $nums[$j++];
            }else{
                $j++;
            }
        }
        return array_splice($nums,0, $i);
    }
```

##### **移除重复元素**

```php
    public function removeDup(){
        $arr = [0,0,2,2,3,5,5,8];
        $len = count($arr);
        $i = 0;
        $j = 0;
        while($j < $len){
            if ($i == 0 || $arr[$j] != $arr[$i-1]){
                $arr[$i] = $arr[$j];
                $i++;
                $j++;
            }else{
                $j++;
            }
        }
        return array_splice($arr,0,$i);
    }
```

##### **盛最多水的容器**

```php
Input: [1,8,6,2,5,4,8,3,7]
Output: 49
function maxArea($height)
    {
        $len = count($height);
        $i = 0;
        $j = $len - 1;
        $res = 0;
        while ($i < $j) {
            $area = ($j - $i) * min($height[$i],$height[$j]);
            $res = max($res, $area);
            if ($height[$i] <= $height[$j]) {
                $i++;
            }else{
                $j--;
            }
        }
        return $res;
    }
```

##### **RGB排序**

```php
    function rgbSort()
    {
        $str = 'GRRRRB';
        $arr = str_split($str);
        $cnt = array_count_values($arr);
        $resR = str_repeat('R',$cnt['R']);
        $resG = str_repeat('G',$cnt['G']);
        $resB = str_repeat('B',$cnt['B']);
        $res = $resR.$resG.$resB;
        return $res;
    }
```

#### **树**

##### **判断高度平衡二叉树。**

```php
/**
 * @param TreeNode $root
 * @return Boolean
 */
class Solution {
function isBalanced($root) {
    $bool = true;
    $this->getDepth($root,$bool);
    return $bool;
}

function getDepth($root,&$bool){
    if ($root ==null) {
        return 0;
    }
    $left = $this->getDepth($root->left,$bool);
    $right = $this->getDepth($root->right,$bool);
    if(abs($left-$right)>1){
        $bool = false;
    }
    return $right>$left?$right+1:$left+1;
}
```
#### **动态规划**

##### **动态规划最长公共子序列LCS**

例如字符串“abcfbc”和“abfcab”，其中“abc”同时出现在两个字符串中，因此“abc”是它们的公共子序列。现在给你两个任意字符串（不包含空格），请帮忙计算它们的最长公共子序列的长度。

```php
function longestCommonSubsequence($str1, $str2) {
        $len_1 = strlen($str1);
        $len_2 = strlen($str2);
        $len = $len_1 > $len_2 ? $len_1 : $len_2;

        $dp = array();
        for ($i = 0; $i <= $len; $i++) {
            $dp[$i] = array();
            $dp[$i][0] = 0;
            $dp[0][$i] = 0;
        }

        for ($i = 1; $i <= $len_1; $i++) {
            for ($j = 1; $j <= $len_2; $j++) {
                if ($str1[$i - 1] == $str2[$j - 1]) {
                    $dp[$i][$j] = $dp[$i - 1][$j - 1] + 1;
                } else {
                    $dp[$i][$j] = $dp[$i - 1][$j] > $dp[$i][$j - 1] ? $dp[$i - 1][$j] : $dp[$i][$j - 1];
                }
            }
        }
        return $dp[$len_1][$len_2];
    }
```

##### **连续子数组的最大和（动态规划）**

一个数组 **[1, -2, 3 , 5, -2, 5 ]**，最大连续子数组为**[ 3 , 5, -2, 5]**， 和为11.

```php
class Solution {
    function maxSubArray($nums) {
        $dp[0] = $nums[0];
        $max = $nums[0];
        for($i=1;$i<count($nums);$i++){
            if($dp[$i-1]<0){
                $dp[$i]=$nums[$i];
            }else{
                $dp[$i]=$nums[$i]+$dp[$i-1];
            }
            if($dp[$i]>$max){
                $max=$dp[$i];
            }
        }
        return $max;
    }
}
```

#### **链表**

##### **选择排序 单链表**

```php
function SelectSort2(LinkList L)
{
    LinkList p,q,small;
    int temp;
 
    for(p = L->next; p->next != NULL; p = p->next)
    {
        small = p;
        for(q = p->next; q; q = q->next)
        {
            if(q->data < small->data)
            {
                small = q;
            }
        }
        printf("循环后，获得最小值为:%d, 此时链表为:", small->data);
        if(small != p)
        {
            temp = p->data;
            p->data = small->data;
            small->data = temp;
        }
    }
    printf("输出排序后的数字:\n");
    return L;
}

```

##### **反转链表**（非递归、递归）

```php
class Solution
{
    /**
     * @param ListNode $head
     * @return ListNode
     */
    function reverseList($head)
    {
        // double pointer
        // 我们可以申请两个指针，第一个指针叫 prev，最初是指向 null 的。
        // 第二个指针 cur 指向 head，然后不断遍历 cur。
        // 每次迭代到 cur，都将 cur 的 next 指向 pre，然后 pre 和 cur 前进一位。
        // 都迭代完了 (cur 变成 null 了)，pre 就是最后一个节点了。
        $prev = null;
        $cur = $head;
        while ($cur) {
            $next = $cur->next;
            $cur->next = $prev;
            $prev = $cur;
            $cur = $next;
        }

        return $prev;
    }
}
```

```php
class Solution {

    /**
     * @param ListNode $head
     * @return ListNode
     */
     // 明确递归函数的含义：传入一个链表（或片段），将其反转，并返回链表头元素
     // 所以，完全反转以后，原来的最后一个元素就变为了链表头元素
    function reverseList($head) {
        if ($head === null || $head->next === null) return $head;
        $last = $this->reverseList($head->next);
        $head->next->next = $head;
        $head->next = null;
        return $last;
    }
}

```

**斐波那契数列**

 fib(n-``1``) + fib(n-``2``)

```php
class Solution {
    /**
     * @param Integer $n
     * @return Integer
     */
    function fib($n) {
            $first = 0;
            $second = 1;
            $sum = 0;
            if ($n == 0 || $n == 1) {
                return $n;
            }
            for ($i = 2; $i <= $n; $i++) {
                $sum = $first + $second;
                $first = $second;
                $second = $sum;
            }
            return $sum;
        }
}

```

#### **数组排序**

##### **冒泡排序**

```php
function bubbleSort ($arr) 
{
     if (!is_array($arr)) return false;
     $len = count($arr);
     if ($len <= 1) return $arr;
    //控制需要处理冒泡次数
    for ($i=1; $i<$len; $i++) {
        //控制数组需要比较冒泡次数
        for ($k=0; $k < $len-$i; $k++)  {
            if ($arr[$k] > $arr[$k+1]) {
                $tmp = $arr[$k];
                $arr[$k] = $arr[$k+1];
                $arr[$k+1] = $tmp;
            }
        }
    }
    return $arr;
}
```

##### **选择排序**

```php
function selectSort(&$arr){
     $len=count($arr);
     if (!is_array($arr)) return false;
     $len = count($arr);
     for($i=0; $i<$len-1; $i++) {
         //先假设最小的值的位置
         $p = $i;
          
         for($j=$i+1; $j<$len; $j++) {
             //$arr[$p] 是当前已知的最小值
             if($arr[$p] > $arr[$j]) {
             //比较，发现更小的,记录下最小值的位置；并且在下次比较时采用已知的最小值进行比较。
                 $p = $j;
             }
         }
         //已经确定了当前的最小值的位置，保存到$p中。如果发现最小值的位置与当前假设的位置$i不同，则位置互换即可。
         if($p != $i) {
             $tmp = $arr[$p];
             $arr[$p] = $arr[$i];
             $arr[$i] = $tmp;
         }
     }
     //返回最终结果
     return $arr;    
}
```

##### **快速排序**

```php
function quickSort($arr)
{
    if (!is_array($arr)) return false;
    $len = count($arr);
    if ($len <= 1) return $arr;
    $right = $left = array();
    for ($i=1; $i<$len; $i++) {
        if ($arr[$i] < $arr[0]) {
            $left[] = $arr[$i];
        } else {
            $right[] = $arr[$i];
        }
    }
    $left  = $quickSort($left);
    $right = $quickSort($right);
    return array_merge($left, array($arr[0]), $right);
}
```

##### **插入排序**

```php
/* 
 * 从后向前扫描
 * 该元素已大于已排序$tmp，则该元素下移一个位置
 **/
function insertSort($arr)
{
    $len = count($arr);
    for ($i=1; $i<$len; $i++) {
        //假设当前值
        $tmp = $arr[$i];
        //需要插入次数
        for ($j=$i-1; $j>=0; $j--) {
            //位置互换
            if ($tmp < $arr[$j]) {
                $arr[$j+1] = $arr[$j];
                $arr[$j]    = $tmp;
            } else {
                break;
            }
        }
    }
    return $arr;
}
```

##### **二分查找**

```php
/**
 * 数据量很大时适合使用
 * 有序且不重复的数组
 **/

function binarySearch($array, $val) {
    if (empty($val) || empty($array)) {
        return false;
    }
    $array = sort($array);
    $count = count($array);
    $low = 0;
    $high = $count - 1;
    while ($low <= $high) {
        $mid = intval(($low + $high) / 2);
        if ($array[$mid] == $val) {
            return $mid;
        }
        if ($array[$mid] < $val) {
            $low = $mid + 1;
        } else {
            $high = $mid - 1;
        }
    }
    return false;
}
```



