# Accelerating spliced alignment of long RNA sequencing reads using parallel maximal exact match retrieval

## 1. How to use parallel MEM retrieval algorithm

### For the first time of use, the program creates the necessary index data of reference genome (sequences) for retrieving MEMs, and serializes them into files, including "index.bwt", "lcpArray.bwt" and "slcpArray.bwt". Then, MEMs between reference genome and RNA-seq reads are extracted into the output file.

### Command:
```
./PMEM -l "length of MEM" "file of reference genome" "file of RNA-seq reads" -o "output file name of MEMs" -p "batch size" -t "number of threads" -i "position of index files"
```

### Example
```
./PMEM -l 20 refs_sequences.fa SIRV.fasta -o SIRV -p 50000 -t 16 -i /index/SIRV
```

## 2. How to integrate parallel MEM retrieval algorithm into [uLTRA](https://github.com/ksahlin/ultra)

### Just add three parameters: batch_num, threads and index_path, to "find_mems_slamem" function in "mem_wrapper.py". When calling "find_mems_slamem" function, pass these parameters.

### Code example:
```
def find_mems_slamem(outfolder, read_path, refs_path, out_path, min_mem, batch_num, threads, index_path):
    subprocess.check_call([ 'slaMEM-MT-i', '-l' , str(min_mem),  refs_path, read_path, '-o', out_path, '-p', str(batch_num), '-t', str(threads), '-i' , index_path], stdout=stdout_file, stderr=stderr_file)


batch_args.append((args.outfolder, read_batch, ref_path, mummer_batch_out_path, args.min_mem, "50000", "16", args.index))
```

#### Note: The source code of parallel MEM retrieval algorithm will be available upon the publication of the paper.
