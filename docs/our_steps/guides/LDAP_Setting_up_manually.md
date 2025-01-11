# LDAP - Setting up manually

```bash
dnf -y install openldap-clients sssd sssd-ldap oddjob-mkhomedir openssl-perl
# create /etc/sssd/sssd.conf
chmod 600 /etc/sssd/sssd.conf

# create /etc/openldap/ldap.conf
systemctl enable sssd.service --now

# nao deveria precisar...
authselect select sssd --force # talvez baste esse, talvez nem esse
#authselect select sssd with-mkhomedir --force
#systemctl enable oddjobd.service --now

yum install nfs-utils autofs

# create /etc/auto.home
# create /etc/auto.master

systemctl enable --now autofs
setsebool -P use_nfs_home_dirs 1

echo 'XDG_CACHE_HOME  DEFAULT="/tmp/.cache_@{PAM_USER}"' >> /etc/security/pam_env.conf

```

```bash
cat  /etc/sssd/sssd.conf
[sssd]
config_file_version = 2
services = nss, pam,autofs
domains = default

[nss]
homedir_substring = /home

[pam]

[domain/default]
id_provider = ldap
autofs_provider = ldap
auth_provider = ldap
chpass_provider = ldap
ldap_uri = ldap://10.68.254.200
ldap_search_base = dc=lsc,dc=ic,dc=unicamp,dc=br
ldap_id_use_start_tls = True
ldap_tls_cacertdir = /etc/openldap/cacerts
cache_credentials = True
ldap_tls_reqcert = allow
```

```bash
cat /etc/openldap/ldap.conf
# Your LDAP server. Must be resolvable without using LDAP.
#host 127.0.0.1

# The distinguished name of the search base.
BASE dc=lsc,dc=ic,dc=unicamp,dc=br

# Another way to specify your LDAP server is to provide an
URI ldap://10.68.254.200
#URI ldap://143.106.24.200

# The LDAP version to use
LDAP_VERSION 3

# The port.
#port 389

TLS_CACERTDIR /etc/openldap/cacerts

# Turning this off breaks GSSAPI used with krb5 when rdns = false
SASL_NOCANON    on

nss_initgroups_ignoreusers avahi,avahi-autoipd,backup,bin,colord,condor,daemon,debian-spamd,dnsmasq,games,gdm,geoclue,gnats,hplip,icecc,irc,kernoops,libuuid,libvirt-dnsmasq,libvirt-qemu,lightdm,list,lp,mail,man,messagebus,munin,news,ntp,nvidia-persistenced,oprofile,proxy,pulse,root,rtkit,saned,speech-dispatcher,sshd,statd,sync,sys,syslog,usbmux,uucp,whoopsie,www-data,x2gouser
```

```bash
cat /etc/auto.home
*    -rw,fstype=nfs4,hard,timeo=15,intr,_netdev 10.68.254.200,10.68.254.201,10.68.254.202:/volume1/homes/&
```

```bash
cat /etc/auto.master
#
# Sample auto.master file
# This is a 'master' automounter map and it has the following format:
# mount-point [map-type[,format]:]map [options]
# For details of the format look at auto.master(5).
#
/misc	/etc/auto.misc
#
# NOTE: mounts done from a hosts map will be mounted with the
#	"nosuid" and "nodev" options unless the "suid" and "dev"
#	options are explicitly given.
#
/net	-hosts
#
# Include /etc/auto.master.d/*.autofs
# The included files must conform to the format of this file.
#
+dir:/etc/auto.master.d
#
# If you have fedfs set up and the related binaries, either
# built as part of autofs or installed from another package,
# uncomment this line to use the fedfs program map to access
# your fedfs mounts.
#/nfs4  /usr/sbin/fedfs-map-nfs4 nobind
#
# Include central master map if it can be found using
# nsswitch sources.
#
# Note that if there are entries for /net or /misc (as
# above) in the included master map any keys that are the
# same will not be seen as the first read key seen takes
# precedence.
#
+auto.master
/home /etc/auto.home --timeout=300
/home.ext /etc/auto.glusterfs
```