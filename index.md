# 单页应用如何提高加载速度

- 路由懒加载
- 代码分割，按需加载，首先加载重要的资源 特别是首屏加载的时候，其余不重要的内容可以推迟加载
- 资源的压缩 gzip、cdn

# 首屏加载如何优化

- 减少入口文件的体积 （怎么做？）
  - 路由懒加载 这样其他路由的文件不会打包进来，首次加载的时候包体积就不会太大
  - vite打包使用manualChunks，手动分包，将一些依赖包拆分成多个子包，这样首次加载的时候就不需要加载一整个大包，因为可能首次加载的时候，其中一些大的依赖文件并不需要加载
- 组件库按需加载 减少组件库包的体积 或者直接上cdn
   

# react 、vue 、ts、vite视频+面试题、uniapp（项目+基础面试题）、js、css （大师课）

# 项目优化

- 解决项目存在已久的内存飙升问题
  - 首先通过代码注释的方式，找到表格的缺省图有问题，导致内存无法回收，排查发现图片是使用 svg 组件的形式书写，查询资料后发现这种方式确实会导致内存无法回收，所以将项目中的 svg 组件全部替换成 iconfont 的形式
  - 后续发现 el-dialog 存在的情况下内存也无法正常回收，再 issue 中发现是 teleport 的问题，该问题再**3.3.6**版本中进行了修复，所以后续对项目 vue 版本进行了升级，参考地址 https://github.com/vuejs/core/pull/6529
  - 再排查 svg 组件问题的时候其实有注意到 issue 中提到的静态提升问题导致的内存飙升的问题，但是在尝试禁用静态提升后发现内存也没有完全回收，原本以为是方向错了，后面经过尝试，发现是 element-plus 和@element-plus/icons-vue 早期版本并没有取消静态提升，后面进行版本的升级。element-plus 2.4.3 版本使用的是新版的 icon
    ![alt text](image.png)
    https://github.com/element-plus/element-plus-icons/pull/65
  - 自己项目内部 vite.config.ts 需要添加配置， 取消静态提升
  ```js
  vue({ template: { compilerOptions: { hoistStatic: false } } });
  ```
  - 后续 vue 的 3.5 版本已经对该问题进行了修复  
    https://github.com/vuejs/core/pull/11067
    ![alt text](image-1.png)
  - 使用requestIdleCallback,优化列表加载。
    需求：每页是50条数据，因为一次性加载50条数据有时候会有卡顿问题，所以使用requestIdleCallback分片加载，首次加载10条数据，然后再当前渲染帧有剩余时间的情况下分批加载10-20条数据

# 多页签通信，项目中如果一个页面 token 过期了，然后退出登录并且重新登录了， 这个时候另一个页面再去请求接口会被踢出去登录页怎么办

- 登录完成之后 把 token 存在 cookie，pinia 中也存一份 token，这份 token 是取的 cookie 中的数据
- 如果一个页面被踢测出了 那么清空 localstorage 的数据
- 再次登录后会更新 cookie 中的 token
- 这个时候第二个页面请求接口会 401，在 axios 中判断如果 cookie 中的 token 和 pinia 中的不一样，那么吧 pinia 中的 token 更新为 cookie 中最新的 token 重新刷新一下页面



# 表格空列兼容Safari浏览器
- 首先表格设置空列存在一下几个问题
  - 表头宽度和内容区域宽度不一致，并且表头中空列的宽度和内容区域空列宽度不一致：针对这个问题，发现表头宽度是我们设置的宽度，列表内容区域的列宽不对，在Safari浏览器下，给相应的列多包一层div，并且直接设置对应的宽度。
  - 
  -后续又发现部分页面是表头的宽度也会异常，colgroup宽度正常，但是th宽度异常，因为直接给th加宽度是无效的，因此通过headerCellStyle方法给表头添加自定义属性,将宽度设置到表头的某个类上，下面的cell元素直接设置定宽
  ```js
      /** 设置表头样式，解决Safari表头错行问题 */
    headerCellStyle({ column }: any) {
      if (column.property) {
        // 非空列情况下设置表头宽度
        return { '--fix-safari-header-width': column.width + 'px' };
      } else {
        // 空列不设置宽度
        return null;
      }
    },
  ```

  # h5在微信内部使用，会员购买，需要微信授权获取用户信息，已经购买成功提供入口直接打开小程序
  - 首先在route的meta中添加字段判断，该页面是否需要进行微信授权，并且获取不到token，且不是快照用户，那么直接打开微信授权的url进行跳转
  - 授权完成后回到重定向空白页，该页面做用户信息获取的操作。这里需要注意有可能获取到的是快照用户，这个时候再sessionstorage里面加个字段判断是否是快照用户，结束在之后，跳转到会员页面.
  - 在会员页内部增加判断，如果是快照用户就是没有token，就不获取相关需要token的接口，如果返回的页面是授权回调等页面，直接关闭浏览器，防止死循环授权
  - 会员购买完成后再新页面提供直接打开小程序的按钮，需要注意ios手机要使用window.location.href进行跳转，安卓直接路由跳转即可，否则会出现ios手机点击按钮无法打开小程序的问题
  - 为什么要在中间页获取用户信息，一方面是为了将授权的功能拆分出来，后续其他页面如果也需要微信授权，也可以采用同样的方式，只需要在meta配置字段即可。其次如果放在单独页面做此操作的话，还要考虑微信授权回来的code有效时间是很短的，如果在页面使用后没有将code从路由的query中剔除，用户刷新页面会导致接口报错等问题。