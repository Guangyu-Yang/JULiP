### <a name="julip"></a> JULiP - An efficient model for accurate intron selection from multiple RNA-seq samples

Copyright (C) 2016-2018, and GNU GPL, by Guangyu Yang, Liliana Florea

Includes portions copyright from:

SAMtools - Copyright (C) 2008-2009, Genome Research Ltd, Heng Li


### <a name="table-of-contents"></a> Table of contents

- [What is JULiP?](#what-is-julip)  
- [Usage](#usage)  
- [Input/Output](#inputoutput)  
- [Example](#example)   
- [Support](#support)  


### <a name="what-is-julip"></a> What is JULiP?

JULiP selects a comprehensive and accurate set of introns simultaneously from multiple RNA-seq samples. 

JULiP produces a reliable set of introns in three stages. Stage 1 collects the introns from multiple samples, extracted from spliced read alignments, filtering low quality introns. Stage 2 divides the introns across the genome into 'gene' regions. Stage 3 uses a Lasso-regularized program to determine a set of introns for each gene region.


### <a name="usage"></a> Usage

Sequential version:  
```
Usage: java -jar julip.jar [options]  
Options:  
 -model VAL       : generateSplicesAndRegions: generate splices & regions
                    generateIntronFiles: generate intron files
                    intronsSelection: perform intron selection 
 -bamFileList VAL : file containing the paths to the bam files
 -clean VAL       : true: delete the temporary files; false: keep the
                    temporary files (default: true)
 -outFile VAL     : file storing the output of JULiP
```
Parallel version:  
```
Usage: hadoop jar julipForHadoop.jar [options]
Options:
 -intronFileList VAL  : file containing the paths to the intron files
```
Configure Hadoop:  

See this official link for setting up a single node cluster:  
https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/SingleCluster.html  
and this link for cluster setup:  
https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/ClusterSetup.html  

### <a name="inputoutput"></a> Input/Output  
The primary input to JULiP is a set of RNA-seq alignment files, each in BAM format and sorted by chromosome and position, for instance as produced with the program [Tophat2](http://tophat.cbcb.umd.edu).

Given a collection of input alignment file {x}.bam files, JULiP generates three types of intermediate data files in each of the directories hosting the BAM files, namely: {x}.region ('gene' regions), {x}.splice (candidate introns) and {x}.intron (selected introns). Additionally, it generates the file gene.txt, containing all the detected gene regions, and three list files, containing the paths to the .region, .splice and .intron files for each input data set, in the working directory.

- The format of the x.region file is:  
chrom_id position #_of_reads_on_the_position  

- The format of the x.splice file is:  
chrom_id start_intron_position end_intron_position #_of_supporting_reads strand  

- The format of the x.intron file is:  
chrom_id start_intron_position end_intron_position #_of_supporting_reads strand

The final output, consisting of selected introns.
- The format of the output file is:  
chrom_id start_intron_position end_intron_position selection_flag gene_region_id beta reads_in_samples


### <a name="example"></a> Example  
Sequential version:  
```
java -jar julip.jar \
	 -model generateSplicesAndRegions \
	 -bamFileList bamFileList.txt

java -jar julip.jar \
	 -model generateIntronFiles

java -jar julip.jar \
     -model intronsSelection \
     -outFile introns_selection_result.txt
```
Parallel version:  
```
hadoop fs -mkdir -p /user/hadoop
hadoop jar julipForHadoop.jar -intronFileList intronFileList.txt
hadoop fs -cat  /user/hadoop/splice_results/* > introns.txt.uniq
```

### <a name="support"></a> Support

Contact: gyang22@jhu.edu, florea@jhu.edu
