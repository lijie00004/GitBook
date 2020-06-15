## UIRedPoint

```
UIRedPoint.addUIPoint(btn, callfun) -- 注册点击事件
```

## UILuaLoader

```lua
UILuaLoader.attachEffect(btnBuy,"outline(0e0600,1)")
```



## NewScene

```lua
var.xmlPageLevel = UILuaLoader.load("uilayout/PanelAvatar_level.uif")
if var.xmlPageLevel then
    util.asyncload(var.xmlPageLevel, "page_reborn_bg", "needload/page_reborn_bg.jpg")
    var.xmlPageLevel:align(display.LEFT_BOTTOM, 0, 0):addTo(var.xmlPanel)
```



## NetClient

```lua
local num = NetClient:getTypeItemNum(itemId)

NetClient.mCharacter.mLevel -- 玩家等级
```

## util

```lua
-- 按钮动画
util.addHaloToButton(var.level_btnLevel, "btn_normal_light13")
var.level_btnLevel:removeChildByName("img_bln")
```



## SceneAndLayer

* `UISceneGame` 主场景
* `UIRightBottom` 主场景左下角技能区域
* `RBPart` 配置，主场景左下角技能区域
* `UIPropModel` 自动战斗和已经隐藏的箭头按钮和其它按钮

