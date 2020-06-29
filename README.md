# NFS clients behind NAT

The NFS server is accessible in the intranet. Some clients behind the NAT are Sparc machines with Solaris 2.6 / 7 / 8 installed. Therefore, neither CIFS (Samba) mount nor NFSv4 can be adopted. The only option is NFS with version<=3.

The NAT runs on a workstation with FreeBSD or Linux. Routed or Bridged network is not preferred in order to make the clients invisible from the intranet.

After NAT, the source port usually >=1024, while NFS server may allow only priviledged source ports (port<1024).


## Approaches

### Option 'insecure' in /etc/exports
The simplest solution is to turn on the 'insecure' option in /etc/exports of NFS servers if no other concern exists. The option 'insecure' makes the server permit source port >=1024.


### NFS-proxy, like NFS-ganesha
I never successed to use NFS-ganesha proxy FSAL to access NFS outside the workstation for some errors (OS: CentOS 7). To solve it beyonded my capability then, honestly.


### NFS re-export, by user nfsd
(never tried)


### NAT + static-port, in FreeBSD PF

### NAT + round-robin priviledged ports, in FreeBSD PF

# References
* [NFS server behind a PF firewall](http://blog.e-shell.org/227) via Google:\<nfs client behind nat>
* [Mount NFS export for machine behind a NAT](https://blog.bigon.be/2013/02/08/mount-nfs-export-for-machine-behind-a-nat/) via google:\<nfs client behind nat>
* 
* [FreeBSD nat via PF: how to change from random UDP ports to incremental?](https://serverfault.com/questions/67249/freebsd-nat-via-pf-how-to-change-from-random-udp-ports-to-incremental) via google:\<pf nat static-port>
* [6.6 pfctl: Invalid argument. when using add with some netmasks](http://openbsd-archive.7691.n7.nabble.com/6-6-pfctl-Invalid-argument-when-using-add-with-some-netmasks-td381455.html) via google:\<freebsd pf 192.168 become "64.168">

[//]: <> (__END__)
