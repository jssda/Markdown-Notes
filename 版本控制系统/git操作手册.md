# 常用操作

## 基本操作

1. 状态查看操作
   git status
   查看文件暂存状态

2. 添加操作
   git add [file name]
   从工作区添加文件到暂存区

3. 提交文件
   git commit -m “commit message” [file name]
   提交文件到本地仓库

   > ![提交文件总结](Untitled.assets/提交总结.png)

4. 查看历史记录操作
   全部显示时候: git log

   > ![1548564347773](Untitled.assets/1548564347773.png)

   多屏显示控制方式: 空格向下翻页, b向上翻页, q退出

   git log –pretty=oneline 一行中显示

   > ![1548564434738](Untitled.assets/1548564434738.png)

   git log –oneline

   > ![1548564502417](Untitled.assets/1548564502417.png)

   git reflog

   > ![1548564567992](Untitled.assets/1548564567992.png)

   HEAD@{number} number是向前移动需要的步数

5. 前进后退
   基于索引: 
   git reset –hart [局部索引值]

   基于^号后退
   git reset –hart HEAD^ 一个抑或符号退一步

   基于~跳转
   git reset –hart HEAD~n 先后跳转n步

6. reset 三个参数对比

   --soft 仅仅是在本地库移动HEAD指针

   --mixed 在本地库移动HEAD指针 同时 重置暂存区

   --hard 在本地库移动HEAD指针 重置暂存区 重置工作区

7. 删除文件并找回
   删除文件之后, 通过向前先后移动进行文件找回

8. 比较文件差异

   git diff [文件名] : 将工作区文件与暂存区文件进行比较

   git diff [版本库中的版本] 文件名 : 将工作区中的文件与版本库中的相比较

   git diff HEAD : 比较所有文件

## 分支操作

1. 什么是分支

   同时推进功能

   > ![1548600675846](Untitled.assets/1548600675846.png)

2. 分支有什么好处呢

   A. 同时并行推进多个功能的开发

   B. 各个分支在开发过程中, 如果某一个分支开发失败, 对主干分支没有任何影响, 直接删除即可

3. 分支的操作

   1. 创建分支

      git branch [分支名]

   2. 查看分支

      git branch -v

   3. 切换分支

      git checkout [分支名]

   4. 合并分支

      第一步, 切换到接受修改的分支上(被合并 , 增加新内容)

      第二步, git merge [分支名] 把需要合并的分支, 合并到所在分支

   5. 解决冲突

      > 1. 冲突的表现
      >
      > ![1548601668451](Untitled.assets/1548601668451.png)
      >
      > 2. 冲突的解决
      >
      >    ![1548601899877](Untitled.assets/1548601899877.png)
