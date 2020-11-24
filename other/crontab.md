# 类Unix定时任务命令

## crontab命令
工具型软件cron是一款类Unix的操作系统下的基于时间的任务管理系统。用户们可以通过cron在固定时间、日期、间隔下，运行定期任务。

包括  Linux， macOS， FreeBSD

#### 命令
```
crontab [ -u user ] file
```
或
```
crontab [ -u user ] { -l | -r | -e }
```


#### 配置内容
```
f1   f2   f3   f4   f5   program
-    -    -    -    -
|    |    |    |    |
|    |    |    |    +----- 星期中星期几 (0 - 7) (星期天 为0)
|    |    |    +---------- 月份 (1 - 12)
|    |    +--------------- 一个月中的第几天 (1 - 31)
|    +-------------------- 小时 (0 - 23)
+------------------------- 分钟 (0 - 59)
```

## 以下是没填打开美餐网页配置实例
```
// terminal 新增(编辑)定时任务
# crontab -e

// vi输入配置并保存退出 输出crontab: installing new crontab表示成功
// 任务说明：每周1-5 16:00 自动打开美餐官网
0 16 * * 1,2,3,4,5 open https://meican.com

// terminal 查看定时任务
# crontab -l

// terminal 删除定时任务
# crontab -r

// 输入crontab -e没有打开vi编辑的话配置下 ~/.profile 文件,加入一行
EDITOR=vi; export EDITOR
```
