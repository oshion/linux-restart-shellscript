# linux-restart-shellscript

#!/bin/sh


export JAVA_HOME=/usr/bin/java
export PATH="$PATH:$JAVA_HOME/bin"
export CATALINA_HOME=톰캣주소

Log=$CATALINA_HOME/logs/restart.log
DATE=`date +%Y년%m월%d일-%H:%M:%S`

# 톰캣 PID 찾기
tomcatPID=`ps -ef | grep tomcat | grep /usr/bin/java | grep -v grep | grep -v vi | awk '{print $2}'`
#tomcatPID=`ps -ef | grep tomcat | grep /usr/bin/java | grep -v grep | grep -v vi`

# 톰캣 프로세스 카운트
tomcatCnt=`ps -ef | grep tomcat | grep /usr/bin/java | grep -v grep | grep -v vi | wc -l`

if [ $tomcatCnt -gt 0 ]
then
    echo "$DATE : TOMCAT이 정상 작동중입니다.(PID : $tomcatPID)" >> $Log
else
    echo "$DATE : TOMCAT을 시작합니다." >> $Log

    # 톰캣 재시작
    sudo $CATALINA_HOME/bin/startup.sh

    tomcatPID=`ps -ef | grep tomcat | grep /usr/bin/java | grep -v grep | grep -v vi | awk '{print $2}'`
    DATE=`date +%Y년%m월%d일-%H:%M:%S`

    echo "$DATE : TOMCAT이 시작되었습니다.(PID : $tomcatPID)" >> $Log
fi
