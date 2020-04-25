# Introduction
This is a project to parallelize current genomic assembly software. 
minimap2 made by Liheng is a versatile pairwise aligner for genomic and spliced nucleotide sequences. 
minimap2 can be used to do de novo genomic assembly using three-generation sequencing data with miniasm. 
The original minimap2 can running with mutual threads, but couldn't be used to running on cluster. Genomic assembly is a high-calculation work, the using of mpi could accelerate the job.
https://github.com/lh3/minimap2

# Instruction
Before you  do following jobs, you should install mpich on your all compute nodes and make a machinefile to make list of them.
 
First, run "git clone https://github.com/lh3/minimap2" to get minimap2.

Second, move the three files into minimap2 folder.

Finally, stay in the minimap2 folder, and run "./minimap2_mpi ${target_file} ${query file} ${process_num} ${machinefile} ${out_file}"
            for example :  ./minimap2_mpi  ont.fq query.fq 4 ~/wzj/hosts ont.paf

note: minimap2_mpi is a bash file. Essentially, it's a variants of "./minimap2 -t 10 -x ava-ont ont.fq query.fq > ont.paf". So if you want to use other functions of minimap2, you need to change the file.

# Disadvantages
1. 
Although we split the query file and run different parts on different nodes, the memory consumption don't have a significant improvement. Because the main  memory consumption come from the index established by target file, and every node need save the index.

2.
The method of spliting could cause the loss of several reads sometimes.

3.
Compared with single-process output, the order of simultaneous output of multiple processes is uncertain. So the results of genomic assembly isn't unique. But the output's size is certain. So the output have all informations of alignment. If you want make the result unique, you should make every node have own outfile and merge them in order.  
