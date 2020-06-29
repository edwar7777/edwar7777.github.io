# NFS cross NAT

The NFS server is accessible in intranet. Some clients are Sparc machines with Solaris 2.6 / 7 / 8 installed. Therefore, neither CIFS (Samba) mount nor NFSv4 can be adopted. The only option is NFS with version<=3.

The NAT is constructed by FreeBSD or Linux. Routed or Bridged network is not preferred so that the clients can be invisible from the intranet.

After NAT, the source port usually >=1024, while NFS server may not allow such source ports for security.


## Approaches

### Option 'insecure' in /etc/exports
The simplest soluation

### NFS-proxy, like NFS-ganesha

### NAT + static-port in FreeBSD PF

### NAT + round-robin in FreeBSD PF
