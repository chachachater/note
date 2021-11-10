## Two Sum

### My submission

第一次用偷吃步的解法，直接把 numx2 放到 000 部分，然後做 bubble sort。
第二次參考[影片](https://www.youtube.com/watch?v=0PHGaGma6j8)認真解了。
merge sort 的做法是先把 nums1 和 nums2 按照大小排列好，nus1 最後面補 0 ，這些補上去的 0 用來把 nums2 給 merge 進來的。
然後開始把 nums2 塞到 nums1 裡面。拿 nums2 的最大值去跟 nums1 的最大值比，如果 `nums2[i]` >= `nums1[i]` 則把 `nums2[i]` 塞到 nums1 的最後一位，否則把 `nums1[i]` 塞到 nums1 的最後一位。
迴圈停止條件是當 nums1 或 nums2 已經被排列完，這裡需要考慮一個 edge case，那就是當回條一值滿足`nums2[i]` < `nums1[i]` 的時候(也就是說迴圈停止的原因是 nums1 已經排列完)。

```javascript=
var merge = function(nums1, m, nums2, n) {

    let l1 = m-1
    let l2 = n-1
    let x = nums1.length - 1
    while(l1 >= 0 && l2 >= 0) {
        if(nums1[l1] <= nums2[l2]) {
            nums1[x] = nums2[l2]
            l2--
            x--
        }
        else {
            nums1[x] = nums1[l1]
            l1--
            x--
        }
    }
    while(l2 >= 0) {
        nums1[x] = nums2[l2]
        l2--
        x--
    }
    return nums1
};
```

### Better one

去尋找補數，沒找到就用一個物件存放每個 array element 的 value/index (key === arr[i], value === i)。

==in operator==: [從物件中找到 key](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/in)
```javascript=
var twoSum = function(nums, target) {
    const map = {}
    for(let i = 0; i < nums.length; i++) {
        const component = target - nums[i]
        if(component in map) return [i, map[component]]
        map[nums[i]] = i
    }
}
```