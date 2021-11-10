## 49. Group Anagrams

題目要求把相同的 "anagrams" 都放在陣列裡面最後回傳陣列的集合

### My submission

一開始解法是不想用內建的函式，所以先把字串用 `charCodeAt()`轉換成 unicode 編碼然後做排列，之後再一一比較...然後就直接超時了。後來任命用內建函式，因為看 discussion 也是這樣做。

`sort()` 會原地(in place)對陣列元素進行排列然後回傳排列後的陣列，預設的排列方式是按照 unicode 編碼
`join()` 會把陣列合併成字串

```javascript=
var groupAnagrams = function(strs) {
    const map = {}
    for(let i = 0; i < strs.length; i++) {
        const sortStr = [...strs[i]].sort().join('')
        if(!map[sortStr]) map[sortStr] = [strs[i]]
        else map[sortStr].push(strs[i])
    }
    return Object.values(map)
};
```

### Better one
