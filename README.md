# thc-rfs-client
Encrypted Remote File System CLIENT

This tool allows one or multiple users to mount and share an encrypted filesystem. The encrypted filesystem is accessible on your local computer in ~/thc/sec. The server is provided by THC as a courtesy (free, as in free beer!). The data is not accessible to us. Only the user with the knowledge of the SEC-PASSWORD can access the data.

It is possible to mount the filesystem on multiple computers at the same time. The data is securely and seamlessly syncronized. The server has no knowledge of the content and can not access to the data.

Features:  
- Does not require root or superuser priviledges
- Server has no access to the data
- FileSystem can be mounted from multiple computers at the same time.
- Multi-User support

Currently supported OS:  
- Linux  
- MacOS  

It is possible to create, mount and share multiple different filesystems. There is no limit per user (for now). 

By using our server ("the service") you agree that you will only use the service for research or for doing good things. You agree you will not use the service for illegal activities. If in doubt then please run your own server.

---
**Pre-Requisite**
```
$ apt-get update -y
$ apt-get install -y git sshfs encfs
```

**Installation**
```
$ git clone https://github.com/hackerschoice/thc-rfs-client.git
$ mkdir -p ~/thc/etc
$ cp thc-rfs-client/id_rsa-rfs ~/thc/etc/
```

Request a new RFS-SECRET from the server. Remember the RFS-SECRET.
```
$ ssh -i ~/thc/etc/id_rsa-rfs rfs-init@rfs.thc.org
```

**Use**

Mount the Remote File Share on your computer:
```
$ mkdir -p ~/thc/rfs
$ sshfs allow_other,default_permissions,IdentityFile=~/thc/etc/id_rsa-rfs <RFS-SECRET>@rfs.thc.org:rw ~/thc/rfs
```

Create an encrypted volume inside the remote file share.
```
$ mkdir -p ~/thc/sec
$ export SECPASS=`dd if=/dev/urandom bs=1 count=32 2>/dev/null | openssl base64 | sed 's/[^a-zA-Z0-9]//g' | head -n1 | cut -c1-16`
$ echo "Enter this SEC-PASSWORD below: sec-${SECPASS}"
$ encfs --standard ~/thc/rfs/encrypted ~/thc/sec
```

Keep your RFS-SECRET and SEC-PASSWORD secure. 

**SUCCESS!. All data written to ~/thc/sec is accessible to anyone with the knowledge of the SEC-PASSWORD and nobody else (not even THC).**

The server does not have access to the SEC-PASSWORD or the data.

---
Unmount everything:
```
$ encfs -u ~/thc/sec
$ umount -f ~/thc/rfs
```

---
**Sharing**

If you receive a RFS-SECRET and SEC-PASSWORD then you can access somebody's else secure share and collaborate. Replaece the RFS-SECRET below and execute:
```
$ mkdir -p ~/thc/rfs
$ sshfs allow_other,default_permissions,IdentityFile=~/thc/etc/id_rsa-rfs <RFS-SECRET>@rfs.thc.org:rw ~/thc/rfs
```

Enter the SEC-PASSWORD when prompted:
```
$ encfs --standard ~/thc/rfs/encrypted ~/thc/sec
```

---
**Running your own sever**

You can run your own server is you desire to do so. This is optional. Please refer to [hackerschoice/docker-thc-rfs-server](https://github.com/hackerschoice/docker-thc-rfs-server) for more information.
