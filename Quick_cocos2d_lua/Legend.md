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
local num = NetClient:getItemNumByType(29160004)
local clothDef = NetClient:getItemDefByPos(Const.ITEM_CLOTH_POSITION)
-- 设置 元宝 和 金钱
cc.EventProxy.new(NetClient,var.xmlPanel)
	:addEventListener(Notify.EVENT_GAME_MONEY_CHANGE, PanelStore.updateGameMoney)
if var.xmlPanel then
    local mainrole = NetClient.mCharacter
    local moneyLabel = {
        {name="lblVcoin",	value =	mainrole.mVCoin or 0	,	},
        {name="lblBVcoin",	value =	mainrole.mVCoinBind or 0,	},
        {name="lblMoney",	value =	mainrole.mGameMoney or 0,	},
        {name="lblBMoney",	value =	mainrole.mGameMoneyBind or 0,},
    }
    for _,v in ipairs(moneyLabel) do
        var.xmlPanel:getWidgetByName(v.name):setString(v.value)
    end
end

NetClient.mCharacter.mLevel -- 玩家等级
NetClient.severDay -- 服务器天数
MainRole._mainAvatar:NetAttr(Const.net_wing) -- 翅膀id
MainRole._mainAvatar:NetAttr(Const.net_fashion) -- 时尚id
MainRole._mainAvatar:NetAttr(Const.net_cloth) -- 衣服id
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

-- 背景
local imgBg = self.m_serverUI:getChildByName("imgSceneBg"):align(display.CENTER, display.cx, display.cy)
asyncload_callback("needload/img_battle.png", imgBg, function (filepath, texture)
	if utilapp.isObjectExist(imgBg) then
    	imgBg:loadTexture(filepath):scale(cc.MAX_SCALE)
    end
end)
```

## Label

```lua
{n="Text_11",id=15,parent=13,ax=0.5,ay=0.5,x=44,y=77,color="253|223|174",fr="DFYuan.ttf",olc="0,0,0,255",type=3,tag=101,v=true,fs=20,text="攻击:",ols=1,},-- 白色
{n="clean_tip",id=29,parent=1,ay=0.5,x=508,y=245,color="24|209|41",fr="DFYuan.ttf",olc="0,0,0,255",type=3,tag=533,v=false,fs=20,text="祝福值清理倒计时:",ols=1,}, -- 绿色
{n="txt_titile",id=32,parent=1,ax=0.5,ay=0.5,x=690,y=184,color="255|62|63",fr="DFYuan.ttf",olc="0,0,0,255",type=3,tag=46,v=true,fs=20,text="祝福值越高进",ols=0,},-- 红色
{n="level_left",id=41,parent=1,w=20,h=60,ax=0.5,ay=0.5,x=51,y=513,color="255|208|66",fr="DFYuan.ttf",olc="76,0,0,255",type=3,tag=98,v=true,fs=20,text="体验",ht=1,ols=1,},-- 黄色加描边
{n="Txt_lingyu",id=52,parent=1,ay=0.5,x=743,y=130,color="255|192|22",fr="DFYuan.ttf",olc="0,0,0,255",type=3,tag=42,v=true,fs=20,text="0/5",ols=1,}, -- 黄色
{n="per",id=299,parent=300,ax=0.5,ay=0.5,x=146,y=160,color="255|244|153",fr="DFYuan.ttf",olc="226,32,0,255",type=3,tag=268,v=true,fs=20,text="精炼",ols=1,}, -- 白色，红色

```

## Animation

```lua
-- 激战boss动画
-- 资源地址 res/cloth
local img_role = box:getChildByName("img_role")
if not img_role then
    img_role = cc.Sprite:create()
    img_role:addTo(box):align(display.CENTER, 95, 20):setName("img_role")
end
-- 参数 1 是类型，写0就可以了
-- 参数 2 是文件名 15009 (1500900)
-- 参数 3 是第几个动画
-- 参数 4 是延迟时间和间隔时间
local animate = cc.AnimManager:getInstance():getPlistAnimate(0,15009,4,12)
if animate then
    img_role:stopAllActions()
    img_role:runAction(cca.seq({
    cca.rep(animate,10000),
    cca.removeSelf()
    }))
end
```

```lua
-- 资源地址 res/effect
local img_role = var.xmlPageBag:getChildByName("img_role")
if not img_role then
    img_role = cc.Sprite:create()
    img_role:addTo(var.xmlPageBag):align(display.CENTER, 226, 370):setName("img_role")
end
local sp = util.addEffect(img_role,"spriteEffect",4,effId,{x = -80 , y = -40})
```

