## Mount OneDrive as WebDAV on Linux
### Get OD4B Cookie
1. Login the office365 website. Choose “Stay signed in?” to “YES”, and keep the period of validity longer.
2. Press **F12** or right click–>Inspect element–>Network to capture the traffic.
3. Open **SharePoint** or **OneDrive** app.
4. Select a HTTP request include *****.sharepoint.com, you can see the rtFa and FedAuth cookie that are required to authorize.
You can also find rtFa and FedAuth keys by other metods (they are in cookies).
### Get WebDav Link
1. Open a folder on OD4B website, copy the link on the **Address Bar** and **URL Decode**
```
https://***-my.sharepoint.com/personal/username_domain_edu/_layouts/15/onedrive.aspx?id=/personal/username_domain_edu/Documents/bookmarks
```
2. Finally, the webdav link of root directory of OD4B:
```
https://***-my.sharepoint.com/personal/username_domain_edu/Documents
```
### Modify Davfs2 config
```
sudo chmod 777 /etc/davfs2/davfs2.conf
echo "use_locks 0" >> /etc/davfs2/davfs2.conf
echo "[/home/test]" >> /etc/davfs2/davfs2.conf
echo "add_header Cookie rtFa=U5/qsS***UAAAA=;FedAuth=77u/PD9****U1A+" >> /etc/davfs2/davfs2.conf
```
### Mount OD4B
```
sudo /sbin/mount.davfs https://***-my.sharepoint.com/personal/username_domain_edu/Documents/bookmarks /home/test
```
### Chech Mounting Status
```
df
```
### Upload file
```
cp text.zip /home/test
```
### Unmount OD4B
```
sudo umount /home/test
```

## Shell Script
For easy use, you can use the simple shell script. If you want to mount multiple accounts, just repeat the operations or run the shell script repeatly.

### Cookie Version
```
wget https://raw.githubusercontent.com/yulahuyed/test/master/OD4B.sh && bash OD4B.sh
```
### Auto Login Version
```
wget https://raw.githubusercontent.com/yulahuyed/test/autologin/OD4B.sh && bash OD4B.sh
```

## Dokers
You can create a davfs2 image with Docker. The Image need to run with:
```
–cap-add SYS_ADMIN –device=/dev/fuse –security-opt apparmor:unconfined
```
or with **–privileged**.

## Errors

### mount.davfs: Mounting failed. 302 Found
Please make sure the link of OD4B is correct. Solution from #69

Reference link：
https://shui.azurewebsites.net/2018/01/13/mount-onedrive-for-business-on-headless-linux-vps-through-webdav/
https://unix.stackexchange.com/questions/337329/mount-webdav-on-linux-with-cookie-authentication
https://micreabog.wordpress.com/2017/02/24/using-duplicity-with-microsoft-sharepointonedrive-for-business/
