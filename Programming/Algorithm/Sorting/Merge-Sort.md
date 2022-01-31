# 归并排序

原始写法：

```cpp
#include <vector>

using namespace std;

void merge_sort(vector<int>&);
void merge_sort(vector<int>&, int, int);
void merge(vector<int>&, int, int, int);

void merge_sort(vector<int>& nums)
{
    merge_sort(nums, 0, nums.size() - 1);
}

void merge_sort(vector<int>& nums, int l, int r)
{
    if (l >= r) return;
    int mid = (l+r) >> 1;
    merge_sort(nums, l, mid);
    merge_sort(nums, mid + 1, r);
    merge(nums, l, mid, r);
}

void merge(vector<int>& nums, int l, int m, int r)
{
    int i = l;
    int j = m + 1;
    int tmp[r - l + 1];

    for (int k = l, v = 0; k <= r; k++, v++)
        tmp[v] = nums[k];

    for (int k = l; k <= r; k++)
    {
        if      (i > m) { nums[k] = tmp[j - l]; j++; }
        else if (j > r) { nums[k] = tmp[i - l]; i++; }
        else if (tmp[i - l] <= tmp[j - l]) { nums[k] = tmp[i - l]; i++; }
        else if (tmp[i - l] >  tmp[j - l]) { nums[k] = tmp[j - l]; j++; }
    }
}
```

## 优化一：内存复用

**解决的问题**

在 merge 操作的时候，每一次调用都需要申请一篇新的数组空间，用完后销毁。频繁对内存进行申请与销毁的开销是很大的。

**解决方法**

直接开辟一片足够使用的内存空间，在 merge 过程中进行复用。

