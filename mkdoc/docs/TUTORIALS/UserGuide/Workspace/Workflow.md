工作流（Workflow），即生物信息分析流程，Miracle Cloud的分析流程支持WDL描述语言，在工作流页面中，您可以选择自定义导入或者从工作流仓库导入，在工作流仓库中您可以找到用于批量分析对应数据集的生物信息分析流程。导入后，即可发起计算分析。<br>

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_0d9b5ace38f37a718783ecc6f7acef93.png)

### 自定义导入

选择自定义导入，输入工作流名称，git地址，git项目tag和token，主工作流路径，简短描述。完成后点击确定。<br>

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_b6dbcb201fe7fdcceb75bc7f26511ceb.png)
**Git** **tag**：在git中，标签用于指定某一次具体的提交，以gitlab为例，选择分支可以看到您所需要的当前标签
![](https://lf3-volc-editor.volccdn.com/obj/volcfe/sop-public/upload_2399a5a8875b54b621241adffe53300a)
**Git** **token：** gitlab在 `2021.8.13` 移除了密码认证的支持，它建议使用 `personal access token` 代替密码认证, 您可以在左侧设置中找到Access Token并复制到参数中
![](https://lf6-volc-editor.volccdn.com/obj/volcfe/sop-public/upload_cb0531ea8aadf4fe67cdc4da911724f9)
Git主工作流路径：您可以找到您所需要导入工作流的文件，并点击复制按钮，直接将当前文件的路径复制到输入参数中
![](https://lf6-volc-editor.volccdn.com/obj/volcfe/sop-public/upload_3e5fe7e45a0993828bb50b55ad42abc3)

### 从工作流仓库导入

该功能目前尚未开放，敬请期待。
### 配置工作流输入

要在Miracle Cloud上运行工作流，您需要指定所有必需的工作流输入变量。

1. 选择实体数据模型，并选择数据
**CallCaching：** 开启后会在之前运行的任务的缓存中搜索具有完全相同的命令和完全相同的输入的任务。 如果缓存命中，将使用前一个任务的结果而不是重新运行，从而节省时间和资源。
![alt](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_cb4f862464cd32d86dfee2dd3fbaa750.png)

2. 配置输入参数
点击输入参数选项卡，配置输入属性值。属性是与工作流中的输入变量相对应的整数、字符串或文件。您将通过选择填写设置表单中所有必需变量的属性字段来指定输入。您可以手动输入或者用this来指定数据模型中的参数，又或者使用JSON来进行属性字段的输入。
![](https://lf3-volc-editor.volccdn.com/obj/volcfe/sop-public/upload_b3683eeab4a45195d36d1f9569565ba2)

**一些常见的属性格式举例** 
整数 - `50` 
字符串 - `string`
数组（用逗号隔开） - `[string1,string2]`
文件路径 - `s3://analysis /sc94ra7leig43voqu4vlg /CramToBamFlow /5d915601-75c3-493b-9c7d-d136c9f9a296 /call-CramToBamTask /execution/script`


### 如何将输出文件元数据写入输入数据表

为什么要将输出写入数据表？写入数据表将生成的输出与输入数据文件相关联（输出文件与表中的输入文件一起写入），并有助于以对您有意义的方式组织输出。它还使使用数据进行下游分析变得容易。

1. 转到输出参数选项卡
2. 对于每个输出变量，单击 属性值 字段。您将看到一个下拉列表，其中列出了根实体数据表中存在的所有列
3. 选择现有列或输入新名称以将新数据列添加到表中。 如果原表格中没有此列则按照列名新加列；如果原表格有对应的列，则会将直接将新结果进行填充或覆盖
![alt](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_3be90dac0235a5560e8b7b915f6eec1e.png)
### 使用Json设置输入和输出参数

##### 什么是 JSON？

JSON 是基于文本的、人类可读的文件，它以 JavaScript 对象表示法 (JSON) 格式存储简单的数据结构和对象。开放标准 JSON 格式将 WDL 配置传输到运行 WDL 的服务器。JSON 文件可以使用文本编辑器进行编辑。

点击输入参数或者输出参数选项卡时，可以选择下载JSON文件，编辑后可以上传 JSON 文件以设置工作流输入或者输出。
![](https://lf6-volc-editor.volccdn.com/obj/volcfe/sop-public/upload_bcd4825074513002bae87fdbd3bf4e99)
您可以以相同的方式下载 JSON 文件，当您想在工作流配置中使用相同的输入时，它会派上用场。例如，如果您创建一个新配置，通常您每次都必须手动输入所有输入，即使此新配置将使用许多与现有配置相同的输入。现在，您可以将先前配置中的输入作为 JSON 下载并上传以填充新配置。
