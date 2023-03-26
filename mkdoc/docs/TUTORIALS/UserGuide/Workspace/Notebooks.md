Miracle Cloud的交互式分析环境集成了Jupyter Notebook。Jupyter Notebook是一种可分享，可复制的分析。

Jupyter Notebook是一个开源分析环境，您可以在其中通过交互式分析和可视化效果实时了解研究数据。您可以导入数据——包括存储在云中的已处理基因组学、表型和转录组学数据——并使用 R 或 Python 中的自定义或预建库进行分析。
Jupyter Notebooks 环境可供新手使用，并具有可移植性和可重复性。Notebook以易于理解和分享的形式将分析方法和发现结合在一个地方。作为传统科学论文的逻辑演变，Jupyter Notebook极大地缩短了阅读分析完成方式和实际重现分析之间的路径。很难夸大这个概念的强大程度以及Notebook对计算科学中发现的可重用性和可重复性的影响。

### 创建新的Notebook

1. 点击新建Notebook，并输入名称和选择语言。目前支持的语言有Python和R语言
![](https://lf3-volc-editor.volccdn.com/obj/volcfe/sop-public/upload_aadc2e0f5939a6e55f54d2d64e52567c)

2. 点击右侧编辑按钮可以对notebook进行编辑
![](https://lf3-volc-editor.volccdn.com/obj/volcfe/sop-public/upload_744dd4ca1361561525a7ced651715ac7)

3. 在 notebook 中运行代码单元的方法有以下三种：
	1. 选择单元格并按键盘上的`Shift` +`Enter`（您的键盘可能会显示“return”而不是“enter”）。
		
	2. 单击菜单栏中的运行图标。
		
	3. 使用Cell 下拉菜单中的适当命令。
		

单元是笔记本的组成部分。每个单元格都有一个“类型”（Code/Markdown/Raw NBConvert/Heading），它决定了应用程序计算将如何解释单元格中的指令。

![](https://lf6-volc-editor.volccdn.com/obj/volcfe/sop-public/upload_b237dfee4891d954287c33700beec5dc)
<br>

### 代码单元如何运行？

当您运行代码单元时，Jupyter计算内核将读取代码，并将这些指令传递到运行Jupyter的实际操作系统（例如 Python、R），并检索结果以在笔记本中显示它们。 当命令运行时，命令的输出日志出现在代码单元的正下方。
如果单元格中的代码与内核的语言不匹配，则应用程序计算将返回错误。如果未指定输出，则通过注意单元格左侧方括号 \[ \] 中的数字，您将知道代码已成功执行。
<br>

### 如何知道代码是否执行

首次启动笔记本时，每个代码单元左侧的方括号为空 \[\]，表示这些单元在此会话期间尚未运行。括号 \[\*\] 中的星号表示单元正在运行。一旦命令被执行，星号将被一个整数代替，该整数表示自内核启动以来执行的命令数。您可以多次执行同一个单元格，或在一个单元格中执行多条命令。

![](https://lf3-volc-editor.volccdn.com/obj/volcfe/sop-public/upload_2ba01fa56b629df0acb7ff1c6bd16526)

如果您通过转到下拉菜单单元格>所有输出并选择“清除”来清除输出，则整数括号将再次被空括号替换。但是，如果您重新启动内核，整数计数只会重置为零。
<br>

### 如何在Markdown单元格中编辑内容

Notebook不仅能进行python或者R语言代码运行，单元格也支持编辑Markdown格式的内容，如下图所示。然后请双击要编辑的单元格。

Markdown 是一种轻量级的纯文本格式化语言。它用简洁的语法代替排版，而不像一般我们用的文字处理软件 *Word* 或 *Pages* 有大量的排版、字体设置。以下为一些语法举例
**在** **Markdown** **中，如果一段文字被定义为标题，只要在这段文字前加 # 号即可。** 
如：# 一级标题
**如果你需要引用一小段别处的句子，那么就要用引用的格式。只需要在文本前加入 > 这种尖括号（大于号）即可。** 
**插入链接与插入图片的语法很像，区别在一个 !号**
图片为：!\[\](){ImgCap}{/ImgCap}
链接为：\[\]()

![](https://lf6-volc-editor.volccdn.com/obj/volcfe/sop-public/upload_759de5f0e676521d41db3e040c848bf2)

#### Notebook使用自定义镜像

Miracle Cloud支持您以自己的镜像作为Notebook的启动环境。这是一个分步指南，用于：

- 构建和发布自定义Docker镜像
	
- 使用修改后的Docker镜像在Bio-OS上运行Notebook
	

在您使用Notebook自定义镜像功能前，以下三点请知悉
注意

- 自定义镜像必须基于Miracle Cloud提供的基础镜像，否则可能会导致Notebook启动失败
	
- 自定义镜像会拉取您设置的镜像，这个过程中可能会产生流量
	
- 过大的镜像可能会导致容器镜像启动超时，不建议您使用15G以上的镜像
	

1. **下载基础镜像****Dockerfile** **[#](https://vsop-online.bytedance.net/doc/manage/detail/6653/detail/?DocumentID=196839#%E4%B8%8B%E8%BD%BD%E5%9F%BA%E7%A1%80%E9%95%9C%E5%83%8Fdockerfile)**
	

首先您需要下载基础镜像的Dockerfile，您有以下两种方式进行下载

- 您可以直接访问： https://gitee.com/bio2s/bioos-baseimage，并点击克隆/下载
	

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_b381ffdbe56d2ca9ad35bb085aec03fa.png)

- 您也可以通过 `git clone` `https://gitee.com/bio2s/bioos-baseimage.git` 直接克隆到本地
	

现在您可以在本地文件夹中找到刚下载的bioos-baseimage文件夹，这个文件夹中的Dockerfile就是我们需要修改的基础镜像Dockerfile。

2. **修改****Dockerfile** **[#](https://vsop-online.bytedance.net/doc/manage/detail/6653/detail/?DocumentID=196839#%E4%BF%AE%E6%94%B9dockerfile)**
	

2.1 打开Dockerfile之后，文件中`#install your own package here`这句描述下新增一行，举例比如 `RUN pip install -i` `https://pypi.tuna.tsinghua.edu.cn/simple` `gsutil` 这句代码会为您在基础镜像的基础上安装gsutil这个工具包，修改完成后点击保存。
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_392f4495dbe7e2618b8d57873fbcc01f.png)
<br>

3. **设置镜像push地址** **[#](https://vsop-online.bytedance.net/doc/manage/detail/6653/detail/?DocumentID=196839#%E8%AE%BE%E7%BD%AE%E9%95%9C%E5%83%8Fpush%E5%9C%B0%E5%9D%80)**
	

本小节将以广实vestack镜像仓库作为镜像推送地址，也建议您使用vestack镜像仓库作为自定义镜像的存储地址。目前在vestack中已经创建了命名空间为notebook以及空间下的制品仓库customimage 您可以将镜像推送到以下地址： registry-vpc.miracle.ac.cn/notebook/customimage

4. **构建并推送自定义镜像** **[#](https://vsop-online.bytedance.net/doc/manage/detail/6653/detail/?DocumentID=196839#%E6%9E%84%E5%BB%BA%E5%B9%B6%E6%8E%A8%E9%80%81%E8%87%AA%E5%AE%9A%E4%B9%89%E9%95%9C%E5%83%8F)**
	

您必须从包含dockerfile的目录中运行您的命令。Docker只能识别名为Dockerfile的dockerfile（没有扩展名），因此您可以在计算机上拥有任意数量的dockerfile，但它们需要位于单独的文件夹中，每个文件夹只有一个dockerfile。当您执行Docker build命令时，它将在您在终端中查看的目录中查找dockerfile。该目录中必须有一个名为Dockerfile的文件，否则该命令将失败。
4.1 使用以下命令进入 bioos-baseimage目录：

```bash
cd bioos-baseimage
bash
```

4.2 执行一下Docker build命令(注意不要漏掉最后的".", "." 代表使用当前路径的dockerfile):

```bash
Docker build -t RESPOSITORY_NAME/DOCKER_IMAGE_NAME:TAG_NAME .
bash
```

![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_e297f8dc3ef5a790e24280d0a351d814.png)
build的过程可能长达10多分钟 4.3 登录镜像仓库

```sql
sudo docker login registry-vpc.miracle.ac.cn -u biocontainer@88 -p Pwd123456!
sql
```

4.4 执行下面的推送命令将您的自定义镜像上传到您的镜像仓库:

```bash
Docker push RESPOSITORY_NAME/DOCKER_IMAGE_NAME:TAG_NAME
bash
```

这个过程可能也会长达10分钟时间，取决于网络和您的镜像大小
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_da5eb7aabed1d4c9d54c2c17a6263a7c.png)
<br>

5. **Notebook运行配置选择自定义镜像** **[#](https://vsop-online.bytedance.net/doc/manage/detail/6653/detail/?DocumentID=196839#notebook%E8%BF%90%E8%A1%8C%E9%85%8D%E7%BD%AE%E9%80%89%E6%8B%A9%E8%87%AA%E5%AE%9A%E4%B9%89%E9%95%9C%E5%83%8F)**
	

5.1 在Notebook列表页面点击【运行资源配置】，在运行资源配置中选择镜像来源【自定义】，并复制您的镜像地址
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_55b382c4b4d46347ec5f4374780f0d97.png)
点击【更新环境】

6. **启动Notebook并等待容器启动完成** **[#](https://vsop-online.bytedance.net/doc/manage/detail/6653/detail/?DocumentID=196839#%E5%90%AF%E5%8A%A8notebook%E5%B9%B6%E7%AD%89%E5%BE%85%E5%AE%B9%E5%99%A8%E5%90%AF%E5%8A%A8%E5%AE%8C%E6%88%90)**
	

选择一个Notebook，并点击【编辑】，Notebook的启动过程会包含镜像的拉取，因此时间可能会比较长，请耐心等待。
