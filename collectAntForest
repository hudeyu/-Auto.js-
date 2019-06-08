//这个是点亮锁屏的函数
function unlock(){
    if(!device.isScreenOn()){
    	//点亮屏幕
        device.wakeUp();
        sleep(1000);
		//滑动屏幕到输入密码界面
        swipe(500, 1900, 500, 1000, 300);
        sleep(1000);
        
       //输入四次 7 （密码为7777） 数字键7的像素坐标为（180,1530）
        click(180,1530);
        sleep(500);
       
        click(180,1530);
        sleep(500);
        
        click(180,1530);
        sleep(500);
        
        click(180,1530);
        sleep(500);
    }
}

//进入蚂蚁森林主页
function enterMyMainPage(){ 
    launchApp("支付宝");
    toastLog("等待支付宝启动");
    sleep(1000);

    click("蚂蚁森林");
    sleep(3000);
}

function collectMyOwnEnergy(){ 
    toastLog("下面开始收集我自己的能量");

    if(!requestScreenCapture()){ 
        toastLog("请求截图失败");
        exit();
    }

    var colorGreen = "#C3FF60";
    var countTopLimit = 10;//通过限制次数来保证程序陷入的情况下也能够退出
    var img = captureScreen();
    //toastLog("循环"+num);
    var pointEnergyBall=findColor(img,colorGreen,{ region: [0, 0, 800, 800],threshold: 10 });
    while(pointEnergyBall){
        toastLog("(^_^)");
        click(pointEnergyBall.x,pointEnergyBall.y+20);
        sleep(1000);
        countTopLimit--;
        if(countTopLimit <= 0){
            toastLog("已经到了最大次数，程序退出");
            break;
        }
        img = captureScreen();
        pointEnergyBall=findColor(img,colorGreen,{ region: [0, 0, 800, 800],threshold: 10 });
    }
    toastLog("收集我自己的能量结束");
    sleep(1000);
}

function collectFriendsEnergy(){
    sleep(1000);
    descEndsWith("查看更多好友").findOne().click();
    sleep(1000);

    if(!requestScreenCapture()){
        toastLog("请求截图失败");
        exit();
    }

    var colorGreenHand="#1DA06D";
    var inviteFriendGreen = "#2EC06E";

    while(true){
        var img = captureScreen();
        var pointHand=findColor(img,colorGreenHand,{ region: [1000, 400],threshold: 10 });

        if(pointHand && text("好友排行榜").exists()){//找到绿色，包括手还有计时
            toastLog("找到了有水的好友，开始偷他的水");
            lastPointHand = pointHand;
            collectEnergy(pointHand);
        }else{
            var inviteFriendBox = findColor(img,inviteFriendGreen,{ region: [1000, 400],threshold: 10 });
            if(inviteFriendBox){
                toastLog("到了好友列表的最后，退出好友排行榜");
                click(60,120);//点击返回到列表
                sleep(1000);
                break;
            }else{
                swipe(500,1800,500,100,1000);//没有到结尾就翻页
                sleep(1000);
            }
        }
      }
}

//以下是在好友主页面收集好友水的函数
function collectEnergy(pointHand){
    click(pointHand.x,pointHand.y+50);
    sleep(3000);

    //下面是尝试用颜色来定位好友能量球的方法
    var colorGreen = "#C3FF60";
    var countTopLimit = 5;
    var img = captureScreen();
    //toastLog("循环"+num);
    var pointEnergyBall=findColor(img,colorGreen,{ region: [0, 0, 800, 1000],threshold: 10 });
    while(pointEnergyBall){
        toastLog("(^_^)");
        click(pointEnergyBall.x,pointEnergyBall.y+20);
        countTopLimit --;
        if(countTopLimit <= 0){
            toastLog("已经到了最大次数，程序退出");
            break;
        }
        sleep(1000);
        img = captureScreen();
        pointEnergyBall=findColor(img,colorGreen,{ region: [0, 0, 800, 1000],threshold: 10 });
    }
    toastLog("该好友收取完毕，返回好友列表");
    click(60,120);//点击返回到列表
    sleep(1000);

    //从好友主页偷完水回来后要往上滑动，以免该好友的图标是倒计时，程序一旦陷入便无法退出
    swipe(500, 2000, 500, 2000-pointHand.y, 800);
    sleep(1500);
}

//该函数的作用是程序运行时可以随时按音量减小键退出
function registEvent() {
    threads.start(function(){
        //在子线程中调用observeKey()从而使按键事件处理在子线程执行
        events.observeKey();
        events.on("key_down", function(keyCode, events){
            //音量键关闭脚本
            engines.stopAllAndToast();
            exit();
        });
    });
}

//1.解锁屏幕
unlock();
//2.启用按键监听
registEvent()
//3.打开蚂蚁森林
enterMyMainPage();
//4.收集自己的水
collectMyOwnEnergy();
//5.点击“查看更多好友”，进入好友排行榜，收集好友能量
collectFriendsEnergy();
