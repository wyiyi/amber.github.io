# [Hexo博客搭建](https://hexo.io/zh-cn/docs/)

## 环境准备
1、设置淘宝镜像
```
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```

2、使用 npm 安装 hexo
```
$ npm install -g hexo-cli
$ cnpm install hexo --save
```

3、 查看 hexo 的版本
```
$ hexo -v
```

4、初始化文件夹 hexo（注意文件夹需要时空文件夹）
```
$ hexo init hexo
```

5、在 hexo 文件夹下
```
$ npm install
```

## 相关命令
```
# 启动命令
$ hexo s -g   

# 打包上传依次执行以下命令,启动即可
$ hexo clean 
$ hexo generate 
$ hexo deploy

# 安装部署命令deploy-git, 用命令部署到GitHub
$ cnpm install hexo-deployer-git  --save
```

直接访问：http://localhost:4000 有页面即搭建成功

## hexo 托管到 Github
- 仓库的名字以xxx.github.io结尾，且使用public

- 配置 `_config.yml` 文件
```
deploy: 
   type: git 
   repo: https://github.com/YourgithubName/YourgithubName.github.io.git 
   branch: master
```

- 在 thems 中clone 想要的主题 `https://hexo.io/themes/`，在 `_config.yml` 的thems 修改对应值
```
git clone xxx
```

`node_modules： 依赖包`

`public：存放生成的页面`

`scaffolds：生成文章的一些模板`

`source：用来存放你的文章`

`themes：放下下载的主题`

`_config.yml： 博客的核心配置文件（设置主体、标题等属性）`

### 博客地址：https://wyiyi.github.io/amber/




