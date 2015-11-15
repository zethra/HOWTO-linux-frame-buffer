#How to set up a frame buffer daemon in linux
##Why would you?
A frame buffer will allow you to run graphical applications or applications that use OpenGL without an X server running.  An example of this would be OpenSCAD.  This would allow you to run the command line features of OpenSCAD without an X server running.
##Step 1
Install the package xvfb from you package manager.<br/>
`sudo apt-get install xvfb`<br/>
`sudo yum install xvfb`
##Step 2
Put this daemons script into `/etc/init.d/xvfb`.
<pre>
#!/bin/sh
### BEGIN INIT INFO
# Provides: Xvfb
# Required-Start: $local_fs $remote_fs
# Required-Stop:
# X-Start-Before:
# Default-Start: 2 3 4 5
# Default-Stop: 0 1 6
# Short-Description: Loads X Virtual Frame Buffer
### END INIT INFO

XVFB=/usr/bin/Xvfb
XVFBARGS=":1 -screen 0 1024x768x24 -ac +extension GLX +render -noreset"
PIDFILE=/var/run/xvfb.pid
case "$1" in
  start)
    echo -n "Starting virtual X frame buffer: Xvfb"
    start-stop-daemon --start --quiet --pidfile $PIDFILE --make-pidfile --background --exec $XVFB -- $XVFBARGS
    echo "."
    ;;
  stop)
    echo -n "Stopping virtual X frame buffer: Xvfb"
    start-stop-daemon --stop --quiet --pidfile $PIDFILE
    echo "."
    ;;
  restart)
    $0 stop
    $0 start
    ;;
  *)
        echo "Usage: /etc/init.d/xvfb {start|stop|restart}"
        exit 1
esac

exit 0
</pre>
Then run `sudo update-rc.d xvfb defaults`
##Step 3
Add the line
`export DISPLAY=:1`
to `/etc/rc.local`
##All done
Now you can run<br/>
`sudo service xvfb start` and<br/>
`sudo service xvfb stop`<br/>
To start and stop the frame buffer.
