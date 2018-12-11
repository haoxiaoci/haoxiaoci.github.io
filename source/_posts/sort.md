---
title: 排序算法整理
comments: true
tags: Algorithm
abbrlink: 5124f222
date: 2018-12-11 14:08:13
categories:
---

### 比较两个算法：
1. 实现并调试
2. 分析算法的基本性质
3. 对其相对性能做出猜想
4. 用实验验证猜想



## 冒泡排序
### 算法思路
从第一个元素开始，跟他下一个元素比较，如果自己比较大，就跟相邻元素换一下，如果隔壁元素比较大，就不换了，继续比较隔壁元素和他的下一个元素的大小，遍历一遍就可以把列表中最大的值放在队尾，然后开始第二次遍历，知道n-1遍历，所有元素归位；

图示：
![BubbleSort.png](https://upload-images.jianshu.io/upload_images/7770956-42d0a30d5520d0dd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 代码实现：


```
def BubbleSort(nums):
    if len(nums) >1:
        for i in range(0, len(nums)):
            for j in range(0, len(nums)-i-1):
                if nums[j] > nums[j+1]:
                    nums[j+1], nums[j] = nums[j], nums[j+1]
```

### 时间复杂度:

最好的情况下是列表本来就是有序的虽然不用交换，但是遍历的次数依然不会少，依然是O(n^2)，最坏的情况下，每次都要交换；

交换操作是非常昂贵的，冒泡排序法每次都是一小步一小步的，交换很多次，所以冒泡排序法是所有排序方法中最低效的方法。

## 选择排序
### 算法思路：
冒泡排序的改进版，遍历一次就找到最大值，然后交换一次，不是每次比较都交换，第二次遍历就找到第二大的数字，并把它放到正确的位置
图示：
![selectionSort.png](https://upload-images.jianshu.io/upload_images/7770956-2bd996b076b37244.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 代码实现：

```
def SelectionSort(nums):
    length = len(nums)
    for i in range(0, length):
        index = 0
        for j in range(0, length-i):
            if nums[index] < nums[j]:
                index = j
        nums[index],nums[length-i-1] = nums[length-i-1], nums[index]
```

### 时间复杂度：
选择排序和冒泡排序的循环遍历次数是一样的，但是交换次数明显少于冒泡排序，所以选择排序相比较于冒泡排序，执行更快一点；
最佳的情况下是列表原本就是有序列表，依然遍历O(n^2)次数，但是没有交换操作，这个也是可以优化的，当第一次发现列表是有序的时候就可以退出；

## 插入排序
### 算法思路：
以第一个元素为例，对比比第一个元素大的，就插入到其左边，比他大就插入到右边
图示：
![InsertSort.png](https://upload-images.jianshu.io/upload_images/7770956-0491bc2310105e2d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
具体的排序过程插入操作一个元素的时候如下图，移动其他元素给目标元素空出位置，移位操作大概需要交换操作的三分之一的时间；
![image.png](https://upload-images.jianshu.io/upload_images/7770956-d2005472c731a3c0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
def SelectSort(a):
    for i in range(len(a)):
        min_index = i
        for j in range(i, len(a)):
            if a[min_index] < a[j]:
                min_index = j
        a[min_index], a[i] = a[i], a[min_index] 
```


### 时间复杂度：
最好的情况下是列表本来就是有序的，时间复杂度是O(n)
最差的情况下是需要比较n-1个整数的总和，时间复杂度是O(n^2),算法

## 希尔排序
### 算法思路：
递减递增排序， 根据增量i，循环列表将i的倍数的项拿出来组成子列表，对子列表进行排序，当增量i为1时，排序操作就是插入排序，当增量不为1时，相比较来说，希尔排序移位的次数小于插入排序
图解：增量i为3
![shellsort.png](https://upload-images.jianshu.io/upload_images/7770956-1e3f9055f0b5180f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

每个子列表排序：
![shellsort2.png](https://upload-images.jianshu.io/upload_images/7770956-4e4b8ab0deb80603.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 代码实现：

```
def ShellSort(a):
    h = 1
    while(h < len(a)/3):
        h = 3*h +1
    print("h: %d" % h)
    while h >=1:
        for i in range(len(a)):
            j = i
            while j in range(1, i+1) and a[j] < a[j-h]:
                a[j], a[j-h] = a[j-h], a[j]
                j -=h
        h = h//3
```

关于h为什么使用递增序列（截图来自算法第四版）：
![image.png](https://upload-images.jianshu.io/upload_images/7770956-5169c93bd31c48f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
希尔排序也可以用在大型数组，对于任意排序（不一定是随机的）的数组表现的也很好，相比于选择排序和插入排序，数组越大，希尔排序的优势越明显，使用sortcompare比较其性能，希尔排序能够解决一些初级算法无能为力的问题，这个例子也是说明了：通过提升速度来解决其他方式无法解决的问题是研究算法的设计和性能的主要原因之一
### 时间复杂度：
乍一看，你可能认为希尔排序不会比插入排序更好，因为它最后一步执行了完整的插入排 序。	然而，结果是，该最终插入排序不需要进行非常多的比较（或移位），因为如上所述， 该列表已经被较早的增量插入排序预排序。	换句话说，每个遍历产生比前一个“更有序”的列 表。	这使得最终遍历非常有效。

希尔排序的时间复杂度倾向于落在O(n)和O(n^2)之间的某处，基于以上所描述的行为。对于	Listing	5中显示的增量，性能为	O(n^2)	。
通过改变增量，例如使用	2^k-1（1,3,7,15,31等等）	，希尔排序可以在	O(n )处执行。

## 归并排序
### 算法思路：

使用分而治之策略，属于一种递归算法，当列表有多项时，我们对列表递归分割操作，直到拆分成最小的单位，每个列表只有一项，然后再比较排序，排序完后向上递归合并，直到合并成完整列表。
当列表为空或列表大小为1时，不需要排序。
列表大小是奇数或者偶数不影响排序，就相差一个元素。

第一步：元素拆分
![image.png](https://upload-images.jianshu.io/upload_images/7770956-0dfa0762735bfba6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

第二步：排序合并
![mergesort2.png](https://upload-images.jianshu.io/upload_images/7770956-27207d9b79be8f9d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 代码实现：
自顶向下的归并排序
实现1
这种写法，算是对于原地的写法吧，但是其中用到了数组的分片，我觉得也是需要额外的空间的

```
def mergeSort(nums):
    import pdb;pdb.set_trace()
    print('splitting', nums)
    if len(nums) > 1:
        mid = len(nums) //2
        left = nums[:mid]
        right = nums[mid:]
        
        mergeSort(left)
        mergeSort(right)

        i = 0
        j = 0
        k = 0
        #下边这段while是主要交换数据的，举例：
        #left = 26, 54 right=17, 93, nums=[54, 26, 93 ,17]
        #经过下边这段while会在 nums = [17, 26, 54, 17]的时候跳出循环，因为最后一个数字是还没写上的
        #所以需要下边两个循环，将最后一个数字补上。。。递归真的绕晕
        while i < len(left) and j < len(right):
            if left[i] < right[j]:
                nums[k] = left[i]
                i += 1
            else:
                nums[k] = right[j]
                j +=1
            k +=1
        print('1i: %d, j: %d, k: %d' %(i, j, k))

        while i < len(left):
            nums[k] = left[i]
            i += 1
            k += 1
        print('2i: %d, j: %d, k: %d' %(i, j, k))
        while j < len(right):
            nums[k] = right[j]
            j +=1
            k +=2
        print('3i: %d, j: %d, k: %d' %(i, j, k))
    print("Merging", nums)

nums=[54, 26, 93, 17, 77, 31, 44, 55, 20]
mergeSort(nums)
print(nums)
```

实现2
这种写法，多出了一个字典专门用来存储其中的数组变量的；
```
def LocalSort(a, low, mid, high):
    i = low
    j = mid +1
    aux = {}
    for k in range(low, high+1):
        aux[k] = a[k]

    k = low
    while k in range(low, high+1):
        if i > mid:
            a[k] = aux[j]
            j +=1
            print("i: %d" %i)
            print("a: ", a[:k+1])
        elif j > high:
            a[k] = aux[i]
            i +=1
            print("j: %d" %j)
            print("a: ", a[:k+1])
        elif aux[j] < aux[i]:
            a[k] = aux[j]
            j +=1
            print("j: %d" %j)
            print("a: ", a[:k+1])
        else:
            a[k] = aux[i]
            i +=1
            print("i: %d" %i)
            print("a: ", a[:k+1])
        k +=1

def MergeSort2(a, low, high):
    if high <= low:
        return
    mid = low + (high-low)//2
    print("low: %d, mid: %d, high: %d"%(low, mid, high))
    MergeSort2(a, low, mid)
    MergeSort2(a, mid+1, high)
    print("a: ", a[low:high+1])
    LocalSort(a, low, mid, high)

a = [8,4,9,1,2,5,0,19,33]
length = len(a)
MergeSort2(a, 0, length -1)
```

自底向上的归并排序：

```
def MergeSortBU(a):
    length = len(a)
    aux = []
    i = 1
    while i < length:
        low = 0
        while low < length - i:
            LocalSort(a, low, low+i-1, min(low+i+i-1, length-1))
            low = low +i +i
        i +=i
LocalSort()函数参考上一段代码
```
自底向上这个算法轨迹：
因为我没看太懂这个算法，留着这个图，以后再看再理解
![image.png](https://upload-images.jianshu.io/upload_images/7770956-f8501bffd71cdbd4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 时间复杂度：

O(nlogn), 拆分列表的时候需要O(logn),合并的时候是需要循环整个列表的需要O(n)，所以总的时间复杂度是O(nlogn),代码1中的切片的时间复杂度O(k)可以后续优化，而且因为是切片操作，切出来的列表需要占用额外的空间，当列表很大时，这里也是一个问题.在代码2和代码3中就不存在这个问题啦，啊哈哈哈~



## 快速排序

### 算法思路：

选择列表的一个数作为基准值base，设置指针，一个从左到右left，一个从右到左的right，当left的值大于时，left停下，当right小于base时停下，交换left和right，
最终知道left和right相遇，对比base的值，base和其中比base小的值交换，这样循环一遍基准值base一定归位，左边全是比base小的，右边全是比base大的，下一次再循环基准值的左半边，和基准值的右半边；
当列表长度小于1时，认为他是有序的，不需要排序

图示：
![quicksort.png](https://upload-images.jianshu.io/upload_images/7770956-026a1379222f2f19.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 代码实现：

```
def quickSort(nums):
    quickSortHelper(nums, 0, len(nums)-1)

def quickSortHelper(nums, first, last):
    #M = 15   当数组元素个数小于15时，改用插入排序，因为插入排序在这时更快
    #if last <= (first + M):
    #    InserSort(nums)
    #    return
    if last <= first:
        return
    split = partition(nums, first, last)
    quickSortHelper(nums, first, split - 1)
    quickSortHelper(nums, split+1, last)


def partition(nums, first, last):
    #import pdb;pdb.set_trace()
    base = nums[first]
    print('base: %d' % base)
    left = first + 1
    right = last
    done = False
    while not done:
        while left <= right and nums[left] <= base:
            left +=1
        while right >= left and nums[right] >= base:
            right -=1
        if left > right:
            done = True
        else:
            nums[left], nums[right] = nums[right], nums[left]  ##左边数字大，右边数字小，两种情况
    nums[right], nums[first] = nums[first], nums[right]  #为什么是right, 因为left还是比base大，但是right是第一个比base小的数
    print('nums', nums)
    return right
    
nums = [54,26,93,17,77,31,44,55,20]
quickSort(nums)
print(nums)
```
### 算法改进：
 1. 对于小数组，快速排序和插入排序对比，还是插入排序更快，多小算小呢，5-15之间，代码修改在上边注释中可以看到，当数组长度小于常数15时，会调用插入排序算法

2. 在实际应用中可能排序的数组中存在着大量重复的元素，有很多重复的元素对于快速排序来说是有优化的空间的：

三项切分的快速排序算法代码：
```
def Quick3way(a, low, high):
    if high <= low:
        return
    lt = low 
    i = low +1
    gt = high
    base = a[low]
    while i <= gt:
        if a[i] < base:
            a[i], a[lt] = a[lt], a[i]
            i +=1
            lt +=1
        elif a[i] > base:
            a[i], a[gt] = a[gt], a[i]
            gt -=1
        else:
            i+=1
    Quick3way(a, low, lt-1)
    Quick3way(a, gt +1, high)
```
有点难理解,来点解释和图：
![image.png](https://upload-images.jianshu.io/upload_images/7770956-1984e9f9eefb4281.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image.png](https://upload-images.jianshu.io/upload_images/7770956-93696c00057d481f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
### 时间复杂度：

最好的情况下，第一个元素作为base，拆分列表就刚好从中间对半对半的拆开，时间复杂度是O(logn), 拆分完在遍历交换元素的时候，是要遍历整个列表的，需要时间复杂度O(n)
最差的情况下,列表就是有序的列表，第一个元素作为base，每次检查一遍发现自己是正确的元素，左半边是空的元素全在有半边，这时的时间复杂度O(n^2)


### 堆排序

使用满二叉树构造出一个二叉堆，堆排序是使用最大堆，最小堆，来解决数据中删除最大或最小的元素，插入一个元素，做完这些操作后堆经过排序，队列仍是有序的，当数据量较多时，也可以解决从数据中查找第k大元素，在以上这些使用场景堆排序很占优势

### 代码实现

```
def HeapSort(a):
    length = len(a)//2  - 1
    for i in range(length, -1, -1):
        sink(a, i, len(a) -1)
     
    for j in range(len(a)-1, 0, -1):
        a[0] ,a[j] = a[j], a[0]
        sink(a, 0, j - 1)

def sink(a, k, N):
    j = 2*k
    while j <= N:
        if j < N and a[j] < a[j+1]:
            j +=1
        elif a[k] < a[j]:
            a[k], a[j] = a[j], a[k]
            k = j
            print("a: ", a)
        else:
            break
```

### 时间复杂度

堆排序算法的时间复杂度是O(nlogn)

算法对比：
![image.png](https://upload-images.jianshu.io/upload_images/7770956-bb3a8996b2d3662d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
