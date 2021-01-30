# Python与远程服务器交互操作

> https://blog.csdn.net/qq_39112101/article/details/94175083

## 下载paramiko

```
pip install paramiko
```



## paramiko ssh应用

```python
import paramiko
 
#创建一个ssh的客户端，用来连接服务器
ssh = paramiko.SSHClient()
#创建一个ssh的白名单
know_host = paramiko.AutoAddPolicy()
#加载创建的白名单
ssh.set_missing_host_key_policy(know_host)
 
#连接服务器
ssh.connect(
    hostname = "10.10.10.177",
    port = 22,
    username = "root",
    password = "root123"
)
 
#执行命令
stdin,stdout,stderr = ssh.exec_command("ls -la")
#stdin  标准格式的输入，是一个写权限的文件对象
#stdout 标准格式的输出，是一个读权限的文件对象
#stderr 标准格式的错误，是一个写权限的文件对象
 
print(stdout.read().decode())
ssh.close() # 关闭连接
```

## paramiko的Shell交互式连接

```python
import paramiko
 
#创建一个ssh的客户端
ssh = paramiko.SSHClient()
#创建一个ssh的白名单
know_host = paramiko.AutoAddPolicy()
#加载创建的白名单
ssh.set_missing_host_key_policy(know_host)
#连接服务器
ssh.connect(
    hostname = "10.10.21.177",
    port = 22,
    username = "root",
    password = "12345"
)
 
shell = ssh.invoke_shell()
shell.settimeout(1)
 
command = input(">>>"+"\n")

while command != 'exit':
		shell.send(command)
    try:
        recv = shell.recv(512).decode()
        if recv:
            print(recv)
        else:
            continue
    except:
        command = input(">>>") + "\n"
        
ssh.close()
```

## *文件上传与下载*

```python
import os
from sys import argv

import paramiko

# xx.py upload 本地文件或目录 远程位置
# xx.py download 远程文件或目录 本地位置
action = argv[1]
file1,file2 = argv[2], argv[3]

trans = paramiko.Transport(
    sock=("10.10.21.177",22)
)
 
trans.connect(
    username="root",
    password="12345"
)
sftp = paramiko.SFTPClient.from_transport(trans)

#上传
if action == 'upload':
  # 如果put不能上传目录，则要遍历目录下的所有文件，再上传
  sftp.put(file1, file2)
elif action == 'download':
  #下载
  #从远程/root/Desktop/hh.py获取文件下载到本地名称为hh.py
  sftp.get(file1,file2)
 
sftp.close()
```

