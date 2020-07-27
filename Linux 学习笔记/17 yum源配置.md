# CentOS yum源配置

## 配置位置

/etc/yum.repos.d

## 配置步骤

1. **备份原来的yum源** 

2.  下载阿里的源 wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo  

3. 添加EPEL源

4.  wget -P /etc/yum.repos.d/ http://mirrors.aliyun.com/repo/epel-7.repo  

5.  **清理缓存并生成新的缓存** 

6. ```
   sudo yum clean all 
   sudo yum makecache 
   ```

