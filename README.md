# rmc-custom-wr-tabs
自定义rac-tabs中的tab栏，布局不再是等分tab选项，而是根据选项的文字自适应underline, 且当tab栏点击开始偏移到中央位置的偏移，并不是以百分比，而是以精确的平移计算出来。

## 之前的源码的tabs思路：
##### 以百分比为计算。当一屏显示5个，则偏移量是20%
##### 下划线的长度会是20%
##### 当点击大于半屏的时候，上边的tabs左移的偏移量也是20%

## 会出现的问题：
##### 当tab选项字数不一样的时候，会出现间距不一致，或多或少
##### 当字数太多，字数会挤在一起，不换行
##### 当大于半屏的时候，上面的左移联动越到后面，位置会偏移到左边，一直到看不见，很多bug

## 更改源码思路：
##### 对下划线的长度，按照点击后会放大后的字数*每个字数的大小。 
```js
18 * props.tabs[props.activeTab].textLen,
```
##### 点击后的偏移量。
```js
left: this.props.tabs[this.props.activeTab].leftValue + 'px'
```
##### 当大于半屏，上边的tab的左移联动的偏移量

```js
var offset = -tabs[activeTab].leftTransform;
if (_this.layout) {
    _lastIndex = tabs.length > _lastIndex ? 0 : _lastIndex;
    var textOffst = (tabs[_lastIndex].textLen - tabs[activeTab].textLen) * 4;
    var canScrollOffset = isVertical ? -_this.layout.scrollHeight + _this.layout.clientHeight : -_this.layout.scrollWidth + _this.layout.clientWidth;
    canScrollOffset += textOffst;
    offset = Math.min(offset, 0);
    offset = Math.max(offset, canScrollOffset);
}
_this.onPan.setCurrentOffset(offset);
_lastIndex = activeTab;
return {
    transform: getPxStyle(offset, 'px', isVertical),
    showPrev: activeTab > center - .5 && tabs.length > page,
    showNext: activeTab < tabs.length - center - .5 && tabs.length > page
};
```

![avator](./等间距.gif)
