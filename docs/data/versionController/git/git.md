### 常用命令
 - 从当前分支 创建新分支
    ```
    git checkout name
    ```
 - 从当前分支 创建新远程分支
    ```
    git chekcout -b name
    ```
 - 修改远程仓库地址  
    一天把github的项目 迁到gitee的私人库
    
    ```
    git remote set-url origin https://github.com/USERNAME/REPOSITORY.git
    ```
    
 - 查看当前分支的远程分支地址
    ```
    git remote -v
    ```
 
 - 把本地库添加到远程库
    ```
    git remote add origin https://github.com/USERNAME/REPOSITORY.git
    ```