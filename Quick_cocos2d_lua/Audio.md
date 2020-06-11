## 预加载和清理

```lua
local callback = function(fn, success)
    if not success then
        print("Fail to load audio: " .. fn)
        return
    end
end
audio.loadFile("audio/bgm.ogg", callback)

audio.unloadFile("audio/bgm.ogg")
audio.unloadAllFile()
```

## 背景音乐

* `audio.playBGM(path, isLoop)` 播放背景音乐，需要预加载
* `audio.playBGMSync(path, isLoop)` 播放背景音乐，自动预加载
* `audio.stopBGM()`
* `audio.setBGMVolume(vol)` 设置背景音量(0 ~ 1)

## 音效

* `local effect = audio.playEffect(path, isLoop)` 播放音效，需要预加载，返回实例

* `audio.playEffectSync(path, isLoop)` 播放音效，自动预加载，无返回值
* `audio.stopEffect()` 停止所有音效
* `audio.setEffectVolume(vol)`
* `effect:pause()`
* `effect:resume()`
* `effect:stop()`
* `effect:setVolume(vol)`
* `local stat = effect:getStat()` 1 初始化 2 播放中 3 暂停中 4 已停止

## 背景音乐和音效 统一控制接口
* `audio.stopAll()`
* `audio.pauseAll()`
* `audio.resumeAll()`

