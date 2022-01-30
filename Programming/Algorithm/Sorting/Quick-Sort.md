# 快速排序
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

**做法**

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

