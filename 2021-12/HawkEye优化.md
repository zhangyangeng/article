# HawkEye 4.1 可优化项

## 新建流程

toolBar 部分代码被注释与隐藏，是否删除？

g6-editor 部分清除数据代码逻辑是否可优化？
- 当前逻辑是将数据源替换为指定数据源，然后移除该指定数据源
- 是否可以直接使用 `graph.clear()` 清除？

flow-chart 部分画布缩放无用，是否删除？

save-chart 问答句输入框的 (valueChange) 事件无效？

item 中新增节点中含有无用属性 `xxx` ？