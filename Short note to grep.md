
# Short note to grep

grep(`g`lobally search a `r`egular `e`xpression and `p`rint) is a powerful command-line search tools that is one of the most useful tools in bioinformatics.
At the most basic level, grep searches for a string of characters that match a pattern and will print lines containing a match. 

## Basic syntax
	
	grep 'pattern' file_to_search
	
## Basic arguments 

|  Argument | Function                                                     |
| --------: | :----------------------------------------------------------- |
|      `-v` | inverts the match or finds lines NOT containing the pattern. |
|      `-F` | interprets the pattern as a literal string.                  |
| `-H`,`-h` | print, don’t print the matched filename                      |
|      `-i` | ignore case for the pattern matching.                        |
|      `-l` | lists the file names containing the pattern (instead of match). |
|      `-n` | prints the line number containing the pattern (instead of match). |
|      `-c` | counts the number of matches for a pattern                   |
|      `-o` | only print the matching pattern                              |
|      `-w` | forces the pattern to match an entire word.                  |
|      `-x` | forces patterns to match the whole line.                     |

## Counting

`grep -c` will print the number of lines of matches.

### counting reads 
	# for FASTA
	grep -c '^>' RefSeq.fa
	
	# for FASTQ
	grep -c '^@' RefSeq.fq
	
### multi-matches

If there are multi-matches on one line, option `grep -c` would still print the number of lines where the matches appear, not the number of matches.

	grep -o PATTERN FILENAME

	# total number 
	grep -o PATTERN FILENAME | wc -l

	# total number of each line
	## print line number followed by number of times
	grep -on "PATTERN" FILENAME | cut -f 1 -d ":" | sort | uniq -c
	
## 获得符合条件的行/文件名

对于报错或者其他场景，我们往往想要知道关键信息所在的行的情况，或者某路径下符合/不符合条件的文件有那些。

	grep -r PATTERN FILENAME

	## printing the matching filename
	grep -rl PATTERN path

	## printing the NOT matching filename
	grep -rL PATTERN path
	
## 获取前/后几行

通过`-B` (for before) 和 `-A` (for after) 参数，很容易获得符合条件的PATTERN所在的前后几行的信息。

	# before
	grep -B 10 "PATTERN" FILENAME

	# after
	grep -A 10 "PATTERN" FILENAME

	# combine 
	grep -B 10 -A 10 "PATTERN" FILENAME
