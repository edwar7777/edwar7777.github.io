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
