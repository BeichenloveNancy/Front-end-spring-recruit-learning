# 网络安全
常见web攻击方式
  - XSS
    Cross Site Script 跨站脚本攻击 因为缩写和css重叠 所以叫XSS
    跨站监本攻击是指通过存在安全漏洞的web网站注册用户的浏览器内运行
    非法的非本站点的HTML标签或JavaScript进行的一种攻击

    跨站脚本攻击有可能造成以下影响：
    1. 利用虚假的输入表单骗取个人信息
    2. 利用脚本窃取用户的Cookie值, 被害者在不知情的情况下，帮助了攻击者
    发送恶意请求
    3. 显示伪造的文章或图片

    - 反射型攻击(url 参数直接注入)
      1. 普通 http://localhost:3000/from=china
      2. alert 尝试 http://localhost:3000/?form=<script>alert(1)</script>
      3. 获取cookie
      4. 短域名伪造

    - 存储型 - (存储到DB后读取时注入)
      1. <script>alert(1)</script>
      2. <script src="http://localhost:4000/hack.js"></script>
  
  危害：
    1. 获取页面数据
    2. 获取Cookies
    3. 劫持前端的逻辑
    4. 发送请求
    5. 偷取网站的任意数据
    6. 偷取用户资料
    7. 偷取用户秘密和登陆状态
    8. 欺骗用户

  防范手段：
    - ejs转义小知识
      <% code %> 用于执行其中的js代码
      <% =code %> 会对code进行html转义
      <% -code %> 不会转义
    - ctx.set('X-XSS-Protection', 0) 禁止XSS过滤 浏览器默认会XSS过滤
    - CSP 内容安全策略 开发者明确告诉浏览器哪些外部资源是
    可以加载的 我们只需要配置规则 如何拦截是浏览器的是 
    中间件中配置 如： ctx.set('Content-Security-Policy', "default-src 'self'")
      (Content-Security-Policy: default-src 'self') 只允许加载本站资源
      (Content-Security-Policy: img-src https://*) 只允许加载HTTPS协议图片
      (Content-Security-Policy: child-src 'none') 不允许加载任何来源框架
    - function escape(str) {
      str = str.replace(/&/g, '&amp')
      str = str.replace(/</g, '&lt')
      str = str.replace(/"/g, '&quto;')
      str = str.replace(/'/g, '&#39;')
      str = str.replace(/`/g, '&#96;')
      str = str.replace(/\//g, '&#x2F;')
    }


  - CSRF (Cross Site Request Forgery)
    跨站请求伪造, 是一种常见的web攻击 它利用用户已登录的身份，在用户不知情的情况下,
    以用户的名义完成非法操作

    危害：
      1. 利用用户登录态(cookie)
      2. 用户不知情
      3. 完成业务请求
      4. 盗窃用户资金(转账、消费)
      5. 获得用户权限
      6. 损害网站名誉

    防御：
      1. 禁止第三方的网站带cookie - 有兼容性问题 SameSite: Strict
      2. Referer Check - Https不发送referer
      3. 验证码 (后端分发token)每次请求都将token传给服务器 

  - 点击劫持
    点击劫持是一种视觉欺骗的攻击手段, 攻击者将需要攻击的网站通过iframe嵌套的方式
    嵌入自己的网页中,应将iframe设置为透明,在页面中透出一个按钮诱导用户点击

    防御：
      1. X-FRAME-OPTIONS 是一个HTTP响应头 就是为了防御用iframe嵌套的点击劫持
        DENY 页面不允许通过iframe方式展示
        SAMEORIGIN 页面相同域名下可以通过iframe方式展示
        ALLOW-FROM 页面可以在指定来源通过iframe方式展示
  - SQL注入
    1'or'1'='
    

  - OS注入
  - 请求劫持
      DNS劫持
        DNS服务器 (DNS解析各个步骤) 被篡改 修改域名解析的结果， 使得访问到的不是预期的ip
        比如 天翼wifi验证 劫持请求

      HTTP劫持
        运营商劫持, 此时大概智能升级HTTPS

  - DDOS (distributed denial of service)




防范方法
  - 密码安全
  - 密码学 (对称与非对称)
    摘要 - md5 sha1 sha256-hash
  - 传输安全 - HTTPS
  - NodeJS安全框架 hemelt
  - CSP策略