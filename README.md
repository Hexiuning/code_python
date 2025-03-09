# lab record
## 1. 登录超算账号
### 在自己的电脑上输入cmd命令行
```cmd
ssh hxn22@211.86.151.113
```
第一次登陆会出现：
```cmd
The authenticity of host '211.86.151.113 (211.86.151.113)' can't be established.
ED25519 key fingerprint is SHA256:B909CGn6GvlVvim97mCMBFCb3pKpzkrQsMj6oyo6USQ.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? 
```
输入yes

出现：
<div style="text-align: center; display: flex; justify-content: center; gap: 10px;">
    <img src="figs/pic2.png" alt="输入密码" />
</div>

### 输入密码
输进去不会显示的，直接闭眼复制再按enter

出现：
<div style="text-align: center; display: flex; justify-content: center; gap: 10px;">
    <img src="figs/pic1.png" alt="输入密码" />
</div>

### 生成验证码
 按照：[https://scc.ustc.edu.cn/2018/0926/c409a339006/page.html](https://scc.ustc.edu.cn/2018/0926/c409a339006/page.html)生成验证码

### 输入验证码
输进去不会显示的，直接闭眼复制再按enter

### 登陆成功
出现：
<div style="text-align: center; display: flex; justify-content: center; gap: 10px;">
    <img src="figs/pic3.png" alt="登陆成功页面" />
</div>
代表登陆成功

## 2. 安装anaconda
## 3. 安装jupyterlab
## 4. 安装python
## 5. 安装r
## 6. 安装termius
## 7. 使用方法
### 7.1 链接jupyterlab
#### 登录超算账号
详见1.
#### 查看自己节点
输入：
```cmd
squeue -u hxn22 # squeue -u 你的账号名字
```
输出：
<div style="text-align: center; display: flex; justify-content: center; gap: 10px;">
    <img src="figs/pic4.png" alt="查看任务" />
</div>
最后一列是节点的名字，等会儿要用

#### 如果显示你没有节点，那么你要申请节点
##### 进入workspace 文件夹
输入：
```cmd
cd workspace
```
此时显示：
<div style="text-align: center; display: flex; justify-content: center; gap: 10px;">
    <img src="figs/pic5.png" alt="进入workspace文件夹" />
</div>
表示成功进入

##### 查看文件夹里有哪些文件
输入：

```cmd
ls
```

显示有哪些文件：
<div style="text-align: center; display: flex; justify-content: center; gap: 10px;">
    <img src="figs/pic8.png" alt="查看有哪些文件" />
</div>

##### 运行其中一个会帮你获取节点的slurm文件
输入：

```cmd
sbatch ju.slurm # sbatch 文件名
```
想要CPU节点就用ju.slurm，要GPU节点就用gpuju.slurm

此时显示：
<div style="text-align: center; display: flex; justify-content: center; gap: 10px;">
    <img src="figs/pic6.png" alt="运行slurm文件" />
</div>
表示在运行了

##### 检查是否成功获取了节点
再次进行上面“查看自己节点”的步骤，看看是不是有了节点
如果显示一个节点名字叫(Priority)：
<div style="text-align: center; display: flex; justify-content: center; gap: 10px;">
    <img src="figs/pic7.png" alt="还在申请节点" />
</div>
说明还在申请节点，再等一会儿就可以了

#### 建立远程连接
打开自己电脑的cmd窗口（打开显示C:\Users\Jingsong He才是自己电脑的cmd窗口，登陆了超算账号的就不是自己电脑的cmd窗口了）

输入：
```cmd
ssh -p 22 -N -f -L localhost:9000:fnode08:8889 hxn22@211.86.151.113 #其中fnode08要改成你申请到的节点的名字
```
你会被要求在输入一遍密码和验证码，之后就不会有任何显示了

#### 打开jupyter
在自己的浏览器里输入网址：[http://localhost:9000/](http://localhost:9000/)
- 登录成功后关掉自己的cmd窗口不会影响。
- **从此以后，如果你不改变计算节点和本地端口（localhost）就不用再执行以上任何步骤，直接打开网址即可进入jupyter。**
#### 打开不成功怎么办
##### 登陆了别人的账号/输入你的密码显示invalid credentials
- 请确定密码没记错
- 回来检查自己的slurm文件（在workspace文件夹里，GPU和CPU各对应一个，你用哪个就检查哪个对应的文件），是不是在某些地方写了别人的号
##### 完全加载不出来
输入：
```cmd
cd ~/workspace/logs
```
进入logs文件夹

输入：
```cmd
more jupyter_gpu.err
```
用GPU节点的是jupyter_gpu.err，CPU节点的是jupyter_fat.err
如果输出中告诉你： 端口 8889 已经被站用, 请尝试其他端口.（如下图）
<div style="text-align: center; display: flex; justify-content: center; gap: 10px;">
    <img src="figs/pic9.png" alt="端口被占用" />
</div>
那么看一下被转移到了哪个端口，比如图中就是被转移到了8890端口，记住新的端口号

重新进行“建立远程连接”操作，把节点名字后面的哪个号码从8889改成一个刚刚记住的新号码

#### 关掉节点
先查看自己的任务：
<div style="text-align: center; display: flex; justify-content: center; gap: 10px;">
    <img src="figs/pic7.png" alt="还在申请节点" />
</div>
第一列是任务id，记住你想要关掉的任务的id

输入：

```cmd
scancel 394403 #这串数字改为你要关掉的任务的id
```
### 7.2 上传文件
#### 法一：直接从自己的电脑上传文件
在自己的电脑cmd窗口输入：
```cmd
cd
```
进入该文件所在的文件夹
再输入：
```cmd
scp INSTINCT-main.zip hxn22@211.86.151.113:/home/qukungroup/hxn22/workspace/CodeField #INSTINCT-main.zip改为你要上传的文件名；两处hxn22改为你的账号名；CodeField改成你想传入的文件夹
```



In [2]: passwd()
Enter password:
Verify password:
Out[2]: 'argon2:$argon2id$v=19$m=10240,t=10,p=8$kIh64pT4juK7/l6OSKJONg$8Zrm7zCwVAStfMkgepjF3VzZMLzoUGefggatpRiw1Aw'
