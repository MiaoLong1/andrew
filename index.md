# Rancher 目录

Rancher提供了应用模板的目录功能，可以让部署复杂的应用堆栈变得更简单。通过点击**目录**选项卡，你可以看到[启用目录]({{site.baseurl}}/rancher/configuration/settings/#catalog)后所有可用的模板。目录**库**包含了[Rancher认证目录](https://github.com/rancher/rancher-catalog)和[社区目录](https://github.com/rancher/community-catalog)。Rancher将只对属于库中的认证目录的模板予以维护支持。

Rancher[管理员]({{site.baseurl}}/rancher/configuration/access-control/#admin)可以有添加和删除目录的权利。已添加的目录可以通过**管理** ->**设置**。填写目录名称和URL路径就可以很简单的添加一个目录。URL路径需包含`git clone` [可隐藏](https://git-scm.com/docs/git-clone#_git_urls_a_id_urls_a)。一旦你添加了一个目录条目，在Rancher目录中它会立刻显示和可用。

如果你是在位于代理的后端来运行Rancher Server的情况，为了Rancher目录生效那么需要[附加特定的环境变量来启动Rancher]({{site.baseurl}}/rancher/installing-rancher/installing-server/#http-proxy)。

### 发起模板

搜索或者使用目录分类过滤来选择你想要的模板。如果你找到相应的模板，点击 **发起**。另外输入填充必要的信息。

1. 在默认情况下，选定的是最新 **版本**，你可以选择一个老的版本如果需要的话。
2. 为堆栈选定一个想要的 **堆栈** 名称以及相应的 **描述**。
3. 输入填充 **配置选项** ，即针对选定模板的一些特定问题。
4. 点击 **创建** 按钮将生成以该模板为基准的堆栈。在堆栈创建之前，你可以通过展开 **预览** 查看`docker-compose.yml` 和 `rancher-compose.yml`这两个文件的内容，它们用来形成对应的堆栈。

当你点击 **创建** 按钮之后，堆栈将被立即创建，但是服务并不会启动。点击堆栈的下拉菜单中的 **启动服务**，以启动堆栈中所有的服务。

### 升级模板

Rancher有个好处就是，如果上传一个新的模板版本到了目录，Rancher会通告你有一个新的版本可以用来升级。当你点击 **可用升级** ，你可以选择你的想要升级的版本。版本选择完成后，在点击 **保存** 按钮之前，还需要检查相应的 **配置选项**。

当所有的服务完成升级，对应的堆栈和服务将会处于 **已升级** 状态。如果你对升级操作较为满意，还需要执行最后一个步骤就是，在堆栈的下拉菜单中点击 **完成升级** 以确认升级。**备注：一旦确认完成升级后，你将无法恢复到旧的版本。**

#### 回滚

如果在升级过程中有遇到问题需要恢复到之前的版本，你可以在堆栈的下来菜单中选择执行 **回滚** 操作。

## 创建私有目录
---

Rancher目录服务要求私有目录以特定格式来组织，以便于目录服务能够将其识别并提交给Rancher。

### 目录结构

Ranher中目录模板的显示方式取决于你处于何种集群管理方式的环境。

* _Cattle_ 环境：UI呈现的接入点是来自于 `templates` 文件夹
* _Kubernetes_ 环境: UI呈现的接入点是来自于 `kubernetes-templates` 文件夹
* _Swarm_ 环境: UI呈现的接入点是来自于 `swarm-templates` 文件夹

```
-- templates OR kubernetes-templates OR swarm-templates
  |-- cloudflare
  |   |-- 0
  |   |   |-- docker-compose.yml
  |   |   |-- rancher-compose.yml
  |   |-- 1
  |   |   |-- docker-compose.yml
  |   |   |-- rancher-compose.yml
  |   |-- catalogIcon-cloudflare.svg
  |   |-- config.yml
...
```
<br>

在主目录中，你需要一个`templates` 文件夹。这个`templates` 文件夹将包含你创建的每个目录接入点的子文件夹。我们强烈建议你为每个目录接入点简单地设置模板名字，与该子文件夹名一致。

在目录接入点对应的文件夹(例如 `cloudflare`)，在其中将包含所有版本的
