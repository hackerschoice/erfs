# thc-rfs-client
An easy-to-use, easy-to-setup, hassle-free secure file system with the encrypted data being stored on a remote cloud server without having to trust the server.

Direct Download: [thc-rfs](https://raw.githubusercontent.com/hackerschoice/thc-rfs-client/master/thc-rfs)  
Medium Article: [THC's encrypted cloud based file system](https://tiny.cc/thcrfs)  
Technical Details: [Technical-Details](https://github.com/hackerschoice/thc-rfs-client/wiki/Technical-Details)

The client is a bash-script. The cloud server is provided by THC for free (as in free beer!).

There is no limit per user, no limit of the number of file systems and no limit of how many locations can access the same file system simoultanously. It supports collaboration and the same filesystem can be accessed from different computers at the same time. The data is securely and seamlessly synchronized.

The server has no knowledge of the content. A rogue server operator can not access the data. All key material is created on the user's computer and never stored or transferred to the server.

**Features:**  
- Does not require root or superuser privileges.
- Data to remain secure even if the server is compromised.
- Encrypts file name and content.
- Can be used by multiple users from many locations at the same time and simulatnously.
- No Public Key Infrastructure. Everything to work by 'Deterministic Key Derivation' (like Bitcoin does).
- Strong AES encryption
- Base58 passwords 24 characters long (enforced).

**Currently supported OSes:**  
- Linux  
- MacOS  

*By using our server ("the service") you agree that you will only use the service for research or for doing good things. You agree that you will not use the service for illegal activities. If in doubt then please run your own server.*

---
**Pre-Requisite**
```ShellSession
$ apt-get update -y
$ apt-get install -y git sshfs encfs
```

**Installation**
```ShellSession
$ git clone https://github.com/hackerschoice/thc-rfs-client.git
$ export PATH=$PATH:${PWD}/thc-rfs-client
```

or if you do not have git:
```ShellSession
$ wget https://raw.githubusercontent.com/hackerschoice/thc-rfs-client/master/thc-rfs
$ curl -OL https://raw.githubusercontent.com/hackerschoice/thc-rfs-client/master/thc-rfs
```

**Usage**

Create a SHARE-SECRET and initialize a new File Share (example):
```ShellSession
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
```ShellSession
$ thc-rfs mount aDe5F2ik3x35x7pfAEAWdC5Y ~/secure
Encrypted partition mounted to /Users/me/secure
$ ls -al ~/secure
```

The server does not have access to the SHARE-SECRET or the data. Keep the SHARE-SECRET secure. Anyone with the knowledge of the SHARE-SECRET can access the data.

---
Unmount everything:
```ShellSession
$ thc-rfs umount <SHARE-SECRET>
```

---
**Sharing**

If you receive a SHARE-SECRET then you can access somebody else's secure share and collaborate at the same time. 
```ShellSession
$ thc-rfs mount <SHARE-SECRET>
```

---
**Security**

Passing SHARE-SECRET using the command line parameter is not secure. A better way is:
```ShellSession
$ X=<SHARE-SECRET>
$ thc-rfs mount /dev/stdin <<<"$X"
$ unset X
```

The tool relies on the underlying security of ssh, sshfs and EncFS.

---
**Tips**

Using a different server:
```ShellSession
$ export THC_RFS_SERVER=1.2.3.4
$ export THC_RFS_PORT=2222
```

Prompting for the SHARE-SECRET (Grugq's idea):
```ShellSession
$ thc-rfs mount -x
Enter SHARE-SECRET: 
```

Reading SHARE-SECRET from a file. Put this into your ~/.bashrc to mount the file system every time you log in:
```ShellSession
$ thc-rfs mount my-share-secret.txt
```

Shorten the commands:
```ShellSession
$ thc-rfs i
$ thc-rfs m
$ thc-rfs u
```

---
**Running your own sever**

You are encouraged to run your own server. This is optional. Please refer to [hackerschoice/docker-thc-rfs-server](https://github.com/hackerschoice/docker-thc-rfs-server) for more information.
