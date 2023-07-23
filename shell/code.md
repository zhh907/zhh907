# 压缩/解压缩
```
# 解包：
tar zxvf filename.tar
# 打包
tar czvf filename.tar dirname
```

# 杀死某个进程
```
ID=`ps -ef | grep xxxxxxx | grep -v 'grep' | awk '{print $2}'`
echo $ID
echo '-------start-------'
for id in $ID
do
	kill -9 $id
done
echo '-------end---------'
```

# 配置定时脚本
```
# 编写定时脚本文件
crontab -e
```

# 进程自动重启脚本
```
#检查进程是否正常运行，<否>则重启
count=`ps -ef | grep com.shsnc.main.HaToKafka | grep -v "grep" | wc -l`
if [ 0 -eq $count ];
then
    nohup java -cp /data/flinkShard/flinkAllTask/flinkAllTask.jar com.shsnc.main.HaToKafka > /dev/null 2>&1 &
    echo "######$(date "+%Y-%m-%d %H:%M:%S") restart ######" >> /data/flinkShard/flinkAllTask/logs/HaToKafka.log
fi
```