```go
func bubbleSortV1(nums []int) []int {
    for end := len(nums) - 1; end > 0; end-- {
        for begin := 1; begin <= end; begin++ {
            if nums[begin] < nums[begin - 1] {
                nums[begin], nums[begin - 1] = nums[begin - 1], nums[begin]
            }
        }
    }
    return nums
}

func bubbleSortV2(nums []int) []int {
    for end := len(nums) - 1; end > 0; end-- {
        sorted := true
        for begin := 1; begin <= end; begin++ {
            if nums[begin] < nums[begin - 1] {
                nums[begin], nums[begin - 1] = nums[begin - 1], nums[begin]
            }
            sorted = false
        }
        if sorted {
            break
        }
    }
    return nums
}

func bubbleSortV3(nums []int) []int {
    for end := len(nums) - 1; end > 0; end-- {
        sortedIdx := 1
        for begin := 1; begin <= end; begin++ {
            if nums[begin] < nums[begin - 1] {
                nums[begin], nums[begin - 1] = nums[begin - 1], nums[begin]
            }
            sortedIdx = begin
        }
        end = sortedIdx
    }
    return nums
}
```
