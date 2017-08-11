---
title: Hexo 快速开始
---
Welcome t Hexo! 这是我的第一篇自己写的blog, 希望以后有更多的文章发表到上面来！

## 快速开始

### 创建一篇博文

```bash
$ hexo new "My New Post"
```

更多信息：[Writing](https://hexo.io/docs/writing.html)

### 运行 hexo 服务

```bash
$ hexo server
```

更多信息：[Server](https://hexo.io/docs/server.html)

### 生成静态文件

``` bash
$ hexo generate
```

更多信息: [Generate](https://hexo.io/docs/generate.html)

### 发布到GitHub服务器上

```bash
$ hexo deploy
```

更多信息：[Deployment](https://hexo.io/docs/deployment.html)

## 第二章

### 第一节

啦啦啦啦.....

### 第二节

巴拉巴拉巴拉拉巴勒....

    protected Object getTargetRepository(RepositoryMetadata metadata) {
        Class<?> repositoryInterface = metadata.getRepositoryInterface();

        if (isBaseRepository(repositoryInterface)) {

            JpaEntityInformation<M, ID> entityInformation = getEntityInformation((Class<M>) metadata.getDomainType());
            BaseRepositoryImpl repository = new BaseRepositoryImpl<M, ID>(entityInformation, entityManager);

            return repository;
        }
        return super.getTargetRepository(metadata);
    }

