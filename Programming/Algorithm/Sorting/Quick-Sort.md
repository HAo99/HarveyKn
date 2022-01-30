# 快速排序

| 稳定 | 空间                          | 时间                                       |
| ---- | ----------------------------- | ------------------------------------------ |
| 否   | 最好：O(logn)<br />最坏：O(n) | 平均：O(nlogn)<br />最坏：O(n<sup>2</sup>) |

原始写法

```cpp
#include <vector>

using namespace std;

void quick_sort(vector<int>&);
void quick_sort(vector<int>&, int, int);
int partition(vector<int&>, int, int);
void swap(vector<int>& data, int, int);

// 对整个数组进行排序
void quick_sort(vector<int>& data)
{
    quick_sort(data, 0, data.size() - 1);
}

// 对数组的指定闭区间进行排序
void quick_sort(vector<int>& data, int left, int right)
{
    if (left >= right) return;
    int p = partition(data, left, right);
    quick_sort(data, left, p - 1);
    quick_sort(data, p + 1, right);
}

// 对数组指定区间进行分区，并返回标定点
int partition(vector<int>& data, int left, int right)
{
    int p = left;
    for (int i = from + 1; i <= to; i++)
        if (data[i] < data[left])
        {
            p++;
            swap(data, i, p);
        }

    swap(data, left, p);
    return p;
}

// 交换指定下标的两个元素
void swap(vector<int>& data, int from, int to)
{
    int tmp = data[from];
    data[from] = data[to];
    data[to] = tmp;
}

```

## 优化 一：随机化

**解决的问题**

原始写法中，因为标定点的选取永远为第一个元素。如果数组有序，每一次分区左区没有元素（因为标定点已经是最小值），全部在右区。则会出现分区过多的问题，快速排序退化为一个 O(n<sup>2</sup>) 的算法。

问题用例：

```
1 2 3 4 5
```

运行结果：

```
[1 2 3 4 5] => 左分区：[]，标定点为：1，右分区为：[2 3 4 5]
1 [2 3 4 5] => 左分区：[]，标定点为：2，右分区为：[3 4 5]
1 2 [3 4 5] => 左分区：[]，标定点为：3，右分区为：[4 5]
1 2 3 [4 5] => 左分区：[]，标定点为：4，右分区为：[5]
1 2 3 4 [5] => 左分区：[]，标定点为：5，右分区为：[]
```


**解决方法**

将选标定点的策略由每次选最左边第一个元素变为随机选点。

```diff
  int partition(vector<int>& data, int left, int right)
  {
      int p = left;
+     int r = (rand() % (right - left)) + left;
+     swap(data, left, r);
      for (int i = from + 1; i <= to; i++)
          if (data[i] < data[left])
          {
              p++;
              swap(data, i, p);
          }

      swap(data, left, p);
      return p;
   }
```

## 优化二：双路快排

**解决的问题**

原始写法或优化一写法中，如果遇到了有多个连续元素相同的数组，分区也会出现一边分区为空，一边分区为 n - 1 的问题。 同样也会导致快速排序的性能退化为 O(n<sup>2</sup>)。

问题用例：

```
0 0 0 0 0
```

运行结果：

```
[0 0 0 0 0] => 左分区：[]，标定点为：0，右分区为：[0 0 0 0]
0 [0 0 0 0] => 左分区：[]，标定点为：0，右分区为：[0 0 0]
0 0 [0 0 0] => 左分区：[]，标定点为：0，右分区为：[0 0]
0 0 0 [0 0] => 左分区：[]，标定点为：0，右分区为：[0]
0 0 0 0 [0] => 左分区：[]，标定点为：0，右分区为：[]
```

**解决方法**

使用双路分区方式：

```cpp
int partition(vector<int>& data, int left, int right)
{
    int r = (rand() % (right - left)) + left;
    swap(data, r, left);

    int l = left + 1, r = right;

    while (true)
    {
        while (l <= r && data[l] < data[left])
            l++;
        while (r >= l && data[r] > data[left])
            r--;
        if (l >= r) break;
        swap(data, l, r);
        l++, r--;
    }
    swap(data, left, r);
    return r;
}
```


## 优化三：三路快排

**解决的问题**

优化二的写法已经在各种情况下都能有比较好的性能，不过仍然在有多个相同元素的情况中，可以进一步优化。因为在 Partition 阶段完全可以了解到当前区间存在的相同元素，这些连续的相同元素无需继续分区，它们已经是「排好序」的了。

**解决方法**

使用三路分区方式：

```cpp
void quick_sort_3ways(vector<int>& data, int left, int right)
{
    if (left >= right) return;

    // Partition start
    swap(data, left, (rand() % (right - left)) + left);
    int lt = left, i = left + 1, gt = right + 1;
    while (i < gt)
    {
        if (data[i] < data[left])
        {
            lt++;
            swap(data, i, lt);
            i++;
        }
        else if (data[i] > data[left])
        {
            gt--;
            swap(data, i, gt);
        }
        else
        {
            i++;
        }
    }
    swap(data, left, lt);
    // Partition end

    quick_sort_3ways(data, left, lt - 1);
    quick_sort_3ways(data, gt, right);
}
```

由于 C++ 不支持多返回值，想要返回多个值需要定义成数据结构。所以此处将 partition 函数内联到 quick_sort_3ways 方法中。
