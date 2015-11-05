---
layout: post
title:  "How to rotate Rails logs"
date:   2015-11-05 17:10:38
categories: rails logs rotate logrotate
comments: true
---

I am assuming you use Ubuntu to run your Rails app.

Firstly, install logrotate utility:

`sudo apt-get update && sudo apt-get install logrotate`

Then, create new logrotate config file for your app:

`sudo nano /etc/logrotate.d/sample-app`

It should look lo this:

{% highlight text %}
/home/yukas/sample-app/log/*.log {   # path to your log folder
        daily           # rotates your log on daily basis
        rotate 2        # 2 days of logs will be kept
        size 10M        # maximum size of log file is 10 Mb
        missingok       # ignore errors in the process and keep working
        compress        # compress log files
        delaycompress   # postpone compress till next rotation
        notifempty      # do not rotate empty log
}
{% endhighlight %}

Try to rotate your logs by running: `sudo logrotate /etc/logrotate.d/sample-app`.

Run `cat /var/lib/logrotate/status` to see which logs got rotated and when.

Now you need to specify cron job to rotate your logs every day automatically.

Run `sudo nano /etc/cron.daily/logrotate` to create cron config for logrotate.

Cron config should look like so:

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

It seem now you all set.

Good day!