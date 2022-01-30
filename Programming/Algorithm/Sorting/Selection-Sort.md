```go



func selectionSort(nums []int) []int {
    for end := len(nums) - 1; end > 0; end-- {
        maxIdx := 0
        for begin := 0; begin <= end; begin++ {
            if (nums[maxIdx] <= nums[begin]) {
                maxIdx = begin
            }
        }
        nums[maxIdx], nums[end] = nums[end], nums[maxIdx]
    }
    return nums
}
```
