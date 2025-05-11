## 🎹 Challenge Name: Doge World 
**Category**: Web  


### 🔍 Investigation:
A quick source code reviews hints at robots.txt file. Opening it, we can see a valid directory endpoint /hidden/.

Exploring manually reveals there are 2 levels of subdir, like /hidden/1/1/,  /hidden/1/2/ and so on till /hidden/9/9/

Wrote a quick js script to run in console of dev tools - 
```
(async () => {
  for (let i = 0; i <= 9; i++) {
    for (let j = 0; j <= 9; j++) {
      const url = `${baseURL}${i}/${j}`;
      try {
        const res = await fetch(url, { method: 'GET' });
        console.log(`[${res.status}] ${url}`);
      } catch (err) {
        console.error(`[ERROR] ${url}`, err);
      }
    }
  }
})();
```

Then I opened networking tab, and sorted by response size. And 2 endpoints had larger response size. And one of them was /hidden/4/3 which had flag.txt. 

Now the flag was encoded, and after trying some common ciphers, I got the flag by using ROT 47 decryption.

`csay{7rAv3R5!n6_d1R3ct0r!3S_1$_FuNn}`