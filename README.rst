README for SMART
----------------
*Time-stamp: <2018-01-31 10:35:00 Hongbo Liu>*

|PyPI| |License| |Build Status| |Platform|

Introduction
------------
**SMART2** is a newly developed tool for deep analysis of DNA methylation data detected by bisulfite sequencing platforms. This tool is focused on four main functions including **de novo identification of differentially methylated regions (DMRs) by genome segmentation, identification of DMRs from predefined regions of interest, identification of differentially methylated CpG sites, and genome segmentation**. In particular, **SMART2 supports Case-Control DMR calling which is useful for disease methylome analsyis**. It is known that DNA methylation plays important roles in the regulation of cell development and differentiation. DNA methylation/unmethylation mechanisms are common in all tissue/cell. However, different cell types with the same genome have different methylomes. Recently, high-throughput sequencing combining bisulfite treatment (**Bisulfite-Seq**) have been used to generate DNA methylomes from a wide range of human tissue/cell types at a genome-wide perspective. In order to de novo identify DMRs across different biological groups, entropy-based procedures facilitated the quantification of **methylation specificity** for each CpG and the determination of the Euclidean distance and similar entropy for each pair of neighboring CpGs. Subsequently, **genome segmentation** based on these quantified parameters segments the genome into primary segments comprising CpG sites with high methylation similarities across all groups. Further, the primary segments in close proximity and sharing similar methylation patterns were merged into larger segments of different types, including **DMRs** and **non-DMRs** which are identified based on methylation specificity and one-way ANOVA analysis. Eventually, the DMRs with specific hypo-/hypermethylation in the minority of groups, **group-specific hypomethylation marks (HypoMarks)** and **the group-specific hypermethylation marks (HyperMarks)**, are identified using a statistical method. To facilitate the mining of methylation marks across cell types and species. In addition, SMART2 also supports the identification of DMRs from pre-defined regions of interest and differentially methylated CpG sites.

Detailed information about SMART2 is available at http://fame.edbc.org/smart.


New Features of SMART2
----------------------
- **Provides more functions** *~~~~~~~~De novo genome segmentation and identification of differentially methylated regions, predefined regions, and CpG sites.*
- **Provides Case-Control DMR calling** *~~~~~~~~SMART2 supports Case-Control DMR calling just by inputing Case-Control pairs via a case-control matrix*
- **Provides statistical analysis** *~~~~~~~One-way ANOVA analysis was added to identify the DMRs which is more reliable.* 
- **Supports various BS-Seq data** *~~~~~~SMART2 supports data analysis for WGBS, RRBS, and targeting bisulfite sequencing techniques including TruSeqEPIC, SureSelect and CpGiant.*
- **Supports replicate samples** *~~~~~~~~~~SMART2 supports replicates for the same group such as samples from the same clinical group.*
- **Supports missing value replacement** *~~~~SMART2 supports the replacement of missing value via the median methylation value of other available samples from the same group.*
- **Be applicable to any species** *~~~~~~~~~~~~Re-designed algorithm workflow makes SMART2 is applicable for any species.*
- **Multiprocessing speeds up analysis** *~~~~~~~Re-designed algorithm workflow makes SMART2 is more quick for huge data analysis.*

Install
-------
.. code-block:: bash

    $ pip install SMART-BS-Seq

`Detailed information can be found in the file 'INSTALL' in the distribution.`


Usage of SMART2
---------------
.. code-block:: bash

    $ SMART MethylMatrix [-t {DeNovoDMR,DMROI,DMC,Segment}]
                         [-r REGION_OF_INTEREST] [-c CASE_CONTROL_MATRIX]
                         [-n PROJECT_NAME] [-o OUTPUT_FOLDER] [-MR MISS_VALUE_REPLACE]
                         [-AG PERCENTAGE_OF_AVAILABLE_GROUPS]
                         [-MS METHYLATION_SPECIFICITY] [-ED EUCLIDEAN_DISTANCE]
                         [-SM SIMILARITY_ENTROPY] [-CD CPG_DISTANCE] [-CN CPG_NUMBER]
                         [-SL SEGMENT_LENGTH] [-PD P_DMR] [-PM P_METHYLMARK]
                         [-PC P_DMR_CASECONTROL] [-AD ABSMEANMETHDIFFER] [-v] [-h]

If above command doesn't work, you can try one of the following solutions:

**(1) Add SMART command to system path**

.. code-block:: bash

    Linux$ export PATH=/usr/local/bin:$PATH
    MacOS$ export PATH=/Library/Frameworks/Python.framework/Versions/2.7/bin:$PATH

Then rerun SMART command as described above.

**(2) Run the source code which can be found in the installation directory**

Installation directory of SMART:

- Linux (Ubuntu 16.04): */usr/local/lib/python2.7/dist-packages/SMART/*
- macOS (Sierra 10.12): */Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/SMART/*

.. code-block:: bash

    $ cd /usr/local/lib/python2.7/dist-packages/SMART/ 
    $ python SMART.py MethylMatrix [-t {DeNovoDMR,DMROI,DMC,Segment}]
                                   [-r REGION_OF_INTEREST] [-c CASE_CONTROL_MATRIX]
                                   [-n PROJECT_NAME] [-o OUTPUT_FOLDER] [-MR MISS_VALUE_REPLACE]
                                   [-AG PERCENTAGE_OF_AVAILABLE_GROUPS]
                                   [-MS METHYLATION_SPECIFICITY] [-ED EUCLIDEAN_DISTANCE]
                                   [-SM SIMILARITY_ENTROPY] [-CD CPG_DISTANCE] [-CN CPG_NUMBER]
                                   [-SL SEGMENT_LENGTH] [-PD P_DMR] [-PM P_METHYLMARK]
                                   [-PC P_DMR_CASECONTROL] [-AD ABSMEANMETHDIFFER] [-v] [-h]


Positional arguments
^^^^^^^^^^^^^^^^^^^^
**MethylMatrix**
    The input methylation file (such as /Example/MethylMatrix.txt) including methylation values in all samples to compare (REQUIRED). The methylation data should be arranged as a matrix in which each row represents a CpG site. The columns are tab-separated. The column names should be included in the first line, with the first three columns representing the location of CpG sites: chrome, start, end. The methylation values start from the fourth column. And the methylation value should be between 0 (unmethylated) to 1 (fully methylated). The missing values should be shown as -. The names of samples should be given as G1_1,G1_2,G2_1,G2_2,G3_1,G3_2,G3_3, in which Gi represents group i. The Methylation matrix can be build based on bed files (chrome start end betavalue) by bedtools as: bedtools unionbedg -i G1_1.bed G1_2.bed G2_1.bed G2_2.bed G3_1.bed G3_2.bed G3_3.bed -header -names G1_1 G1_2 G2_1 G2_2 G3_1 G3_2 G3_3 -filler - > MethylMatrix.txt. [Type: file]

    The example data is also available `here <http://fame.edbc.org/smart/Example_Data_for_SMART2.zip>`_.

Optional arguments
^^^^^^^^^^^^^^^^^^
**-t {DeNovoDMR,DMROI,DMC,Segment}**
    Type of project including 'DeNovoDMR','DMROI', 'DMC' and 'Segment'. DeNovoDMR means de novo identification of differentially methylated regions (DMRs) based on genome segmentation. DMROI means the comparison of the methylation difference in regions of interest (ROIs) across multiple groups. DMC means identification of differentially methylated CpG sites (DMCs). It should be noted DMC is time-consuming for whole-renome methylation data. Segment means de novo segmentation of genome based on DNA methylation in all samples [Type: string] [DEFAULT: 'DeNovoDMR']
**-r Region_of_interest**
    Genome regions of interest in bed format without column names (such as /Example/Regions_of_interest.bed) for project type DMROI (REQUIRED only for DMROI). The regions in the file should be sorted by chromosome and then by start position (e.g., sort -k1,1 -k2,2n in.bed > in.sorted.bed). If this file is provided, SMART treat each region as a unit and compare its mean methylation across groups by methylation specificity and ANOVA analysis. This parameter is only for project type DMROI. DEFAULT: '' [Type: string]
**-c Case_control_matrix**
    Case-control matrix in txt format without column names (such as /Example/Case_control_matrix.txt) for project types DeNovoDMR and DMROI. In this file, each row represents a pairwise comparison between a case group (first column) and a control group (second column). Two columns should be tab-separated. If this file is provided, SMART will carry out DMR calling for each pairwise based on two sample t test. The results would be output to 7_DMR_Case_control.txt (DeNovoDMR) or 7_DMROI_Case_control.txt (DMROI) treat each region as a unit and compare its mean methylation across groups by methylation specificity and ANOVA analysis. This parameter is only for project types DeNovoDMR and DMROI. DEFAULT: '' [Type: string]
**-n Project_Name**
    Project name, which will be used to generate output file names. This parameter is for all project types. DEFAULT: "SMART" [Type: string]
**-o Output_Folder** 
    The folder in which the result will be output. If specified all output files will be written to that directory. This parameter is for all project types. [Type: folder] [DEFAULT: the directory named using project name and current time (such as SMART20140801132559) in the current working directory]
**-MR Miss_Value_Replace**
    Replace the missing value with the mediate methylation value of available samples in the corresponding group. The user can control whether to replace missing value by setting this parameter from 0.01 (meaning methylation values are available in at least 1% of samples in a group) to 1.0 (meaning methylation values are available in 100% of samples in a group, i.e there is no missing values). This parameter is for all project types. [Type: float] [Range: 0.01 ~ 1.0] [DEFAULT: 0.5]
**-AG Percentage_of_Available_Groups**
    Percentage of available groups after missing value replacement. The user can use this parameter to filter CpG sites those have not enough available methylation levels by setting this parameter from 0.01 (meaning methylation values are available in at least 1% of groups) to 1.0 (meaning methylation values are available in 100% of groups, i.e there is no missing values). This parameter is for all project types. [Type: float] [Range: 0.01 ~ 1.0] [DEFAULT: 1.0]
**-MS Methylation_Specificity**
    Methylation Specificity Threshold for DMC or DMR calling. This parameter can be used to identify DMC or DMR as the CpG site or region with methylation specificity which is greater than the threshold. This parameter is for all project types. [Type: float] [Range: 0.2 ~ 1.0] [DEFAULT: 0.5]
**-ED Euclidean_Distance**
    Euclidean Distance Threshold for methylation similarity between neighboring CpGs which is used in genome segmentation and de novo identification of DMR. The methylation similarity between neighboring CpGs is high if the Euclidean distance is less than the threshold. This parameter is only for project types DeNovoDMR and Segment. [Type: float] [Range: 0.01 ~ 0.5] [DEFAULT: 0.2]
**-SM Similarity_Entropy**
    Similarity Entropy Threshold for methylation similarity between neighboring CpGs which is used in genome segmentation and de novo identification of DMR. The methylation similarity between neighboring CpGs is high if similarity entropy is less than the threshold. This parameter is only for project types DeNovoDMR and Segment. [Type: float] [Range: 0.01 ~ 1.0] [DEFAULT: 0.6]
**-CD CpG_Distance**
    CpG Distance Threshold for the maximal distance between neighboring CpGs which is used in genome segmentation and de novo identification of DMR. The neighboring CpGs will be merged if the distance less than this threshold. This parameter is only for project types DeNovoDMR and Segment. [Type: int] [Range: 1 ~ 2000] [DEFAULT: 500]
**-CN CpG_Number**
    Segment CpG Number Threshold for the minimal number of CpGs of merged segment and de novo identified DMR. The segments/DMRs with CpG number larger than this threshold will be output for further analysis. This parameter is only for project types DeNovoDMR and Segment. [Type: int] [Range: > 1] [DEFAULT: 5]
**-SL Segment_Length**
    Segment Length Threshold for the minimal length of merged segment and de novo identified DMR. The segments/DMRs with a length larger than this threshold will be output for further analysis. This parameter is only for project types DeNovoDMR and Segment. [Type: int] [Range: > 1] [DEFAULT: 20]
**-PD p_DMR**
    p value of one-way analysis of variance (ANOVA) which is carried out for identification of or DMCs or DMRs across multiple groups. The segments with p value less than this threshold are identified as DMC or DMR. This parameter is for all project types. [Type: float] [Range: 0 ~ 1] [DEFAULT: 0.05]
**-PM p_MethylMark**
    p value of one sample t-test which is carried out for identification of Methylation mark in a specific group based on the identified DMRs. The DMRs with p value less than this threshold is identified as group- specific methylation mark (Hyper methylation mark or Hypo methylation mark). This parameter is only for project types DeNovoDMR and DMROI [Type: float] [Range: 0 ~ 1] [DEFAULT: 0.05]
**-PC p_DMR_CaseControl**
    p value of two-sample t-test which is carried out for identification of DMRs between case group and control group. The region with p value less than this threshold is identified DMR. This parameter is only for project types DeNovoDMR and DMROI. [Type: float] [Range: 0 ~ 1] [DEFAULT: 0.05]
**-AD AbsMeanMethDiffer**
    Absolute mean methylation difference between case group and control group. The region with absolute mean methylation difference more than this threshold is identified DMR. The DMR showing hyper methylation in case group is identified as Hyper mark and that showing hypo methylation in case group is identified as Hypo mark. This parameter is only for project types DeNovoDMR and DMROI. [Type: float] [Range: 0 ~ 1] [DEFAULT: 0.3]
**-v, --version**
    Show program's version number and exit
**-h, --help**
    Show this help message and exit

Example
-------
Example data
^^^^^^^^^^^^
The example data can be found in the directory Example under the installation directory of SMART, and is also available `here <http://fame.edbc.org/smart/Example_Data_for_SMART2.zip>`_. In this example, 10,000 CpG sites in each of human chromosomes were extracted for the test of SMART. The user can use the following command to test SMART.
It should be noted that the location of installation directory of SMART may be different in different Operating System.

- Linux (Ubuntu 16.04): */usr/local/lib/python2.7/dist-packages/SMART/*
- macOS (Sierra 10.12): */Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/SMART/*


Example command
^^^^^^^^^^^^^^^
.. code-block:: bash

    Linux$ cd /usr/local/lib/python2.7/dist-packages/SMART/
    macOS$ cd /Library/Frameworks/Python.framework/Versions/2.7/lib/python2.7/site-packages/SMART/
    $ SMART ./Example/MethylMatrix_Test.txt -t DeNovoDMR -o ./Example/
    $ SMART ./Example/MethylMatrix_Test.txt -t DeNovoDMR -c ./Example/Case_control_matrix.txt -o ./Example/
    $ SMART ./Example/MethylMatrix_Test.txt -t DMROI -r ./Example/CpGisland_hg19.bed -o ./Example/
    $ SMART ./Example/MethylMatrix_Test.txt -t DMROI -r ./Example/CpGisland_hg19.bed -c ./Example/Case_control_matrix.txt -o ./Example/
    $ SMART ./Example/MethylMatrix_Test.txt -t DMC -o ./Example/
    $ SMART ./Example/MethylMatrix_Test.txt -t Segment -o ./Example/


Output Files
------------

The results for **DeNovoDMR** are given in the folder **DeNovoDMR** Folder including:

- **1_AvailableCpGs.txt** ~ *CpG sites with available methylation in all groups*
- **2_MergedSegment.bed** ~ *Merged segments based on small segments for visualization in UCSC browser*
- **3_MergedSegment_Methylation.txt** ~ *Merged segments with DNA methylation in samples and groups*
- **4_DMR_Methylation.txt** ~ *DMRs identified by SMART*
- **5_DMR_Group_Specificity.txt** ~ *Group specificity of DMRs*
- **6_DMR_Methylmark.txt** ~ *Group specific methylation marks*
- **7_DMR_Case_control.txt** ~ *DMRs between Case group and Control group*
- **Summary.txt** ~ *Summary of SMART analysis*

The results for **DMROI** are given in the folder **DMROI** Folder including:

- **1_AvailableCpGs.txt** ~ *CpG sites with available methylation in all groups*
- **2_ROI.bed** ~ *ROIs in bed format for visualization in UCSC browser*
- **3_ROI_Methylation.txt** ~ *ROIs and their methylation levels in samples and groups*
- **4_DMROI_Methylation.txt** ~ *Differentially methylated ROIs with methylation values*
- **5_DMROI_Group_Specificity.txt** ~ *Differentially methylated ROIs with group specificity*
- **6_DMROI_Methylmark.txt** ~ *Group specific methylation marks of DifferMethlROIs*
- **7_DMROI_Case_control.txt** ~ *DMROIs between Case group and Control group*
- **Summary.txt** ~ *Summary of SMART analysis*

The results for **DMC** are given in the folder **DifferMethlCpG** Folder including:

- **1_AvailableCpGs.txt** ~ *CpG sites with available methylation in all groups*
- **2_DifferMethlCpGs.txt** ~ *Differentially methylated CpG sites*
- **Summary.txt** ~ *Summary of SMART analysis*

The results for **Segment** are given in the folder **Segment** Folder including:

- **1_AvailableCpGs.txt** ~ *CpG sites with available methylation in all groups*
- **2_MergedSegment.bed** ~ *Merged segments based on small segments for visualization in UCSC browser*
- **3_MergedSegment_Methylation_Sample.txt** ~ *Merged segments with DNA methylation in samples*
- **4_MergedSegment_Methylation_Group.txt** ~ *Merged segments with DNA methylation in groups*
- **Summary.txt** ~ *Summary of SMART analysis*


Other useful links
------------------
:SMART: http://fame.edbc.org/smart/
:Forum: https://groups.google.com/forum/#!forum/smart-announcement
:QDMR:  http://fame.edbc.org/qdmr/


Citation
--------
Hongbo Liu et al. *Systematic identification and annotation of human methylation marks based on bisulfite sequencing methylomes reveals distinct roles of cell type-specific hypomethylation in the regulation of cell identity genes.* Nucleic Acids Res: 2016 ,44(1),75-94.

Contact
-------
:For any help:  you are welcome to write to Hongbo Liu (hongbo919@gmail.com) at http://cce.edbc.org/members/HongboLiu.html.

.. url-marker
.. |PyPI| image:: https://img.shields.io/pypi/v/SMART-BS-Seq.svg
   :target: https://pypi.python.org/pypi/SMART-BS-Seq
.. |License| image:: https://img.shields.io/pypi/l/Django.svg?style=plastic   :target: 
.. |Build Status| image:: https://img.shields.io/pypi/status/Django.svg?style=plastic   :target: 
.. |Platform| image:: https://img.shields.io/conda/pn/conda-forge/python.svg
   :target: https://pypi.python.org/pypi/SMART-BS-Seq