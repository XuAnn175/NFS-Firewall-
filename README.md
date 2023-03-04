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
## NFS Firewall
- Only accept packet from 10.113.59.0/24 to access HTTP/HTTPS.  
- All IP can’t send ICMP echo request packets to server,except 10.113.59.254 and 10.113.59.2
- Implementation using pf
  - Steps
    - Enabling pf by adding below in **/etc/rc.conf** and save  
    ![](https://imgur.com/8W1GqnS.jpg)
    - Specifying who to block by editing **/etc/pf.conf,
    first block all and then pass the users under specific domain**  
    ![](https://imgur.com/NmT2S6u.jpg)
    - And then we restart to enable the service
    ```
      service pf restart
    ```
 - SSH failed login
   - If someone attempts to login via SSH but failed for 3 times in 1 minute, then their IP will be banned from SSH for 60 seconds automatically
   - Implementation using blacklistd
     - Steps
       -  Enabling blacklistd by adding below in **/etc/rc.conf** and save  
       ![](https://imgur.com/BNZW72r.jpg) 
       - Specifying the banning rule by editing **/etc/blacklistd.conf**  
         
       ![](https://imgur.com/TWLzT4J.jpg) 
       - And then we restart to enable the service
        ```
        service blacklistd restart
        ```
  - Extra : Unban an IP
    - Write a shell script ‘iamgoodguy’ to unban an IP.
    - Usage : **iamgoodguy** *TheSelectedIP*
    - Implementation :  
        Write a shell script ‘iamgoodguy’ under **/bin**  
        Shell Script :  
        ![](https://imgur.com/Z7o431c.jpg)  
        And then we restart to enable the service
        ```
        service blacklistd restart
        ```
