## Linux笔记

### 文件名含特殊字符
Linux文件名允许含特殊字符如“&“， ”|“， ”-“，会为命令行传参带来麻烦
处理方法：
- 一般用反斜杠转义
- 若开头为”-“，则可以使用形如`cat -- -myspecfile`的形式

### SSH
SSH（Secure Shell)可以用于安全的远程登录，也可以充当加密的通信协议。
SSH支持口令验证以及公钥验证
口令验证：传统的用户名-口令验证，只用验证通过才能登录。缺点是**口令明文传输**，且每次登录都要重新输入口令。
公钥验证：客户端请求登录后，服务器发送使用公钥加密的随机数密文到客户端，客户端需要使用匹配的私钥解密，再将随机数发送回服务端，服务端验证随机数正确后才可以登录，并且通信的内容全程加密。好处是不用反反复复地输入口令，且**不需要传输口令和密钥**，更安全。
#### Quick Start
```bash
ssh-keygen # 生成密钥对，已有密钥可以跳过
ssh-copy-id host
# 或者手动上传到服务器： scp ~/.ssh/id_rsa.pub username@host:~/.ssh/authorized_keys
# 首次登录需要使用口令
ssh username@host # 远程登录
```
#### 使用
- 生成密钥对（已有的可以跳过)
  `ssh-keygen`会在`$HOME/.ssh/`下生成两个文件：`id_rsa.pub, id_rsa`，分别是公钥和私钥
  只需上传公钥，保管好私钥
- SSH默认监听的端口是22，如果服务器监听的是别的端口，可以使用`-p`来指定
- 使用`service sshd restart`重启SSH服务
- 修改`/etc/ssh/sshd_config`来配置SSH服务
  - 限制登录用户：
    `AllowUsers u1 u2 ...`
    `DenyUsers u3 u4 ...`
  - 不允许口令验证，只允许密钥验证：
    `PasswordAuthentication no`
    `PubkeyAuthentication yes`
- `/etc/ssh/ssh_config`用于配置SSH客户端，`/etc/ssh/sshd_config`用于配置SSH服务
- `scp`是远程版的`cp`指令，用于上传/下载文件，底层采用SSH进行通信，指令语法形似`scp from to`，`from`和`to`形如`username@host:/path/to/file`，本地位置可以只写路径。

#### 其他
- 区别于HTTPS，SSH不存在CA（证书颁发机构），故公钥是没有第三方保证安全性，服务器密钥是*可以伪造的*，借此可以实施经典的“中间人攻击”来窃听篡改所有通信内容。因此，SSH采用公钥验证。第一次登录某一服务器时SSH会记录该服务器的对应的公钥的MD5指纹，然后后面每一次登录都会检查指纹是否变化了，若变化了则可能是遭到了攻击，SSH将警告客户端。
- 此外，即使采用了公钥验证，中间人攻击依然可以伪装成服务端与客户端通信（任何人都可以请求得到公钥），但由于没有私钥，中间人无法伪造信息发送到服务端，也无法解密服务器发送的信息，只能转发；中间人也只能窃听并转发客户端发送的内容而无法篡改。
- SSH似乎在闲置一段时间后自动断开

### SFTP
`scp`管理远程文件非常麻烦。而传统的FTP服务器可以解决这个问题。通过在服务器上搭建FTP服务器可以方便地管理服务器的文件。
SFTP是基于SSH通信的FTP服务器，其通用性使得它支持多种客户端访问（无论是Linux Shell的sftp命令行还是Windows的各种图形化FTP客户端）

#### Quick Start
1. 添加ftp用户
    ```bash
    useradd ftpuser
    passwd ftpuser
    ```
2. 修改`/etc/ssh/sshd_config`
   ```conf
   #Subsystem sftp /usr/libexec/openssh/sftp-server # 注释原有的服务
   Subsystem sftp internal-sftp #用SSH的internal-sftp
    
   Match User ftpuser         #sftp用户
    ChrootDirectory /         #sftp用户登入的目录（注意这个目录的属主必须是root)
    ForceCommand internal-sftp
    X11Forwarding no
    AllowTcpForwarding no
   ```
3. 重启SSH服务: `service sshd restart`
4. 客户端登录`sftp ftpuser@host`

#### sftp命令
- sftp登入后，命令行可以指向基本的文件操作如`ls, cd, pwd`等，但都在远程端的文件系统内进行；在原命令前面加l就变成了在本地文件系统执行的命令如`lls, lcd, lpwd`
- `get/put`用于下载/上传文件，参数就是路径，不用指定host等

#### 其他
支持SFTP的客户端很多，在Windows下可以使用FileZilla图形化工具，它支持口令登录与密钥登录