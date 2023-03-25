### Differences in data management between cloud and traditional bioinformatics analytics

In traditional biological information research, users need to download data from the public standard bioinformatics database to be stored locally, while in the cloud-based bioinformatics analysis platform, the data is always stored in the cloud and users are able to access the data as link anytime, anywhere. To obtain and use data, users do not need to download and store, which avoids the wastage of time, transmission errors and storage costs.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_78a089c128c1ed0043ddf16de7665331.png)

### Where is Miracle Cloud data stored

Compared with the traditional data usage method that needs to copy data to the local, the usage method of cloud scenarios may not be intuitive. When discussing the data in the workspace in the Miracle Cloud, In addition to the data stored in the corresponding bucket of your Workspace, it is also common to link data to your Workspace in the form of a link (which does not actually exist in the corresponding bucket of your Workspace).
In fact, when analyzing sample data, except that the sample data needs to be uploaded, while the other data does not need to be copied to your Workspace bucket. All analysis is done in the cloud, and only the generated data will be stored in the Workspace storage in the bucket.
Typically, data analyzed in Miracle Cloud will be located in the following three locations:
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_b9d21b92635044028ed05edd85be465b.png)

- **Workspace bucket:** Typically, personal data you upload (such as sample data), output data from workflow output, and ipynb files from notebooks are stored in the Workspace bucket
- **Outside Workspace Bucket :** Most of the data you use will be stored in some other data store in the cloud (like the publicly reference genome data provided by Miracle Cloud), which you can access and use in Miracle Cloud as long as you have the correct permissions and authorizations.
- **Container Cluster:** When using the Notebook module for interactive analysis, the generated output data is stored by default in the persistent storage PVC of the container cluster. Additionally, if you need to share this data with others, you can copy it to a Workspace bucket
### Advantages of Data Models
Since Miracle Cloud provides buckets for storing and sharing data, why does it provide a data model for organizing data? That's because although it takes some time to follow certain rules to make data tables initially, the advantages of data models will emerge as the amount of data grows:

- Ability to organize, display and manage tens of thousands of samples for easy tracking, browsing and sorting
	

- Ability to easily perform parallel computing. By selecting the data entity in the data model, it can be easily specified as the input of the workflow. The workflow of analyzing 1 sample and analyzing 1000 samples is almost the same, which makes it very good scalability.
	

- Ability to write output data back to the data model by setting workflow output parameters, which will automatically associate output results from input data
	

- Writes output back to the data model that can be used as input to the next run without having to find the file path in the workspace bucket
	

