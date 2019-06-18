# 为什么需要同源策略

首先同源策略是浏览器为了某些安全因素，设置的限制。一提到同源策略，很多人会想到 ajax 跨域和 cros，实际上同源策略不仅仅是限制请求不能跨域。还对一些重要的资源作出了限制。

## 具体的限制的资源包括:

1. cookie
2. localStorage, sessionStorage, indexDB
3. ajax
4. dom

## 下面说一下为什么需要限制这些资源的跨域访问

1. cookie  
   以前的网站很多是依靠 cookie 做身份验证的，假设有一个用户已经登陆一个银行网站(a.com), 那么如果用户点击进入一个 b.com，b.com 就可以获取用户的 cookie，那么 b.com 就可以拿到 a.com 的 cookie，那么 b 网站就可以利用这个 cookie，在 a.com 登陆该用户的账户。
2. localStorage, sessionStorage, indexDB  
   这个应该是禁止其他不同域名的网站，获取存储在本地缓存中的敏感信息吧
3. ajax  
   还是假设用户已经登陆了一个银行网站(a.com),并且用户点击进入了 b.com,如果 b.com 中有脚本向 a.com 的接口发起请求，并且此时携带的 cookie 是 a.com 的，那么就可以以该用户的身份进行操作。
4. dom  
   假设用户访问的 a 网站是 b 网站嵌套的一个 iframe,那么 b 网站就可以通过 window.frames 来拿到 a 网站的 window 对象，那么 b 网站的脚本就可以控制 a 网站。假设用户进入 a 网站正在通过表单填写用户名和密码，那么 b 网站就可以通过 dom 操作，修改表单的提交 url 地址，这样黑客就可以很容易的拿到用户名和密码信
