## 929. Unique Email Addresses

如標題，把透過`.``+`這種符號來產生的分身信箱給排除掉，回傳真實信箱的數量。

### My submission

靠著正規表達式來達成，陣列存放信箱的 local name 和 domain name，例如：`[['testmail', 'leet.com.tw'], ['testmail', 'leetcode.com.tw']]`

沒想到時間和空間複雜度輸了 95% 的答案...

```javascript=
var numUniqueEmails = function(emails) {
    const arr = []
    const domainMap = {}
    for(let i=0; i<emails.length; i++) {
        let nameARR = emails[i].split('@')
        let realName = nameARR[0].replace(/\+.*/, '').split('.').join('')
        let domainName = nameARR[1]
        if(!arr.length) arr.push([domainName, realName])
        for(let j=0; j<arr.length; j++) {
            console.log(j)
            if(arr[j][0] === domainName && arr[j][1] === realName) break
            if(j===arr.length-1) arr.push([domainName, realName])
        }

    }
    return arr.length
};
```

### Better one

- 改良正規表達式的寫法，`/(\+.*)|(\.)/g`，`|` 條件1 or 條件2，`g`表示比對多個成功的結果，如果沒有 `g`的話，在條件1 比對成功後就不會再比對條件2
- 信箱在做完正規表達式的處理之後，放到物件的 key，透過 `map['emailName']` 是否有值來做查詢，原本 value 是存放`null`，但後來發現`if(!map[`${realLocal}@${domain}`])` 會判斷 null 為真。

這樣的寫法也輸了 80% 的空間和時間複雜度，後來發現`for(each of arr)` 的複雜度遠低於一般的迴圈。
```javascript=
var numUniqueEmails = function(emails) {
    const map = {}
    let count = 0
    for(let i=0; i<emails.length; i++) {
        const [local, domain] = emails[i].split('@')
        let realLocal = local.replace(/\+.*|\./g, '')
        if(!map[`${realLocal}@${domain}`]) {
            map[`${realLocal}@${domain}`] = true //null
            count++
        }
    }
    return count
};
```

```javascript=
var numUniqueEmails = function(emails) {
    const map = {}
    let count = 0
    for(const email of emails) {
        const [local, domain] = email.split('@')
        let realLocal = local.replace(/\+.*|\./g, '')
        if(!map[`${realLocal}@${domain}`]) {
            map[`${realLocal}@${domain}`] = true //null
            count++
        }
    }
    return count
};
```