# Idea新装笔记

## 安装环境

1. 本地win10
2. 安装idea版本为2019.3
3. 有IntelliJ 账户, 同步过以前的设置

## 设置步骤

1. 右下角![image-20191205175904530](https://raw.githubusercontent.com/jssda/picbed/master/image-20191205175904530.png), 点击同步

2. 重启

3. 打开安装目录, 其中有个`idea64.exe.vmoptions`文件, 打开

   ```
   -Xms128m
   -Xmx750m
   -XX:ReservedCodeCacheSize=240m
   -XX:+UseConcMarkSweepGC
   -XX:SoftRefLRUPolicyMSPerMB=50
   -ea
   -XX:CICompilerCount=2
   -Dsun.io.useCanonPrefixCache=false
   -Djava.net.preferIPv4Stack=true
   -Djdk.http.auth.tunneling.disabledSchemes=""
   -XX:+HeapDumpOnOutOfMemoryError
   -XX:-OmitStackTraceInFastThrow
   -Djdk.attach.allowAttachSelf=true
   -Dkotlinx.coroutines.debug=off
   -Djdk.module.illegalAccess.silent=true
   ```

   关键的三个参数 

   第一个 -Xms 是最小启动内存参数 

   第二个 -Xmx 是最大运行内存参数

   第三个-XX:ReservedCodeCacheSize 保留代码占用的内存容量参数

   我的配置

   ```
   -Xms1024m
   -Xmx2048m
   -XX:ReservedCodeCacheSize=500m
   -XX:+UseConcMarkSweepGC
   -XX:SoftRefLRUPolicyMSPerMB=50
   -ea
   -XX:CICompilerCount=2
   -Dsun.io.useCanonPrefixCache=false
   -Djava.net.preferIPv4Stack=true
   -Djdk.http.auth.tunneling.disabledSchemes=""
   -XX:+HeapDumpOnOutOfMemoryError
   -XX:-OmitStackTraceInFastThrow
   -Djdk.attach.allowAttachSelf=true
   -Dkotlinx.coroutines.debug=off
   -Djdk.module.illegalAccess.silent=true
   ```

## 优化

### 智能提示不区分大小写

![image-20191205181717603](https://raw.githubusercontent.com/jssda/picbed/master/image-20191205181717603.png)

### 自动导包

![image-20191205181814728](https://raw.githubusercontent.com/jssda/picbed/master/image-20191205181814728.png)

### 鼠标滚轮调整编辑区大小

![image-20191205182016154](https://raw.githubusercontent.com/jssda/picbed/master/image-20191205182016154.png)

### 修改编译时期堆大小

![image-20191205182134269](https://raw.githubusercontent.com/jssda/picbed/master/image-20191205182134269.png)

### 设置自带的terminal为gitbash

![image-20191205182309012](https://raw.githubusercontent.com/jssda/picbed/master/image-20191205182309012.png)

### 设置方法间的分割线

![image-20191205182855198](https://raw.githubusercontent.com/jssda/picbed/master/image-20191205182855198.png)

### 隐藏.idea文件夹和.iml文件

![image-20191205183859943](https://raw.githubusercontent.com/jssda/picbed/master/image-20191205183859943.png)

### 修改类注释模板

![image-20191206110310757](https://raw.githubusercontent.com/jssda/picbed/master/image-20191206110310757.png)

