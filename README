
 Reprever: REPeat REsolVER - find and reconstruct extra copies given copy number gain regions.

 User Manual.
	
   0. Quick synopsis
   I. Requirements
  II. Preparing input data
 III. Running Reprever
  IV. Options and parameters
   V. Output files
  VI. Examples



 0. Quick synopsis

 Note: Please read I. Requirements before running 'Reprever' first time.
 After getting all requirements, you can run Reprever like below:

 >java -jar Reprever.jar -R reference.fa -C input.bam -I cnv_region.txt -g2 reference.2bit

 I. Requirements

 	To run Reprever, the following programs are required and must be prepared before.
	
	1. Blat server/client program (gfServer and gfClient): 	
 	'gfServer' and 'gfClient' must be installed before running Reprever. The executable can be downloaded from UCSC Genome
	browser's download page (http://hgdownload.cse.ucsc.edu/admin/exe/), under "blat" directory. Download them, 
	and set appropriate path. You should be able to run both programs anywhere in your local machine.

	2. faToTwoBit:
	'faToTwoBit' can be downloaded from UCSC genome browser page (http://hgdownload.cse.ucsc.edu/admin/exe/). Select
	your platform and you will find the executable. 

	3. samtools:
	samtools are required to sort and index your resources. Download it from here (http://samtools.sourceforge.net/), or
	you can simply install it by executing in Linux platform.
	
	>sudo apt-get install samtools


 II. Preparing input data

	Reprever requires majorly 3(three) kinds of data: 1) Reference genome (fasta) and it's index files, 2) Paired-end 
	read alignments (bam) and its index, and 3) a list of candidate (copy number increased) regions

	1. Reference genome: 
	fasta format reference genome (.fa or .fasta) is needed. Additionally, you must generated 2(two) more files from it.

		A. 2bit file: 2bit file can be generated from fasta file. Try,
			>faToTwobit [reference].fa [reference].2bit

		B. fai index: fai index (.fai) can be generated using 'samtools faidx'. Try,
			>samtools faidx [reference].fa
		You will have [reference].fa.fai shortly. 

	2. Paired-end alignment:
	the sam/bam format (bam is recommended) data must be sorted by coordinate. If you are not sure about this, you can sort
 	it using samtools,
		
		>samtools sort [alignment].bam [alignment_sorted]
		You will get [alignment_sorted].bam. Be sure not to use '-n' option.
	
	When sorted, run 'samtools index' to generated an index file.
		>samtools index [alignment_sorted].bam
		You will get [alignment_sorted].bam.bai. 

	3. Candidate (CNV - copy number increased) regions. 
	In the file, each line represents one region. Chromosome, start, and end position are tab-delimmited in the file.
	Columns after 4 can be anything; you may annotate the regions. 
	For exmaple, a quick look of [regions].txt is like below:
	
		...
		chr1	34878343	34879955	+
		chr9	11241520	11249980	+
		chrX	111442	120091	+
		...

	Be sure that, the name of chromosome (chr1, chr9, chrX) match the ones in the referece and alignment file.
	Sometimes just a number (instead of preceeding "chr") is used as contig name. 
	To check the contig name, you can try:

		>cat [reference].fa.fai
		This will print out all the contigs in the reference.
		
		>samtools view -H [alignment].bam
		This will show the header part of your alignment data. Contigs names are shown in the lines with @SQ


 III. Running Reprever

	Reprever is an executable JAR file. You can run Reprever like below:
	
	>java -jar Reprever.jar

	It is highly recommended you specify sufficient memory in JVM. For example,

	>java -jar -Xms2g -Xmx8g Reprever.jar
	 				
	allows maximum 8 GB heap space to JVM. 

	Running without argument will bring the usage including mandatory and optional parameters.

	-------------------------------------------------------------------------------------
	>java -jar Reprever.jar

	Reprever: REPeat REsolVER - find and reconstruct extra copies given copy number gain regions.
	Version: 0.10

	Usage: java -jar Reprever.jar -R <reference.fa> -C <coord_sorted_indexed.bam> -I <input_region.txt> [options]

	Input options:

	    -g          construct gfServer. this is mandatory in current version
	    -2 FILE     .2bit file for input reference genome.
	    -t INT      maximum number of threads [1]

	Output options:

	    -v          verbose
	    -o PATH     name of the directory where reconstructed result will be saved ["reconstructed"]

	Parameters for breakpoint finding (RepreverLoc)

	    -l INT      read length (bp) [Reprever's inference]
	    -i INT      insert size (bp) [Reprever's inference]
	    -m INT      minimum % identity for blat match [95]
	    -s INT      maximum score for blat match as % size to the read length [90]
	    -p INT      percentile threshold for discordant pair [1]
	    -k INT      minimum score for breakpoint call [2]

	Parameters for reconstruction (RepreverSeq)

	    -x FLOAT    gap open penalty (gop) for Smith-Watermain alignment [10.0]
	    -y FLOAT    gap extension penalty (gep) for Smith-Watermain alignment [1.5]
	>
	--------------------------------------------------------------------------------------------

	The mandatory arguments are -R, -C, and -I, which denote reference file, alignment file and CNV region file,
	respectively.

	> java -jar Reprever.jar -R [reference].fa -C [alignment].bam -I [regions].txt

	Make sure [reference].fa.fai and [reference].2bit exist in the same folder with [reference].fa, and
	[alignment].bam.bai exists in the same folder with [alignment].bam. 

	If 'gfServer' and 'gfClient' is already installed, this will run Reprever.


 IV. Options and Parameters

	There are many parameters that correspond to one the the following categories: 1) Input options, 2) Output options,
	3) parameters for RepreverLoc and 4) parameters for RepreverSeq

	1) Input Options: 
	
		-g	flag that indicates you want to set up your own gfServer and gfClient. This is mandatory in current
			version, and automatically turned on; this saves enourmous amount of time when you blat repeatedly.
		-2 FILE	specifies the location of [reference].2bit file. If not used, Reprever assumes that it is in the
			same reference directory
		-t INT	maximum number of thread. Using appropriate number of threads (e.g. #CPUs) will speed up overall
			process.

	2) Output Options:

		-v	verbose. This will print out everything that is going on in Reprever. Use this if your are a reviewer
			or a debugger.
		-o PATH	the directory where reconstructed results are saved. 

	3) RepreverLoc parameters:

		-l INT	read length. If this is not specified, Reprever tries to infer it by reading 100,000 reads.
		-i INT	insert size. If not specified, Reprever tries to infer it by reading 100,000 reads.	 
		-m INT	minimum % identity for blat match. This is used in homolog search and finding multiple 
			mapping reads. 
		-s INT	value for setting minimum score of blat match (blat's match score). The match score is proportional
			to the length of matched region. The minimum score is calculated by s% of the read length. 
		-p INT	threshold percentile for discordant pair. Default is 1%
		-k INT	minimum breakpoint score to be valid breakpoint. see the article for more detail.

	4) RepreverSeq parameters:
		
		-x FLOAT gap open penalty for smith-wateran pairwise alignment. 
		-y FLOAT gap extension penalty for smith-waterman pairwise alignment.


 V. Output Files

	There are many intermediate/final output files that can be used for further insepction. 

	1) Final outputs:

		A. [regions].txt.breakpoint - a list of breakpoints discovered by RepreverLoc. Each line has the following
						information

		'breakpoint'	'(breakpoint range)'	'breakpoint score'	#FU+,#RU- / #OV / #FD+,#RD-
		For example:
		chr5:33481 (chr5:33161-34003) 83.0      44+,0- / 0 / 0+,39-

		'breakpoint' is the most probable breakpoint in the donor genome.
		'breakpoint range' is the possible range of the breakpoint. 
		'breakpoint score' is the value that corresponds to the confidence of the breakpoint. RepreverLoc
		calls breakpoint when the score is bigger than 2 (by default). 
		
		#FU: # of forward reads upstream of the breakpoint.
		#RU: # of reverse reads upstream of the breakpoint.
		#FD: # of forward reads downstream of the breakpoint.
		#RD: # of reverse reads downstream of the breakpoint.
		#OV: # of mixed set of forward/reverse reads around the breakpoint (25 bp range)

		Because #RU and #FD reads are the counter evidence of the breakpoint, filtering out where #RU!=0 or #FD!=0
		increases breakpoint specificity (while possible losing sensitivity). However, the read specificity 
		is fundamentally important, so you should always check the breakpoint score first (see Article).

		B. reconstructed sequences. 
	 	
		Final sequences including reconstructed donor duplicons can be found in:
	
		{working directory}/reconstructed/[region_name]/Result_[region_name].fa

		The final output is fasta format.

	2) Intermediate output

		Under working directory, one subdirectory is generated for each given region.
		The region name is formatted to "chrom_start_end". 
		Under the subdirectory, we have the followings:

		A. Reference sequence of the given region (and its flanking regions):

		chrom_start_end.fa
		chrom_start_end.left.fa
		chrom_start_end.right.fa

		B. It has subsubdirectories each of which corresponds to the homolog of given region.
		Under a subsubdirectory, we have a few bam files.

		COE.bam	: indexed concordant one-end reads mapped to the homolog
		DOE.bam	: indexed discordant one-end reads mapped to the homolog
		TE.bam	: indexed two-end reads mapped to the homolog
		OOE.bam	: indexed orphan one-end reads mapped to the homolog

		C. Merged reads from all homologs (subsubdirectories).

		mergedCOE.bam	: indexed merged concordant one-end reads
		mergedDOE.bam	: indexed merged discordant one-end reads
		
		Visualizing the merged bam file using a view (e.g. IGV) help analyze breakpoing manually.

		D. Special orphan one-end reads that are collected from candidate breakpoints.
	
		FE.bam	: a list of one-end reads, one of whose end is mapped around candidate breakpoints
			  but the other end is orphan. We conduct pairwise alignment between the unmapped
			  read and given regions.

		E. Blat searched discordant one-end reads (.psl)


 VI. Examples

 There are two example data sets are included. 'example1' contains a small (5x50000bp chromosome) genome.
 The 'original.fa' is a universal reference (like hg18) genome. The 'reference.fa' is a conventional 
 reference in which general SNPs are included; this genome is introduced to represent more realistic situation
 (we get samples from different individuals, not reference person). 'donor.fa' is a donor genome that contains
 more SNPs, indels and copy number variations. 'example2' contains a much longer genome (5x1Mbp).

 You can run the example like below:
 
 >java -jar Reprever.jar -R original.fa -C bwaout_sorted.bam -I cnv_region.txt -g2 original.2bit

 Note that -g option is used to construct gfServer. And -2 was used to indicate .2bit file.

 Discovered breakpoints are saved in "cnv_regions.txt.breakpoint"
 Reconstructed sequences can be found under "reconstructed/[region_name]/Result_[region_name].fa



	
	

		
			
	
