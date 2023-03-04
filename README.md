# NFS-and-Firewall
Implementation of NFS server and NFS client on FreeBSD.  
NFS Firewall : allow only users under a specific domain to access HTTP/HTTPS  
## NFS server
- WireGuard IP : 10.113.59.2
- Restrict other hosts to mount the storage bt editing the export table
  - Only 10.113.59.0/24 can mount /net/data/public
    - Read only
  - Only 10.113.59.1/32 can mount /net/data/stu59
    - Allow read & write
- Implementation  
   - Add below in /etc/exports and save  
  
    ![](https://imgur.com/LU3Ctb9.jpg)  
  
   - And then we restart NFS to enable the service
    ```
    service nfs restart
    ```
 
## NFS client
