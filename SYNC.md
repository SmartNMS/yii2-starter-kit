# Yii 2 Starter Kit Extra


##SYNC A FORK

### Configuring a remote for a fork
To sync changes you make in a fork with the original repository, you must configure a remote that points to the upstream repository in Git.
(https://help.github.com/articles/configuring-a-remote-for-a-fork/#platform-linux)

Open Terminal.

List the current configured remote repository for your fork.
```
git remote -v
origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin  https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
```

Specify a new remote upstream repository that will be synced with the fork.

git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git
```
git remote add upstream https://github.com/trntv/yii2-starter-kit.git
```

Verify the new upstream repository you've specified for your fork.
```
git remote -v
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (fetch)
origin    https://github.com/YOUR_USERNAME/YOUR_FORK.git (push)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (fetch)
upstream  https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITORY.git (push)
```

### Syncing a fork
Sync a fork of a repository to keep it up-to-date with the upstream repository.

Before you can sync your fork with an upstream repository, you must configure a remote that points to the upstream repository in Git.
(https://help.github.com/articles/syncing-a-fork/#platform-linux)

1. Open Terminal.
2. Change the current working directory to your local project.
3. Fetch the branches and their respective commits from the upstream repository. Commits to master will be stored in a local branch, upstream/master.
```
git fetch upstream
```

4. Check out your fork's local master branch.
```
git checkout master
```

5. Merge the changes from upstream/master into your local master branch. This brings your fork's master branch into sync with the upstream repository, without losing your local changes.
```
git merge upstream/master
```

If your local branch didn't have any unique commits, Git will instead perform a "fast-forward":
```
git merge upstream/master
```

```
Tip: Syncing your fork only updates your local copy of the repository. To update your fork on GitHub, you must push your changes.
```


## Composer Tips

###1. 仅更新单个库
只想更新某个特定的库，不想更新它的所有依赖，很简单：
```
composer update foo/bar  
```
此外，这个技巧还可以用来解决“警告信息问题”。你一定见过这样的警告信息：
```
Warning: The lock file is not up to date with the latest changes in composer.json, you may be getting outdated dependencies, run update to update them.  
```
擦，哪里出问题了？别惊慌！如果你编辑了composer.json，你应该会看到这样的信息。比如，如果你增加或更新了细节信息，比如库的描述、作者、更多参数，甚至仅仅增加了一个空格，都会改变文件的md5sum。然后Composer就会警告你哈希值和composer.lock中记载的不同。

那么我们该怎么办呢？update命令可以更新lock文件，但是如果仅仅增加了一些描述，应该是不打算更新任何库。这种情况下，只需update nothing：
```
$ composer update nothing
Loading composer repositories with package information  
Updating dependencies  
Nothing to install or update  
Writing lock file  
Generating autoload files  
```
这样一来，Composer不会更新库，但是会更新composer.lock。注意nothing并不是update命令的关键字。只是没有nothing 这个包导致的结果。如果你输入foobar，结果也一样。

如果你用的Composer版本足够新，那么你可以直接使用--lock选项：
```
composer update --lock  
```
###2. 不编辑composer.json的情况下安装库
你可能会觉得每安装一个库都需要修改composer.json太麻烦，那么你可以直接使用require命令。
```
composer require "foo/bar:1.0.0"  
```
这个方法也可以用来快速地新开一个项目。init命令有--require选项，可以自动编写composer.json：（注意我们使用-n，这样就不用回答问题）
```
$ composer init --require=foo/bar:1.0.0 -n
$ cat composer.json
{
    "require": {
        "foo/bar": "1.0.0"
    }
}
```
###3. 派生很容易
初始化的时候，你试过create-project命令么？
```
composer create-project doctrine/orm path 2.2.0  
```
这会自动克隆仓库，并检出指定的版本。克隆库的时候用这个命令很方便，不需要搜寻原始的URI了。

###4. 考虑缓存，dist包优先
最近一年以来的Composer会自动存档你下载的dist包。默认设置下，dist包用于加了tag的版本，例如"symfony/symfony": "v2.1.4"，或者是通配符或版本区间，"2.1.*"或">=2.2,<2.3-dev"（如果你使用stable作为你的minimum-stability）。

dist包也可以用于诸如dev-master之类的分支，Github允许你下载某个git引用的压缩包。为了强制使用压缩包，而不是克隆源代码，你可以使用install和update的--prefer-dist选项。

下面是一个例子（我使用了--profile选项来显示执行时间）：
```
$ composer init --require="twig/twig:1.*" -n --profile
Memory usage: 3.94MB (peak: 4.08MB), time: 0s

$ composer install --profile
Loading composer repositories with package information  
Installing dependencies  
  - Installing twig/twig (v1.12.2)
    Downloading: 100%

Writing lock file  
Generating autoload files  
Memory usage: 10.13MB (peak: 12.65MB), time: 4.71s

$ rm -rf vendor

$ composer install --profile
Loading composer repositories with package information  
Installing dependencies from lock file  
  - Installing twig/twig (v1.12.2)
    Loading from cache

Generating autoload files  
Memory usage: 4.96MB (peak: 5.57MB), time: 0.45s  
```
这里，twig/twig:1.12.2的压缩包被保存在~/.composer/cache/files/twig/twig/1.12.2.0-v1.12.2.zip。重新安装包时直接使用。

###5. 若要修改，源代码优先
当你需要修改库的时候，克隆源代码就比下载包方便了。你可以使用--prefer-source来强制选择克隆源代码。
```
composer update symfony/yaml --prefer-source  
```
接下来你可以修改文件：
```
composer status -v  
You have changes in the following dependencies:  
/path/to/app/vendor/symfony/yaml/Symfony/Component/Yaml:
    M Dumper.php
```
当你试图更新一个修改过的库的时候，Composer会提醒你，询问是否放弃修改：
```
$ composer update
Loading composer repositories with package information  
Updating dependencies  
  - Updating symfony/symfony v2.2.0 (v2.2.0- => v2.2.0)
    The package has modified files:
    M Dumper.php
    Discard changes [y,n,v,s,?]?
```
###为生产环境作准备
最后提醒一下，在部署代码到生产环境的时候，别忘了优化一下自动加载：
```
composer dump-autoload --optimize  
```
安装包的时候可以同样使用--optimize-autoloader。不加这一选项，你可能会发现20%到25%的性能损失。

###两种方式启用本镜像服务

系统全局配置： 即将配置信息添加到 Composer 的全局配置文件 config.json 中。见“例1”

单个项目配置： 将配置信息添加到某个项目的 composer.json 文件中。见“例2”

例1：修改 composer 的全局配置文件（推荐方式）

打开命令行窗口（windows用户）或控制台（Linux、Mac 用户）并执行如下命令：
```
composer config -g repo.packagist composer https://packagist.phpcomposer.com
```
例2：修改当前项目的 composer.json 配置文件：

打开命令行窗口（windows用户）或控制台（Linux、Mac 用户），进入你的项目的根目录（也就是 composer.json 文件所在目录），执行如下命令：
```
composer config repo.packagist composer https://packagist.phpcomposer.com
```
上述命令将会在当前项目中的 composer.json 文件的末尾自动添加镜像的配置信息（你也可以自己手工添加）：
```
"repositories": {
    "packagist": {
        "type": "composer",
        "url": "https://packagist.phpcomposer.com"
    }
}
```

###NPM/Bower Dependency Manager for Composer
```
$ composer global require "fxp/composer-asset-plugin:~1.1"
```
