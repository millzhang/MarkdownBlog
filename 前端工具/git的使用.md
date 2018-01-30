
### 下载代码

```
git clone https://github.com/MillZhang/work-desk.git
```

### 添加文件

```
git commit -m '注释'
```

### 提交三步曲

```
git add
git pull
git commit -m ''
git push
```

### git status
     
 > 查询repo的状态.
 > git status -s: -s表示short, -s的输出标记会有两列,第一列是对staging区域而言,第二列是对working目录而言.


### 查看当前协议

```
git remote -v  
```

### https切换回ssh

```
git remote set-url origin git@github.com:MillZhang/work-desk.git
```

### 不输入用户名密码提交指南

1. https切换回ssh
2. 生成ssh-key

> 1.  打开git,git config --global user.name "xxx"
> 2.   ssh-keygen -t rsa -C "xxxx@qq.com"
> 3. 打开https://github.com/settings/keys 添加ssh-key


### 错误

1. `Changes not staged for commit`
 
 ```
 git commit -am "" or git commit -m 'msg' -a
 //-a 表示 all
 ```

2. git commit 进入 vim 如何操作?

> 按i然后写入，写入后按esc键退出编辑状态，然后输入:wq,回车即可

### 附录

1. [常用命令](http://www.cnblogs.com/mengdd/p/4153773.html)
2. [git emoji](https://github.com/liuchengxu/git-commit-emoji-cn)
3. [无权限提交](http://blog.csdn.net/u014343528/article/details/48787221)
4. [ssh-key的生成](http://blog.csdn.net/qq_34291777/article/details/55052201?locationNum=1&fps=1)
