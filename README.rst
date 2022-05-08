========================
README for SMART (1.0.4)
========================
Time-stamp: <2014-08-02 10:25:37 Hongbo Liu>

Introduction
============

It is known that DNA methylation plays important roles in regulation
of cell development and differentiation. DNA methylation/unmethylation
mechanisms are common in all tissue/cell. However, different cell 
types with the same genome have different methylomes. Recently,
high-throughput sequencing combining bisulfite treatment (Bisulfite
-Seq) have been used to generate DNA methylomes from a wide range of
human tissue/cell types at a genome-wide perspective. To characterize
the genome regions that consist of continuous CpGs with similar 
methylation specificity, we developed the Specific Methylation Analysis
and Report Tool (SMART) based on the quantified methylation specificity,
Euclidean distance and similarity entropy. SMART aims to segment the 
genome into different functional regions based on the methylation
pattern in multiple cell types. Continuous scanning is firstly carried
out to obtain the primary segments composed by CpG sites with high 
methylation similarity across all cell types. Then, the primary segments
those localize in close proximity and share similar methylation pattern
are merged into different types of segments including high specificity 
segment (HighSpe), low specificity segment (LowSpe) and almost no 
cell-specificity segment (NoSpe). At last, the cell-type-specific 
methylation marks were identified to facilitate the epigenetic analysis.

Install
=======

Please check the file 'INSTALL' in the distribution.

Usage of SMART
==============

:usage: SMART MethyDir CytosineDir [-h] [-n PROJECTNAME] [-o OUTPUTFOLDER] [-v]  

Examples
-----------------------
If not export SMART bin to PATH, change directory to the SMART bin
````````````````````````````````````````````````````````````````````````````````````````````````````````````````````

python SMART /home/hbliu/Example/BSSeq_fortest/ /home/hbliu/Example/CLoc_hg19_fortest/ -n Test -o /home/hbliu/Example/Example_Results/



If have exported SMART bin to PATH, you can carry out analysis in any directory
````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````````

SMART /home/hbliu/Example/BSSeq_fortest/ /home/hbliu//Example/CLoc_hg19_fortest/ -n Test -o /home/hbliu/Example/Example_Results/

positional arguments
-----------------------
MethyDir
```````````````
The directory (such as /liuhb/BSSeq/) of the folder including methylation data files formated in wig.gz (such as H1.wig.gz). REQUIRED.

CytosineDir
``````````````````
The directory (such as /liuhb/CLoc_hg19/) of the folder including cytosine location files for all chromesomes formated in txt.gz (such as chr1.txt.gz). REQUIRED.

optional arguments
----------------------
-h, --help
``````````````````
show this help message and exit

-n PROJECTNAME
`````````````````````````````
Project name, which will be used to generate output file names. DEFAULT: "SMART"

-o OUTPUTFOLDER
````````````````````````````````
If specified all output files will be written to that directory. Default: the directory named using projectname and currenttime (such as SMART20140801132559) in the current working directory.

-v, --version
```````````````````
show program's version number and exit

Output Files 
==============
1. Folder SplitedMethy is a a output directory to store the splited Methylation data.
   The methylation data are stored in different chromosome sub-folders. In each
   sub-folder, the methylation data for all samples are included. 
2. Folder MethylationSpecificity is a output directory to store the methylation
   levels and specifity for each C which is common across all samples. These files are
   stored in chromosomes. In this folder, MethylationSpecificity.wig.gz includes
   the methylation specifity of all common C. And this file can be uploaded to UCSC
   browser for visualization.
3. Folder MethylationSegment includes three sub-folders: GenomeSegment, GenomeSegmentMethy,
   and MergedGenomeSegment. The sub-folder GenomeSegment stores all small segments
   identified by SMART in each chromosome. And the sub-folder GenomeSegmentMethy stores
   the methylation levels of each small segments across all samples which may be useful for
   users' local further analysis. The sub-folder MergedGenomeSegment stores the larger 
   segments merged based on the small segments in each chromosome. The final results are
   generated based on these merged segments.
4. Folder FinalResults includes all intresting results which may be concerned by users.
   In this folder, there are six files. 

   -The first file 1SmallSegmentBed.txt.gz stores all small segments in bed format,  which  can be uploaded to UCSC browser for visualization.

   -The second file 2MergedSegmentBed.txt.gz stores all merged segments in bed format, which  can be uploaded to UCSC browser for visualization.

   -The third file 3MergedSegment.txt stores all merged segments in txt format, which is useful  for local further analysis.

   -The fourth file 4MergedSegmentwithmethylation.txt stores the methylation levels of all  merged segments across all samples, which is useful for local further analysis.

   -The fifth file 5MergedHighLowSpeSegmentwithspecificity.txt stores the methylation specificity and p values of t-test for each merged HighSpe/LowSpe segement, which is useful for further analysis on cell-type-specificity for each HighSpe/LowSpe segement. The positive p value represents the segment is hyper-methylated in the corresbonding cell-type, while the negative p value represents the segment is hypo-methylated in the corresbonding cell-type.

   -The sixth file 6CellTypeSpecificMethymarkPvalue.txt is a reformated file for the fifth file. In this file, only the HighSpe/LowSpe segements which show significant hypo- or hyper-methylation in some cell-types are remained. This file is usefull for users to select and analyze cell-type-specific methylation marks including HypoMarks and HyperMarks.

Other useful links
==================
:Homepage: http://fame.edbc.org/smart/
:Example data: http://methymark.edbc.org/SMART/Example.html
:PyPI package: https://pypi.org/project/SMART-BS-Seq/
:QDMR: http://fame.edbc.org/qdmr/

Contact 
==================
:For any help:  you are welcome to write to Hongbo Liu (hongbo919@gmail.com).
