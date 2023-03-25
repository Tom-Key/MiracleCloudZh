Workflow is the biological information analysis process, Miracle Cloud's analysis process supports WDL description language. On the workflow page, you can choose **custom import** or I**mport from the workflow repository** . In the workflow repository you can find the bioinformatics analysis process for batch processing genomics data. After import, computational analysis can be initiated.<br>
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_ffddf84ad5f376d4fbff515e153c5c06.png)
#### **Custom Import**

Select **Custom Import**, enter workflow name, git address, git project tag and token, main workflow path, short description. Then click OK when done.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_73f90388d422dd180ae26bf45d0dde77.png)
**Git** **tag:** In git, tags are used to specify a specific commit. Take gitlab as an example, select the branch to see the current tag you need.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_0d287fdf4a109ba75713bddb9e7ba75c.png)
**Git** **token:**  Gitlab removed support for password authentication on 2021.8.13, it recommends using personal access token instead of password authentication, you can find Access Token in the settings on the left menu and copy it into the parameters.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_717e8bd40c4fc46166ce87aa8bf4fbe9.png)
**Main workflow path:** You could find the WDL file you need to import the workflow, and click the Copy button to directly copy the path of the current file into the input parameters.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_f1e9927f1d2ebf59ccb22c49aa84b73a.png)
**CallCaching:** When enabled, the cache of previously run tasks is searched for tasks with the exact same command and the exact same input. If the cache hits, the results of the previous task will be used instead of being re-run, saving time and resources.
#### Import from workflow repository

This function is still under development...
#### Configure workflow inputs

After creating a workflow , to run a workflow on Miracle Cloud, you need to specify all required workflow input variables.

1. Specify Entity Data Model, and click **Select Data**.
**CallCaching:** When enabled, the cache of previously run tasks is searched for tasks with the exact same command and the exact same input. If the cache hits, the results of the previous task will be used instead of being re-run, saving time and resources.
![alt](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_f46bbadcc71af38ee2e0cd835883f2e0.png)

2. Configure input parameters
Click the Input Parameters tab to configure input attribute values. Attributes are integer, string, or file types that correspond to input variables in the workflow. You should specify input parameters by filling in all required variables in the attribute value column. You could manually input or use this to specify the input parameters in entity data model. Or you could upload json file to input the attribute value.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_1a670fc6bd9b5e1637dfaaf9c6d141d7.png)
Examples of some common attribute value formats:
Int - `50` 
String - `string`
Array（Separated by commas） - `[string1,string2]`
File Path - `s3://analysis /sc94ra7leig43voqu4vlg /CramToBamFlow /5d915601-75c3-493b-9c7d-d136c9f9a296 /call-CramToBamTask /execution/script`
#### How to write metadata of output file to input data table
**Why we need to write output to a data table?** Writing output back to data table will associate the output of a workflow with an input data file which helps organize the output in a meaningful way. It also makes it easy to use the data for next step analysis.

1. Go to the **Output Parameters** tab.
2. For each output variable, click the **Attribute Value** field. You will see a drop-down list of all columns that exist in the selected entity data model.
3. Select an existing column or enter a new name to add a new data column to the data model. If there is no such column in the original data model, a new column will be added according to the column name; if the original data model has a corresponding column with same name, the new result will be populated or overwritten directly.
![alt](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_fa95d680ed04bb732c7c11d7ec06dbb7.png)
#### Use Json to set input and output parameters

What is JSON? JSON is a text-based, readable file that stores simple data structures and objects in JavaScript Object Notation (JSON) format. The open standard JSON format transfers WDL configuration to a server running WDL. JSON files can be edited using a text editor.

When clicking the Input Parameters or Output Parameters tab, you can choose to download the JSON file template. After editing, you can upload the JSON file to set the workflow input or output.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_baa61d26b880a13b4ac17ac45a35b45c.png)
For different workflows, it comes in handy when you want to use the same input in your workflow configuration. For example, if you create a new configuration, usually you have to manually enter all the inputs every time, even though this new configuration will use many of the same inputs as the existing configuration. You can now download the input from the previous configuration as JSON and upload it to fill into the new configuration.
