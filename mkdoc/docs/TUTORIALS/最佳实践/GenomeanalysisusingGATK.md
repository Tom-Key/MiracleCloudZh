## 使用GATK进行基因组分析

本章节介绍了如何使用Genome Analysis Toolkit(GATK)在 Miracle Cloud 上运行基因组分析工作流。本章节中使用的工具来自 GATK ，首先将Cram格式的序列转换为Bam格式，然后通过GATK工具包进行变异分析，得到变异中间结果文件gvcf文件。该工作流使用 WDL编写，并在 Cromwell 引擎上运行。

:::tip
**GATK**是*Genome Analysis Toolkit*的缩写，是用来处理高通量测序数据的一套软件、工具包。最初，GATK被设计用来分析人类基因组和外显子，主要用来寻找SNP和indel。后来，GATK的功能越来越丰富，增加了short variant calling、Copy number variation（CNV）和结构变异（SV）等新功能。同时，GATK也越来越广泛地应用于其他物种的数据分析中。现在，GATK已经成为了基因组和RNA-seq分析过程中寻找变异的行业标准。
:::

### 第一部分：运行Cram2Bam数据格式转换工作流

您可以通过这部分了解数据的上传以及成功运行工作流的方法。 该工作流程是一个文件格式转换流程，用于将基因组文件从一种格式 (CRAM) 转换为另一种格式 (BAM) 以进行下游分析。
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_ef05c8dcd02105510bd2ee283b21d996.png)

1. #### 登录Miracle Cloud
使用账号密码登录Miracle Cloud, 账号密码可联系平台管理员获取。
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_5bf5c8c838c692ee8d4a5fbb4c730d51.png)
在页面上方有概览、Workspace和Digger三个菜单，其中Digger是可公开的Workspace，通常您可以选择将编辑完成的完整Workspace发布到Digger中，您也可以将Digger中的Workspace复制回Workspace进行重新编辑。选择Workspace，点击新建Workspace，输入名称：GATK-Workflow 以及简短描述。完成创建Workspace之后我们就将开始对于Workspace的构建。
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_b6054b8c6c9ffb21a13ee67d37019ebb.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_978a3c68ac6ecca3b0765a1a3d0cf9ef.png)

2. #### 上传数据
通常上传所需要分析的数据是开启工作流分析的第一步，在这个实践中，我们需要先将所用到的数据集传到存储桶中，并制作对应的数据模型；您有两种制作和获得分析所用的数据模型：
- 可以根据以下链接先下载样本数据集文件和参考数据集文件至本地，然后再上传至Workspace对应的存储桶中，最后可以根据文件对应的S3路径制作数据模型
- 可以直接下载此最佳实践对应的数据模型sample.csv文件，选用此方式可以直接从step d开始
```bash
#本教程中用到的数据集下载链接：
#CSV数据模型:
https://tutorials-data.tos-cn-guangzhou.volces.com/cram-to-bam/sample.csv
#样本数据集文件：
https://tutorials-data.tos-cn-guangzhou.volces.com/cram-to-bam/sample-data/NA12878.cram
https://tutorials-data.tos-cn-guangzhou.volces.com/cram-to-bam/sample-data/my-sample-data.cram
#参考数据集文件：
https://tutorials-data.tos-cn-guangzhou.volces.com/cram-to-bam/reference-data/Homo\_sapiens\_assembly38.dict
https://tutorials-data.tos-cn-guangzhou.volces.com/cram-to-bam/reference-data/Homo\_sapiens\_assembly38.fasta
https://tutorials-data.tos-cn-guangzhou.volces.com/cram-to-bam/reference-data/Homo\_sapiens\_assembly38.fasta.fai
```

点击平台左侧边栏**数据-文件列表**，进入文件数据管理页面，通过点击上传文件，可以根据引导上传本地数据文件至此Workspae对应的对象存储桶中。![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_df7377f3e58f3fdefa6d040ac5d8027b.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_e9ab9f548d359f57ab085433e2c81218.png)
数据上传完成后，下载csv模版并组织数据。点击左侧数据栏，点击**实体数据模型**列表右侧加号，出现上传数据模型选项框，点击下载数据模型模版。
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_70cc5fbed2ec6f06048fd40e9bc2b754.png)
其中sample\_id列为样本的唯一ID，之后的所有列皆为属性列，在属性列中，属性可以是具体的整数，字符串，当然更多的时候属性列会关联到云中的数据文件，在这里我们使用数据模型关联已经上传的数据集文件。
	a. 首先需要进入到**数据-文件列表**； 
	b. 点击所需文件后的链接标签，会显示此文件对应的URL路径和S3路径；
	c. 点击S3路径后的“复制”按钮，并将链接粘贴至对应的属性列中
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_87e0774121a5ec756e2582069c024c13.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_da5c10cae6ef65cdcf032bdfefe196b0.png)
	d.上传实体数据模型：点击实体数据模型右侧的"+"，出现上传数据模型选项框，上传编辑好的csv文件。
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_bea0206d7cbb6bb6a96fd33ce4609cc7.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_07816eb000c8714a7789c860744657ee.png)

3. #### 导入工作流
Miracle Cloud使用工作流描述语言 (WDL) 中的工作流来处理基因组学数据。所有接收映射读取数据的 GATK 工具都期望 BAM 文件作为主要格式。有些支持CRAM格式，但我们观察到直接从 CRAM 文件工作时会出现性能问题，因此，我们首先将 CRAM 转换为 BAM。

:::tip
SAM、BAM 和 CRAM 都是原始 SAM 格式的不同形式，这些格式是为保存对齐（或更准确地说，*映射*）的高通量测序数据而定义的。SAM 代表序列比对图，并在[此处](http://samtools.github.io/hts-specs)的标准规范中进行了描述。BAM 和 CRAM 都是 SAM 的压缩形式；BAM（用于二进制对齐图）是一种无损压缩，而 CRAM 的范围可以从无损到有损，具体取决于您想要实现多少压缩（*实际上*最多）。BAM 和 CRAM 拥有与其 SAM 等价物相同的信息，结构方式相同；它们之间的不同之处在于文件本身的编码方式。
:::
接下来我们需要将Cram-to-Bam 工作流导入到Workspace中，
	a. 点击导入工作流
	b. 选择自定义导入
	c. 输入对应的输入项（*输入项的填写方法可查看用户指南中的自定义导入*）
		- Git address： [https://gitee.com/joy\_lee/seq-format-conversion01](https://gitee.com/joy_lee/seq-format-conversion01)
		- Tag: v0.47
		- Main path：CramToBam.wdl
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_19282b30883efcecc1ed242a8022de98.png)

这样就完成了我们所需要的Cram to Bam的转换工作流导入。

4. #### 运行工作流
	
刚才我们已经完成了工作流的导入，那么接下来我们将运行导入的工作流。
	a. 运行环境关联：点击**环境管理**，选择**工作流——关联集群**，从候选集群列表中选择集群，点击关联。![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_c694e576c2d426c506526b7f21aee1ec.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_e0bcb694962037971a22242e30119b7a.png)
	b. 选择我们刚才导入的Cram-to-Bam工作流
	c. 在这里我们需要配置运行选项和运行参数，在运行选项中，选择刚才我们第一步中所上传的数据实体，并指定实体数据。![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_2ebc45d1ba9316e34894039e21def8b1.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_c495d00c54369999060a69d67dbc1bf8.png)
	d. 配置输入参数，选中输入参数选项卡，并按照如下参数进行输入，其中"this"表示此次分析所用到的csv文件，"this"后面的内容为对应csv中的列名称；当输入参数较多时，可以使用上传JSON文件的方式进行快速地导入。![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_8be5120d48939a49bf74d65c40c41e9e.png)
	e. 配置输出参数，使用this.bai作为bai文件的输出属性列，this.bam作为bam文件的属性列
:::tip
通过this.列名，指定输出的结果输出到所选择的数据表的指定列中；如果原表格中没有此列则按照列名新加列；如果原表格有对应的列，则会将直接将新结果进行填充或覆盖
:::
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_949488e50beff75c79b1709417f5daea.png)
	f. 点击右上角开始分析，此时工作流会通过Cromwell工作流引擎进行分析，结果可在分析历史中进行查看
### 第二部分：针对上一步输出的BAM通过GATK进行分析

这个章节中我们需要使用第一部分所输出的Bam以及Bai文件通过GATK工作流进行进一步分析，最终输出所需要的gvcf文件。
1. #### 导入GATK工作流
我们还需要导入针对Bam序列进行进行变异分析，得到变异中间结果文件gvcf文件的工作流。点击左侧**工作流**\>**导入工作流，** 填写对应的输入信息：
	a. Git address：https://gitee.com/joy\_lee/gatk-pipeline
	b. Tag: v0.34	
	c. Main path：Hello-GATK.wdl
	
2. #### 运行GATK工作流
	
	a. 选择**数据：** sample，仍然选择NA12878和my\_sample\_data，这部分我们将使用第一部分中所输出的bam文件作为这一部分的输入。
	b. 点击**输入参数**选项卡进行配置 ，如选择Hello\_GATK.input\_bam, 在属性值下拉选择this.bam，将所有输入配置完成后如下所示![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_40eeb798c61a1a5ef2f5b0befb7b348d.png)
	c. 点击**输出参数**选项卡，您可以在其中填写 output\_gvcf 变量的属性。要写入数据表，首先键入“this”。然后为该属性添加一个名称。工作流将为在sample数据表中生成一列。
	d. 点击右上角 **运行分析** 按钮以提交您的工作流
如果您的结果正确提交，那么在分析历史中查看到当前工作流的分析状态。稍等一段时间之后您就会看到分析状态转为分析成功![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_c93a8f59d241ed106e98515fd67c05a0.png)
3. #### 查看分析历史
等待分析完成之后可以在点击左侧菜单栏**分析历史**， 查看输出的参数，并将对应样本的gvcf文件进行下载。
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_53bc1b80810d11c4bc35f1993b2b5dcc.png)
至此我们也完成了两个工作流的运行，完成了从Cram到Bam文件的格式转换，并通过GATK对Bam序列，进行变异分析，得到变异中间结果文件gvcf文件。

### 第三部分：完成Workspace概述

如果您已经完成了所有的工作流分析工作，您可能还希望为您的Workspace写一个摘要或者介绍，您可以点击左侧Dashboard，Dashboard同样集成了Jupyter Notebook，您可以使用Jupyter Notebook中使用MarkDown进行文档的编写，完成编辑后点击**保存**并**退出编辑。**
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_45b21dca9b449a6eecba49c939ae29ec.png)
