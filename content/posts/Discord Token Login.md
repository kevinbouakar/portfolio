---
title: "Discord Token Login"
date: 2024-09-27T13:21:08+03:00
lastmod: 2024-09-27T13:21:08+03:00
draft: false
tags: []
slug: "discord-token-login"
---

# Login:

1. Press ==CTRL + Shift  I==
2. Go to **Console**
3. Copy & paste the script below:

```javascript
function login(token) {
setInterval(() => {
document.body.appendChild(document.createElement `iframe`).contentWindow.localStorage.token = `"${token}"`
}, 50);
setTimeout(() => {
location.reload();
}, 0);
}
login("PASTETOKENINHERE")
```

4. Replace `PASTETOKENINHERE` with your Discord Token
5. Press **Enter**

- - -

