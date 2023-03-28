### Workspace理念

在Miracle Cloud生信分析云平台中，workspace是对一次生信研究过程的完整封装，包括数据，代码，操作计算过程和结果以及作为整体介绍的dashboard，是实现科学研究和临床应用可执行，可移植，可复现，可分享，可发表的基本单位。
![](https://lf3-volc-editor.volccdn.com/obj/volcfe/sop-public/upload_1dd84551a1dafcdf3ab8d252ef230f72)

- 生信数据集管理：Miracle Cloud中的数据存储在云端，用户能够通过链接的形式进行直接使用，而不用下载到本地存储，从而节省传输时间成本和存储成本
	

- 生信数据模型：存储在云中不同位置的数据能够通过数据表格的形式有效的组织和展示，能够作为向量化计算的基础，同时也可将计算结果写回至数据表格
	

- 工作流：能够灵活、便捷的通过工作流开展基任意规模的因分析任务
	

- 实时交互分析环境：通过提供符合生信从业人员使用习惯的实时交互分析环境，能够帮助用户可视化和分析任何规模的数据
	

- Dashboard：作为对在Workspace中所做的所有研究的整体介绍，能够记录研究过程中的数据资源、工作流资源以及操作步骤等信息，此外还能通过具备的交互分析环境能够复现Workspace中的研究
	

- 云原生化调度：支持容器集群作为后端计算资源，保证资源利用率，同时能够保证环境统一，可复现
	

- GA4GH开放标准支持：从数据、工具等层面全面支持GA4GH开放标准，融入社区生态
	

### Workspace功能总览

#### 在Dashboard中记录和复现研究过程

Dashboard基于JupyterHub实现，一方面，作为概述介绍，通过Markdown文档能够记录在Workspace中研究的全部过程，包含数据、工作流以及操作步骤和结果等，能够方便快速的让他人了解此workspace中的全部研究内容；另一方面，通过提供实时的交互分析环境，能够调用Notebook和工作流运行的API，能够快速复现在workspace中开展的工作。
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_d2bae9a82dbd64d03b9e514be10e09cf.png)
此外，Dashboard中基本信息模块会展示此Workspace的名称、描述、创建时间、管理员信息，另外也会展示此Workspace对应的存储信息。
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_a6cf1ced472c4c0e83840b04a7feeba7.png)

#### 将数据上传并存储到存储桶中

Miracle Cloud中每个Workspace均对应一个存储桶，此存储桶信息可以从环境信息中查看，Workspace对应的存储桶可以存储一下内容：

- 用户自身的样本数据，可通过页面、命令行等方式上传
	

- 工作流运行过程中输出结果、运行日志等信息
	

- Notebook 文件，如`.ipynb`文件
	

注意：在Notebook中分析中产生的数据不会存储到存储桶中，而是默认存储在Notebook所在Server本地，如有需要，需手动将数据复制到存储桶中。

#### 在数据模块中组织和管理数据

Miracle Cloud提供数据模型功能，能够在平台以数据表格的形式组织、展示和整理数据

- 能够将用户的样本数据只作为数据表格，作为工作流的输入，简化批量任务投递的操作
	

- 能够将工作流的输出结果数据写回至表格，一方面，实现了输入、输出数据的统一展示，另一方面输出数据能够作为下一个步骤运行的输入
	

- 实体数据表格中每一行均表示为一个实体，每个实体均有且有唯一实体ID（第一列数据）；实体数据表格中每一列为每个实体的属性值
	

1. **实体数据模型**
	

实体数据模型可以分为实体数据表格和实体数据集合表格。实体数据表格中，第一列的实体ID对应一个数据实体，数据实体以链接的形式链接到云中真实存放的数据集信息。
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_cf0e1fbf72b86b5fe4198a7e597107a4.png)
实体数据集合表格中，第一列的实体集合ID对应一组数据实体组成的集合，集合内容为数据实体ID组成的数组。注意：
- 实体数据集合表和实体数据表的关系一一对应，如sample实体表和sample\_set实体集合表一一对应
- 实体集合表本身没有数据实体链接，而是会用过列名称索引到实体数据表中的数据
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_b13a935ef3cb26fd3f138f9f908737f6.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_9f2caff5ed0dd4ba7b3f166a3333311b.png)

2.**Workspace级别数据模型**
Workspace级别的数据模型，顾名思义，代表此数据表格中的内容为整个Workspace内所有样本数据均需要的数据，如参考基因组数据、Docker镜像等。![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_86ed3c025a881be75afd664aec36ec12.png)

3. **文件数据管理**

文件数据管理中的文件列表是存储在存储桶中的一些文件数据，文件数据可用于在notebook插入读取或者在实体数据模型中进行关联。
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_c7830fa42dcd10b74f6e472bd9fec8d1.png)

#### 使用Notebooks实时分析数据

Miracle Cloud集成了Jupyterhub开源组件，为用户提供了进行生信数据实时交互分析环境。
![alt](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_a2b650f1d342818462e1713e5f91abc2.png)

#### 使用工作流简化生信工作流批量分析

工作流模块能够提供灵活、便捷的工作流导入、查看、配置、投递全部操作，此外与数据模型结合使用，能够将工作流的输入数据、输出数据快速组织起来，实现生信数据的高效管理。![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_366197ea6b6e9c4327e459c0a228fd7b.png)

#### 在分析历史页面查看任务分析过程及Troubleshooting

Miracle Cloud分析历史页面是将样本数据选择某条工作流进行分析的历史记录，用户的一次投递视为一次任务的分析，一次任务的分析包含多个样本批量分析（即存在多条工作流批量运行），其中每条工作流的运行可能又会拆分成多个步骤的运行。在分析历史中能够精确、细粒度的查看投递、工作流运行、task运行三个级别的历史记录。
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_35b4b29878fee8dbb1f88aa820824943.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_c772530880f2d910bbd0ad454d1b51d0.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_c3122fdf4758794cc2c297be261d2870.png)
当分析过程出错时，能够根据task级别日志和工作流级别日志能够进行故障排查工作
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_39550c32af1fb41c29d48015bd5aeae2.png)
此外，分析历史中能够记录本次工作流运行的配置信息（含输入样本数据、参数配置），能够点击直接跳转至当时配置页面，方面进行快速复现此次分析。
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_c3f9d5fd647da1e3bd5ee6b076cc7727.png)
