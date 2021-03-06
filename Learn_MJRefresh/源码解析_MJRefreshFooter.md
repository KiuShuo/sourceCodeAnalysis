### Learn_MJRefreshFooter

会弹回到底部的上拉刷新控件

需要思考的问题：  
1. 怎么判断状态是否需要变化？或者是怎么做到实时监控更新刷新控件的状态  
2. 刷新控件变化后怎么更改UI状态  
3. 根据上面的问题描述出上拉刷新整个过程的代码执行  

#### 继承关系
refreshFooter的继承关系从上到下依次为：  
`MJRefreshComponent`  
[`MJRefreshFooter`](#Learn_MJRefreshFooter)  
[`MJRefreshAutoFooter`](#Learn_MJRefreshAutoFooter)   
[`MJRefreshAutoStateFooter`](#Learn_MJRefreshAutoStateFooter)   
[`MJRefreshAutoNormalFooter`](#Learn_MJRefreshAutoNormalFooter) [`MJRefreshAutoGifFooter`](#Learn_MJRefreshAutoGifFooter)

`MJRefreshComponent`  
[`MJRefreshFooter`](#Learn_MJRefreshFooter)  
[`MJRefreshBackFooter`](#Learn_MJRefreshBackFooter)   
[`MJRefreshBackStateFooter`](#Learn_MJRefreshBackStateFooter)   
[`MJRefreshBackNormalFooter`](#Learn_MJRefreshBackNormalFooter) [`MJRefreshBackGifFooter`](#Learn_MJRefreshBackGifFooter)

<font color='red'>`MJRefreshAutoFooter`与`MJRefreshBackFooter`的区别：</font>  

`MJRefreshAutoFooter`的`mj_footer`紧跟在`scrollView`的内容显示区域的底部; `MJRefreshBackFooter`的`mj_footer`紧跟在`scrollView`的底部（值的是内容或者`scrollView`相比高度更大者的底部）。

`MJRefreshAutoFooter`和`MJRefreshBackFooter`显示位置不同。由于前者显示位置的特殊性，决定了他只有普通闲置、正在刷新、没有更多数据三种状态；而后者跟下拉刷新控件类似 还有松开就可以进入刷新状态状态。 所以前者也可以叫做可以自动刷新的上拉刷新控件。


#### <a id='Learn_MJRefreshFooter'></a>`MJRefreshFooter`

1. 提供两个便利构造方法；
2. 提供两个供外部使用的更新状态方法；

	```
	/** 提示没有更多的数据 
	-> 将当前的state设置为MJRefreshStateNoMoreData没有更多数据状态*/
	- (void)endRefreshingWithNoMoreData;
	
	/** 重置没有更多的数据（消除没有更多数据的状态）
	->  将当前的state设置为MJRefreshStateIdle闲置状态 */
	- (void)resetNoMoreData;
	```
3. 提供`ignoredScrollViewContentInsetBottom`存储属性, 当`scrollView设置过insetBottom时 调整与scrollView显示区域的间距`；
4. 提供`automaticallyHidden`属性（[实现原理](/Users/liushuo199/Documents/LearnNote/LearnSourceCode/Learn_MJRefresh/源码解析_MJRefresh.md)）


## <a id='Learn_MJRefreshAutoFooter'></a>`MJRefreshAutoFooter`

会自动刷新的上拉刷新控件  `mj_footer`一直吸附在`scrollView`的内容展示区域之后。

只有三个状态：`MJRefreshStateIdle`普通闲置状态、`MJRefreshStateRefreshing`正在刷新中状态、`MJRefreshStateNoMoreData`所有数据加载完毕没有更多数据状态。

正是由于`mj_footer`一直吸附在`scroolView`的内容展示区域之后 能够在正常状态完全显示出来，所以不需要考虑正在刷新状态时的悬停问题。

<font color='red'>核心功能：</font>  
在`mj_footer`将要添加到`scrollView`上的时候 设置其位置，使其吸附在`scrollView`内容展示之后 设置`scroolView`的`contentInset.bottom`使其能和`scollView`的内容一样可以正常展示出来。  
通过KVO监控`scrollView contentSize`的变化 调整`mj_footer`的位置 使其一直吸附在`scrollView`内容展示之后。  
通过KVO监控`scrollView contentOffset`的变化 来判断需要自动刷新的时机。  
通过KVO监控`scrollView`平移手势的变化 来判断手势结束后是否需要开启上拉刷新。  

#### <a id='Learn_MJRefreshAutoStateFooter'></a>`MJRefreshAutoStateFooter`
<font color='red'>核心功能：</font> 布局`mj_footer` 设置各个状态下`mj_footer`上要显示的文本 当状态变化时更新`mj_footer`上显示的内容。  

#### <a id='Learn_MJRefreshAutoStateFooter'></a>`MJRefreshAutoStateFooter`
<font color='red'>核心功能：</font> 添加菊花视图 控制不同状态下的菊花动画开启关闭状态。  

#### <a id='Learn_MJRefreshAutoGifFooter'></a>`MJRefreshAutoGifFooter`
<font color='red'>核心功能：</font> 创建动画视图 控制不同状态下动画视图要显示的图片。

## <a id='Learn_MJRefreshBackFooter'></a>`MJRefreshBackFooter`
一直吸附在`scrollView`的最后面(当`scrollView`能滑动时，吸附在能滑动的最后面；当`scrollView`不能滑动时，吸附在`scroolView`的最后面)

<font color='red'>核心功能：</font>   
通过KVO监控`scrollView contentSize`的变化 调整`mj_footer`的位置 使其一直吸附在`scroolView`的最后面。  
通过KVO监控`scrollView contentOffset`的变化 调整刷新控件的状态。  

#### <a id='Learn_MJRefreshBackStateFooter'></a>`MJRefreshBackStateFooter`
<font color='red'>核心功能：</font> 布局`mj_footer` 设置各个状态下`mj_footer`上要显示的文本 当状态变化时更新`mj_footer`上显示的内容。   

#### <a id='Learn_MJRefreshBackNormalFooter'></a>`MJRefreshBackNormalFooter`
<font color='red'>核心功能：</font> 布局`mj_footer` 添加箭头和菊花 根据状态的变化更新箭头菊花的样式。  

#### <a id='Learn_MJRefreshBackGifFooter'></a>`MJRefreshBackGifFooter`
<font color='red'>核心功能：</font> 布局`mj_footer` 添加动画显示的`imageView` 设置不同状态下要显示的动画图片 根据状态的变化更新动画。
