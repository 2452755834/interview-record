## ts的target
  tsc 的代码降级编译并不能完全处理兼容性。需要引入其他的polyfill。ts应该只作为类型检查工具来使用，至于代码降级兼容性还是应该交给babel来做
  ![alt text](image.png)

## babel和esbuild的关系
![alt text](image-1.png)