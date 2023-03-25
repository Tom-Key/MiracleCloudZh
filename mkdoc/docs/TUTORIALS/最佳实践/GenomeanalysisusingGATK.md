## Genomic analyses Using GATK Workflow

This chapter describes how to use the Genomic Analysis Toolkit (GATK) to run a secondary genomic analysis workflow on Miracle Cloud. The tools used in this chapter is GATK, which is used to convert the sequence in Cram format to Bam format and then perform mutation analysis to obtain the mutation intermediate result gvcf file. The workflow is written in WDL and runs on the Cromwell.

### GATK Introduction
GATK is the abbreviation of Genome Analysis Toolkit, which is a set of software and toolkit used to process high-throughput sequencing data. Originally, GATK was designed to analyze the human genome and exons, mainly to find SNPs and indels. Later, the functions of GATK are becoming more and more abundant, adding new functions such as short variant calling, Copy number variation(CNV) and structural variation (SV). At the same time, GATK is also increasingly used in data analytics of other species. Now, GATK has become the industry standard for finding variants during genomic and RNA-seq analysis.

### Part 1: Running cram2Bam data format conversion workflow
In this section you can learn how to upload data and run the workflow successfully. This workflow implements file format conversion from cram to bam.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_ddc77860ab98ce97e54beaeae3b5a35b.png)
1. #### Login to Miracle Cloud
Using the account and password to login to Miracle Cloud. The account and password can be obtained by contacting the administrator.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_fc507354a38fc20034a465f7bc868646.png)
There are three menus at the top of the page: Overview, Workspace and Digger. Digger is a public Workspace. Usually, you can choose to publish the completed Workspace to Digger, or you can copy the Workspace in Digger back to Workspace for re-editing.Select **Workspace**, click **New Workspace**, entering name: GATK-Workflow and a short description. After creating the Workspace we will start building the Workspace.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_c8bd9e8208b9a89f6305e835ca235b7d.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_b6f05b086d089d1324dac6f5fb3d80ac.png)

2. #### Upload data
Usually， uploading the sample data is the first step in starting a workflow. In this practice, we need to transfer the data set to the storage bucket first, and make the corresponding data model. You have two methods to get data models:
	- You can download the sample-datasets file and reference-datasets file to the local according to the link below, then upload them to the storage bucket corresponding to the corresponding Workspace, and finally create a data model according to the S3 path corresponding to the file
	- You can directly download the data model sample.csv file corresponding to this best practice. If you choose this method, you can start directly from step d.
```bash
# Dataset download links used in this tutorial：
# DataModel(CSV):
https://tutorials-data.tos-cn-guangzhou.volces.com/cram-to-bam/sample.csv
#sample-datasets：
https://tutorials-data.tos-cn-guangzhou.volces.com/cram-to-bam/sample-data/NA12878.cram
https://tutorials-data.tos-cn-guangzhou.volces.com/cram-to-bam/sample-data/my-sample-data.cram
#reference-datasets：
https://tutorials-data.tos-cn-guangzhou.volces.com/cram-to-bam/reference-data/Homo\_sapiens\_assembly38.dict
https://tutorials-data.tos-cn-guangzhou.volces.com/cram-to-bam/reference-data/Homo\_sapiens\_assembly38.fasta
*https://tutorials-data.tos-cn-guangzhou.volces.com/cram-to-bam/reference-data/Homo\_sapiens\_assembly38.fasta.fai
```
Click Data-File List on the left sidebar of the platform to enter the file data management page. By clicking Upload File, you can upload the local data file to the object storage bucket corresponding to this Workspae according to the guidance.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_46c319cf5f25b9468a77d0267bbc348a.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_f03ef7ec2fa8a30b9f4651f95e85886b.png)
After the data is uploaded, download the csv template and organize the data. Click the data column on the left, click the "+" on the right side of the **entity data model** list, the upload data model option box appears, and download the data model template.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_1a9c8f284579b6b86f082129cfd7aa29.png)
After the data is uploaded, download the csv template and organize the data. Click the data column on the left, click the "+" on the right side of the **entity data model** list, the upload data model option box appears, and download the data model template.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_ce1c8245cdd65309d307f8cadf7e4f0a.png)

The sample\_id column is the unique ID of the sample file, and the other columns are attribute columns. In the attribute column, the attribute can be a specific integer or string. Of course, more often, the attribute column will be associated with the data file on the cloud. Here we use the data model to associate the uploaded dataset files.
a. First, you need to enter the D**ata** and the F**ile lists**;
b. Clicking the link label behind the file, and the URL path and S3 path corresponding to the file will be displayed;
c. Clicking the "Copy" button of the S3 path, and paste the link into the corresponding attribute column in the data model![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_da4551e13e81da31d1707fdc8a92129e.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_aca9f1ce248c18d4a02ca7cf05385a7c.png)
d. Uploading the entity data model: click "+" on the right side of the **entity data model**, the upload data model option box appears, and upload the edited csv file.
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_71e0da1c3adbe9515c58fa92910e82b2.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_39f5f74f17c33acd469a2bb6ca8701a9.png)

3. #### Import workflow
Miracle Cloud uses workflows in Workflow Description Language (WDL) to analysis for genomics data. GATK tools that read input data all expect BAM files as the primary format. Some support the CRAM format, but we observed performance issues when working directly from CRAM files. So we first converted CRAM to BAM.

SAM, BAM, and CRAM are all different forms of the original SAM format defined to hold aligned (or more precisely, mapped) to high-throughput sequencing data. SAM stands for Sequence Alignment/Map format and is described in the standard specification *http://samtools.github.io/hts-specs/*. Both BAM and CRAM are compressed forms of SAM; BAM is a lossless compression, while CRAM can range from lossless to lossy, depending on how much compression you want to achieve (actually most). BAM and CRAM have the same information as their SAM equivalents and are structured in the same way; the difference between them is how the files themselves are encoded.

Next we need to import the Cram-to-Bam workflow into Workspace：
a. Click **Import Workflow**
b. Select **Custom Import**	
c. Enter the corresponding input items (for how to fill in the input, please read the Custom Import in the user manual)
	- Git address： [https://gitee.com/joy\_lee/seq-format-conversion01](https://gitee.com/joy_lee/seq-format-conversion01)
	- Tag: v0.47
	- Main path：CramToBam.wdl
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_9a3880ab48df16757028add31992ad57.png)

Now we have completed the import of the Cram to Bam pipeline workflow.

4. #### Run the workflow
We have just completed the import of the workflow, then we will run the imported workflow.
	a. Associate environment: Click **Environment Management**, select **Workflow** **- Associate Cluster**, and select a cluster from the list of candidate clusters, click Associate.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_ca4869b53885f8d3637850d9af8713a9.png)![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_36fc0db30232908f7e0e1ef88dfd31fa.png)
	b. Select the Cram-to-Bam workflow we just imported
	c. Here we need to configure the run options and parameters . In the Run Options, select the data entity we just uploaded in the first step, and specify the entity data.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_400b0f2c108c9b128f18fe309cbc761d.png)
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_cc0091f1c35f10e6c79f55f5523410ba.png)
	d. Configuring the input parameters, select the **input parameters** tab, and input according to the figure below. "this" indicates the csv file used in this analysis, and the content after "this" is the column name in the corresponding csv. When there are many input parameters, you can use the method of uploading a JSON file for quick import.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_298634d073cac5dacdff1100fe441e95.png)
	e. Configuring **output parameters**, use this.bai as the output attribute column of the bai file, this.bam as the attribute column of the bam file
Through **"this.column"** , the analysis result is written back to the specified column of the data model; if there is no such column in the original table, a new column will be added; if the original table has a corresponding column, the new result will be directly filled or overwritten.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_e5019303c2e5b8a7885acb9eb0da06dc.png)
	f. Click on the upper right corner to start the analysis, the workflow will be analyzed through the Cromwell pipeline engine, and the results can be viewed in the analysis history.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_9f85e5bee607472db86420e95e542e61.png)
### Part 2: Analyzing the BAM from the part 1
In this chapter, we need to use the Bam and Bai files output in the first part for further analysis through the GATK workflow, and finally output the required gvcf files.

1. #### Import GATK workflow
We also need to import a workflow to take a genomic data file in BAM format and convert to gvcf format result files.Click **Workflows** on the left and click **Import Workflow** and fill in the corresponding input fields
	a. Git address：https://gitee.com/joy\_lee/gatk-pipeline
	b. Tag: v0.34
	c. Main path：Hello-GATK.wdl
![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_808d9c08adcc14d09bf477920ffc95e6.png)
2. Run GATK workflow
a. Select **data**: sample, still select NA12878 and my\_sample\_data, we will use the bam file output in the first part as the input for this part.
b. Click the **Input Parameters** tab, select Hello\_GATK.input\_bam, and drop down in Attribute Values to select this.bam,After configuring all inputs as follows:![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_10bbe837fc0fc18c4c8eed49a5b424cf.png)
c. Click the **Output Parameters** tab, where you can fill in the Attribute value of the output\_gvcf variable. To write to the data table, first type "this". Then add a name to the attribute. The analysis will generate a column in the sample data table.	
d. Click the **Start Analyzing** button in the upper right corner to submit your workflow
If your results are submitted correctly, the analysis status of the current workflow can be viewed in the job history. After a while you will see the analysis status change to analysis successful

3. #### View Job History
After waiting for the analysis job to complete, you can click **Job History** on the left menu bar to view the output parameters, and download the gvcf file of the corresponding sample.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_73b568f73f7fb2c29d34d6089813aef2.png)So far, we have also completed the running of two workflows, completed the format conversion from Cram to Bam file, and performed analysis on the Bam sequence through GATK, and obtained the result gvcf file.
### Part 3: Complete Workspace Overview
If you have completed all the workflow and analysis job, you may also want to write a summary or introduction for your Workspace. You can click **Dashboard** on the left. Dashboard also integrates Jupyter Notebook. You can use MarkDown in Jupyter Notebook for document writing. When finished editing, click **Save** and **Cancel editing**.![](https://portal.volccdn.com/obj/volcfe/cloud-universal-doc/upload_d8db47d38adf31a0ed284b34ce65e936.png)

