# NFS cross NAT

The NFS server is accessible in the intranet. Some clients behind the NAT are Sparc machines with Solaris 2.6 / 7 / 8 installed. Therefore, neither CIFS (Samba) mount nor NFSv4 can be adopted. The only option is NFS with version<=3.

The NAT runs on a workstation with FreeBSD or Linux. Routed or Bridged network is not preferred in order to make the clients invisible from the intranet.

After NAT, the source port usually >=1024, while NFS server may not allow such source ports for security.


## Approaches

### Option 'insecure' in /etc/exports
The simplest solution is to turn on the 'insecure' option in /etc/exports of NFS servers if no other concern exists. The option 'insecure' makes the server permit source port >=1024.


### NFS-proxy or re-export, like NFS-ganesha
I do not have a successful NFS-ganesha proxy to access NFS outside the workstation for some errors. To debug it may beyond my capability.


### NAT + static-port in FreeBSD PF

### NAT + round-robin in FreeBSD PF
