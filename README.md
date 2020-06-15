# thc-rfs-client
Encrypted Remote File System CLIENT

Direct Download: [thc-rfs](https://raw.githubusercontent.com/hackerschoice/thc-rfs-client/master/thc-rfs)  
Technical Details: [Technical-Details](https://github.com/hackerschoice/thc-rfs-client/wiki/Technical-Details)

This bash-script allows one or multiple users to mount and share a remote encrypted filesystem. The server is provided by THC as a courtesy and free (as in free beer!).

It is possible to mount the multiple filesystems from multiple computers at the same time. The data is securely and seamlessly syncronized. There is no limit per user (for now).

The server has no knowledge of the content and we can not access the data. All key material is created on the user's computer and never stored or transfered to the server.

Features:  
- Does not require root or superuser priviledges
- Server has no access to the data
- FileSystem can be mounted from multiple computers at the same time.
- Multi-User support
- 256 bit AES encryption
- 24 characters base58 password

Currently supported OS:  
- Linux  
- MacOS  

*By using our server ("the service") you agree that you will only use the service for research or for doing good things. You agree you will not use the service for illegal activities. If in doubt then please run your own server.*

---
**Pre-Requisite**
```
$ apt-get update -y
$ apt-get install -y git sshfs encfs
```

**Installation**
```
$ git clone https://github.com/hackerschoice/thc-rfs-client.git
$ export PATH=$PATH:${PWD}/thc-rfs-client
```

or if you do not have git:
```
$ wget https://raw.githubusercontent.com/hackerschoice/thc-rfs-client/master/thc-rfs
$ curl -OL https://raw.githubusercontent.com/hackerschoice/thc-rfs-client/master/thc-rfs
```

**Usage**

Create a SHARE-SECRET and initialize a new File Share:
```
$ thc-rfs init
```

Mount the Remote File Share on your computer:
```
$ thc-rfs mount <SHARE-SECRET> ~/secure
```

The server does not have access to the SHARE-SECRET or the data. Keep the SHARE-SECRET secure. Anyone with the knowledge of the SHARE-SECRET can access the data.

---
Unmount everything:
```
$ thc-rfs umount <SHARE-SECRET>
```

---
**Sharing**

If you receive a SHARE-SECRET then you can access somebody's else secure share and collaborate at the same time. 
```
$ 
$ thc-rfs mount <SHARE-SECRET>
```

---
**Security**

Passing the SHARE-SECRET by command line parameter is insecure. A better way is:
```
$ X=<SHARE-SECRET>
$ thc-rfs mount /dev/stdin <<<"$X"
$ unset X
```

The tools relies on the security of ssh, sshfs and EncFS.

---
**Tips**

Using a different server:
```
$ export THC_RFS_SERVER=1.2.3.4
$ export THC_RFS_PORT=2222
```

Prompting for the SHARE-SECRET (Grugq's idea):
```
$ thc-rfs mount -x
Enter SHARE-SECRET: 
```

Reading SHARE-SECRET from a file. Put this into your ~/.bashrc to mount the file system every time you log in:
```
$ thc-rfs mount my-share-secret.txt
```

Shorten the commands:
```
$ thc-rfs i
$ thc-rfs m
$ thc-rfs u
```

---
**Running your own sever**

You are encouraged to run your own server. This is optional. Please refer to [hackerschoice/docker-thc-rfs-server](https://github.com/hackerschoice/docker-thc-rfs-server) for more information.
