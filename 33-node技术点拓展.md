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


###node基础知识点
    **node
        1：概念：nodejs是JavaScript运行环境，可以解析js代码
        2：作用：
            2-1：让JavaScript成为与php，python等平起平坐的语言
            2-2：前端脱离后端，直接通过js写项目(其次：接口，爬虫，桌面应用，聊天系统等)
        3：与node相关的工具
            3-1：nvm工具：实现nodejs任意版本切换
            3-2：npm工具：下载nodejs所需模块(工具库) 
            3-3：nrm工具：切换npm下载源
        
    **模块系统CommonJS规范
        1：概念：官方内置的Nodejs模块(os/url/fs/http),第三方的Nodejs模块(moment/mysql),自定义的Nodejs模块
        2：模块规范
            2-1：概念：一个文件就是一个模块
            2-2：用法：
                通过exports和module.exports来导出模块中的成员(声明模块中那些功能可以使用)
                通过require来加载模块

            事例：自定义Nodejs模块

                ```js
                    // 导出成员
                    exports.属性名 = 值 (注：exports和module.exports互为别名)
                    // 引入模块
                    var 变量名 = require('./模块文件')
                ```

            事例：第三方Nodejs模块
                概念：用人用js代码封装好的文件
            
            事例：内置Nodejs模块
                    用法：
                        引入官方模块
                        调用模块成员即可

                        ```js
                            var os = require('os') //引入官方模块os 主要用来获取操作系统信息
                            console.log(os.totalmem()) //调用模块成员

                            var path = require('path') //操作文件路径
                            console.log(path.extname('c:/app/view/godos/add.html')) //获取文件后缀

                            var url = require('url') //操作浏览器url
                            var urlObj = url.parse('http://zt.cn?name=张三&age=18',true)
                        ```

            事例：内置Nodejs模块(fs模块)
                作用：写入文件和读取文件

            事例：内置Nodejs模块(http模块)
                作用：创建http服务器
            

        
        