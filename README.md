## JULiP - An efficient model for accurate intron selection from multiple RNA-seq samples

Copyright (C) 2012-2016, and GNU GPL, by Guangyu Yang, Liliana Florea

Includes portions copyright from:

SAMtools - Copyright (C) 2008-2009, Genome Research Ltd, Heng Li


## Table of contents

- JULiP - An efficient model for accurate intron selection from multiple RNA-seq samples  
- Table of contents  
- What is CLASS?  
- Usage  
- Input/Output  
- Example  
- Versions  
- Terms of use  
- Support  


## What is JULiP?

JULiP is a program for Intron selection from RNA-seq reads aligned to a genome from multi samples. 

Julip produces high quality in introns in two stages. Stage 1 collect the introns from multiple samples extracted from spliced read alignments, filtered the low quality introns. Stage 2 use Lasso regulated linear programming to determine a set of introns for each gene.

determine the gene regions by introns and mapped regions


## Usage

Sequantial version:  
Usage: java -jar FeatureSelection.jar [options]  
Options:  
 -bamFileList VAL     : a file contain the paths of the subExon files  
 -gene VAL            : geneId ID  
 -geneList VAL        : a file contain the gene names  
 -intronFileList VAL  : a file contain the paths of the intron files  
 -model VAL           : generateSpliceAndRegionFiles: generate splice & region files  
                        generateIntronFiles: generate intron files  
                        intronsSelection: do intron selection  
 -noiseLevel N        : noise level (default: 6)  
 -outFile VAL         : a file contain the paths of the output file  
 -p N                 : p-norm (default: 1.0)  
 -regionFileList VAL  : a file contain the paths of the region files  
 -spliceFileList VAL  : a file contain the paths of the splice files  
 -stepSize N          : step size (default: 1.0)  
 -subExonFileList VAL : a file contain the paths of the subExon files  

Parallel version:  
configure Hadoop  
See this official link for setting up a single node cluster  
https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/SingleCluster.html  
and this link for cluster setup: https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/ClusterSetup.html  

## Input/Output  
The primary input to CLASS is a set of short read alignments in BAM format and sorted by chromosome and position, for instance one produced with the program Tophat2 (http://tophat.cbcb.umd.edu).

Given a set of alignment input .bam files, JULiP produces two intermediate data files, x.depth and x.splice in the temporary working directory.


## Example  
Sequential version:  
```
java -jar FeatureSelection.jar \  
	 -model generateSplicesAndRegions \  
	 -bamFileList bamFileList.txt  

java -jar FeatureSelection.jar \  
	 -model generateIntronFiles  

java -jar FeatureSelection.jar \  
     -model intronsSelection \  
     -gene all \  
     -geneList genes.txt \  
     -outFile introns_selection_result.txt  
```
Parallel version:  
```
hadoop fs -mkdir -p /user/hadoop  
hadoop jar FeatureSelectionForHadoop.jar  
hadoop fs -cat  /user/hadoop/splice_results/* > introns.txt.uniq  
```

## Terms of use  

This program is free software; you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation; either version 2 of the License, or (at your option) any later version.  
This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU General Public License for more details.  
You should have received (LICENSE.txt) a copy of the GNU General Public License along with this program; if not, you can obtain one from http://www.gnu.org/licenses/gpl.txt or by writing to the Free Software Foundation, Inc., 59 Temple Place, Suite 330, Boston, MA 02111-1307 USA  


## Support

Contact us at: gyang22@jhu.edu, florea@jhu.edu
