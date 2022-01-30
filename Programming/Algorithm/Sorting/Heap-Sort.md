```go
func sortArray(nums []int) []int {
    return heapSort(nums)
}

func heapSort(nums []int) []int {
    // transform to heap
    for i := 0; i < len(nums); i++ {
        siftUp(nums, i)
    }
    hsize := len(nums)
    for hsize > 0 {
        nums[0], nums[hsize - 1] = nums[hsize - 1], nums[0]
        hsize--;
        siftDown(nums, 0, hsize)
    }
    return nums
}

func parent(i int) int {
	if i == 0 {
		panic("index 0 has no parent")
	}
	return (i - 1) / 2
}

func leftChild(i int) int {
	return i << 1 + 1
}

func rightChild(i int) int {
	return i << 1 + 2
}

func siftUp(data []int, idx int) {
	cur := idx
	for cur > 0 && data[cur] > data[parent(cur)] {
		data[parent(cur)], data[cur] = data[cur], data[parent(cur)]
		cur = parent(cur)
	}
}

func siftDown(data []int, idx, size int) {
	cur := idx
	for leftChild(cur) < size {
		max := leftChild(cur)
		if max + 1 < size && data[max] < data[max + 1] {
			max = rightChild(cur)
		}
		if data[cur] >= data[max] {
			break
		}
		data[cur], data[max] = data[max], data[cur]
		cur = max
	}
}
```
