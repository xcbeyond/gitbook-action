# Gitbook Action

基于 [ZanderZhao/gitbook-action](https://github.com/ZanderZhao/gitbook-action)，进行了功能的修改、增强。

利用 Github 的 Action 功能，实现 Gitbook 的自动化编译、部署，并发布到 gh-pages 分支。

## 1、特性

* 自动完成 gitbook 的编译，并自动发布到 gh-pages 分支。
* 解决 gitbook 发布后，所有文章的更新时间都被修改为统一更新时间。 （解决后，只更新当前修改文章的时间。）
* 解决 favicon.icon 不能正常显示的问题。

## 2、使用方法

### 2.1 添加 github action

在你 gitbook 仓库中，添加 `.github/workflows/gitbook-action.yml` 文件，内容如下：

```yaml
name: 'Gitbook Deploy Action'
on:
  push:
    branches:
      - main  # trigger branch
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Git Checkout
      uses: actions/checkout@v2
    - name: Gitbook Action
      uses: xcbeyond/gitbook-action@main
      with:
        token: ${{ secrets.PERSONAL_TOKEN }}  # remember add this in settings/secrets as following
        source_branch: main
        time_zone: Asia/Shanghai            # set time zone
        source_edit_time: true              # source time
        publish_commit_message: ${{ github.event.head_commit.message }}  # use last commit message
```

### 2.2 生成 Token，并添加 Secret

（1）在 [https://github.com/settings/tokens](https://github.com/settings/tokens) 中创建 token，并妥善保管。(如果之前已创建过，并保存有，则可重复使用。)

（2）在 `https://github.com/yourname/yourrepo/settings/secrets` 中 创建 secrets：

* Name: `PERSONAL_TOKEN`。（需与 `.github/workflows/gitbook-action.yml` 中配置的 token 保持一致）
* Value: 刚刚创建的 token 值。
