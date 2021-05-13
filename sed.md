# Short note to sed

Sed (The `s`treamline `ed`itor) reads one or more text files, and makes changes according to editing script.

## Basic syntax

    sed 'OPERATION/REGEXP/REPLACEMENT/FLAGS' FILENAME

## Basic arguments 

|  Argument | Function                                                     |
| --------: | :----------------------------------------------------------- |
| delimiter | `_` (underscore), `\|`(pipe), `#`, or `:`(colon)           |
| `-i`      | find and replace directly on the original file |
| `-n`      | Print specific lines of the file |
|`OPERATION`| `s`( substitution)，`y`(transformation)，`i`(insertion)，`d`(deletion) |

## Find and replace directly

    sed -i 's/PATTERN/substitution/g' FILENAME

## Search and replace 

    # replace all
    sed 's/PATTERN/substitution/g' FILENAME > NEWFILE

    # replace first occurrence
    sed 's/PATTERN/substitution/1' FILENAME > NEWFILE

    # keep a old version
    sed -i.old 's/PATTERN/substitution/g' FILENAME

    # if stringA is in the line, then perform substitution
    sed '/stringA/s/PATTERN/substitution/g' FILENAME > NEWFILE
  
## Print specific lines

    # print 10th line
    sed -n '10p' FILENAME

    # print 10th and 15th line
    sed -n -e '10p' -e '15p' FILENAME

    # print lines 10 to 50
    sed -n '10,50p' FILENAME
    
    # Every tenth line starting from 10
    sed -n '10~10p' FILENAME

    # odd-numbered lines
    sed -n '1~2p' FILENAME

    # print 1,2,5,6,9,10 lines
    sed -n '1~4p;2~4p' FASTQ_FILE

    # print 1 to 10, and then multiples of 10
    sed -n '1,10~10p'  FILENAME
    
## Insert specific lines 
  
  Using `i` to insert text in the file.
  
    # put “line to insert” in the second line
    sed '2 i line to insert' FILENAME
    
## Delete specific lines
  
    # delete first line
    sed "1d" FILENAME

    # delete lines 1 to 3
    sed "1,3d" FILENAME

    # delete blank lines
    sed 's/^$//g' FILENAME

## fastq to fasta  

    sed -n '1~4p;2~4p' FASTQ | sed 's/^@/>/g' > FASTA
  
