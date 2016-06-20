### <a name="julip"></a> JULiP - An efficient model for accurate intron selection from multiple RNA-seq samples

Copyright (C) 2016-2018, and GNU GPL, by Guangyu Yang, Liliana Florea

Includes portions copyright from:

SAMtools - Copyright (C) 2008-2009, Genome Research Ltd, Heng Li


### <a name="table-of-contents"></a> Table of contents

- [JULiP - An efficient model for accurate intron selection from multiple RNA-seq samples](#julip)  
- [Table of contents](#table-of-contents)  
- [What is JULiP?](#what-is-julip)  
- [Usage](#usage)  
- [Input/Output](#inputoutput)  
- [Example](#example)   
- [Support](#support)  


### <a name="what-is-julip"></a> What is JULiP?

JULiP is a program for Intron selection from RNA-seq reads aligned to a genome from multi samples. 

JULiP generate reliable introns in two stages. Stage 1 collect the introns from multiple samples extracted from spliced read alignments, filter the low quality introns, detect gene regions. Stage 2 use Lasso regulated linear programming to determine a set of introns for each gene region.


### <a name="usage"></a> Usage

Sequantial version:  
```
Usage: java -jar JULiP.jar [options]  
Options:  
 -model VAL           : generateSpliceAndRegionFiles: generate splice & region files  
                        generateIntronFiles: generate intron files  
                        intronsSelection: do intron selection  
 -gene VAL            : gene region id, "all" for all gene regions  
 -geneList VAL        : the file contains the gene region ids  
 -noiseLevel N        : noise level (default: 6)  
 -outFile VAL         : the file storing the output of JULiP  
 -p N                 : p-norm (default: 1.0)  
 -stepSize N          : step size (default: 1.0)  
 -bamFileList VAL     : the file contains the paths of the bam files  
 -regionFileList VAL  : the file contains the paths of the region files  
 -spliceFileList VAL  : the file contains the paths of the splice files 
 -intronFileList VAL  : the file contains the paths of the intron files  
```
Parallel version:  
```
Usage: hadoop jar JULiPForHadoop.jar [options]  
Options:  
 -noiseLevel N        : noise level (default: 6)  
 -p N                 : p-norm (default: 1.0)  
 -stepSize N          : step size (default: 1.0)  
 -intronFileList VAL  : the file contains the paths of the intron files  
```
Configure Hadoop:  
See this official link for setting up a single node cluster,  
https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/SingleCluster.html  
and this link for cluster setup,  
https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/ClusterSetup.html  

### <a name="inputoutput"></a> Input/Output  
The primary input to JULiP is a set of short read alignments in BAM format and sorted by chromosome and position, for instance one produced with the program Tophat2 (http://tophat.cbcb.umd.edu).

Given a set of alignment input x.bam files, JULiP produces three type of intermediate data files, regions.txt, splices.txt and introns.txt in the .bam file directory. An gene.txt (contains all the detected gene region) and three list files (contain the paths of the regions.txt, splices.txt and introns.txt) in working directory.

- The format of the regions.txt file is:  
chrom_id position #_of_reads_on_the_position  

- The format of the splices.txt file is:  
chrom_id start_intron_position end_intron_position #_of_supporting_reads strand  

- The format of the introns.txt file is:  
chrom_id start_intron_position end_intron_position #_of_supporting_reads strand

The final output, consisting of selected introns.
- The format of the output file is:  
chrom_id start_intron_position end_intron_position selection_flag gene_region_id

NOTE: place .bam files in different directories.


### <a name="example"></a> Example  
Sequential version:  
```
java -jar JULiP.jar \  
	 -model generateSplicesAndRegions \  
	 -bamFileList bamFileList.txt  

java -jar JULiP.jar \  
	 -model generateIntronFiles  

java -jar JULiP.jar \  
     -model intronsSelection \  
     -gene all \  
     -geneList genes.txt \  
     -outFile introns_selection_result.txt  
```
Parallel version:  
```
hadoop fs -mkdir -p /user/hadoop  
hadoop jar JULiPForHadoop.jar -intronFileList intronFileList.txt  
hadoop fs -cat  /user/hadoop/splice_results/* > introns.txt.uniq  
```

### <a name="support"></a> Support

Contact us at: gyang22@jhu.edu, florea@jhu.edu
