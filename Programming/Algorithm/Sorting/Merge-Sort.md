```c
void merge_sort(int* nums, int l, int r);
void merge(int* nums, int l, int m, int r);

int* sortArray(int* nums, int numsSize, int* returnSize){
    merge_sort(nums, 0, numsSize - 1);
    *returnSize = numsSize;
    return nums;
}

void merge_sort(int* nums, int l, int r)
{
    if (l >= r) return;
    int mid = (l+r) >> 1;
    merge_sort(nums, l, mid);
    merge_sort(nums, mid + 1, r);
    merge(nums, l, mid, r);
}

void merge(int* nums, int l, int m, int r)
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
