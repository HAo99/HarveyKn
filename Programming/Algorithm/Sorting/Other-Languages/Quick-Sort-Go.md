```go
func sortArray(nums []int) []int {
    sort(nums, 0, len(nums)-1)
    return nums
}

func sort(arr []int, l, r int) {
	if l >= r {
		return
	}
	p := partition(arr, l, r)
	sort(arr, l, p-1)
	sort(arr, p+1, r)
}

func partition(arr []int, l, r int) int {
	j := l
	p := rand.Intn(r-l) + l

	arr[l], arr[p] = arr[p], arr[l]

	for i := l + 1; i <= r; i++ {
		if arr[i] < arr[l] {
			j++
			arr[i], arr[j] = arr[j], arr[i]
		}
	}
	arr[l], arr[j] = arr[j], arr[l]
	return j
}
```
