---
layout: post
title:  "How to rotate rails logs"
date:   2015-11-05 17:10:38
categories: rails logs rotate logrotate
comments: true
---

I am assuming you use ubuntu to run your Rails app.

First, install logrotate utility:
`sudo apt-get update && sudo apt-get install logrotate`

Then, configure it to rotate your logs:
`sudo nano /etc/logrotate.d/sample-app` — will create new config file for your app

{% highlight text %}
/home/yukas/sample-app/shared/log/*.log {
        daily           # rotates your log on daily basis
        rotate 2        # 2 days of logs will be kept
        size 10M        # maximum size of log file is 10 Mb
        missingok       # ignore errors in the process and keep working
        compress        # compress log files
        delaycompress   # postpone compress till next rotation
        notifempty      # do not rotate empty log
}
{% endhighlight %}

Now you need to specify cron job for rotation to run every day:
`sudo nano /etc/cron.daily/logrotate`

{% highlight text %}
#!/bin/sh

# Clean non existent log file entries from status file
cd /var/lib/logrotate
test -e status || touch status
head -1 status > status.clean
sed 's/"//g' status | while read logfile date
do
    [ -e "$logfile" ] && echo "\"$logfile\" $date"
done >> status.clean
mv status.clean status

test -x /usr/sbin/logrotate || exit 0
/usr/sbin/logrotate /etc/logrotate.conf
{% endhighlight %}

Test your setup:
`cat /var/lib/logrotate/status`

File contains information about what logs got rotated and when.

Good day!