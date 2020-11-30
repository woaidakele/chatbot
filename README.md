# ChatBot 聊天机器人

一个功能简单、稳定的聊天机器人.  
你可以通过该机器人接入一些有趣的 API 来实现一些好玩的应用,例如快递查询、毒鸡汤、斗图等,也可以把自己人接入自己的服务中,作为告警、定时通知等服务.

### ⚠️ 关于群内机器人注意事项 ⚠️

每个人只能将机器人拉入一个群,多的机器人会自动退群

## 接入流程

![流程图](./images/flow.png)

> 图上蓝色部分都是你需要做的内容

## 具体步骤

1. 添加机器人为好友,一般在 1-2 分钟后会自动通过
2. 根据提示操作,可以发送`#帮助#`来获取具体指令,激活机器人
3. 编写代码,连接机器人的 WebSocket Server 用于接收推送消息
4. 编写 Http Client 的代码,用于请求机器人接口,让机器人发送消息给你

## 核心流程的数据结构

```javascript
//所有消息的包装
{
    //消息类型,1000为转发的聊天消息,1001为群内事件消息
    "msgType": 10000,
    //data在msgType不同的时候会有不同的数据结构,目前会有转发消息和群事件消息两种
    "data": {}
}

//机器人转发的消息,包括文本、图片、视频等格式
{
    //下载语音的时候会用到
    "newMsgId":0,
    //发送人,这个也会是你发送消息的对象,即接口toUser字段的值就是这个
    "fromUser":"test",
    //消息中@了谁
    "atList":[],
    "createTime":0,
    //群内消息会变成"xxx发送了一个图片"这样的简短说明
    "pushContent":"",
    //机器人id
    "clientUserName":"test",
    //消息接收人id
    "toUser":"test",
    //暂时无用
    "imgBuf":"",
    //消息类型,具体看demo中的注释
    "msgType":1,
    //具体的消息内容,如果是其他格式的会是xml
    //如果是群内消息会变成 发言人id:消息内容 这样的格式
    "content":"test",
    //暂时无用
    "msgSource":"<msgsource />",
    // ======================================
    // 下面这个三个字段只有群内消息有用
    // 方便开发者调用,省去来自己解析content这一步
    // ======================================
    //谁@了机器人
    "whoAtBot":"",
    //发言人id
    "groupMember":"",
    //发言内容
    "groupContent":""
}

//群内事件
{
    //具体的事件id,这里100003只是举个例子,是成员离开事件
    "event":100003,
    //事件的中文描述
    "EventText":"群成员离开",
    //发生事件的群基本信息
    "group":{
        //群id
        "groupUserName":"",
        //群名称
        "groupNickName":"",
        //群头像
        "groupHeadImg":""
    },
    //发生事件的群成员信息
    "members":[
        {
            //群成员id
            "userName":"",
            //群成员昵称
            "nickName":"",
            //群成员头像
            "headImg":""
        }
    ]
}
```

## HTTP 接口

主要用于机器人主动发送消息给用户或者群

[点击这里查看接口文档](./HTTP.md)

## DEMO

-   [ChatBot-Go](https://github.com/chatrbot/chatbot-go)
-   [ChatBot-Node](https://github.com/chatrbot/chatbot-node)
-   [ChatBot-PHP](https://github.com/chatrbot/chatbot-php)

## 功能

### 个人

-   [x] 机器人接收文本、图片、表情、视频、音频消息
-   [ ] 接收和发送文件

### 群聊

-   [x] 机器人接收文本、图片、表情、视频、音频消息
-   [x] 机器人被拉入群内事件
-   [x] 机器人被踢出群事件
-   [x] 群内成员新增事件
-   [x] 群内成员退出/被踢事件
-   [ ] 接收和发送文件
-   [ ] 机器人踢人

### 机器人命令

-   [ ] 查看机器人加入的群
-   [ ] 命令机器人退群

## 示例截图

![demo](./images/demo.png)

## FAQ

Q:如何获取 Token?  
A:加入技术交流群,添加群内机器人为好友,然后发送相关指令激活获取  
Q:如何激活群内机器人?  
A:把机器人拉入群内就算激活,每个人目前只能绑定一个群.而且同一个群内只能存在一个机器人,多了无效.  
第一个拉机器人进群的用户会收到转发消息

## 联系方式

![qrcode_group](./images/qrcode_group.png)
