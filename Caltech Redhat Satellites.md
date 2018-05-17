https://www.imss.caltech.edu/news/1447


Red Hat Satellite v5, has reached end of life and will be decommissioned on Monday, November 27th. After November 27th, please follow instructions below to upgrade your servers to the new Red Hat Satellite v6 system.

Please issue the following commands as root:

"mv /etc/sysconfig/rhn/systemid /etc/sysconfig/rhn/systemid.bck"
"yum install http://satellite.caltech.edu/pub/katello-ca-consumer-latest.noarch.rpm"
For example, if the machine is a Red Hat Linux 7 Server, issue the command:

"subscription-manager register --activationkey=rhel-7-server --org=IMSS"
Note: Please use IPAC, GPS, or Seismo for “--org=” if you are part of these organizations. Otherwise, please use “--org=IMSS”.

Below is a list of valid activation keys:

rhel-7-client
rhel-7-workstation
rhel-6-server-x86_64
rhel-6-server-i386
rhel-6-workstation-x86_64
rhel-6-workstation-i386
rhel-6-client-x86_64
rhel-6-client-i386
After successful registration, please issue the following commands:
“subscription-manager attach”
“rm -rfv /var/cache/yum”
“yum install katello-agent”
