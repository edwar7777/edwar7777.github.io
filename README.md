# NFS clients behind NAT

The NFS server is accessible in the intranet. Some clients behind the NAT are Sparc machines with Solaris 2.6 / 7 / 8 installed. Therefore, neither CIFS (Samba) mount nor NFSv4 can be adopted. The only option is NFS with version<=3.

The NAT runs on a workstation with FreeBSD or Linux. Routed or Bridged network is not preferred in order to make the clients invisible from the intranet.

After NAT, the source port usually >=1024, while NFS server may allow only priviledged source ports (port<1024).


## Approaches

### Option 'insecure' in /etc/exports
The simplest solution is to turn on the 'insecure' option in /etc/exports of NFS servers if no other concern exists. The option 'insecure' makes the server permit source port >=1024.


### NFS proxy, like NFS-ganesha
I never success to use NFS-ganesha proxy FSAL to access NFS outside the workstation for some errors (OS: CentOS 7). To solve it beyonded my capability then, honestly.


### NFS re-export, by user mode nfs server, like unfsd
I never try it. Some discussions in [NFS export of unsupported filesystems (e.g. NFS re-export)](https://groups.google.com/forum/#!topic/alt.os.linux/oXW6JjIcqAw) mention lock-down may occurs.


### NAT + static-port, in FreeBSD PF
Add 'static-port' for *nat* in pf.conf, the configuration file of FreeBSD PF program. The option keeps source port number unchanged.

/etc/pf.conf:

	ext_if="em0"
	int_if="em1"
	nat  on $ext_if inet  from $int_if:network to any -> ($ext_if) static-port

In my first try with 4 NFS clients (Solaris 8), the first 2 clients lose server responses after the 3rd and the 4th client mount the NFS.
A very bad instance may occur: all clients use the same port.


### NAT + priviledged ports, in FreeBSD PF

Add an extra *nat* with priviledged ports before the ordinary *nat* statement:

	 nat on $ext_if inet  from $lan_nfs_cli    to $mainnas -> ($ext_if) port 111:1023
	 nat on $ext_if inet  from $int_if:network to any      -> ($ext_if)



# References
* [NFS server behind a PF firewall](http://blog.e-shell.org/227), via Google:\<nfs client behind nat>
* [Mount NFS export for machine behind a NAT](https://blog.bigon.be/2013/02/08/mount-nfs-export-for-machine-behind-a-nat/), via google:\<nfs client behind nat>
* [FreeBSD nat via PF: how to change from random UDP ports to incremental?](https://serverfault.com/questions/67249/freebsd-nat-via-pf-how-to-change-from-random-udp-ports-to-incremental), via google:\<pf nat static-port>
* [pfctl: Invalid argument. when using add with some netmasks](http://openbsd-archive.7691.n7.nabble.com/6-6-pfctl-Invalid-argument-when-using-add-with-some-netmasks-td381455.html), via google:\<freebsd pf 192.168 become "64.168">

[//]: <> (__END__)
