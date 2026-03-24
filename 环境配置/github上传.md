1.新设备 / 初次使用

先绑定：

* 生成密钥：`ssh-keygen -t ed25519 -C "邮箱"`

* 获取公钥：`cat ~/.ssh/id_ed25519.pub`

* 将内容复制到 `Github Settings` $\rightarrow$ `SSH and GPG keys`


2.切换路径到要上传的文件夹，依次执行：

`git config --global user.name "用户名"`

`git config --global user.email "邮箱"`

`git branch -m main`

`git init`

`git remote add origin git@github.com:用户名/仓库名`

3.日常提交流程
`git add .`

`git commit -m "xyz"`

`git push origin main`（仅第一次用 后面直接 `git push`即可）