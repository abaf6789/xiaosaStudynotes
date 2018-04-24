# JavaScript排序

## 快速排序

快速排序在同为O(N*logN)的几种排序方法中效率较高，因此常被采用，再加上快速排序的思想———**分治法**很实用。

### 什么是快速排序

- 从数组中取出一个数为**基准**。
- 分区过程：把比这个基准大的数全部放到**基准**的右边，小于或等于**基准**的数全放在**基准**的左边。
- 再对左右区间重复第二步，直到各区间只有一个数。

```javascript
var quickSort = function(arr) {
    if( arr.length <=1 ) return arr
    var pivotIndex = Math.floor(arr.length/2)  //基准位置
    var pivot = arr.splice(pivotIndex, 1)[0]   //基准数
    var left = []
    var right = []
    for(var i = 0; i < arr.length; i++) {
        if(arr[i]<pivot) {
            left.push(arr[i])
        } else {
            right.push(arr[i])
        }
    }
    return quickSort(left).concat([pivot], quickSort(right)) //把左右数组与基准数结合
}
```

