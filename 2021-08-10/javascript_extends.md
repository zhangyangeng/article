# 函数方法中新增：

- bind():
- 该方法主要讲函数绑定到某个对象中，可以解决 this 相关问题
```javascript
function().bind(this);
// 等价于
this.function()
```