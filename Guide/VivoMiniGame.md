[Vivo教程网站](https://minigame.vivo.com.cn/documents/#/lesson/open-ability/ad)

---

CocosCreator构建配置: ![CocosCreator构建配置](.\images\vivoCreateCofig.png)

![服务器布置](.\images\serverVivo.png)

---

```javascript
createAdBanner() {
    console.log("创建Banner广告")
    this.bannerAd = qg.createBannerAd({
        posId: 'c29da96e6edb4020a445bef2dfb7fc19',
        style: {} // 底部中间位置
    });
    this.bannerAd.onError(err => {
        // console.log("banner广告加载失败", err);
        // qg.showToast({
        //     message: "banner广告加载失败" + JSON.stringify(err)
        // })
    });
    // 手动关闭广告
    this.bannerAd.onClose(err => {
        this.countTime = 0;
        // qg.showToast({
        //     message: "banner广告关闭" + JSON.stringify(err)
        // })
    });

    this.bannerAd.show().then(()=>{ 
        // console.log('banner广告展示完成');
    }).catch((err)=>{
        // qg.showToast({
        //     message: 'banner广告展示失败' + JSON.stringify(err)
        // })
    })
},
```

```javascript
createRewardedAd(foo) {
    console.log("创建激励广告")
    this.foo = foo; // 正常播完后，奖励函数
    if (this.countTime_reward <= 60) {
        qg.showToast({
            message: "时间间隔要大于60秒"
        })
        return false;
    }
    if (this.rewardedAd) {
        this.rewardedAd.load()
        return true;
    }
    this.rewardedAd = qg.createRewardedVideoAd({
        posId:'3a4c61abe6304f08b9a4f3e60fbf749e',
    });
    // this.rewardedAd.onError(err => {
    //     qg.showToast({
    //         message: "激励视频广告加载失败" + JSON.stringify(err)
    //     })
    // });
    this.rewardedAd.onLoad(res => {
        if(gb.isVoice) {
            gb.VoiceMgr.pauseBg();
        }
        // console.log('激励视频广告加载完成-onload触发', JSON.stringify(res));
        this.rewardedAd.show().then(()=>{ 
            // qg.showToast({
            //     message: "激励视频广告展示完成"
            // })
        }).catch((err)=>{
            // qg.showToast({
            //     message: "激励视频广告展示失败" + JSON.stringify(err)
            // })
        }) 
    })
    const func = (res)=>{
        if(gb.isVoice) {
            gb.VoiceMgr.resumeBg();
        }
        this.countTime_reward = 0;
        if (res && res.isEnded) {
            this.foo()
            // qg.showToast({
            //     message: "正常播放结束，可以下发游戏奖励"
            // })
        } else {
            // qg.showToast({
            //     message: "播放中途退出，不下发游戏奖励"
            // })
        }
    }
    this.rewardedAd.onClose(func);
    return true
}
```

```javascript
updateNativeAd() {
    this.levelPreNode.getChildByName("ad").active = false;
    console.log("创建原生广告")
    if (this.nativeAd) {
        this.nativeAd.load()
        return;
    }
    
    this.nativeAd = qg.createNativeAd({
        posId:'2a7f56a5142941dea9fe9830d4d6f2e8',
    });
    this.nativeAd.onLoad(res => {
        let nativeCurrentAd;
        if (res && res.adList){
            nativeCurrentAd = res.adList.pop();
        }
        
        var adNode = this.levelPreNode.getChildByName("ad");
        var imgUrlListNode = adNode.getChildByName("imgUrlList");
        var iconNode = adNode.getChildByName("icon");
        var titleNode = adNode.getChildByName("title");
        var descNode = adNode.getChildByName("desc");
        var advertisingNode = adNode.getChildByName("advertising");
        if (!nativeCurrentAd) {
            return;
        }
        adNode.active = true;
        imgUrlListNode.active = false;
        iconNode.active = false;
        titleNode.anchorX = 0.5;
        descNode.anchorX = 0.5;
        titleNode.x = 0;
        descNode.x = 0;

        this.nativeCurrentAd = nativeCurrentAd;
        this.nativeAd.reportAdShow({ adId: nativeCurrentAd.adId.toString() });

        let imgUrl_url = nativeCurrentAd.imgUrlList[0]
        if (imgUrl_url) {
            cc.loader.load(imgUrl_url.toString(), (err, tex) => {
                imgUrlListNode.active = true;
                let spr = new cc.SpriteFrame(tex);
                let sprRect = spr._rect;
                imgUrlListNode.scale = Math.floor(280 / sprRect.height * 100) / 100;
                imgUrlListNode.getComponent(cc.Sprite).spriteFrame = spr;
                cc.loader.setAutoRelease(tex, true)

                advertisingNode.x = imgUrlListNode.width * imgUrlListNode.scale / 2 - 10
            });
        }

        let icon_url = nativeCurrentAd.icon.toString();
        if (icon_url != "") {
            cc.loader.load(icon_url, (err, tex) => {
                iconNode.active = true;
                titleNode.anchorX = 0;
                descNode.anchorX = 0;
                titleNode.x = -50;
                descNode.x = -50;
                let spr = new cc.SpriteFrame(tex);
                let sprRect = spr._rect;
                iconNode.scale = Math.floor(100 / sprRect.height * 100) / 100;
                iconNode.getComponent(cc.Sprite).spriteFrame = spr;
                cc.loader.setAutoRelease(tex, true)
            });
        }

        titleNode.getComponent(cc.Label).string = nativeCurrentAd.title.toString();
        descNode.getComponent(cc.Label).string = nativeCurrentAd.desc.toString();

    })
    this.nativeAd.onError(err => {
        var adNode = this.levelPreNode.getChildByName("ad");
        adNode.active = false;
    });
},
```

---

## 广告错误码信息

| 错误码 | 错误描述                                                     | 解决方案                                                     |
| ------ | ------------------------------------------------------------ | :----------------------------------------------------------- |
| -1     | 未知原因                                                     | 联系技术对接                                                 |
| -2     | 外部SDK错误                                                  | 联系技术对接                                                 |
| -3     | 广告拉取太频繁                                               | 广告拉取时间间隔建议在10s以上                                |
| -4     | 激励视频一分钟之内只能调用一次                               | setTimeout 一分钟后再load                                    |
| -100   | 网络超时，广告加载不出来                                     | 换个手机换个网络试试                                         |
| 1      | 应用id或者广告位id配置信息不存在                             | 检查对应ID是否正确填写，确认已申请广告位                     |
| 2      | 应用被冻结                                                   | 咨询广告联盟客服冻结原因                                     |
| 3      | 广告位被冻结                                                 | 咨询广告联盟客服冻结原因                                     |
| 4      | 没有对应的广告                                               | 检查posId参数与申请的一致，并确认申请了对应广告位            |
| 5      | 异常                                                         | 联系技术对接                                                 |
| 101    | 网络异常                                                     | 检查是否可以正常上网                                         |
| 102    | 本地JSON解析异常                                             | 联系技术对接                                                 |
| 103    | 服务器返回错误                                               | 联系技术对接                                                 |
| 104    | 解密失败                                                     | 联系技术对接                                                 |
| 105    | 素材加载失败                                                 | 联系技术对接                                                 |
| 106    | 广告参数错误                                                 | 检查广告参数是否填写正确                                     |
| 107    | 广告信息加载超时（开屏有时间限制）                           | 延时再次加载                                                 |
| 108    | 不存在广告                                                   | 广告填充率问题，开发者忽略即可                               |
| 200    | ---                                                          | 200不需要处理，忽略即可                                      |
| 500    | 网络问题（1052引擎版本是调用频率过高参见30009）              | 该问题是由于网络问题导致，部分CP会遇到，可以试试4G或者换个网络测试 |
| 20000  | 广告加载失败                                                 | 检查posId是否正确填写                                        |
| 30000  | 广告对象长时间不用会被回收导致或者是创建初始化未完成或未初始化 (等待初始化首次加载完成) | 重新创建或者是等待加载完成在onload里面去show广告             |
| 30002  | 加载广告失败                                                 | banner、插屏广告重新create然后show，激励视频可以直接去load，需要等到10秒之后调用 |
| 30003  | 新用户有广告免打扰期                                         | 测试时可以将手机时间调成一天之后                             |
| 30006  | 插屏广告播放次数已达限制                                     |                                                              |
| 30007  | banner广告播放次数已达限制                                   |                                                              |
| 30008  | 启动来源不支持展示广告                                       | 检查启动来源是否在申请广告位ID时填写的信息范围内             |
| 30009  | 10秒内调用广告次数超过1次                                    | 降低广告展示频率，建议最少间隔10s                            |
| 100000 | 请求格式错误                                                 | 检查请求代码格式是否与示例一致                               |
| 100125 | 广告位宽高无效                                               | 联系技术对接                                                 |
| 101000 | 请求 ID 信息缺失                                             | 联系技术对接                                                 |
| 102006 | 无广告                                                       | 联系技术对接                                                 |
| 103050 | 应用操作系统信息错误                                         | 联系技术对接                                                 |
| 103060 | 应用包名和注册包名不一致                                     | 检查包名是否和申请广告位时填写的一致                         |
| 104000 | 设备信息缺失                                                 | 联系技术对接                                                 |
| 104010 | 设备类型缺失                                                 | 联系技术对接                                                 |
| 104011 | 设备类型信息错误                                             | 联系技术对接                                                 |
| 104020 | 操作系统信息缺失                                             | 联系技术对接                                                 |
| 104021 | 操作系统信息错误                                             | 联系技术对接                                                 |
| 104030 | 操作系统版本信息缺失                                         | 联系技术对接                                                 |
| 104040 | 操作系统主版本信息缺失                                       | 联系技术对接                                                 |
| 104050 | 厂商信息缺失                                                 | 联系技术对接                                                 |
| 104060 | 设备型号信息缺失                                             | 联系技术对接                                                 |
| 104070 | 设备唯一标识符缺失                                           | 联系技术对接                                                 |
| 104071 | 设备唯一标识符错误                                           | 联系技术对接                                                 |
| 104080 | android id缺失                                               | 联系技术对接                                                 |
| 104081 | android id错误                                               | 联系技术对接                                                 |
| 104090 | 屏幕尺寸信息缺失                                             | 联系技术对接                                                 |
| 104100 | 屏幕尺寸宽度缺失                                             | 联系技术对接                                                 |
| 104110 | 屏幕尺寸高度缺失                                             | 联系技术对接                                                 |
| 105000 | 网络环境信息缺失                                             | 联系技术对接                                                 |
| 105010 | 网络地址信息缺失                                             | 联系技术对接                                                 |
| 105011 | 网络地址信息格式错误                                         | 联系技术对接                                                 |
| 105020 | 网络连接类型缺失                                             | 联系技术对接                                                 |
| 105021 | 网络连接类型错误                                             | 联系技术对接                                                 |
| 105030 | 运营商类型缺失                                               | 联系技术对接                                                 |
| 105031 | 运营商类型错误                                               | 联系技术对接                                                 |
| 105040 | Wi-Fi热点地址信息缺失                                        | 联系技术对接                                                 |
| 107000 | 广告位ID缺失                                                 | 确认posId参数正确填写                                        |
| 107010 | 广告尺寸信息缺失                                             | 联系技术对接                                                 |
| 107020 | 广告位尺寸宽度缺失                                           | 联系技术对接                                                 |
| 107030 | 广告位尺寸高度缺失                                           | 联系技术对接                                                 |
| 107040 | 广告位信息缺失                                               | 联系技术对接                                                 |
| 200000 | 无广告返回                                                   | 检查posId是否正确填写                                        |
| 201000 | 广告无数据                                                   | 联系技术对接                                                 |

## 广告使用限制

| 广告类型     | 新手保护限制        | 拉取间隔限制             | 显示间隔限制 | 用户单日展示次数限制       | 曝光和点击限制                                               |
| ------------ | ------------------- | ------------------------ | ------------ | -------------------------- | ------------------------------------------------------------ |
| banner广告   | 运营配置，目前为1天 | 10s                      | 10s          | 目前为100次                | 每个广告只能产生1次有效点击收益                              |
| 插屏广告     | 目前为1天           | 10s                      | 15s          | 目前为100次                | 每个广告只能产生1次有效点击收益                              |
| 激励视频广告 | -                   | 1min（之后会取消或缩短） | -            | 每个视频广告只能被播放一次 | -                                                            |
| 原生广告     | -                   | 10s                      | -            | -                          | 每个广告只能产生1次有效点击收益，且必须先上报曝光才能上报点击 |

## 广告

  * 次数超过上限，可以将时间调整到第二天
  * 次数超过上限，也可以[清理快应用缓存](https://minigame.vivo.com.cn/documents/#/lesson/question/question-environment)
