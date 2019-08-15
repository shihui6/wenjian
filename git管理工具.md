***git仓库
    初始化版本库：git init
    添加文件到版本库 git add .
                    git commit -m
    查看仓库状态  git status
     
***git查看当前全部项目
    1-点击头像->Your profile
***git项目的删除
    1-进入要删除的羡慕->settings->划到最下面danger Zone->删除时需要写入仓库名删除即可

***用git push origin master 这里的orgin是在clone下来的时候管理下来的(才会有的)，没有clone就没有orgin

***查看orgin
    git remote 可以查看远程仓库地址的变量(origin)

***添加git push orgin这里的orgin的别名，以后就可以直接使用git push orgin master
    git remote add orgin + 远程仓库的地址

***以后可以直接使用git push相当于git push orgin master
    git push -u orgin master 这句话将orgin master和git push进行绑定

***提交github上有一个注意点：
    git init初始化的使用，在本地初始化之后，在github创建仓库的时候就不要初始化了，不然会产生冲突


***在地址栏中输入git相关地址可以直接访问自己写的项目
    将仓库名字命名成 账号.github.io  就可以实现

***版本回退操作
    git reset --hard + 版本号    可以回退到之前的版本，要是想回到当前的版本，执行次操作加上相应的版本号就可以了


***github关于分支的管理
    分支的特点：各个分支之间互相没有影响
        解释：各个分支都可以完成一定的任务，想完成登录功能就开一个登录分支做登录功能，想要写注册就专门开一个分支写注册，将来登录功能完全写完了，注册功能完全写完了，然后一起合并到主干上，最后所有的功能都写好了。

    ****分支的基本操作
        分析git运行机制：
            1-初始化提交的时候(就是执行到git commit -m'初始提交')，会生成一个版本C1，并且会 有一个默认的master分支指向这个版本C1
            2-如果切换到login分支，然后在进行提交也会生成一个版本C2，并且这个login分支也会  从前面的版本移动到当前版本C2上(执行git checkout + 分支名 相应的head也会跟着这个   分支走，head代表在当前在哪个分支上head就在哪个分支上面)
            3-我在login分支上写的代码，切换到master分支是看不到login上写的代码的
        合并分支操作：
            login上面所有的内容都写好了，想要在最终版本上展示，就需要合并分支
            具体操作：
                1-切换到master分支
                2-git merge + 分支名 (如git merge login 就把login分支上的内容合并到主分支上了 所谓的合并分支，就是将指针指向了最新的版本，就是将master(head)指针指向login分支的最新版本)
                3-git branch -d + 分支名 login分支合并之后，代码已经全部合并过来了，但是除了master分支 之前的login分支还是存在的，所以要删除login分支
        分支合并冲突的解决：
            咋们创建了2个分支(一个login分支一个register分支),现在要将login分支和register分支上的代码合并到一起
            具体操作：
```js   
        // 合并分支出现的俩种情况
        // 第一种情况：如果当前分支是合并分支的祖辈分支(login和register的祖辈分支就是master)，会直接进行快速合并
        切换到master上合并login分支代码 git merge login 合并login分支的代码(实质上是master指针指向login对应的最新版本，此时master分支和未合并的register分支就不是祖辈分支关系了，所以是接下来的第二种情况)
        // 第二种情况：如果不是祖辈分支，可能会产生冲突(因为俩个分支修改了同一个文件)
        冲突如下：
        冲突的部分会用如下记号标记(因为俩个分支对统一个页面修改了，标记里面就是俩者修改同一个页面的不同部分)：
        <<<<<<<<HEAD
        冲突部分内容
        =========
        冲突部分内容
        >>>>>>>>register

        如果此时 gitbach上面操作文件后面跟有 master|MERGING 表明分支正在合并中，只要进行接下来的操作即可
        方法：只要将标记全部删掉 然后进行提交 git commit -a -m '处理冲突' 


```
        具体操作点：
            1.git branch 查看分支 (执行命令之后，如果分支名是绿色的，而且前面有小菊花说明是当前分支，并且head一开始默认的是指向maser分支的)
            2.git branch + 分支名  创建分支 (如：git branch login 咋们创建了login分支，这个login和默认的maser都是指向当前版本的)
            3.git checkout + 分支名  切换分支 (如：git checkout login 就切换到了login分支，所以当前head就从master分支指向login分支了)
            4.git checkout -b + 分支名 创建并且切换分支
            5.git commit -a -m '初始提交'  git add . 和 git commit -m '初始提交'的俩个命令的合并


    