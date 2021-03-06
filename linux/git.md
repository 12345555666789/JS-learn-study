## git相关命令
- ls -a(-all) ~/.ssh    列出.ssh目录中的文件
    - 公钥的文件名是以下之一：\
      id_dsa.pub\
      id_ecdsa.pub\
      id_ed25519.pub\
      id_rsa.pub
- cat ~/.ssh/id_rsa.pub 查看当前的ssh密钥
- ssh-keygen -t rsa -C "your_email@example" 创建新的ssh密钥


- git remote set-url 原连接名 新的连接  修改连接地址
- git config --list 查看所有的config配置

- error: GH007:Your push would publish a private email address. 

  由于设置邮箱为隐私邮箱，解决方法：
  - 1.在github上的settings=>Emails=>Keep my email address private去掉勾选
  - 2.命令行中配置邮箱为 username@users.noreply.github.com   `git config --global user.email 'username@users.noreply.github.com'`

- ssh: connect to host `gitlab.huayingjuhe.com` port 22: Connection timed out
fatal: Could not read from remote repository.

原因可能是防火墙修改，导致网路无法连接。如果是超时这种情况使用http协议而不是ssh这种方式，只需要将配置文件的url更改为http。

  - ssh -T git@github.com  检查网络连接
  - git config --local -e 直接修改git的配置文件，将url=git@github.com:username/repo.git修改为url=`https://github.com/username/repo.git`
  - git config --global url."http://github.com/".insteadOf git@github.com:

>  WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED! (远程连接服务异常)
```
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ECDSA key sent by the remote host is
SHA256:DY4p81gRCppTGt4f+3882AUe7wmG7BUIGFRmiF1Bypg.
Please contact your system administrator.
Add correct host key in /c/Users/Juphy/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /c/Users/Juphy/.ssh/known_hosts:2
ECDSA host key for 58.87.114.124 has changed and you have requested strict checking.
Host key verification failed.
```
由于中间有一句"Add correct host key in /c/Users/Juphy/.ssh/known_hosts to get rid of this message.
Offending ECDSA key in /c/Users/Juphy/.ssh/known_hosts:2"
vi ~/.ssh/known_hosts将第二行删掉。

### github不允许100MB大文件上传
- 如果大文件是最近一次commit的，还没有push
  - 1.在.gitignore中忽略你需要忽略的文件的名字（跟忽略一般文件一样）
  - 2.从本地仓库移除掉大文件 git rm --cached 文件名（要移除的文件）
  - 3.提交操作： git commit --amend -CHEAD

- 突破github限制，支持单个文件超出100M
  - 1.安装[GIT LFS](https://git-lfs.github.com/)
  - 2.git lfs install（设置一次即可）
  - 3.git lfs track "*.pdf" 跟踪一下要push的大文件的文件名或者文件类型，还可以直接编辑.gitattributes文件
  - 4.设置完毕就可以正常的add，commit，push
  