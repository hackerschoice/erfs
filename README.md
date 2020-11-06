# erfs
An easy-to-use, easy-to-setup, hassle-free secure file system with the encrypted data being stored on a remote cloud server without having to trust the server.

Direct Download: [erfs](https://raw.githubusercontent.com/hackerschoice/erfs/master/erfs)  
Medium Article: [THC's encrypted cloud based file system](https://tiny.cc/thcrfs)  
Technical Details: [Technical-Details](https://github.com/hackerschoice/erfs/wiki/Technical-Details)

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
**Pre-Requisite - Linux**
```ShellSession
$ apt-get update -y
$ apt-get install -y git sshfs encfs
```
(MacOS Users please [read below](#macos_install))

**Installation**
```ShellSession
$ git clone https://github.com/hackerschoice/erfs.git
$ sudo cp erfs/erfs /usr/local/bin
```

or:
```ShellSession
$ sudo curl -o /usr/local/bin/erfs -OL https://raw.githubusercontent.com/hackerschoice/erfs/master/erfs
$ sudo chmod +x /usr/local/bin/erfs
```

**Usage**

Create a SHARE-SECRET and initialize a new File Share (example):
```ShellSession
$ erfs init
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
$ erfs mount aDe5F2ik3x35x7pfAEAWdC5Y ~/secure
Encrypted partition mounted to /Users/me/secure
$ ls -al ~/secure
```

Unmount the share:
```ShellSession
$ erfs umount ~/secure
```

Unmount everything:
```ShellSession
$ erfs umount
```

The server does not have access to the SHARE-SECRET or the data. Keep the SHARE-SECRET secure. Anyone with the knowledge of the SHARE-SECRET can access the data.

---
**Collaborating**

If you receive a SHARE-SECRET from somebody then you can access their secure share simoultanously. 
```ShellSession
$ erfs mount <SHARE-SECRET>
```

---
**Security**

Passing SHARE-SECRET using the command line parameter is not secure. A better way is:
```ShellSession
$ SEC=<SHARE-SECRET> erfs mount
```

The tool relies on the underlying security of ssh, sshfs and EncFS.

---
<a id="macos_install"></a>
**Pre-Requisite - MacOS**

Open a Terminal: Applications > Utilities > Terminal. Install Homebew by typing this command:
```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

Install encfs and sshfs
```
$ brew cask install osxfuse
$ brew install sshfs
$ brew install encfs
```

Make sure to use sshfs version 2.10 or above or your speed will be 0.2Mbit ([Bug #57](https://github.com/osxfuse/sshfs/issues/57)):
```
$ sshfs --version
SSHFS version 2.10
OSXFUSE 3.10.6
FUSE library version: 2.9.7
```

**Automatically mount share on every login - MacOS**

1. Create '~/Library/Startup' with this content:
```
#! /bin/bash
erfs mount <SHARE-SECRET>
```

2. Make it executeable: `chmod +x ~/Library/Startup`. 

3. Add this file in System Preferences > Users & Groups > Login Items

**Automatically mount share on every login - Linux**

Put this into your ~/.bashrc to mount the file system every time you log in and put your SHARE-SECRET into my-share-secret.txt:
```ShellSession
$ erfs mount my-share-secret.txt
```

---
**Tips**

Using a different server:
```ShellSession
$ export THC_RFS_SERVER=1.2.3.4
$ export THC_RFS_PORT=2222
```

Prompting for the SHARE-SECRET (Grugq's idea):
```ShellSession
$ erfs mount -x
Enter SHARE-SECRET: 
```

Shorten the commands:
```ShellSession
$ erfs i
$ erfs m
$ erfs u
```

---
**Running your own sever**

You are encouraged to run your own server. This is optional. Please refer to [hackerschoice/docker-erfs-server](https://github.com/hackerschoice/docker-erfs-server) for more information.
