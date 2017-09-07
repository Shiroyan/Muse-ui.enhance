# 修改说明
## 基于`Muse-ui`的一些增强
> **Muse-ui** [文档](http://www.muse-ui.org/#/)

## 增强的组件
### Infinite Scroll
*  增加 `scrolldown` 和  `scrollup` 事件, 返回当前 `滚动高度`\
*  scrolldown 滚动条往下  scrollup 滚动条往上
* 应用场景 - 滚动隐藏底部按钮            
``` 
<mu-infinite-scroll 
        :scroller="container" 
        :loading="loading" 
        @load="loadMore" 
        @scrollup="_showButton" 
        @scrolldown="_hideButton" />
```

