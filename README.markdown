# A Recipe for a haproxy 1.6 stable version RPM on CentOS

## TLDR Jenkins Friendly Build

### Pre-Req

yum install -y rpmdevtools pcre-devel
yum groupinstall -y 'Development Tools'
yum install -y openssl-devel

### Jenkins Build Script

```mkdir -p $WORKSPACE/rpmbuild/SOURCES
mkdir -p $WORKSPACE/rpmbuild/SPECS

wget http://www.haproxy.org/download/1.6/src/haproxy-1.6.9.tar.gz
mv haproxy-1.6.9.tar.gz $WORKSPACE/rpmbuild/SOURCES/
cp SOURCES/* $WORKSPACE/rpmbuild/SOURCES/
cp SPECS/* $WORKSPACE/rpmbuild/SPECS/

rpmbuild -vv --define "_topdir $WORKSPACE/rpmbuild" -ba SPECS/haproxy.spec```

## Deltas from Fork Source for CSMS

* Upgraded to 1.6.9
* Removed LUA from the build

## Create an RPM Build Environment

Perform the following on a build box as a regular user.

Install rpmdevtools from the [EPEL][epel] repository:

    sudo yum install rpmdevtools pcre-devel
    rpmdev-setuptree

## Install Prerequisites for RPM Creation

    sudo yum groupinstall 'Development Tools'
    sudo yum install openssl-devel

## Download haproxy

    wget http://www.haproxy.org/download/1.6/src/haproxy-1.6.9.tar.gz
    mv haproxy-1.6.9.tar.gz ~/rpmbuild/SOURCES/

## Get Necessary System-specific Configs

    git clone https://github.com/atosorigin/csms-rpm-haproxy.git
    cp rpm-haproxy/SOURCES/* ~/rpmbuild/SOURCES/
    cp rpm-haproxy/SPECS/* ~/rpmbuild/SPECS/

## Build the RPM

    cd ~/rpmbuild/
    rpmbuild -ba SPECS/haproxy.spec

The resulting RPM will be in ~/rpmbuild/RPMS/x86_64

## Credits

Based on the Red Hat 6.4 RPM spec for haproxy 1.4.

Maintained by [Martijn Storck](martijn@bluerail.nl)

[EPEL]: http://fedoraproject.org/wiki/EPEL#How_can_I_use_these_extra_packages.3F
