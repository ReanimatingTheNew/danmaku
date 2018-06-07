## danmaku
各直播平台弹幕协议和开放平台API

## 支持列表
* **douyu.com**

```go
package main

import (
	"fmt"
	"github.com/songtianyi/barrage/douyu"
	"github.com/songtianyi/rrframework/logs"
)

func chatmsg(msg *douyu.Message) {
	level := msg.GetStringField("level")
	nn := msg.GetStringField("nn")
	txt := msg.GetStringField("txt")
	logs.Info(fmt.Sprintf("level(%s) - %s >>> %s", level, nn, txt))
}

func main() {
	client, err := douyu.Connect("openbarrage.douyutv.com:8601", nil)
	if err != nil {
		logs.Error(err)
		return
	}

	client.HandlerRegister.Add("chatmsg", douyu.Handler(chatmsg), "chatmsg")
	if err := client.JoinRoom(288016); err != nil {
		logs.Error(fmt.Sprintf("Join room fail, %s", err.Error()))
		return
	}
	client.Serve()
}
```

* **live.bilibili.com**

```
package main

import (
	"github.com/songtianyi/barrage/bilibili"
	"github.com/songtianyi/rrframework/logs"
)

func danmu(msg *bilibili.Message) {
	logs.Debug(">>> ", string(msg.Bytes()))
}

func main() {
	// uri, userid, handlerRegister
	client, err := bilibili.Connect("https://live.bilibili.com/43783", -1, nil)
	if err != nil {
		logs.Error(err)
		return
	}
	client.HandlerRegister.Add(bilibili.DANMU_MSG, bilibili.Handler(danmu), "danmu")
	client.Serve()
}
```

* **padatv.com**
```
package main

import (
	"github.com/songtianyi/barrage/panda"
	"github.com/songtianyi/rrframework/logs"
)

func danmu(msg *panda.DecodedMessage) {
	logs.Debug("(%s) - %s >>> %s", msg.Type, msg.Nickname, msg.Content)
}

func main() {
	// uri, handlerRegister
	client, err := panda.Connect("https://www.panda.tv/66666", nil)
	if err != nil {
		logs.Error(err)
		return
	}
	client.HandlerRegister.Add(panda.DANMU_MSG, panda.Handler(danmu), "danmu")
	client.Serve()
}
```

## demo
![douyu-barrage-demo](http://ww1.sinaimg.cn/large/006HJ39wgy1fhjnykako6j30ik0g5adm.jpg)

## 微信交流群
<img src="http://owm6k6w0y.bkt.clouddn.com/17-9-21/70665214.jpg" width="480" height="480"/>
