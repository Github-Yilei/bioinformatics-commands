## Displaying file names with contents when using the "cat"

```
ls | grep hit.tsv | sed 's/_hit.tsv//' | while read id; do echo ${id} `cat ${id}_hit.tsv`; done >blat_stats.tsv
```

## libstdc++.so.6: version CXXABI_1.3.8' not found

Take a look of standard library in your machine
```
strings /lib64/libstdc++.so.6 | grep '^CXXABI_'
```

Mostly, the standard library of Ubuntu is very old. When the dynamic linker found the wrong version of the libstdc++ shared library.

First, one should find the right libstdc++ in your machine.

```
strings ~/anaconda3/lib/libstdc++.so.6.0.28 | grep '^CXXABI_'
```

Then, add the library's path to the LD_LIBRARY_PATH environment variable.

```
export LD_LIBRARY_PATH=~/anaconda3/lib
```

## Combine files

Combine multi fa files useing cat. 

```
cat sample_names | xargs -i cat {} > combied.fa

ls | grep "fa" | awk 'NR<=50 {print $1}' | xargs -i cat {} > combied_50.fa
```
1. xargs -i = xargs -I {}: Replace {} with the parameters from standard input line by line.
2. cat {}: `{}` contain the samples name.
3. test1: cat sample_names | xargs -i echo {}
4. test2: cat sample_names | xargs -I {} echo {}
5. test3: cat sample_names | xargs echo


## Link

Create and remove a link to TARGET with the name LINK_NAME.

```
# make symbolic links instead of hard links
ln -s fil1 file2

# unlink
unlink file2
```

## Grep + Rm
	
	rm `ls | grep -v [1-2].fq`

## Sequence length

Return sequence length of every entry in a multi-fasta file and build a entry table.

```
awk '/^>/ {if (seqlen){print seqlen}; print ;seqlen=0;next; } { seqlen = seqlen +length($0)}END{print seqlen}' test.fa |  paste - -
```	

## Distribution of sequence length

Return the distribution of sequence length in a multi-fasta file and build a table.

1. NR%4 == 2: Returns the second lines with a 4 lines step size. 
2. length($0): Returns the seq-length
3. if seq-length is already in lengths, then add 1 to the appendix.(like dict.setdefault in python)

```
# for zipped FASTQ
zcat file.fastq.gz | awk 'NR%4 == 2 {lengths[length($0)]++} END {for (l in lengths) {print l, lengths[l]}}'

# for FASTA
awk 'NR%2 == 0 {lengths[length($0)]++} END {for (l in lengths) {print l, lengths[l]}}' test.fa or <test.fa
	
# put file name in the table for plot 
awk 'NR%2 == 0 {lengths[length($0)]++} END {for (l in lengths) {print "filename", l, lengths[l]}}' test.fa | paste - - 
```

## Randomly group/select samples

1. generating random numbers with `awk rand()`. 
2. reorder the samples accoding to the random numbers above.
3. selectting samples.

```
awk 'BEGIN{srand(13)} {print rand()"\t"$0}' sample_list |  sort -k1 -n | awk 'NR<=5 {print $2}'

# step=10, group=5
for i in {1..5}; do awk 'BEGIN{srand(13)} {print rand()"\t"$0}' sample_list |  sort -k1 -n | awk '10*("'${i}'"-1)<NR && NR<=10*"'$i'" {print $2}'; done
```	

1. srand(13): set.seed=13, the program will generates the same results each time with same seed. 
2. srand() will result in the different random numbers in each run.
3. rand(): generating random numbers.

## return absence record

Checking and returning the absence itmes according to the 2ed file.

```
awk 'NR==FNR{file1[$0]} NR>FNR{if(!($1 in file1)) print $0}' blastn_uniq.txt Mummer_Unqry.list > whole_UnHit.list
```

1. NR==FNR means `awk` are working with 1st file, then saving whole line of 1st file (`$0`) to 'file1';
2. FNR will equal to 0 while 2ed file are opend which means NR>FNR is true, then check if `$1` in `file` and print to screen while return true. 

## md5sum

Error: md5sum: 'xxxx.gz $'\r': No such file or directory
		'FAILED open or read xxxx.gz'

```
# show-all information
cat -A md5.txt

# global (default) 
sed 's#\r##' md5.txt | md5sum -c -
```

## Check Corrupted fastq file

**message**: FastQC.Sequence.SequenceFormatException: ID line didn't start with '@'

```
zcat test_1.fq.gz | awk 'NR%4==1{if($1~/^@/) print $1; else exit}' >test_1_id.txt

# return clean lines
cat test_1_id.txt | echo $((`wc -l`*4 - 1))
# return clean fastq1
zcat test_1.fq.gz | awk 'NR <= clean_lines {print}' > test_1.fq
zcat test_2.fq.gz | awk 'NR <= clean_lines {print}' > test_2.fq

## with sed
sed -n '1,1000p' test_1.fq.gz > test_1.fq
```


## split the report of quast

```
sed "s/\t/index/" quast_out/report.tsv  | awk -Findex '{print $2}'
```

## extract flagstat and make a tsv

The result of samtools flagstat were saved into a file (MapStatistics).

```
ls MapStatistics | while read name; do awk 'BEGIN{print "'${name}'"}; {print $1}' MapStatistics/${name} | paste - - - - - - - - - - - - - - ; done > flagstat
```

## rename

```
rename old new target.old

# return  target.new
```

## sort and uniq according to specific col(1,1)

![image](https://user-images.githubusercontent.com/82864917/136163838-042af6c7-3df1-4be5-9e70-e3bc9e13c7f4.png)

```
awk '{print $1 "\t" $7}' specific.blast | sort -u -k 2 | sort -u -k 1,1 > sorted_uniq.blast
```

## awk and sub(gsub）

1. sub match the first words, sed 's//'
2. gsub match all of the words, sed 's//g' 。 

```
awk '{gsub("\\.[0-9]*","",$2);print }' sorted_uniq.blast
```

## statistic of gff3

```
grep $'\t'gene$'\t' input.gff3 | awk '{leng+=($5-$4)} END {print "average = " leng/NR"\t" NR}'

grep $'\t'mRNA$'\t' input.gff3 | grep \\.1 | awk '{leng+=($5-$4)} END {print "average = " leng/NR"\t" NR}'
```

## bin-freq of plink missing-stats

```
awk '{printf "%.2f\n", $5}' prefix.lmiss | awk -v OFS="\t" '{binfreq[$1]++} END{for (f in binfreq) {print "prefix", f, binfreq[f]}}' > prefix_lmiss_bin_freq.tsv

awk -v OFS="\t"  '{temp = sprintf("%.2f", $5); binfreq[temp]++} END{for (f in binfreq) {print "prefix", f, binfreq[f]}}' prefix.lmiss > prefix_lmiss_bin_freq.tsv
```
## Gzip open vcf.gz

TypeError: startswith first arg must be bytes or a tuple of bytes, not str

```
with gzip.opener(vcf_file, "rt") as vcf:
```

## Genome coverage 

```
~/miniconda3/bin/bedtools genomecov -bga -ibam  AligementOut/sample.sorted.bam > Coverage/sample.sorted.tsv
ls Coverage | sed 's/.sorted.tsv//' | while read id ;  do echo "awk '{sum += "'$4'"} END {print sum/NR}' Coverage/${id}.sorted.tsv > Cov/${id}.cov"; done > cmd.list
ls Cov | sed 's/.cov//' | while read id ; do awk '{print "'${id}'" "\t" $1}' Cov/${id}.cov | paste --; done > genome_coverage.tsv
```

## AWK with Shell variables

-v: assign a variable

```
id = "myid"
awk -v id="${id}" '{print id"_"$1"\t"$2"\t"$3"\t"$4}' ${id}.bed
```
