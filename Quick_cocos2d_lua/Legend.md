## UIRedPoint

```
UIRedPoint.addUIPoint(btn, callfun) -- 注册按钮点击事件
```

## UILuaLoader

```lua
UILuaLoader.attachEffect(btnBuy,"outline(0e0600,1)") -- 设置描边
```



## Create new scene

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
NetClient.severDay -- 服务器天数
```

## util

```lua
-- 按钮动画
util.addHaloToButton(var.level_btnLevel, "btn_normal_light13")
var.level_btnLevel:removeChildByName("img_bln")
```



## SceneAndLayer

* `UISceneGame` 主场景
* `PanelAvatar` 角色
* `PanelBag` 背包
* PanelRecycle 物品回收
* `UIRightBottom` 主场景左下角技能区域
* `RBPart` 配置，主场景左下角技能区域
* `UIPropModel` 自动战斗和已经隐藏的箭头按钮和其它按钮

## Sprite

```lua
img_wingl = cc.Sprite:create()	img_wingl:addTo(var.xmlPanel:getWidgetByName("img_wing_view")):align(display.CENTER,23, 23):setName("img_wing")

local filepath = "stateitem/wing/"..data_wing[level+1]..".png"
asyncload_callback(filepath, img_wingl, function(filepath, texture)
img_wingl:setTexture(filepath)
end)
```

## Label

```lua
{n="Text_11",id=15,parent=13,ax=0.5,ay=0.5,x=44,y=77,color="253|223|174",fr="DFYuan.ttf",olc="0,0,0,255",type=3,tag=101,v=true,fs=20,text="攻击:",ols=1,},-- 白色
{n="clean_tip",id=29,parent=1,ay=0.5,x=508,y=245,color="24|209|41",fr="DFYuan.ttf",olc="0,0,0,255",type=3,tag=533,v=false,fs=20,text="祝福值清理倒计时:",ols=1,}, -- 绿色
{n="txt_titile",id=32,parent=1,ax=0.5,ay=0.5,x=690,y=184,color="255|62|63",fr="DFYuan.ttf",olc="0,0,0,255",type=3,tag=46,v=true,fs=20,text="祝福值越高进",ols=0,},-- 红色
{n="level_left",id=41,parent=1,w=20,h=60,ax=0.5,ay=0.5,x=51,y=513,color="255|208|66",fr="DFYuan.ttf",olc="76,0,0,255",type=3,tag=98,v=true,fs=20,text="体验",ht=1,ols=1,},-- 黄色加描边
{n="Txt_lingyu",id=52,parent=1,ay=0.5,x=743,y=130,color="255|192|22",fr="DFYuan.ttf",olc="0,0,0,255",type=3,tag=42,v=true,fs=20,text="0/5",ols=1,}, -- 黄色
```