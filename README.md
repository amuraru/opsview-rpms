RPMS available at: https://raw.githubusercontent.com/amuraru/opsview-rpms/master/centos/7

## How to build

### Check docs
 http://docs.opsview.com/doku.php?id=developer:build:rpm_opsview

### Create an account with Opsview 
http://www.opsview.com/products/opsview-core

### Checkout code
```
cd /usr/local/src
svn co --username amuraru https://secure.opsview.com/svn/opsview/trunk/  opsview
svn co --username amuraru https://secure.opsview.com/svn/opsview-perl/trunk/  opsview-perl
```


### Build
`cd opsview/opsview-base`
Quirks
`nagios-plugins-1.4.17-dev.tar.gz` is not building on Centos7 - `gets is a security hole`) Need to untared and remote gets usage
`wmi-1.3.16.tar.bz2` is not building - TLS isssue - needs to be untarred and removed unknown fn usage

#### Build agent RPM
```
cd opsview-base
env os=centos7 make agent-tar
edit Makefile and add a target to build agent-rpm (similar to base.rpm)
```

#### Build opsview server RPMs

```
cd /usr/local/opsview
env os=centos7 make rpmpkg
```

opsview-base and opsview-core generated RPMs have conflicting files
Do a dry-run `yum install opsview` and check the conflictings directories.
I used `rpmrebuild` to remove the %files directive from `opsview-core.rpm` after build
 

### Build opsview-perl RPM
Quirks : on Centos7  `vendor_perl` path is not added in PERLIB during build, so I had to:

```
cp -r /usr/share/perl5/vendor_perl/* /usr/share/perl5/
cp -r /usr/lib64/perl5/vendor_perl/* /usr/lib64/perl5/
```
