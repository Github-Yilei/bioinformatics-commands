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

## Corrupted fastq file

**message**: FastQC.Sequence.SequenceFormatException: ID line didn't start with '@'

```
zcat test_1.fq.gz | awk 'NR%4==1{if($1~/^@/) print $1; else exit}' >test_1_id.txt

# return clean lines
cat test_1_id.txt | echo $((`wc -l`*4 - 1))
# return clean fastq1
zcat test_1.fq.gz | awk 'NR <= clean_lines {print }' > test_1.fq
zcat test_2.fq.gz | awk 'NR <= clean_lines {print }' > test_2.fq
```
