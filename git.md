
    # 初始化一个Git仓库
    git init 
    # 从github remote repository下载一个git repository到本地
    git clone 
    git add readme.txt / git add .
    git rm test.txt
    # -m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。
    git commit -m "add 3 files."

    #将本地repository更新到github网站，即remote repository
    git push -u origin master
    # 拉起github上更新
    git pull origin master

    git status
