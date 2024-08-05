# ajax的状态码
- 0 未初始化 未调用open方法
- 1 初始化  调用open方法但是未调用send
- 2 发送 调用send 但是还没收到响应
- 3 收到部分响应数据
- 4 收到全部响应数据 可以在客户端使用

# 为什么会有跨域问题？如何解决？
  跨域是因为浏览器的同源策略导致的，同源策略会阻止一个域与另一个域之间的交互
  - jsonp 但是只能get请求 理论上的方法 因为此方法实际上是使用Script标签的形式发送请求
  - 开发阶段通过proxy代理
  - 后端基于cors access-controls-allow-origin
  - nginx代理


 # cookie的前世今生
 - cookie的出现主要是为了解决http请求无状态的问题，让服务器知道是哪个用户在访问数据
 - 将token放在自定义头部 可以有效的防止CORS
 - 防止cors还可以在服务器判断请求的origin和referer，也可以在设置cookie的时候设置SameSite 
 - 防止xss攻击设置HttpOnly 不运行通过脚本获取cookie的值
 - 既然可以直接通过cookie鉴权，前端为什么还要在Authorization 中设置token？ 因为api设计不一定是给浏览器使用的，如果是其他系统，并没有cookie这东西，所以Authorization 更加通用