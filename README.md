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

Julip produces high quality in introns in two stages. Stage 1 collect the introns from multiple samples extracted from spliced read alignments, filtered the low quality introns. Stage 2 use Lasso regulated linear programming to determine a set of introns for each gene.

determine the gene regions by introns and mapped regions


### <a name="usage"></a> Usage

Sequantial version:  
```
Usage: java -jar JULiP.jar [options]  
Options:  
 -model VAL           : generateSpliceAndRegionFiles: generate splice & region files  
                        generateIntronFiles: generate intron files  
                        intronsSelection: do intron selection  
 -gene VAL            : geneId ID  
 -geneList VAL        : a file contain the gene names  
 -noiseLevel N        : noise level (default: 6)  
 -outFile VAL         : a file contain the paths of the output file  
 -p N                 : p-norm (default: 1.0)  
 -stepSize N          : step size (default: 1.0)  
 -bamFileList VAL     : a file contain the paths of the subExon files  
 -regionFileList VAL  : a file contain the paths of the region files  
 -spliceFileList VAL  : a file contain the paths of the splice files 
 -intronFileList VAL  : a file contain the paths of the intron files  
```
Parallel version:  
```
Usage: hadoop jar JULiPForHadoop.jar [options]  
Options:  
 -intronFileList VAL  : a file contain the paths of the intron files  
```
Configure Hadoop:  
See this official link for setting up a single node cluster,  
https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/SingleCluster.html  
and this link for cluster setup,  
https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/ClusterSetup.html  

### <a name="inputoutput"></a> Input/Output  
The primary input to CLASS is a set of short read alignments in BAM format and sorted by chromosome and position, for instance one produced with the program Tophat2 (http://tophat.cbcb.umd.edu).

Given a set of alignment input x.bam files, JULiP produces two type of intermediate data files, regions.txt, splices.txt and introns.txt in the .bam file directory.

- The format of the regions.txt file is:  
chrom_id position #_of_reads_on_the_position  

- The format of the splices.txt file is:  
chrom_id start_intron_position end_intron_position #_of_supporting_reads strand  

- The format of the introns.txt file is:
chrom_id start_intron_position end_intron_position #_of_supporting_reads strand

The final output, consisting of selected introns.
- The format of the output file is:
chrom_id start_intron_position end_intron_position selection_flag


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
