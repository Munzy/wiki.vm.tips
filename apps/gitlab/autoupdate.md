<!-- TITLE: Gitlab Autoupdate -->
<!-- SUBTITLE: A quick summary of Autoupdate -->

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
```
