---
title: Gitlab Autoupdate
description: A quick summary of Autoupdate
published: true
date: 2020-02-06T23:18:24.987Z
tags: 
---

# Autoupdate

Gitlab is **constantly** putting out new releases. Keeping up to date with all these updates can be a challenge.

If you are running gitlab on linux, however, there is a possible solution.

## unattended-upgrades

### Install

Firstly we need to install unattended-upgrades.
```
apt-get update

apt-get install apt-listchanges -y 

echo unattended-upgrades unattended-upgrades/enable_auto_updates boolean true | debconf-set-selections
apt-get install unattended-upgrades -y 
```

### Add Gitlab Repo

```
nano /etc/apt/apt.conf.d/50unattended-upgrades
```

Now add:
```
"origin=packages.gitlab.com/gitlab/gitlab-ce,archive=${distro_codename}";
"origin=packages.gitlab.com/gitlab/gitlab-ee,archive=${distro_codename}";
```

Inbetween:

```
[...]
Unattended-Upgrade::Origins-Pattern {
        // Codename based matching:
        // This will follow the migration of a release through different
        // archives (e.g. from testing to stable and later oldstable).
        // Software will be the latest available for the named release,
        // but the Debian release itself will not be automatically upgraded.
        //      "origin=Debian,codename=${distro_codename}-updates";
        //      "origin=Debian,codename=${distro_codename}-proposed-updates";
        "origin=Debian,codename=${distro_codename},label=Debian";
        "origin=Debian,codename=${distro_codename},label=Debian-Security";
        
				// !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!
				// Add it in here
				// !!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!!

        // Archive or Suite based matching:
        // Note that this will silently match a different release after
        // migration to the specified archive (e.g. testing becomes the
        // new stable).
        //      "o=Debian,a=stable";
        //      "o=Debian,a=stable-updates";
        //      "o=Debian,a=proposed-updates";
        //      "o=Debian Backports,a=${distro_codename}-backports,l=Debian Backports";
};
[...]
```

## Crontab and Daily Run Time

This should run on the daily update cron schedule. You can see the cron at:
```
nano /etc/crontab
```

and will look something like:

```
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# Example of job definition:
# .---------------- minute (0 - 59)
# |  .------------- hour (0 - 23)
# |  |  .---------- day of month (1 - 31)
# |  |  |  .------- month (1 - 12) OR jan,feb,mar,apr ...
# |  |  |  |  .---- day of week (0 - 6) (Sunday=0 or 7) OR sun,mon,tue,wed,thu,fri,sat
# |  |  |  |  |
# *  *  *  *  * user-name command to be executed
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 0    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 0    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 0    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#

```

Which indicates the daily cron job will run at 12:25 AM with a random delay added.