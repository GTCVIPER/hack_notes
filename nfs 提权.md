发现靶机开启了 nfs_acl 服务，查看到可以挂载到 **/home/vulnix** 目录

![image-20231023143154564](C:\Users\52252\Desktop\image-20231023143154564.png)

```bash
mount -t nfs 192.168.58.133:/home/vulnix /mnt/vulnix -o nolock

# -o nolock 取消挂载目录时加锁
```

发现挂载后所有者和所属组是`nobody:nogroup`无法进入查看目录

![image-20231023143638278](C:\Users\52252\Desktop\image-20231023143638278.png)

因为挂载的靶机目录是 **vulnix** 用户的家目录，可能需要 以 **vulnix** 用户的身份 ，才能进入到挂载目录，可以在本地创建  **vulnix** 用户，但我们不知道 **vulnix** 用户的 **uid** 和 **gid**

![image-20231023171345004](C:\Users\52252\Desktop\image-20231023171345004.png)

尝试用低版本的 **nfs** 挂载目录试试

![image-20231023144334145](C:\Users\52252\Desktop\image-20231023144334145.png)

发现 `vers=3` 时目录挂载成功，对应的 **vulnix** 目录的所属组和所有者变成 **2008**,可以猜测这就是 **vulnix** 用户的 **uid** 和 **gid**！！！

![image-20231023144349971](C:\Users\52252\Desktop\image-20231023144349971.png)

通过密码爆破SSH登录到靶机，发现 **vulnix** 用户的 **uid** 和 **gid **正是**2008**

![image-20231023144707918](D:\hack_notes\assets\image-20231023144707918.png)

```bash
sudo useradd -u 2008 vulnix

# 创建用户 uid 为 2008 的用户 vulnix
```

![image-20231023144920193](C:\Users\52252\Desktop\image-20231023144920193.png)

以该用户的身份成功进入靶机挂载来的目录

![image-20231023145328912](C:\Users\52252\Desktop\image-20231023145328912.png)

因为挂载来的是用户家目录，所以可以进行SSH污染，获取 **vulnix** 用户的权限。

![image-20231023145529497](C:\Users\52252\Desktop\image-20231023145529497.png)

成功写入 **authorized_keys** 文件	

![image-20231023145729550](D:\hack_notes\assets\image-20231023145729550.png)

可是进行密钥登录失败，还是要输入密码

![image-20231023145837427](C:\Users\52252\Desktop\image-20231023145837427.png)

```bash
ssh vulnix@192.168.58.133 -vv

# -vv 详细输出 ssh 登录过程
```

发现报了不识别的签名算法

![image-20231023150102874](C:\Users\52252\Desktop\image-20231023150102874.png)

添加可接受的签名算法 **ssh-rsa** 成功进行密钥文件登录 

![image-20231023150206616](C:\Users\52252\Desktop\image-20231023150206616.png)

查看 sudo 权限，发现可以免密编辑 **/etc/exports** 文件，可以更改 nfs 挂载目录和权限的配置文件

![image-20231023150237518](C:\Users\52252\Desktop\image-20231023150237518.png)

更改为 挂载靶机的根目录，且以 **root** 身份登录，不会限制 **root** 的权限

![image-20231023150304897](C:\Users\52252\Desktop\image-20231023150304897.png)

因为没有重启 nfs 的权限，只能重启靶机了

![image-20231023150636610](C:\Users\52252\Desktop\image-20231023150636610.png)

再次到靶机 **root** 目录，再次进行 SSH 污染，成功获得靶机系统权限！！！ 

![image-20231023150905445](C:\Users\52252\Desktop\image-20231023150905445.png)