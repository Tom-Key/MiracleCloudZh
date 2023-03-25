The data in Miracle Cloud is stored in the cloud-native platform, and users can use it directly in the form of a link instead of downloading to local storage, thereby saving transmission time and storage costs. There are three parts of the data: Entity Data Model, Workspace Data Model and Data files.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_6c86953630a23f19265aeb5d74c1ea57.png)
#### Entity Data Model

The Entity Data Model is to organize, manage and display the information data in the form of data tables, and also provide the basis for the batch operation of the workflow to complete the bioinformatics workflow analysis.

##### Create New Data Model

1. Click Data on the left panel of workspace and then click the blue + button on the right side of the entity data model, "Import entity table" pop-up appears.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_cccb818aedec8a5a4a305d6cc7e72201.png)

2. Here you can **download the CSV file template** and edit the data . The template should contain at least one entity line. You could upload the CSV file after editing, just drag and drop to complete the file uploading.
	

3. Finally, click **Import Data Table** to complete the import of the data model. ![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_95e908d22d68125ed30d9e7cb41c3049.png)

##### Generate Entity Set

Generating entity set is used to combine two data rows to generate a new entity set, without creating the array content by the user himself.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_cba3a6762b53d3f09cc6afa6951dcec7.png)

1. On the Entity Data Model page, select at least two entities and click **Generate entity collection**
	

2. Input the Entity Collection ID and click **OK.** 
	

The Default ID is: Entity set name \_set-year-month-day-hour-minute-second
<br>

#### **Workspace Data Model**

Workspace Data Model is public data required by different workflows in the entire Workspace, and it is no longer necessary to attach these common resources for each sample in the entity table, such as public reference data, Docker images url etc. For example, you can upload data such as reference genomes data to the Workspace data model.
Click **Workspace Data** - **Import**, and then drag and drop the edited CSV file to import data table. You can also download and delete imported files.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_90a6e239e9f93699bf5ae7c05c99081b.png)

#### Data File Management

Click **Data** on the left menu, select **Data file Management** - **File Lists** - **Upload File/Upload Folder**, support users to drag and drop to upload or click Select Local File Upload. In addition to web page uploading, Miracle Cloud also supports uploading, downloading and querying files through the CLI command line.

Note: When uploading a folder, all files in the folder directory will be uploaded to the current directory. The folder supports uploading up to 2000 files, and the maximum limit of each file is 2G. It is recommended to use the CLI tool for uploading and downloading large files

Attention：if you want to use file or image from file list in Notebook，you need to change the file permission to **Public Read** . Please check the operation below.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_91ea527ff6e9cc188c29e6d4c9184ee4.png)

CLI file upload
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_8be18543cf619211220b0de722605549.jpg)

1. **Install** **CLI** **tool**
	

Miraclecloud CLI is a tool for uploading and downloading files to TOS of Miracle Cloud. You could use Terminal to install in environments that require upload or download operations, such as PC.

```
pip install -i https://test.pypi.org/simple/ --extra-index-url https://pypi.org/simple miraclecloud==0.0.10
```

If volcengine or tos dependencies are missing after execution, please install volcengine and tos first. Volcengine and tos are the official sdk of Volcano Engine. You need to install it first. Execute the command in termianl as follows:

```
pip install volcengine
pip install tos
```

2. Configure the environment
	

In this step, we need to obtain four environment parameters: AccessKey ID, AccessKey Secret, Miracle IP address and Workspace ID.

- Get Miracle Cloud Access Key
	

Log in to the console, click profile photo in the upper right corner of the page, select **Access Control** in the drop-down menu, and click **Key Management** on the left Click **Create Key** and note the AccessKey ID and AccessKey Secret content that pops up on the page.

- If you login account is sub account, you need to additionally configure key authorization for sub account.
	
- Key usage authorization configuration method:
	
	> Only sub accounts require this step
	
	- Log in to the miracle cloud console (referred as console below) with the main account.
		
	- Click on the profile photo in the upper right corner of the page, select **Access Control** in the drop-down menu.
		
	- Click **Policy Management** on the left, then click **Create Policy**, enter the following content and click **OK**:
		
	
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
	
	- Click **User Management** on the left, find the sub account in the user list, and click **Association Policy**
		
	- Then click **Add Policy**, find AccessKeyFullAccess, tick the Optional Policy and move to **Selected Policy**, and finally click **OK** button
		

- Get Miracle Cloud ip address and Workspace ID
	

Click to enter the notebook of the current Workspace, select Edit the current notebook or create a new notebook, execute the following code in the notebook cell, get the Miracle IP address and Workspace ID and record them.

```
import os
# 输出Miracle ip-adress
os.environ['MIRACLE\_ENDPOINT']  
# 输出Workspace id
os.environ['JUPYTERHUB_SERVER_NAME']
```

<br>

3. Configure environment parameters
	

After obtaining the four environment variables in the previous step, run the following four command lines in the terminal on your environment, replacing the corresponding parameters with the parameters just obtained

```
export JUPYTERHUB_SERVER_NAME=wc*****************0g
export MIRACLE_ENDPOINT=http://*********
export MIRACLE_ACCESS_KEY=AKLT**************************************NiNjM
export MIRACLE_SECRET_KEY=WVdV**************************************************5XWQ==
```

<br>

4. Use the cli command to upload and download file to TOS of Miracle Cloud
	

- **Query file list**
	

Returns all files in the target path directory, the number of files, and the file size.

```
#For ex, list all files under root directory： miraclecloud.cli list /
miraclecloud.cli list <target_path> <-options>
```

| Option | Parameter description | If required |
| --- | --- | --- |
| \-n or --num | Set the number of queries this time, the default is 0, means all queries | NO |\
||||\
|||\
|||

- **Upload file**
	

Upload file to target directory

```
miraclecloud.cli upload <local_path> <target_path> <-options>
```

| Option | Parameter description | If required |
| --- | --- | --- |
| \-r or --recursive | For uploading file folder | NO |\
||||
| \--ignore <fileregexp> | According to the regex, a certain type of file can be ignored，such as .\*txt , .\*doc | NO |
| \--include <fileregexp> | According to the regex, a certain type of file can be specified，such as .\*txt , .\*doc | NO |

Example: To upload the genomics.ipynb files in the local Download folder to the notebook folder under root directory:

```
miraclecloud.cli upload ~/Downloads/genomics.ipynb notebook
```
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_543f7b2a16ae1c8c07d30c8448151bc6.png)
After the upload is successful, the file appears in the notebook folder:
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_02de651ef309db922659d3377b8efc41.png)
- **Download file**
```
miraclecloud.cli download <target_path> <local_path> <-options>
```

| Option | Parameter Description | If required |
| --- | --- | --- |
| \-r or --recursive | For download folder | NO |\
||||
| \-f or --force | If the option is not input, then if there is a file with the same name, the download will be skipped. If the option is input, the file with same name will be overwritten. | NO |\
||
| \--ignore <fileregexp> | According to the regex, a certain type of file can be ignored，such as .\*txt , .\*doc | NO |
| \--include <fileregexp> | According to the regex, a certain type of file can be specified，such as .\*txt , .\*doc | NO |

- **Delete file**
	

```
miraclecloud.cli delete <target_path> <-options>
```

| 选项 | 参数说明 | If required |
| --- | --- | --- |
| \-r or --recursive | For delete file folder | NO |\
||||
| \--ignore <fileregexp> | According to the regex, a certain type of file can be ignored，such as .\*txt , .\*doc | NO |
| \--include <fileregexp> | According to the regex, a certain type of file can be specified，such as .\*txt , .\*doc | NO |\
||
