### Git yoho flow
目前的分支主要有:master,develop,hotfix,test
其中,
##### Master：这个分支最近发布到生产环境的代码,这个分支只能从其他分支合并,不能在这个分支直接修改。
##### Develop:这个分支是我们是我们的主开发分支,包含所有要发布到下一次Release的代码,这个主要与其他分支合并,比如Feature(个人)分支合并
##### Test:当你需要一个发布一个次Release的时候,我们基于Develop分支创建一个test分支,完成test后,我们合并到Master和Develop分支
##### Hotfix:当我们在Master发现新的Bug时候，我们需要创建一个Hotfix,完成Hotfix,我们合并回Master和Develop分支,所以Hotfix的改动会进入下一个test

## 1 版本发布/灰度
### (1)master为基准
### (2) 发布的时候自动打tag:
     2.1灰度自动打的tag为:V_gray_201607061130 
     2.2线上自动打的tag为:V_release_201607061130  
## 2 线上版本有重大bug,需要通过打补丁的方式来解决
### (1) 补丁branch为hotfix
### (2) hotfix需要从V_release_2016mmddhhmm(时间戳为最新的时间戳)拉取
### (3) hotfix上完成bug修复,在自测通过之后,需要有：
    3.1  hotfix  ->  （merge） dev
    3.2  hotfix  ->  （merge） test
    3.3  hotfix  ->  （merge） master
### (4) hotfix在merge完代码后,需要发布到灰度进行验证
    4.1发布修改过的制定模块的版本:V_hotfix_2016mmddhhmm(mmddhhmm为月日小时分钟)
    4.2发布未修改的其他模块的版本:V_release_2016mmddhhmm()
### (5) hotfix在灰度环境验证通过后,线上发布V_hotfix_2016mmddhhmm,发布工具自动打tag为:V_release_2016mmddhhmm