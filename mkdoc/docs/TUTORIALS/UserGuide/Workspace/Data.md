Miracle Cloud中的数据存储在云端，用户能够通过链接的形式进行直接使用，而不用下载到本地存储，从而节省传输时间成本和存储成本。在数据中有实体数据模型，Workspace数据模型以及文件列表。![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_8d090eccdad7be6946d3171e167903be.png)

### 数据模型

数据模型是通过数据表格的形式对生信数据进行整理、组织和展示，也为工作流批量运行实现向量化计算提供基础，同时能够同时作为工作流统一呈现输入数据和输出结果，是工作流的起点和终点。

#### 创建新数据模型

1. 点击实体数据模型右侧的蓝色 + 按键，弹出导入实体表弹窗。![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_16fdfd0c7f8aea4eacf2b522c5a99279.png)

2. 在这里你可以通过点击**下载CSV文件模板**，并进行编辑数据，csv中至少包含一个实体行，完后编辑后上传CSV文件，拖拽到对应位置可完成文件上传。
3. 最后点击**导入表**完成数据模型的创建。
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_3781e18c89a837edd6d1eec3075d319a.png)

#### 生成实体集合

生成实体集合主要是将两个数据行组合生成新的实体集合，无需用户自己创建数组内容
在实体数据模型页，勾选所需数据样本（2个以上）后，点击生成实体集合，数据实体集名称，即作为数据实体集表的ID。ID默认为：实体集名称\_set-年份-月份-日-小时-分钟-秒。
### Workspace数据模型

Workspace级别数据是针对整个Workspace中不同工作流所需用到的公共数据进行统一展示，不再需要对于实体表中每一例样本都需要在附加这些共同的资源，如用到的公共的参考数据、镜像地址等等。一般来说您可以将如参考基因组的数据关联到Workspace数据模型。
点击导入，弹窗后拖拽已编辑的CSV文件进行上传。您也可以对已导入的文件进行下载和删除。![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_939e73401e3ef540a8ac4fd8e2a7a714.png)
### 文件列表

注意：文件夹最多支持上传2000个文件，每个文件最大限额为2G。大文件建议使用CLI工具进行上传下载

点击左侧菜单**数据，** 选择**文件列表**\-**上传文件**/**上传文件夹**，支持用户拖拽上传或点击选择本地文件上传。除了网页上传之外，Miracle Cloud还支持通过CLI命令行进行文件的上传、下载和查询。

#### CLI文件管理

CLI([Command-line interface](https://en.wikipedia.org/wiki/Command-line_interface))是命令行工具，Miracle Cloud支持通过命令行进行上传下载和查看文件列表的操作，使用CLI进行文件操作适用于批量以及大文件的上传下载。

![alt](https://lf6-volc-editor.volccdn.com/obj/volcfe/sop-public/upload_25ea44629db80538cd4fdc9b5e651d5e.png)

1. 安装cli工具
	

miraclecloud是提供针对Miracle Cloud文件上传下载的工具，您可以在需要上传或者下载操作的环境（如PC电脑）使用Terminal进行安装。

```shell
pip install -i https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple miraclecloud==0.0.10
```


如果执行后出现volcengine,tos依赖缺失的情况，请先安装volcengine以及tos，volcengine以及tos为火山引擎的官方sdk，需要优先安装，在termianl中执行命令如下：

```shell
pip install volcengine
pip install tos
```
<br>

2. 获取环境参数
	

在这一步当中我们需要获取四个环境参数，分别是AccessKey ID, AccessKey Secret, MIRACLE\_ENDPOINT以及JUPYTERHUB\_SERVER\_NAME

1. 获取Miracle Cloud访问密钥
	

登录控制台，点页面右上角头像，在下拉菜单中选择「**访问控制**」，点击左侧边的「**密钥管理**」

点击「**创建密钥**」，记下页面上弹出的`AccessKey ID`和`AccessKey Secret`内容。

- **如果是子账号身份**，需要联系主账号管理员配置密钥使用授权。密钥使用授权配置方法：
	- 主账号登陆miracle cloud控制台
		
	- 点页面右上角头像，在下拉菜单中选择「访问控制」，进入IAM页面
		
	- 点击左侧边的「策略管理」，再点击「创建策略」，输入以下内容后点「确定」：
		1. 策略名，填：`AccessKeyFullAccess`
			
		2. 描述，填：`该策略包含密钥管理全读写权限`
			
		3. 配置方式，选：`JSON配置`
			
		4. 策略内容 ，填：
			
		
		```
		{
		  "Statement": [
		    {
		      "Effect": "Allow",
		      "Action": [
		        "iam:*AccessKey",
		        "iam:ListAccessKeys",
		        "iam:*SecretKey",
		        "iam:ListSecretKeys"
		      ],
		      "Resource": [
		        "*"
		      ]
		    }
		  ]
		}
		```
		
	- 点击左侧边的「用户管理」，在用户列表中找到接下来要使用的子账号，点击「关联策略」
		
	- 再点击「添加策略」，找到`AccessKeyFullAccess`，勾上加入「已选策略」，最后「确定」
		

2. 获取Miracle Cloud ip地址以及Workspace ID

在这一步当中我们需要获取四个环境参数，分别是AccessKey ID, AccessKey Secret, MIRACLE\_ENDPOINT以及JUPYTERHUB\_SERVER\_NAME

1. 获取Miracle Cloud访问密钥
	

登录控制台，点页面右上角头像，在下拉菜单中选择「**访问控制**」，点击左侧边的「**密钥管理**」

点击「**创建密钥**」，记下页面上弹出的`AccessKey ID`和`AccessKey Secret`内容。

- **如果是子账号身份**，需要联系主账号管理员配置密钥使用授权。密钥使用授权配置方法：
	- 主账号登陆miracle cloud控制台
		
	- 点页面右上角头像，在下拉菜单中选择「访问控制」，进入IAM页面
		
	- 点击左侧边的「策略管理」，再点击「创建策略」，输入以下内容后点「确定」：
		1. 策略名，填：`AccessKeyFullAccess`
			
		2. 描述，填：`该策略包含密钥管理全读写权限`
			
		3. 配置方式，选：`JSON配置`
			
		4. 策略内容 ，填：
			
		
		```
		{
		  "Statement": [
		    {
		      "Effect": "Allow",
		      "Action": [
		        "iam:*AccessKey",
		        "iam:ListAccessKeys",
		        "iam:*SecretKey",
		        "iam:ListSecretKeys"
		      ],
		      "Resource": [
		        "*"
		      ]
		    }
		  ]
		}
		```
		
	- 点击左侧边的「用户管理」，在用户列表中找到接下来要使用的子账号，点击「关联策略」
		
	- 再点击「添加策略」，找到`AccessKeyFullAccess`，勾上加入「已选策略」，最后「确定」
		
2. 获取环境参数
在这一步当中我们需要获取四个环境参数，分别是AccessKey ID, AccessKey Secret, MIRACLE\_ENDPOINT以及JUPYTERHUB\_SERVER\_NAME
a. 获取Miracle Cloud访问密钥
登录控制台，点页面右上角头像，在下拉菜单中选择「**访问控制**」，点击左侧边的「**密钥管理**」
点击「**创建密钥**」，记下页面上弹出的`AccessKey ID`和`AccessKey Secret`内容。
- **如果是子账号身份**，需要联系主账号管理员配置密钥使用授权。密钥使用授权配置方法：
	- 主账号登陆miracle cloud控制台
	- 点页面右上角头像，在下拉菜单中选择「访问控制」，进入IAM页面
	- 点击左侧边的「策略管理」，再点击「创建策略」，输入以下内容后点「确定」：
		- 策略名，填：`AccessKeyFullAccess`
		- 描述，填：`该策略包含密钥管理全读写权限`
		- 配置方式，选：`JSON配置`
		- 策略内容 ，填：
		```
		{
		  "Statement": [
		    {
		      "Effect": "Allow",
		      "Action": [
		        "iam:*AccessKey",
		        "iam:ListAccessKeys",
		        "iam:*SecretKey",
		        "iam:ListSecretKeys"
		      ],
		      "Resource": [
		        "*"
		      ]
		    }
		  ]
		}
		```
		
	- 点击左侧边的「用户管理」，在用户列表中找到接下来要使用的子账号，点击「关联策略」
		
	- 再点击「添加策略」，找到`AccessKeyFullAccess`，勾上加入「已选策略」，最后「确定」
		

2. 获取MIRACLE\_ENDPOINT地址以及UPYTERHUB\_SERVER\_NAME
	

点击进入当前Workspace的Dashboard，选择编辑当前任一notebook或者创建新的notebook，在notebook单元格中执行下列语句，获取到MIRACLE\_ENDPOINT地址以及UPYTERHUB\_SERVER\_NAME并加以记录。
```
import os
# 输出MIRACLE_ENDPOINT地址
os.environ['MIRACLE_ENDPOINT']  
# 输出JUPYTERHUB_SERVER_NAME地址
os.environ['JUPYTERHUB_SERVER_NAME']
```

3. 配置环境参数	

在前两步获取了四个环境变量之后，在本机环境（使用本地环境需要配置4个环境变量，在notebook里使用时则仅需配置AK和SK）的terminal中执行以下四个命令行，用刚才获取的变量替换对应参数

```在terminal里可输入
export JUPYTERHUB_SERVER_NAME=wc*****************0g
export MIRACLE_ENDPOINT=https://*****
export MIRACLE_ACCESS_KEY=AKLT**************************************NiNjM
export MIRACLE_SECRET_KEY=WVdV**************************************************5XWQ==
```
```在Notebook里可输入
os.environ ['MIRACLE_ACCESS_KEY'] =="AKLT**************************************NiNjM"
os.environ['MIRACLE_SECRET_KEY'] ="WVdV**************************************************5XWQ==""
```

<br>
4. 使用cli命令操作文件上传下载
- 查询文件列表
返回targetpath目录下所有文件列表、文件数量和文件大小。

```shell
#比如列出根目录下所有文件： miraclecloud.cli list /
miraclecloud.cli list <target_path> <-options>
```

|选项 |参数说明 |是否必选 |
|---|---|---|
|\-n或--num |设置本次查询数量，默认为0，即全部查询 |否\
||| |

- 上传文件
	

上传指定文件到指定路径

```shell
miraclecloud.cli upload <local_path> <target_path> <-options>
```

|选项 |参数说明 |是否必选 |
|---|---|---|
|\-r或--recursive |用于上传文件夹 |否\
||| |
|\--ignore <fileregexp> |根据正则，可以忽略某一类文件，如 .\*txt,.\*doc |否 |
|\--include <fileregexp> |根据正则，可以指定某一类文件，如 .\*txt,.\*doc |否 |

举例：如需将本地Download文件夹中的genomics.ipynb文件上传到当前根目录下notebook文件夹中：

```shell
miraclecloud.cli upload ~/Downloads/genomics.ipynb notebook
```

![](https://lf3-volc-editor.volccdn.com/obj/volcfe/sop-public/upload_02da640465b4dd2c1eab9d96aa0f6d24)
上传成功后，文件出现在notebook文件夹下
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_eb30b933a0e892687118df48f5dbb22d.png)

- 下载文件
```shell
miraclecloud.cli download <target_path> <local_path> <-options>
```

|选项 |参数说明 |是否必选 |
|---|---|---|
|\-r或--recursive |用于下载文件夹 |否\
||| |
|\-f或--force|不传的话存在同名文件则跳过下载，如果传本参数会强制覆盖 |否 \
| |
|\--ignore <fileregexp> |根据正则，可以忽略某一类文件，如 .\*txt,.\*doc |否 |
|\--include <fileregexp> |根据正则，可以指定某一类文件，如 .\*txt,.\*doc |否 |

- 删除文件
	

```shell
miraclecloud.cli delete <target_path> <-options>
```

|选项 |参数说明 |是否必选 |
|---|---|---|
|\-r或--recursive |用于删除文件夹 |否\
||| |
|\--ignore <fileregexp> |根据正则，可以忽略某一类文件，如 .\*txt,.\*doc |否 |
|\--include <fileregexp>|根据正则，可以指定某一类文件，如 .\*txt,.\*doc |否 \
| |
