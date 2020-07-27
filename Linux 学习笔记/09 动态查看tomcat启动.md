### 动态查看tomcat启动(linux)

tail命令可以查看文件的末尾几行

-f参数 可以在源文件的末尾有改动的时候, 能够动态显示出来

所以, tomcat启动的时候, 想要动态查看日志, 可以使用这个命令

```sh
./startup.sh | tail -f ../logs/catalina.out
```

