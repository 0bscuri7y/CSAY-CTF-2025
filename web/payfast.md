## 🎹 Challenge Name: PayFast Bank
**Category**: Web  

## Overview
We are given a web page, where we can register, login, claim bonus once, and add money twice. But that won't get you to the flag price.

## Strategy
Quick look over to the network tab, nothing seems suspicious. However, these web bank challs are common in CTFs and are usually related to race conditions. 

So I wrote just right clicked on add money api call, copied as fetch, and wrapped it around for loop - 

```
for (let i = 0; i < 10; i++){
    fetch(...)
}
```

But this didn't work. So I though, maybe this is secure, but claim bonus would work. So I created a fresh account with no claimed bonus, and just replaced the fetch request above with one to claim bonus. This time, I was able to claim bonus 6 times. Then I just clicked add money twice, and tried to buy the flag. And it worked!!

