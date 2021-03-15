# mqttAgent-android
基于mosquitto库用c++ 封装的mqtt的aar库,适用android(arm64-v8a)


# 前提
MQTT（Message Queuing Telemetry Transport，消息队列遥测传输协议），是一种基于发布/订阅（publish/subscribe）模式的"轻量级"通讯协议，该协议构建于TCP/IP协议上，由IBM在1999年发布。MQTT最大优点在于，可以以极少的代码和有限的带宽，为连接远程设备提供实时可靠的消息服务。作为一种低开销、低带宽占用的即时通讯协议，使其在物联网、小型设备、移动应用等方面有较广泛的应用。
一个基于客户端-服务器的消息发布/订阅传输协议。MQTT协议是轻量、简单、开放和易于实现的，这些特点使它适用范围非常广泛。在很多情况下，包括受限的环境中，如：机器与机器（M2M）通信和物联网（IoT）。其在，通过卫星链路通信传感器、偶尔拨号的医疗设备、智能家居、及一些小型化设备中已广泛使用.

# 使用（限于android arm64-v8a架构,只编译此架构的so库）
- 在项目的build.gradle 下面添加仓库地址
```
allprojects {
    repositories {
        google()
        jcenter()
        maven { url "https://github.com/oceanhub168pub/maven_repo/raw/master" }  // 这一句
    }
}
```
- 在模块中引用
> api 'com.hub168.yh:mqttAgent-android:1.0.0@aar'

# 方法 

- 方法说明：

     class MqttWarp{

         private Messageable  messageableCallBack
         
         public void setMessageCallback(Messageable msgCallback)
         {
            this.messageableCallBack=msgCallback;
         }
         
        /**
         * 
         * @param conext  上下文
         * @param deviceId  客户端id
         * @param rootCA  客户端根证书 连接Aws 和  Azure 会需要
         * @param deviceCertKey  设备认证信息  连接Aws 和  Azure 会需要
         * @param devicePrivateKey  设备的私钥 连接Aws 和  Azure 会需要
         */
        private native void mqttlibInit(Context conext, String deviceId, String rootCA, String deviceCertKey, String devicePrivateKey);

        /**
         * 
         * @param handler  funtion mqttlibInit 返回的hander
         * @param deviceId  客户端id  
         */
        private native void mqttlibInit(long handler, String deviceId);

        /**
         * 
         * @param handler  funtion mqttlibInit 返回的hander
         * @param host  mqtt服务器地址
         * @param topic  监听的topic
         */
        private native void connect(long handler,String host,String... topic);

        /**
         * 
         * @param handler funtion mqttlibInit 返回的hander
         * @param topic   发送的topic
         * @param data  数据
         */
        private native void sendData(long handler, String topic, String data);

        /**
         * 
         * @param handler   funtion mqttlibInit 返回的hander
         */
        private native void stop(long handler);

    }
    
    
    
        public  interface Messageable {

            
            /**
             *
             *  接受云端的消息
             * @param topic  返回的topic
             *  @param topic  接受的数据
             *  
             */
            public void onReceiverData(String topic, String msg);


        }
        
       
  - 具体实现：
        
        1.自定义类实现Messageable接口,接受收到的数据
        2.创建实例对象 MqttWarp mqttAgent=new MqttWarp();
          mqttAgent.setMessageCallback(自定义的类实现Messageable接口);



