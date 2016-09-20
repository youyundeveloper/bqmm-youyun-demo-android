# **表情SDK 集成**

## 说明
此文档为IM聊天中发送表情集成方式，IM通道为游云SDK提供，表情SDK为表情云提供。

[游云IM文档](http://wiki.17youyun.com/index.php?title=Android_IM%E8%AF%B4%E6%98%8E%E6%96%87%E6%A1%A3)

[表情云官方文档](http://open.biaoqingmm.com/doc/sdk/index.html)

### 1、创建应用
在[游云开发者中心](http://17youyun.com/web/app/applicationManage)创建应用时选着开启表情云服务，然后在应用详情中获取表情云AppId和AppSecret。

### 2、下载 SDK
您可以到 [游云官方网站](http://wiki.17youyun.com/index.php?title=Android) 下载游云SDK。下载包中分为如下两部分：

- 游云SDK+表情云Module - 游云IM通讯能力库和相关库和表情云Module
- 游云表情云集成 bqmm-youyun-demo-android

## 集成开发
### 1、IM集成方式请查看[游云IM文档](http://wiki.17youyun.com/index.php?title=Android_IM%E8%AF%B4%E6%98%8E%E6%96%87%E6%A1%A3)

### 2、表情云集成方式请查看[表情云文档](http://open.biaoqingmm.com/doc/sdk/index.html)

### 3、IM表情云通道

```
BQMM.getInstance().setBqmmSendMsgListener(new IBqmmSendMessageListener() {

            /**
             * 单个大表情消息
             */
            @Override
            public void onSendFace(Emoji face) {
                String json = YouyunUtils.makeJsonStr(face);
                sendEmojiText(json);
            }

            /**
             * 图文混排消息
             */
            @Override
            public void onSendMixedMessage(List<Object> emojis, boolean isMixedMessage) {
                if (isMixedMessage) {
                    // 图文混排消息
                    String json = YouyunUtils.makeJsonStr(emojis);
                    sendEmojiText(json);
                } else {
                    // 纯文本消息
                    String msgString = BQMMMessageHelper.getMixedMessageString(emojis);
                    sendText(msgString);
                }
            }
        });
```

在上面发送回调中sendEmojiText方法中调用游云SDK的发送表情方法，可以将表情封装text或padding中，如下：

```
@param msgId 消息ID (必填)
@param touid 接收方 (必填)
@param text 发送的内容
@param convType single or group 单聊/群聊 (必填)
@param padding 附加信息
@param timeout 超时 单位秒
@return boolean 是否进入发送消息队列
boolean sendEmojiText(String msgId, String touid, String text, ConvType convType, byte[] padding, int timeout);
```
### 4、接收表情消息

```
// 阻塞接收文件
WeimiNotice weimiNotice =  (WeimiNotice)NotifyCenter.clientNotifyChannel.take();
NoticeType type = weimiNotice.getNoticeType();
swiwch(type){
    case emotion: // 表情
        TextMessage textMessage = (TextMessage) weimiNotice.getObject();
        break;
}
```

注：具体请查看文档和Demo

[游云IM文档](http://wiki.17youyun.com/index.php?title=Android_IM%E8%AF%B4%E6%98%8E%E6%96%87%E6%A1%A3)

[表情云文档](http://open.biaoqingmm.com/doc/sdk/index.html)






