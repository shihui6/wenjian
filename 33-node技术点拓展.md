###nodejs技术点扩展
    **node-schedule
        1-概念：执行定时任务工作，定时导出数据，定时发送消息或邮件给用户
        2-用法：
            2-1：安装：npm install node-schedule --save
            2-2:实例：

                ```js
                    const schedule = require('node-schedule');  
                    var rule = new schedule.RecurrenceRule()   //schedule.scheduleJob引入参数规则rule
                    const  scheduleCronstyle = ()=>{
                    //2-2-1每分钟的第30秒定时执行一次:
                    // day of week (0-7)(0 or 7 is Sun)
                    // month (1-12)
                    // day of month (1-31)
                    // hour (0-23)
                    // minute (0-59)
                    // second (0-59,OPTIONAL)
                    rule.second = 30
                        schedule.scheduleJob(rule,()=>{
                            console.log('scheduleCronstyle:' + new Date());
                        }); 
                    }
                    scheduleCronstyle();

                    //2-2-2:一个星期中的某些天的某个时刻执行20点执行
                    const  scheduleCronstyle = ()=>{
                    rule.dayOfWeek = [0,new schedule.Range(1,6)]
                    rule.hour = 20
                    rule.minute = 0
                        schedule.scheduleJob(rule,()=>{
                            console.log('scheduleCronstyle:' + new Date());
                        }); 
                    }
                    scheduleCronstyle();

                    //2-2-3:每5秒钟执行
                    const  scheduleCronstyle = ()=>{
                    var times = [1,6,11,16,21,26,31,36,41,46,51,56]
                    rule.second = times
                        schedule.scheduleJob(rule,()=>{
                            console.log('scheduleCronstyle:' + new Date());
                        }); 
                    }
                    scheduleCronstyle();


                    //2-2-4：取消定时器
                    const  scheduleCronstyle = ()=>{
                    var times = [1,6,11,16,21,26,31,36,41,46,51,56]
                    rule.second = times
                        const j = schedule.scheduleJob(rule,()=>{
                            console.log('scheduleCronstyle:' + new Date());
                        }); 

                        j.cancel()   //定时器取消操作
                    }
                    scheduleCronstyle();
                ```
