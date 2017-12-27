# README
| Author        |     E-mail      |
| ------------- |:---------------:|
| gufei         | 799170694@qq.com|


# RNILiveExample
[react-native-ilive](https://github.com/midas-gufei/react-native-ilive)

### 实例代码说明：

*   将ILiveView、index、RtcEngine三个文件拷贝到你的项目相应目录下

*   在你项目的直播功能js文件中：

  *  import {RtcEngine, ILiveView} from './src/index';

  *  componentWillMount中初始化直播引擎：
   ```
  // 初始化iLive
  const options = {
      appid: '1400027849',
      accountType: '11656'
  };
  RtcEngine.init(options);
  // 添加AVListener，此方法必须在rn的componentWillMount()方法中执行，render()之前执行
  RtcEngine.iLiveSetAVListener();
 ```
*  componentDidMount中先登录腾讯的TLS系统,use id&&sig（自己服务器端生成）
 ```
RtcEngine.iLiveLogin('learnta111', 'eJxlj8tOg0AARfd8BWGLMTPA8HBHoBGsQNqipitC59GOtTBOB4Ua-70Vm0ji3Z6Te3O-NF3XjfJxdVtj3HaNqtQgqKHf6QYwbv6gEJxUtapsSf5B2gsuaVUzReUIIULIAmDqcEIbxRm-Gm*0lo2qIYQT50j21Tj0W*JcGizPd4KpwrcjzGbrKF1Ekjmv73M-jEN0wsn88HQ-K1lcgLIzdxvVnfIMic-ewy3bpruwiJNscPmQPjsvy1zYyWogOe43idq766I000XsRmYg8UMxmVT8QK*vALI9Pwi8Cf2g8sjbZhQsABG0bPATQ-vWzhSzXpU_');
 ```
*  所有的原生通知统一管理
 ```
RtcEngine.eventEmitter({
    onLoginTLS: (data) => {
        var result = data.code === '1000';
        this.setState({isLoginSuccess: result});
        // TLS登录成功
        if (result) {
            // 自己创建直播间 hostId=自己的id,roomNum=自己的房间号,清晰度
            // 加入别人的房间 hostId=主播的id,roomNum=主播的房间号,userRole=0,清晰度
            RtcEngine.iLiveCreateRoom('learnta111', 662556, "HD");
            // RtcEngine.iLiveJoinRoom('learnta111', 662556, 0, "Guest");
        }
    },
    onLogoutTLS: (data) => {
        console.log(data);
    },
    // 创建房间
    onCreateRoom: (data) => {
        console.log(data);
    },
    // 加入房间
    onJoinRoom: (data) => {
        console.log(data);
    },
    // 离开房间
    onLeaveRoom: (data) => {
        console.log(data);
    },
    // 退出房间
    onExitRoom: (data) => {
        console.log(data);
    },
    // 切换角色
    onChangeRole: (data) => {
        console.log(data);
    },
    // 上麦
    onUpVideo: (data) => {
        console.log(data);
    },
    // 下麦
    onDownVideo: (data) => {
        console.log(data);
    },
    // 开始录制视频
    onStartVideoRecord: (data) => {
        console.log(data);
    },
    // 停止录制视频
    onStopVideoRecord: (data) => {
         console.log(data);
    },
    // 开始录屏幕
    onStartScreenRecord: (data) => {
       console.log(data);
    },
    // 停止录屏幕
    onStopScreenRecord: (data) => {
        console.log(data);
    },
    // 与房间断开连接
    onRoomDisconnect: (data) => {
        console.log(data);
    },
    // 切换摄像头
    onSwitchCamera: (data) => {
        console.log(data);
    },
    // 开关摄像头
    onToggleCamera: (data) => {
        console.log(data);
        var result = data.code === '1000';
        this.setState({
            bCameraOn: result
        });
    },
    // 开关声麦
    onToggleMic: (data) => {
        console.log(data);
        var result = data.code === '1000';
        this.setState({
            bMicOn: result
        });
    },
    onError: (data) => {
        console.log(data);
        // 错误!
    }
})
  ```
*  页面销毁退出直播间，移除回调
 ```
componentWillUnmount() {
        RtcEngine.iLiveLeaveRoom();
        RtcEngine.removeEmitter()
    }
 ```
*  退出房间、上麦、下麦、录制视频流、录制屏幕、切换摄像头、开关摄像头、开关声麦方法如下
 ```
handerLeavelRoom() {
    console.log('handerLeavelRoom');
    // 通知腾讯TLS服务器
    RtcEngine.iLiveLeaveChannel();
    // 移除监听事件
    RtcEngine.removeEmitter();
};

handlerUpVideo (uid) {
    console.log('handlerUpVideo');
    RtcEngine.iLiveUpVideo(uid);
};

handlerDownVideo(uid) {
    console.log('handlerDownVideo');
    RtcEngine.iLiveDownVideo(uid);
};

handlerStartRecord (fileName, recordType) {
    console.log('handlerStartRecord');
    RtcEngine.iLiveStartRecord(fileName, recordType);
};

handlerStopRecord() {
    console.log('handlerStopRecord');
    RtcEngine.iLiveStopRecord();
};

handlerStartScreenRecord () {
    console.log('handlerStartScreenRecord');
    RtcEngine.iLiveStartScreenRecord();
};

handlerStopScreenRecord() {
    console.log('handlerStopScreenRecord');
    RtcEngine.iLiveStopScreenRecord();
};

handlerSwitchCamera =() => {
    RtcEngine.iLiveSwitchCamera();
};

handlerToggleCamera =(bCameraOn) => {
    RtcEngine.iLiveToggleCamera(bCameraOn);
};

handlerToggleMic =(bMicOn) => {
    RtcEngine.iLiveToggleMic(bMicOn);
};
 ```
*  render()中添加直播component
 ```
  <ILiveView style={styles.localView} showVideoView={true}/>
 ```
