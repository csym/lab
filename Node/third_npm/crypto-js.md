# crypto-js 代码片段

> localStorage 本地存储 加密数据，使用时解密

```
const CryptoJS = require("crypto-js");
const tripledes = require("crypto-js/tripledes");

const key = 'xxxx';

export function getAuthority() {
  if (localStorage.getItem('authority')) {
    const dec = tripledes.decrypt(localStorage.getItem('authority'), key).toString(CryptoJS.enc.Utf8);
    return dec;
  }
  return null;
}

export function setAuthority(authority) {
  if (authority) {
    const enc = tripledes.encrypt(authority, key).toString();
    localStorage.setItem('authority', enc);
  } else {
    localStorage.removeItem('authority');
  }
}

```

## 参考
- [crypto-js](https://www.npmjs.com/package/crypto-js)