---
layout: post
title: "Logstash init script"
tags:
- tech
---
{% include JB/setup %}

Getting init to play nice with running a java jar file as a service was a bit of a hassle. The built-in daemonize function wasn't able to correctly create a .pid file so here's something that finally worked.

{:.bash}
    #!/bin/sh
    #
    # chkconfig: 2345 70 40
    # description: logstash startup script
    #
    LOGSTASH=/opt/logstash/logstash-1.1.1-monolithic.jar
    CONF=/opt/logstash/logstash.conf
    TMPDIR=/dev/shm
    export TMPDIR

    . /etc/rc.d/init.d/functions

    RETVAL=0

    case "$1" in
       start)
          echo -n "Starting logstash: "
          [ -f $LOGSTASH ] || exit 1
          daemon "java -jar $LOGSTASH agent -f $CONF &" 2>> /var/log/messages
          pgrep -f /opt/logstash/logstash-*-monolithic.jar > /var/run/logstash.pid
          RETVAL=$?
          echo
          [ $RETVAL -eq 0 ] && touch /var/lock/subsys/logstash
          ;;

      stop)
          echo -n "Shutting down logstash: "
          killproc  logstash 
          RETVAL=$?
          echo
          [ $RETVAL -eq 0 ] && rm -f /var/lock/subsys/logstash
          ;;

      restart|reload)
        $0 stop
        $0 start
        RETVAL=$?
        ;;
      status)
        status logstash 
        RETVAL=$?
        ;;
      *)
        echo "Usage: $0 {start|stop|restart|status}"
        exit 1
    esac

    exit $RETVAL
