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
   - Add below in **/etc/rc.conf** and save    
    ![](https://imgur.com/SA5tQLv.jpg)
   - Add below in **/etc/exports** and save  
  
      ![](https://imgur.com/LU3Ctb9.jpg)  
  
   - And then we restart NFS to enable the service
      ```
      service nfs restart
      ```
 
## NFS client
- WireGuard IP : 10.113.59.1
- Mount automatically : Will be mounted automatically when accessed(using autofs)
- Implementation  
  - Enabling NFS client by adding below in **/etc/rc.conf** and save  
    ![](https://imgur.com/yO4KWnY.jpg)
  - Enabling autofs by adding below in **/etc/rc.conf** and save  
    ![](https://imgur.com/zYLBmjG.jpg)
  - Specifying what to automount by editing **/etc/auto.indirect** and save,**remembert their respective access privilege**   
    ![](https://imgur.com/SCciyIN.jpg)
  - And then we restart to enable the service
    ```
    service autofs restart
    service nfs restart
    ```
