①首先在github上新建仓库，权限为public

②点击头像 $\rightarrow$ settings $\rightarrow$ Developer Settings $\rightarrow$ Personal access tokens $\rightarrow$ Tokens(classic) $\rightarrow$ Generate new token(classic) $\rightarrow$ 随便填个名字，把下面的repo部分全勾上，然后最下方点击生成 $\rightarrow$ 立刻先复制那串字符，不然的话退出后就找不到了

③下载PicGo

④打开PicGo $\rightarrow$ 图床设置 $\rightarrow$ github $\rightarrow$ 新建，填入仓库名（你的用户名/仓库名），分支名（一般是main），Token（填刚刚复制的那串字符），存储路径可填可不填

如果你用代理的话，那么打开PicGo设置 $\rightarrow$ 设置代理和镜像地址 $\rightarrow$ 把你的端口号填进上传代理位置，一般是http://127.0.0.1:7890

实测如果找免费的cdn加速节点，在typora中会出现图片下载失败的情况

不开代理的情况下，有时也是可以访问到github图床的，上传也没问题

⑤打开typora $\rightarrow$ 文件 $\rightarrow$ 偏好设置 $\rightarrow$ 图像

插入图片时，选择上传图片，并勾选 “对本地位置的图片应用上述规则”， “对网络位置的图片应用上述规则”

上传服务设定，上传服务选择PicGo(app)，PicGo路径选择你的PicGo应用的路径

全部完成后，点验证图片上传选项，显示成功即可

![image-20260330095804156](https://raw.githubusercontent.com/Sanshi2006/ImagesResources/main/img/20260330095804209.png)

⑥如果你之前写的文章有很多本地路径的图片，你可以：

打开那篇文章，左上角 格式 $\rightarrow$ 图像 $\rightarrow$ 上传所有本地图片	