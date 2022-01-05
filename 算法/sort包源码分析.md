---
title: sort包源码分析
date: 2020-10-25 11:27:27
tags: [Go,算法]
---

姊妹篇:

[排序算法汇总](https://dashen.tech/2019/03/26/%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95%E6%B1%87%E6%80%BB/)

<br>


> sort.Sort的实现: 当待排序元素≤12时，使用希尔排序；当阈值为0时，使用堆排序。否则使用快速排序。在确定其分区点时，有一个三值取中的逻辑（元素数量不大于40时进行一次，大于40时进行三次）。根据注释的Tukey's Ninther，了解到这是取中值的一个算法…


## sort.Sort,不稳定排序

<br>


```go
// Sort sorts data.
// It makes one call to data.Len to determine n, and O(n*log(n)) calls to
// data.Less and data.Swap. The sort is not guaranteed to be stable.

// 对data.Len进行一次调用以确定n，对data.Less和data.Swap进行O（n * log（n））次调用。 不能保证排序是稳定的。

// 注: 如果需要稳定排序,调用sort.Stable

func Sort(data Interface) {
    n := data.Len()
    

    //从下面maxDepth的注释中可以看出,并不一定是快速排序
	quickSort(data, 0, n, maxDepth(n))
}



// maxDepth returns a threshold at which quicksort should switch
// to heapsort. It returns 2*ceil(lg(n+1)).

//  该方法返回一个阈值，在该阈值处，快速排序应切换为堆排序。 它返回2 * ceil（lg（n + 1））。
// ceil为向上取整函数. 如n=7,则该值为2*3=6

func maxDepth(n int) int {
	var depth int
	for i := n; i > 0; i >>= 1 {
		depth++
	}
	return depth * 2
}
```

<br>


### quickSort

<br>

```go

// 为0,b为n,即切片中中元素的个数
func quickSort(data Interface, a, b, maxDepth int) {

    // 如果切片元素<=12,则用希尔排序
    for b-a > 12 { // Use ShellSort for slices <= 12 elements
    
       // 当maxDepth为0时,进行堆排序
		if maxDepth == 0 {
			heapSort(data, a, b)
			return
		}
        maxDepth--
        

        //Pivot即快排的分区点,可参考之前的内容
		mlo, mhi := doPivot(data, a, b)
		// Avoiding recursion on the larger subproblem guarantees
        // a stack depth of at most lg(b-a).
        
        //记得初始时a=0,b=n. mlo-a为分区点左边那段内容,b-mhi为分区点右边那段内容
    
		if mlo-a < b-mhi {
            //对较小的那一段递归调用quickSort
            quickSort(data, a, mlo, maxDepth)
            
            //数据较大的赋值为mhi,继续进入for循环 
			a = mhi // i.e., quickSort(data, mhi, b)
		} else {
			quickSort(data, mhi, b, maxDepth)
			b = mlo // i.e., quickSort(data, a, mlo)
		}
    }
    


	if b-a > 1 { //b-a=1,即切片只有1个元素,无需排序
		// Do ShellSort pass with gap 6
        // It could be written in this simplified form cause b-a <= 12

         // 元素个数<=12,进行一次gap为6的希尔排序

        // 希尔排序(不稳定排序),是插入排序的改良版.(在堆排和快排出现前,算是最优的排序算法)
        //希尔排序时一种改进后的插入排序，因为插入排序对已经排好序的数据操作时更为有效，所以希尔排序先通过一定的间隔将元素划分成几个区域来先进行排序，然后逐步缩小间隔进行排序，最后采用插入排序，因为已经基本都排好了，所以插入排序的效率就很高。
		for i := a + 6; i < b; i++ {
			if data.Less(i, i-6) {
				data.Swap(i, i-6)
			}
        }

        // 而Golang中希尔排序中使用的Gap值是6，也就是间隔6位的为一组，先进行排序，然后不同于一般的希尔排序将Gap值减半，而是直接进行插入排序。

		insertionSort(data, a, b)
	}
}
```

来看一下Golang中的插入排序:


```go
// Insertion sort
func insertionSort(data Interface, a, b int) {
	for i := a + 1; i < b; i++ {
		for j := i; j > a && data.Less(j, j-1); j-- {
			data.Swap(j, j-1)
		}
	}
}

```

<br>


#### heapSort


<br>

```go
func heapSort(data Interface, a, b int) {
	first := a
	lo := 0
	hi := b - a

    // Build heap with greatest element at top.
    //用最顶部的元素构建堆。
    //步骤一:(hi - 1) / 2到0的元素 向下堆化heapify
	for i := (hi - 1) / 2; i >= 0; i-- {
		siftDown(data, i, hi, first)
	}

    // Pop elements, largest first, into end of data.
    //将元素按照 从大到小 Pop进数据结尾
    //步骤二:排序,将最大值放到最后面,剩余部分再进行堆化
	for i := hi - 1; i >= 0; i-- {
		data.Swap(first, first+i)
		siftDown(data, lo, i, first)
	}
}
```


<br>


##### siftDown

<br>


堆化操作

> 一般来说，堆排序的第一步是构建最大堆，第二步是从堆顶取出当前堆最大元素，与堆尾交换，并使堆大小减1；循环第二步，直到堆中没有元素。<br>
sort.go 中堆排序的核心函数是 siftDown(data Interface, lo, hi, first int)，它用于维护（和构建）最大堆的性质。


<br>

```go
// siftDown implements the heap property on data[lo, hi).
// first is an offset into the array where the root of the heap lies.

// siftDown在data [lo，hi）上实现了heap属性。
// 首先是堆根所在的数组的偏移量。
func siftDown(data Interface, lo, hi, first int) {
	root := lo
	for {
		child := 2*root + 1
		if child >= hi {
			break
		}
		if child+1 < hi && data.Less(first+child, first+child+1) {
			child++
		}
		if !data.Less(first+root, first+child) {
			return
		}
		data.Swap(first+root, first+child)
		root = child
	}
}

```

<br>

#### doPivot

<br>

选择分区点

根据数据量是否大于40,进行了三值取中或九值取中,最终的目的都是找到一个好的分区点


<br>

```go

func doPivot(data Interface, lo, hi int) (midlo, midhi int) {
    m := int(uint(lo+hi) >> 1) // Written like this to avoid integer overflow.
    //这样写可以避免整数溢出
    

	if hi-lo > 40 {
		// Tukey's ``Ninther,'' median of three medians of three.
		s := (hi - lo) / 8
		medianOfThree(data, lo, lo+s, lo+2*s)
		medianOfThree(data, m, m-s, m+s)
		medianOfThree(data, hi-1, hi-1-s, hi-1-2*s)
	}
	medianOfThree(data, lo, m, hi-1)

	// Invariants are:
	//	data[lo] = pivot (set up by ChoosePivot)
	//	data[lo < i < a] < pivot
	//	data[a <= i < b] <= pivot
	//	data[b <= i < c] unexamined
	//	data[c <= i < hi-1] > pivot
	//	data[hi-1] >= pivot
	pivot := lo
	a, c := lo+1, hi-1

	for ; a < c && data.Less(a, pivot); a++ {
	}
	b := a
	for {
		for ; b < c && !data.Less(pivot, b); b++ { // data[b] <= pivot
		}
		for ; b < c && data.Less(pivot, c-1); c-- { // data[c-1] > pivot
		}
		if b >= c {
			break
		}
		// data[b] > pivot; data[c-1] <= pivot
		data.Swap(b, c-1)
		b++
		c--
	}
	// If hi-c<3 then there are duplicates (by property of median of nine).
	// Let's be a bit more conservative, and set border to 5.
	protect := hi-c < 5
	if !protect && hi-c < (hi-lo)/4 {
		// Lets test some points for equality to pivot
		dups := 0
		if !data.Less(pivot, hi-1) { // data[hi-1] = pivot
			data.Swap(c, hi-1)
			c++
			dups++
		}
		if !data.Less(b-1, pivot) { // data[b-1] = pivot
			b--
			dups++
		}
		// m-lo = (hi-lo)/2 > 6
		// b-lo > (hi-lo)*3/4-1 > 8
		// ==> m < b ==> data[m] <= pivot
		if !data.Less(m, pivot) { // data[m] = pivot
			data.Swap(m, b-1)
			b--
			dups++
		}
		// if at least 2 points are equal to pivot, assume skewed distribution
		protect = dups > 1
	}
	if protect {
		// Protect against a lot of duplicates
		// Add invariant:
		//	data[a <= i < b] unexamined
		//	data[b <= i < c] = pivot
		for {
			for ; a < b && !data.Less(b-1, pivot); b-- { // data[b] == pivot
			}
			for ; a < b && data.Less(a, pivot); a++ { // data[a] < pivot
			}
			if a >= b {
				break
			}
			// data[a] == pivot; data[b-1] < pivot
			data.Swap(a, b-1)
			a++
			b--
		}
	}
	// Swap pivot into middle
	data.Swap(pivot, b-1)
	return b - 1, c
}


```

<br>

**medianOfThree**

```go
// medianOfThree moves the median of the three values data[m0], data[m1], data[m2] into data[m1].
func medianOfThree(data Interface, m1, m0, m2 int) {
	// sort 3 elements
	if data.Less(m1, m0) {
		data.Swap(m1, m0)
	}
	// data[m0] <= data[m1]
	if data.Less(m2, m1) {
		data.Swap(m2, m1)
		// data[m0] <= data[m2] && data[m1] < data[m2]
		if data.Less(m1, m0) {
			data.Swap(m1, m0)
		}
	}
	// now data[m0] <= data[m1] <= data[m2]
}

```

<br>

关于"Tukey's ``Ninther,'' median of three medians of three.",更多浏览如下:

[The Ninther — Approximating Medians](https://www.google.com/search?q=Ninther&oq=Ninther&aqs=chrome..69i57j69i60l3.196j0j1&sourceid=chrome&ie=UTF-8)

[The Ninther, a Technique for Low-Effort Robust (Resistant) Location in Large Samples](https://www.sciencedirect.com/science/article/pii/B9780122047503500241)

[Median](https://en.wikipedia.org/wiki/Median)

[普林斯顿大学红宝书之QUICKSORT](https://www.cs.princeton.edu/courses/archive/fall12/cos226/lectures/23Quicksort.pdf)


涉猎即可,不需深究


---


<br>


## sort.Stable,稳定排序

<br>


从算法实现上看, `Sort `的速度会比 `Stable`
快,再不要求稳定排序时, 优先使用 `Sort`


<br>

参考:


 [从Golang的排序算法看如何来优化排序](https://zhuanlan.zhihu.com/p/97965012)

[sort 包源码分析](https://learnku.com/articles/30404)

[【Go 源码分析】从 sort.go 看排序算法的工程实践](https://segmentfault.com/a/1190000016514382)