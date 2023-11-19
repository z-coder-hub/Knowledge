#### 动画属性

##### animation

> 以下是CSS Animation常用的属性值：
>
> - animation-name：关键帧动画名称。
> - animation-duration：动画的持续时间。使用s或ms作为单位。
> - animation-timing-function：动画计时函数，定义动画如何在时间上变化。
> - ease：渐效缓和，慢短开始、慢长结束
> - linear：线性均速，匀速运动
> - ease-in：加速渐显，慢短开始、快长结束
> - ease-out：减速渐隐, 快短开始、慢长结束 
> - ease-in-out：缓动运动，在起点及终点时卡顿，其他地方均匀
> - step-start：分步启动，动画每秒跳一次(start 帧)，等同于steps(1,start)
> - step-end：分步结束，动画每秒跳一次(end 帧)，等同于 steps(1,end)
> - steps(n,start|end) 根据时间节点将动画分成n步运行，start 视图选第一帧显示完毕，而 end 视图则是选最后一帧完全显示之后才切换到下一个状态
> - animation-delay：动画延迟，指定动画何时开始。使用s或ms作为单位。
> - animation-iteration-count：动画重复次数，无限重复可以使用infinite。
> - animation-direction：指定是否应该轮流反向播放动画，reverse，alternate-reverse或normal（默认）。
> - animation-fill-mode：指定动画结束后应用样式的方式，none、forwards、backwards和both。
> - animation-play-state: 控制动画播放状态，可以是paused或running。
>
> 这些是最常用的属性值，还有其他更高级的选项，可根据需要进行深入研究。



##### @keyframes

> ```css
> @keyframs 起个名字{
>     form{
>         // 刚开始的样式
>     }
>     to{
>         // 结束时的样式
>     }
> }
>  
> // 或者
> @keyframs 起个名字{
>     0%{
>         // 动画刚开始执行的样式
>     }
>     25%{
>         // 执行到1/4时的样式
>     }
>     50%{
>         
>     }
>     75{
>         
>     }
>     100%{
>         
>     }
> }
> ```



#### 改变元素位置

##### transform

> 参数总结
>
> ```css
> transform: translate(); x y 轴的偏移距离，px结尾
> transform: scale(); 改变宽高大小，倍数
> transform: rotate(); 旋转度数 deg结尾
> transform: skew(); 改变盒子的倾斜角度
> ```
> 



