## Combine files

cat file1 file2

paste file1 file2

# link

unlink

# grep + rm
	
	rm `ls | grep -v [1-2].fq`

## Sequence length

Return sequence length of every entry in a multi-fasta file and build a entry table.

	awk '/^>/ {if (seqlen){print seqlen}; print ;seqlen=0;next; } { seqlen = seqlen +length($0)}END{print seqlen}' test.fa |  paste - -
	
## distribution of sequence length

Return the distribution of sequence length in a multi-fasta file and build a table.

1. NR%4 == 2: Returns the second lines with a 4 lines step size. 
2. length($0): Returns the seq-length
3. if seq-length is already in lengths, then add 1 to the appendix.(like dict.setdefault in python)

	# for zipped FASTQ
	zcat file.fastq.gz | awk 'NR%4 == 2 {lengths[length($0)]++} END {for (l in lengths) {print l, lengths[l]}}'

	# for FASTA
	awk 'NR%2 == 0 {lengths[length($0)]++} END {for (l in lengths) {print l, lengths[l]}}' test.fa or <test.fa
	
	# put file name in the table for plot 
	awk 'NR%2 == 0 {lengths[length($0)]++} END {for (l in lengths) {print "filename", l, lengths[l]}}' test.fa | paste - - 
	
	
