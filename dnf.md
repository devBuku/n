Duth National Flag algorithm
1. 3 pointers = low, mid, high
2. Three situations:
	1. 0s = 0 to low - 1
	2. 1s = low to mid - 1
	3. 2s = high + 1 to n - 1
3. unsorted part will be = mid to high
4. mid = 0 and high = n - 1 and low = 0
5. while (mid <= high)