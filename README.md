# Intercrop
gunzip *

# Rename
2 ways:
first = 
for f in ./*_1*.fq
do 
	newname=$( echo $f | sed -r 's/_R1/_1/' )
	mv $f $newname 
done

second=
rename '_1' '_R1' *.fq
rename '_2' '_R2' *.fq

Check quality of the reads
module purge
module load FastQC/0.11.5-Java-1.8.0_162

for file in Workspace/intercrop/*.fq
do
fastqc -f fastq ${file} -o QC/
done

Step 1: Merging and filtering paired end reads
./usearch64 -fastq_mergepairs Workspace/intercrop/*R1.fq -relabel @ -fastq_maxdiffs 15 -fastq_minmergelen 440 -fastq_maxmergelen 520 -fastq_maxee 1 -fastqout mergedFastq/merged.fq -report logFiles/report.txt -alnout logFiles/aln.txt	
Totals:
  13919362  Pairs (13.9M)
   5882020  Merged (5.9M, 42.26%)
   9642332  Alignments with zero diffs (69.27%)
    118804  Too many diffs (> 10) (0.85%)
   2006140  Fwd tails Q <= 2 trimmed (14.41%)
   1568274  Rev tails Q <= 2 trimmed (11.27%)
     29640  Fwd too short (< 64) after tail trimming (0.21%)
    100159  Rev too short (< 64) after tail trimming (0.72%)
   1713230  No alignment found (12.31%)
         0  Alignment too short (< 16) (0.00%)
   6075509  Merged too short (< 460)
         0  Merged too long (> 520)
       380  Staggered pairs (0.00%) merged & trimmed
     31.03  Mean alignment length
    468.31  Mean merged length
      0.68  Mean fwd expected errors
      0.74  Mean rev expected errors
      0.61  Mean merged expected errors

Totals:
  13919362  Pairs (13.9M)
  11929778  Merged (11.9M, 85.71%)
   9642332  Alignments with zero diffs (69.27%)
    118804  Too many diffs (> 10) (0.85%)
   2006140  Fwd tails Q <= 2 trimmed (14.41%)
   1568274  Rev tails Q <= 2 trimmed (11.27%)
     29640  Fwd too short (< 64) after tail trimming (0.21%)
    100159  Rev too short (< 64) after tail trimming (0.72%)
   1713230  No alignment found (12.31%)
         0  Alignment too short (< 16) (0.00%)
     27751  Merged too short (< 440)
         0  Merged too long (> 520)
       380  Staggered pairs (0.00%) merged & trimmed
     42.09  Mean alignment length
    456.80  Mean merged length
      0.89  Mean fwd expected errors
      0.86  Mean rev expected errors
      0.55  Mean merged expected errors

module spider cutadapt
module spider cutadapt/1.16-Python-3.6.4
ml icc/2018.1.163-GCC-6.4.0-2.28  impi/2018.1.163

Step 2: Trim primers
cutadapt --discard -a ACTCCTACGGGAGGCAGCA -a GGACTACHVGGGTWTCTAAT -o mergedFastq/cut_merged.fq mergedFastq/merged.fq

This is cutadapt 1.16 with Python 3.6.4
Command line parameters: --discard -a ACTCCTACGGGAGGCAGCA -a GGACTACHVGGGTWTCTAAT -o mergedFastq/cut_merged.fq mergedFastq/merged.fq
Running on 1 core
Trimming 2 adapters with at most 10.0% errors in single-end mode ...
Finished in 561.99 s (47 us/read; 1.27 M reads/minute).

=== Summary ===

Total reads processed:              11,929,778
Reads with adapters:                11,673,923 (97.9%)
Reads written (passing filters):       255,855 (2.1%)

Total basepairs processed: 5,449,523,057 bp
Total written (filtered):    116,432,307 bp (2.1%)

=== Adapter 1 ===

Sequence: ACTCCTACGGGAGGCAGCA; Type: regular 3'; Length: 19; Trimmed: 11673903 times.

No. of allowed errors:
0-9 bp: 0; 10-19 bp: 1

Bases preceding removed adapters:
  A: 0.0%
  C: 0.0%
  G: 0.0%
  T: 0.0%
  none/other: 99.9%

Overview of removed sequences
length  count   expect  max.err error counts
3       9       186402.8        0       9
4       3       46600.7 0       3
5       926     11650.2 0       926
440     1522    0.0     1       1214 308
441     4651    0.0     1       3258 1393
442     30959   0.0     1       16289 14670
443     406280  0.0     1       179757 226523
444     2962700 0.0     1       2866454 96246
445     578980  0.0     1       529865 49115
446     427191  0.0     1       413817 13374
447     67364   0.0     1       60582 6782
448     141232  0.0     1       60644 80588
449     1022504 0.0     1       1001278 21226
450     38522   0.0     1       35469 3053
451     15070   0.0     1       13380 1690
452     22427   0.0     1       18070 4357
453     49318   0.0     1       47437 1881
454     12847   0.0     1       11370 1477
455     16075   0.0     1       15108 967
456     10373   0.0     1       6657 3716
457     46399   0.0     1       42818 3581
458     38333   0.0     1       36287 2046
459     18739   0.0     1       15390 3349
460     50571   0.0     1       40487 10084
461     124926  0.0     1       119894 5032
462     40421   0.0     1       30880 9541
463     125608  0.0     1       108521 17087
464     192534  0.0     1       186814 5720
465     26791   0.0     1       23563 3228
466     44930   0.0     1       31557 13373
467     211264  0.0     1       159253 52011
468     883106  0.0     1       636179 246927
469     3256205 0.0     1       3142872 113333
470     712921  0.0     1       691939 20982
471     54141   0.0     1       51879 2262
472     3672    0.0     1       3507 165
473     272     0.0     1       232 40
474     543     0.0     1       526 17
475     94      0.0     1       62 32
476     640     0.0     1       321 319
477     3863    0.0     1       3706 157
478     2255    0.0     1       1343 912
479     10885   0.0     1       9998 887
480     9523    0.0     1       8889 634
481     5675    0.0     1       5528 147
482     551     0.0     1       521 30
483     44      0.0     1       37 7
484     44      0.0     1       36 8

=== Adapter 2 ===

Sequence: GGACTACHVGGGTWTCTAAT; Type: regular 3'; Length: 20; Trimmed: 20 times.

No. of allowed errors:
0-9 bp: 0; 10-19 bp: 1; 20 bp: 2

Bases preceding removed adapters:
  A: 75.0%
  C: 10.0%
  G: 10.0%
  T: 5.0%
  none/other: 0.0%

Overview of removed sequences
length  count   expect  max.err error counts
3       1       186402.8        0       1
4       18      46600.7 0       18
7       1       728.1   0       1

Step 3: Dereplicate (finding unique) sequences
./usearch64 -fastx_uniques mergedFastq/merged.fq -fastqout mergedFastq/uniques_merged.fastq -sizeout
00:54 12.0Gb  100.0% Reading mergedFastq/merged.fq
00:54 12.0Gb CPU has 20 cores, defaulting to 10 threads
01:10 14.6Gb  100.0% DF
01:11 14.7Gb 11929778 seqs, 5981016 uniques, 5365767 singletons (89.7%)
01:11 14.7Gb Min size 1, median 1, max 17684, avg 1.99
06:12 12.8Gb  100.0% Writing mergedFastq/uniques_merged.fastq

Remove singletons
./usearch64 -sortbysize mergedFastq/uniques_merged.fastq -fastqout mergedFastq/nosig_uniques_merged.fastq -minsize 2
00:27 6.1Gb   100.0% Reading mergedFastq/uniques_merged.fastq
00:27 6.0Gb  Getting sizes
00:28 6.1Gb  Sorting 615249 sequences
00:34 6.1Gb   100.0% Writing output

./usearch64 -usearch_global mergedFastq/nosig_uniques_merged.fastq -id 0.97 -db /mnt/research/ShadeLab/WorkingSpace/SILVA_128_QIIME_release/rep_set/rep_set_16S_only/97/97_otus_16S.fasta -strand plus -uc results/ref_seqs.uc -dbmatched results/closed_reference.fasta -notmatched results/failed_closed.fa
00:01 37Mb      0.1% Reading /mnt/research/ShadeLab/WorkingSpace/SILVA_128_QIIME_release/rep_set/rep_set_16S_only/97/97_otus_16S.00:02 143Mb    41.8% Reading /mnt/research/ShadeLab/WorkingSpace/SILVA_128_QIIME_release/rep_set/rep_set_16S_only/97/97_otus_16S.00:03 282Mb    99.2% Reading /mnt/research/ShadeLab/WorkingSpace/SILVA_128_QIIME_release/rep_set/rep_set_16S_only/97/97_otus_16S.00:03 284Mb   100.0% Reading /mnt/research/ShadeLab/WorkingSpace/SILVA_128_QIIME_release/rep_set/rep_set_16S_only/97/97_otus_16S.fasta
00:03 250Mb     0.1% Masking (fastnucleo)                                                                                        00:06 250Mb   100.0% Masking (fastnucleo)
00:13 251Mb   100.0% Word stats
00:13 251Mb   100.0% Alloc rows
00:24 1.1Gb   100.0% Build index
00:24 1.1Gb  CPU has 20 cores, defaulting to 10 threads
03:58 1.8Gb   100.0% Searching nosig_uniques_merged.fastq, 79.0% matched

./usearch64 -sortbysize results/failed_closed.fa -fastaout results/sorted_failed_closed.fasta

./usearch64 -cluster_otus results/sorted_failed_closed.fasta -minsize 2 -otus results/denovo_otus.fasta -relabel OTU_dn_ -uparseout results/denovo_otu.up --threads 40

01:22 82Mb    100.0% 6950 OTUs, 10154 chimeras


cat results/closed_reference.fasta results/denovo_otus.fasta > results/full_rep_set.fasta

creat job

nano mapping.sb
#!/bin/bash --login
########## Define Resources Needed with SBATCH Lines ##########
 
#SBATCH --time=4:00:00             	# limit of wall clock time - how long the job will run (same as -t)
#SBATCH --ntasks=1                  	# number of tasks - how many tasks (nodes) that you require (same as -n)
#SBATCH --cpus-per-task=1           	# number of CPUs (or cores) per task (same as -c)
#SBATCH --mem=400G                    	# memory required per node - amount of memory (in bytes)
#SBATCH --job-name 16S_bean_development	# you can give your job a name for easier identification (same as -J)
#SBATCH --mail-user=hujian2@msu.edu        # replace with your email address
#SBATCH --mail-type=BEGIN,END		# will send you email when the job started and ended 
########## Command Lines to Run ##########

cd /mnt/research/ShadeLab/Hu/    # change to the directory where your code is located
 

# example for mapping

./usearch64 -usearch_global mergedFastq/merged.fq -db results/full_rep_set.fasta -strand plus -id 0.97 -uc results/OTU_map.uc -otutabout results/OTU_table.txt -biomout results/OTU_jsn.biom

 

scontrol show job $SLURM_JOB_ID     # write job information to output file



sbatch mapping.sb
sbatch: Adding hujian2 to the accounting database
Submitted batch job 50513783

qstat -u hujian2
dev-intel14.i:
                                                                               Req'd  Req'd   Elap
Job id               Username Queue    Name                 SessID NDS   TSK   Memory Time Use S Time
-------------------- -------- -------- -------------------- ------ ----- ----- ------ ----- - -----
50513783             hujian2  general- 16S_bean_development --         1     1     -- 04:00 R 00:04
[hujian2@dev-intel14 Hu]$
[hujian2@dev-intel14 Hu]$ qstat -u hujian2
26:21 343Mb    99.3% Searching merged.fq, 88.2% matched
26:22 343Mb    99.4% Searching merged.fq, 88.2% matched
26:23 343Mb    99.5% Searching merged.fq, 88.2% matched
26:24 343Mb    99.6% Searching merged.fq, 88.2% matched
26:25 343Mb    99.7% Searching merged.fq, 88.2% matched
26:26 343Mb    99.8% Searching merged.fq, 88.3% matched
26:27 343Mb    99.9% Searching merged.fq, 88.3% matched
26:27 343Mb   100.0% Searching merged.fq, 88.3% matched
10529381 / 11929778 mapped to OTUs (88.3%)             
26:27 343Mb  Writing results/OTU_table.txt
26:27 343Mb  Writing results/OTU_table.txt ...done.
26:28 343Mb  Writing results/OTU_jsn.biom
26:28 343Mb  Writing results/OTU_jsn.biom ...done.
JobId=50515841 JobName=mapping_usearch_7h
   UserId=hujian2(1036803) GroupId=ShadeLab(2275) MCS_label=N/A
   Priority=65823 Nice=0 Account=general QOS=hujian2
   JobState=RUNNING Reason=None Dependency=(null)
   Requeue=1 Restarts=0 BatchFlag=1 Reboot=0 ExitCode=0:0
   RunTime=00:26:35 TimeLimit=07:00:00 TimeMin=N/A
   SubmitTime=2019-11-26T16:42:35 EligibleTime=2019-11-26T16:42:35
   AccrueTime=2019-11-26T16:42:35
   StartTime=2019-11-26T16:42:35 EndTime=2019-11-26T23:42:35 Deadline=N/A
   SuspendTime=None SecsPreSuspend=0 LastSchedEval=2019-11-26T16:42:35
   Partition=general-long-bigmem AllocNode:Sid=dev-intel14.i:58033
   ReqNodeList=(null) ExcNodeList=(null)
   NodeList=qml-000
   BatchHost=qml-000
   NumNodes=1 NumCPUs=4 NumTasks=1 CPUs/Task=4 ReqB:S:C:T=0:0:*:*
   TRES=cpu=4,mem=600G,node=1,billing=93388
   Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
   MinCPUsNode=4 MinMemoryNode=600G MinTmpDiskNode=0
   Features=[intel14|intel16|intel18] DelayBoot=00:00:00
   OverSubscribe=OK Contiguous=0 Licenses=(null) Network=(null)
   Command=/mnt/research/ShadeLab/Hu/mapping.sb
   WorkDir=/mnt/research/ShadeLab/Hu
   Comment=stdout=/mnt/research/ShadeLab/Hu/slurm-50515841.out 
   StdErr=/mnt/research/ShadeLab/Hu/slurm-50515841.out
   StdIn=/dev/null
   StdOut=/mnt/research/ShadeLab/Hu/slurm-50515841.out
   Power=

cancel a job
 scancel 50513783

Use mothur (need to creat a job befor use mothur)
module spider mothur (find mothur version)

module spider Mothur/1.41.3-Python-2.7.13

ml icc/2017.1.132-GCC-6.3.0-2.27  impi/2017.1.132 (dependancies)

ml Mothur/1.41.3-Python-2.7.13

mothur

classify.seqs(fasta=full_rep_set.fasta, template=../silva.nr_v128.align, taxonomy=../silva.nr_v128.tax, method=wang)


creat a job for classify
nano classify.sb
#!/bin/bash --login
########## Define Resources Needed with SBATCH Lines ##########
 
#SBATCH --time=4:00:00             	# limit of wall clock time - how long the job will run (same as -t)
#SBATCH --ntasks=1                  	# number of tasks - how many tasks (nodes) that you require (same as -n)
#SBATCH --cpus-per-task=1           	# number of CPUs (or cores) per task (same as -c)
#SBATCH --mem=400G                    	# memory required per node - amount of memory (in bytes)
#SBATCH --job-name 16S_bean_development	# you can give your job a name for easier identification (same as -J)
#SBATCH --mail-user=hujian2@msu.edu        # replace with your email address
#SBATCH --mail-type=BEGIN,END		# will send you email when the job started and ended 
########## Command Lines to Run ##########

cd /mnt/research/ShadeLab/Hu/results    # change to the directory where your code is located

ml icc/2017.1.132-GCC-6.3.0-2.27  impi/2017.1.132
ml Mothur/1.41.3-Python-2.7.13

mothur mothur_classify

nano mothur_classify (need to do in a different script (not the same as classify.sb) for mothur classify)
classify.seqs(fasta=full_rep_set.fasta, template=../silva.nr_v128.align, taxonomy=../silva.nr_v128.tax, method=wang)

sbatch classify.sb
(Submitted batch job 51704286)

qstat -u hujian2
dev-intel14.i:
                                                                               Req'd  Req'd   Elap
Job id               Username Queue    Name                 SessID NDS   TSK   Memory Time Use S Time
-------------------- -------- -------- -------------------- ------ ----- ----- ------ ----- - -----
51704286             hujian2  general- 16S_bean_development --         1     1     -- 04:00 R 00:02

wget https://www.drive5.com/muscle/downloads3.8.31/muscle3.8.31_i86linux64.tar.gz

gunzip *

creat a job for alignment (run another job at the same time if the job are independant)

nano alignment.sb
#!/bin/bash --login
########## Define Resources Needed with SBATCH Lines ##########
 
#SBATCH --time=12:00:00             	# limit of wall clock time - how long the job will run (same as -t)
#SBATCH --ntasks=1                  	# number of tasks - how many tasks (nodes) that you require (same as -n)
#SBATCH --cpus-per-task=1           	# number of CPUs (or cores) per task (same as -c)
#SBATCH --mem=400G                    	# memory required per node - amount of memory (in bytes)
#SBATCH --job-name alignment	# you can give your job a name for easier identification (same as -J)
#SBATCH --mail-user=hujian2@msu.edu        # replace with your email address
#SBATCH --mail-type=BEGIN,END		# will send you email when the job started and ended 
########## Command Lines to Run ##########

cd /mnt/research/ShadeLab/Hu/results    # change to the directory where your code is located

/mnt/home/hujian2/muscle3.8.31_i86linux64 -in full_rep_set.fasta -out alignment_otus.fasta -maxiters 2 -diags1

sbatch alignment.sb

creat a job for phylogenetics

nano phylogenetictree.sb
#!/bin/bash --login
########## Define Resources Needed with SBATCH Lines ##########
 
#SBATCH --time=12:00:00             	# limit of wall clock time - how long the job will run (same as -t)
#SBATCH --ntasks=1                  	# number of tasks - how many tasks (nodes) that you require (same as -n)
#SBATCH --cpus-per-task=1           	# number of CPUs (or cores) per task (same as -c)
#SBATCH --mem=400G                    	# memory required per node - amount of memory (in bytes)
#SBATCH --job-name phylogenetictree	# you can give your job a name for easier identification (same as -J)
#SBATCH --mail-user=hujian2@msu.edu        # replace with your email address
#SBATCH --mail-type=BEGIN,END		# will send you email when the job started and ended 
########## Command Lines to Run ##########

cd /mnt/research/ShadeLab/Hu/results    # change to the directory where your code is located

ml icc/2018.1.163-GCC-6.4.0-2.28  impi/2018.1.163
ml FastTree/2.1.10

FastTree -gtr -nt /mnt/home/hujian2/alignment_otus.fasta > otuTree_fasttree.tre

sbatch phylogenetictree.sb

qstat -u hujian2


 


