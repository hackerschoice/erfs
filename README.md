# thc-rfs-client
THC-RFS is an easy-to-use, easy-to-setup, no-headache secure file system with the encrypted data being stored on a remote server/cloud and inaccessible to a rogue server operator.

Direct Download: [thc-rfs](https://raw.githubusercontent.com/hackerschoice/thc-rfs-client/master/thc-rfs)  
Technical Details: [Technical-Details](https://github.com/hackerschoice/thc-rfs-client/wiki/Technical-Details)

This bash-script allows one or multiple users to mount and share a remote encrypted filesystem. The server is provided by THC as a courtesy and free (as in free beer!).

It is possible to mount multiple filesystems from multiple computers at the same time. The data is securely and seamlessly syncronized. There is no limit per user (for now).

The server has no knowledge of the content. A rogue server operator can not access the data. All key material is created on the user's computer and never stored or transfered to the server.

Features:  
- Does not require root or superuser priviledges.
- Data to remain secure even if the server is compromised.
- Can be used by multiple users from multiple locations at the same time and simulatnously.
- No Public Key Infrastructure. Everything to work by 'Deterministic Key Derivation' (like Bitcoin does).
- 256 bit AES encryption
- Base58 passwords 24 characters long (enforced).

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

Create a SHARE-SECRET and initialize a new File Share (example):
```
$ thc-rfs init
Server: Creating a new Remote File Share....

--> You MUST remember this SHARE-SECRET. Access to the data is lost <--
--> *FOREVER* if the SHARE-SECRET is lost. KEEP IT SAFE.            <--

        ##############################################
        ##                                          ##
        ##  SHARE-SECRET: aDe5F2ik3x35x7pfAEAWdC5Y  ##
        ##                                          ##
        ##############################################
```

Mount the Remote File Share on your computer (example):
```
$ thc-rfs mount aDe5F2ik3x35x7pfAEAWdC5Y ~/secure
Encrypted partition mounted to /Users/me/secure
$ ls -al ~/secure
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
