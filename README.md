支持linux或者是cygwin。windows用户可以考虑安装cygwin


0.首先在你的Github上建立一个名字为root的项目。确保本机上安装了openssl和tar

1.在你本机上建立一个文件夹，假设叫some_dir吧，把GithubHub里面的那个github.sh 给复制进来。

2.然后在some_dir里git clone你在Github上的root项目，这样就会在some_dir中出现一个root文件夹。

3.打开终端，运行‘github.sh init’。这个命令会在当前文件夹中创建加密用的密钥和一个名字为leaf的文件夹。

4.进入leaf文件夹，用'git init --bare'新建一个本地的裸git库，假设名字叫secret。

5.随便换到其他一个目录下，git clone path/some_dir/leaf/secret，就建立了裸库的工作目录了，然后在这个目录下像平常一样做一些修改，提交，推送。这时的推送（push）是将更改的内容push到本地leaf/secret的裸git库中，如果要更新到github上，还需要下面一步。

6.调用github.sh push secret，其中secret是你想push到github上的库的名字，这个命令会把leaf文件件下的secret文件夹打包压缩，然后放到root文件夹下。然后在root下执行git add secret && git commit .. & git push。至此，你的内容就被加密存放到Github上root库下了。

7.当你想从Github上获取加密的文件时，请用github.sh pull secret。这个命令会将root的内容pull到本地，然后把root下的secret解密解压到leaf文件夹下成为secret文件夹，之后从本机的其他文件夹里继续pull就可以了。


关于密钥文件：

       加密和解密的文件时对应且不可重复生成的，所以这两个密钥文件可要好好保存，一旦丢失话，就不能对已经存上去的内容解密了。。。

当然，你如果想更换密钥话，请先保证leaf下都是最新的已解密git库，然后替换密钥文件，执行相应的github.sh命令就可以了。
