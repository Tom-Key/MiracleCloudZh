## Biomedical Research in Workscpace

### Workspace Concept

In Miracle Cloud platform, workspace is a complete encapsulation of a bioinformatics research process, including data, environment, code, operational calculation procedures, results, and dashboard as an overview. It is the basic unit that realizes executable, transportable, reproducible, shareable and publishable scientific research and biological application.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_83e22654c9c3ef0782052da7ac51848c.png)

- **Bioinformatics Dataset Management:** The data in Miracle Cloud is stored in the cloud, and users can use it directly in the form of links instead of downloading it to local storage, thereby saving transmission time and storage costs.
	

- **Bioinformatics data model:** Data stored in different locations in the cloud can be effectively organized and displayed in the form of data tables, which can be used as the basis for vectorized calculations, and the calculation results can also be written back to the data table.
	

- **Workflow:** Support flexible and convenient workflow for bioinformatics analysis tasks of any size
	

- **Interactive Analysis:** Providing a real-time interactive analysis environment that conforms to the usage habits of bioinformatics practitioners, it can help users visualize and analyze data of any scale
	

- **Dashboard:** As an overall introduction to all research done in Workspace, it's able to record information such as data resources, workflow resources, and operation steps in the research process, and can also reproduce research in Workspace through the available interactive analysis environment.
	

- **Cloud Native** **Scheduling:** Support container clusters as back-end computing resources to ensure resource utilization, and at the same time ensure a unified and reproducible environment
	

- **GA4GH Open Standards Support:**  Fully support the GA4GH open standard from the level of data and tools, and integrate into the community ecology

### Overview of Workspace Features

#### Record and Reproduce research process in Dashboard

Dashboard is integrated with Jupyterhub. On one hand, as an overview, the entire process of research in Workspace can be recorded through Markdown documents, including data, workflow, operation steps and results, etc., which can easily and quickly let others understand all the research content in this workspace. On the other hand, by providing a real-time interactive analysis environment, it can call the API of notebook and workflow which can quickly reproduce the work in the workspace.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_4a6df6cd11dd5267dd2367da8c671307.png)
In addition, the basic information module in the Dashboard will display the name, description, creation time, and administrator information of the Workspace，and will also display the storage information corresponding to the Workspace.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_c9bd920376012c7c52a43b11ae1a7ffd.png)

#### Upload and store data in a bucket

Each Workspace in Miracle Cloud corresponds to a bucket. This bucket information can be viewed from environmental information. The bucket corresponding to the Workspace can store the following contents:

- User's own sample data which can be uploaded through pages, command lines, etc
	

- Output results, logs and other information during workflow operation
	

- Notebook file such as `.ipynb` file
	

Note: The data generated in the Notebook will not be stored in the bucket, but will be stored locally on the server where the Notebook is located by default. If necessary, the data needs to be manually copied to the bucket.

#### Organize and manage data in data modules

Miracle Cloud provides data model to organize, display and manage data in the form of data tables on the platform:

- Use the user's sample data as a data table and as the input of the workflow, which simplifies the batch operation of task delivery.
	

- The output result data of the workflow can be written back to the table. On the one hand, it realizes the unified display of input and output data, and on the other hand, the output data can be used as the input for the next step.
	

- Each row in the entity data table is represented as an entity, and each entity has a unique entity ID (first column of data); Each column in the entity data table is the attribute value of each entity.

1. **Entity Data Model**
Entity data model can be divided into entity data table and entity data set table. In the entity data table, the entity ID in the first column corresponds to a data entity, and the data entity is linked to the data set information actually stored in the cloud in the form of a link.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_a63db9e2a6ae0b98371f124c475072cd.png)
In the entity data set table, the entity set ID in the first column corresponds to a set of data entities, and the content of the set is an array composed of data entity IDs.
Note:
- Entity data set table and entity data table is one-to-one correspondence
- The entity collection table itself does not have a data entity link, but will index the data in the entity data table with the column name.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_c6e758f0f9e2f20cbe73edad98d74b41.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_c6be8fa88f9f5f0164f15fa914a77703.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_1dc7351c80a8ac784bfc0c9540335e3e.png)
2. **Workspace data model**
The Workspace-level data model, as the name suggests, means that the content in this data table is the data required by all sample data in the entire Workspace, such as reference genomic data, Docker images url, etc.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_cae80fb386e29935096082a16ae5d1e8.png)
3. **Data file Management**
	
File lists are some file data stored in a bucket that can be inserted into a notebook for reading or linked to an entity data model.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_836c3201a718220d4eb7bb9465296c28.png)
<br>

#### Interactive Analysis using Notebooks
Miracle Cloud integrates Jupyterhub open source components to provide users with an environment for real-time interactive analysis of bioinformatics data.![alt](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_541e79e4c99b8e62e89bece54673ba81.png)

#### Using Workflows to Simplify Bioinformatics Batch Analysis

The workflow module can provide flexible and convenient workflow operation such as import, viewing, configuration, and delivery. In addition, it can be used in combination with the data model to quickly organize the input data and output data of the workflow to achieve efficient management of bioinformatics data.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_d47814495d9a79678f3f6a513d2c5f89.png)

#### View the task analysis process and troubleshooting in Job History

The Miracle Cloud analysis history page is a historical record of selecting a certain workflow for analysis of sample data. One delivery by the user is regarded as the analysis of one task. The analysis of one task includes batch analysis of multiple samples (if there are multiple workflows running in batches). The operation of each workflow may be split into multiple steps. In the Job history, you can accurately view the history records of delivery, workflow operation, and task operation.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_f99023b868fb17e2da9555f717733ab5.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_47664a7dd8fa16ef0682655bb55c1d1e.png)
You could do troubleshooting based on task level logs and workflow level logs when the analysis process goes wrong.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_042e3f68ba3587bd5a26d2fb4890eabf.png)
In addition, the configuration information of this workflow operation (including input sample data and parameter configuration) can be recorded in the analysis history, and the analysis can be quickly reproduced by clicking to jump directly to the configuration page at that time.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_fbd87900b2ba830dd373e564e6146bc2.png)
