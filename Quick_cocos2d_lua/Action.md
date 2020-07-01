## 动作

* `cc.MoveTo:create(2, cc.p(0,0))`
* `cc.MoveBy:create(2, cc.p(0,0))`
* `cc.JumpTo:create(2, cc.p(0,0), 50, 2)` 跳跃高度50，跳跃次数2
* `cc.JumpBy:create(2, cc.p(0,0), 50, 2)`
* `cc.BezierTo:create(2, {cc.p(display.right, display.top), cc.p(200, 200), cc.p(50, 100)})` 参数table贝塞尔曲线数据，控制点1的坐标，控制点2的坐标，目标终点
* `cc.BezierBy:create(2, {cc.p(display.right, display.top), cc.p(200, 200), cc.p(50, 100)})`
* `cc.ScaleTo:create(2, 0.5)`
* `cc.RotateTo:create(2, 180)`
* `cc.FadeIn:create(2)` 淡入
* `cc.FadeOut:create(2)` 淡出
* `cc.FateTo:create(2, 110)` 变化到指定透明度
* `cc.TintTo:create(1, 0, 255, 0)`
* `cc.Blink:create(1, 2)`
* `cc.DelayTime:create(2)`
* `cc.Follow:create(followedNode, rect)` 一个节点跟随另一个节点，超出rect就不再跟随，常用来设置Layer跟随Sprite

## 容器

* `cc.Repeat:create(action, times)`
* `cc.RepeatForever:create(action)`
* `cc.Spawn:create(action1, action2...)`
* `cc.Sequence:create(action1, action2...)`

## 速度

* `cc.Speed:create(action, speed)`

> In: 表示动作执行先慢后快
> Out: 表示动作执行先快后慢
> InOut: 表示动作执行慢-快-慢

* `cc.EaseSineIn:create(action)`
> `EaseExpoIn EaseExpoOut EaseExpoInOut` 指数缓冲
> `EaseSineIn EaseSineOut EaseSineInOut` sine缓冲
> `EaseElasticIn` 弹性缓冲
> `EaseBounceIn` 跳跃缓冲
> `EaseBackIn` 回震缓冲

## 动作管理

* `node:runAction(action)`
* `node:stopAllActions()`
* `node:stopAction(action)`
* `node:stopActionByTag(tag)`
* `node:getActionByTag(tag)`
* `node:setTag(tag)`
* `action:setTag(tag)`