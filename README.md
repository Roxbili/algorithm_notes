# 算法面试知识点

## 知识点

### 排序算法性能对比
| 名称     | 时间复杂度-最好 | 时间复杂度-最差 | 时间复杂度-平均 | 额外空间 | 稳定性 | 原地 |
|----------|-----------------|-----------------|-----------------|----------|--------|------|
| 冒泡排序 | O( n<sup>2</sup> )         | O( n<sup>2</sup> )         | O( n<sup>2</sup> )         | O(1)     | 是     | 是   |
| 选择排序 | O( n<sup>2</sup> )         | O( n<sup>2</sup> )         | O( n<sup>2</sup> )         | O(1)     | 是     | 是   |
| 插入排序 | O( n )          | O( n<sup>2</sup> )         | O( n<sup>2</sup> )         | O(1)     | 是     | 是   |
| 希尔排序 | O( nlog(n) )    | O( n<sup>2</sup> )         | O(n log2 n)     | O(1)     | 否     | 是   |
| 快速排序 | O( nlog(n) )    | O( n<sup>2</sup> )         | nlog(n)         | O(1)     | 否     | 是   |
| 归并排序 | O( nlog(n) )      | O( nlog(n) )      | O( nlog(n) )      | O(n)     | 是     | 否   |
| 堆排序   | O( nlog(n) )      | O( nlog(n) )     | O( nlog(n) )    | O(1)     | 否     | 是   |
| 计数排序 | O( n+k )        | O( n+k )        | O( n+k )        | O( n+k ) | 是     | 否   |
| 桶排序 | O( k+n )      | O( k+n )      | O( n<sup>2</sup> )      | O( n+k ) | 是     | 否   |
| 基数排序 | O( k*n )      | O( k*n )      | O( k*n )      | O( n+k ) | 是     | 否   |

## 算法

### 二分查找
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        if len(nums) == 0:
            return -1

        left = 0
        right = len(nums) - 1
        while left <= right:
            mid = (right - left) // 2 + left
            if nums[mid] == target:
                return mid
            elif nums[mid] > target:
                right = mid - 1
            else:
                left = mid + 1
        return -1
```

### 排序

#### 快速排序
关键点：随机选择一个哨兵并存储在tmp中，把哨兵位置和left交换，这样相当于最左侧就空出来了

```python
class Solution:
    def partition(self, arr: List[int], left, right):
        if left >= right:   # 为了能够随机生成哨兵必须添加的条件，否则randint报错
            return

        pivot_idx = random.randint(left, right)
        pivot = arr[pivot_idx]
        arr[left], arr[pivot_idx] = arr[pivot_idx], arr[left]
        while left < right:
            while left < right and arr[right] > pivot:
                right -= 1
            arr[left] = arr[right]
            while left < right and arr[left] <= pivot:  # 等于的话选一边放即可
                left += 1
            arr[right] = arr[left]
        arr[left] = pivot
        return left

    def quick_sort(self, arr, left, right):
        if left < right:
            pos = self.partition(arr, left, right)
            self.quick_sort(arr, left, pos - 1)
            self.quick_sort(arr, pos + 1, right)
```

#### 归并排序
归并的过程就是新开一个数组，然后依次比较两个有序数组大小填入其中，最后再赋值回原来的数组

```python
class Solution:
    def merge(self, arr, left1, right1, left2, right2):
        p1, p2 = left1, left2
        tmp = []
        while p1 <= right1 and p2 <= right2:
            if arr[p1] < arr[p2]:
                tmp.append(arr[p1])
                p1 += 1
            else:
                tmp.append(arr[p2])
                p2 += 1

        while p1 <= right1:
            tmp.append(arr[p1])
            p1 += 1
        while p2 <= right2:
            tmp.append(arr[p2])
            p2 += 1
            
        for i in range(len(tmp)):
            arr[left1 + i] = tmp[i]

    def merge_sort(self, arr, left, right):
        if left < right:
            mid = (right - left) // 2 + left
            self.merge_sort(arr, left, mid)
            self.merge_sort(arr, mid + 1, right)
            self.merge(arr, left, mid, mid + 1, right)
```

#### 冒泡排序
每一次内循环都能把一个最大值放到数组末尾

```python
class Solution:
    def bubble_sort(self, arr: list):
        for i in range(1, len(arr)):
            for j in range(0, len(arr) - i):
                if arr[j] > arr[j + 1]:
                    arr[j], arr[j + 1] = arr[j + 1], arr[j]
```

#### 简单选择排序
每一次内循环都找到一个最小值放到`i`索引处

```python
class Solution:
    def select_sort(self, arr: list):
        for i in range(len(arr)):
            min_idx = i
            for j in range(i, len(arr)):
                if (arr[j] < arr[min_idx]):
                    min_idx = j
            arr[i], arr[min_idx] = arr[min_idx], arr[i]

    def MySort(self , arr: List[int]) -> List[int]:
        if len(arr) == 0:
            return arr
        self.select_sort(arr)
        return arr
```

#### 直接插入排序
把`i`位置插入到前面的有序序列中，所以前方依次往后移

```python
class Solution:
    def insert_sort(self, arr):
        for i in range(1, len(arr)):
            tmp = arr[i]
            j = i
            while j > 0 and arr[j - 1] > tmp:
                arr[j] = arr[j - 1]
                j -= 1
            arr[j] = tmp
```
