解压 cocos_demo , cocos creator 2.1版本打开。

因为 安卓打包过后，包体达到了800m+ 所以剔除android工程进行压缩。

首先将 android 打包工程压缩包解压后 放入原有路径。

cocos_demo -》 build -> jsb-link -> frameworks -> runtime-src -> 与 proj.ios_mac 同级目录。

 cocos 工程中 包含 登录，挖矿行为，跳转交易所。
 
 登录与挖矿直接查看cocos工程中的  
 
 GameManager.js             ->   初始化与挖矿方法。
 GameLoginMenu.js         -》 登录方法。
 HttpRequest.js                -》  http请求。
 NetConfig.js                    -》  路由地址，服务器地址。
 
 
 跳转交易所 
 
 1.  cocos工程中 查看  WebDeal.js 文件
 
 2.  ios工程  (导入自己工程中)
        proj.ios_mac -》 ios -》webView 文件夹。 
        
            首先将 webview 中所有的文件导入自己工程中。
        
        在 proj.ios_mac -》 ios -》AppController.mm  引入 WebDeal.h
        
            #import "WebDeal.h" 
            
        在 AppController.mm 的方法  application :(UIApplication *)application 中初始化 WebDeal
        
            在 app->start(); 这句话之前添加 [WebDeal setMainView:_viewController];
        
        必要引入的包体 ,如果工程内已经包含不需要重复引入：
        
                WebKit.framework
                UIKit.framework
                Foundation.framework
                AVFoundation.framework
                
        打包构建即可。
        
        
 3.  android工程 （导入自己工程中）
        proj.android-studio -> app -> src -> 中 有两个文件夹 webview utils。
        将之放入自己工程中相同目录。
        
        然后修改 proj.android-studio -> app -> src -> org -> cocos2 dx -> javascript ->AppActivity.java 文件。

        引入文件。
        
            import android.support.v4.app.ActivityCompat;
            import android.util.Log;
            import utils.APKVersionCodeUtils;
            import webview.WebDeal;
            
        android.support.v4 需要手动引入android包
        
            在 android studio 中打开菜单栏 
            file > Projects Structure > 你自己的app名字 > Depandencies + 第一项 搜索v4 选择com.android.support:support-v4:xxxxx
            引入后等待工程自动gradle构建。
            
        添加webview初始化方法。
        
            在 AppActivity.java 的 onCreate 方法中 调用写好的 load() 方法。
            
            public void load() {
            // DO OTHER INITIALIZATION BELOW
            APKVersionCodeUtils.context = getContext();
            WebDeal.setActivity(this);
            SDKWrapper.getInstance().init(this);
            }
            
        打包运行即可。
