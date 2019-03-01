# Alarm_UserNotification1
利用用户通知UserNotification推送功能，来模拟做了一个闹钟。不管app在前台、在后台、完全退出，都能够在设定的时间进行通知。
----

![image](https://github.com/Kimsswift/Alarm_UserNotification1/blob/master/Alarm_UNNotification/n00.gif)

设定好时间时，发送到通知中心
--------
    //视图即将显现时
    override func viewWillAppear(_ animated: Bool) {
        super.viewWillAppear(animated)
        
        if manager.alarmTime != "" {    //如果设定了闹钟
            alarmTimeLabel.text = "设定了\(manager.alarmTime)闹钟"
            
            //设置的小时
            let hRange = manager.alarmTime.startIndex ... manager.alarmTime.index(manager.alarmTime.startIndex, offsetBy: 1)
            let hour = manager.alarmTime[hRange]
            
            //设置的分钟
            let mRange = manager.alarmTime.index(manager.alarmTime.endIndex, offsetBy: -2) ... manager.alarmTime.index(manager.alarmTime.endIndex, offsetBy: -1)
             let minute = manager.alarmTime[mRange]
            
            //调用通知方法
            notification(Int(String(hour)), Int(String(minute)))
            
        }else {  //如果没有设定了闹钟
            alarmTimeLabel.text = "无设定的闹钟"
            
            //调用通知方法
            notification(nil, nil)
        }
    }
    
    //用户通知方法
    func notification(_ hour: Int?, _ minute: Int?) {
        //配置通知
        let content = UNMutableNotificationContent()
        content.title = "Alarm_UNNotification"
        content.body = "闹钟响了"
        content.badge = nil
        content.sound = UNNotificationSound.default

        //设置时间
        var component = DateComponents()
        component.hour = hour ?? nil
        component.minute = minute ?? nil
        
        //设置通知的触发器
        let trigger = UNCalendarNotificationTrigger(dateMatching: component, repeats: false)
        
        //设置通知请求
        let request = UNNotificationRequest(identifier: "alarm", content: content, trigger: trigger)
        
        //把请求发送到通知中心
        UNUserNotificationCenter.current().add(request) { (error) in
            if error != nil {
                print(error!)
            }
        }
    }
