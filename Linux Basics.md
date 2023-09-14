https://radcamp.github.io/Marseille2020/

https://radcamp.github.io/Marseille2020/07_momi2_API.html

https://radcamp.github.io/AF-Biota/07_momi2_API.html

## Quotation

1. Single quotation(' '): in single quotation marks, all characters, including special characters (`$`, `’`, `‘`, and `\`) are interpreted as characters themselves and become ordinary characters.

2. Back quotation(\` \`): the string in the back quote will be interpreted as a shell command to execute.

3. Double quotation(" "): `$`, `‘`, `’`, `!`, and `\` in double quotation marks will be evaluated before display.

4. Double quotation + Single quotation("'$1'"): the $1 will be interpreted as a shell command to execute in a single quotation marks.

```
id = test
awk '{sum += $4} END {print "'${id}'" "\t" "average = " sum/NR}' test.txt
```
## lsof

**List open files**:  List all the files opened

## Shell

This option applies to the shell environment and each subshell environment separately, and may cause shells to exit before executing all the commands in the subshell while one of the command return 'error'.

```
set -e 
```

## System and Memory

1. system information

		uname -a

2. memory of all file systems

		df -h
		
3. the physical RAM

		free -g 
		
4. summarize disk usage of the FILE

		du -chs
		
5. detail of CPU

		 lscpu
		 cat /proc/cpuinfo

6. number of Physical CPU

		cat /proc/cpuinfo | grep 'physical id' | sort | uniq | wc -l
		
7. number of CPU Cores

		cat /proc/cpuinfo | grep 'core id' | sort | uniq | wc -l

8. number of Logical CPU

		cat /proc/cpuinfo|grep "processor"|wc -l

9.  view the environment

		printenv
		
## Protable Batch System

```
# kill jobs according to user
qselect -u yilei | xargs qdel -W force

# kill jobs according to job
ps -ef | grep job_names | awk '{print $2}' | xargs kill -9
```

### Using PBS Variable

```
#!/bin/bash
#PBS -N Test
#PBS -o test.log
#PBS -e test.err
#PBS -q workq
#PBS -l nodes=1:ppn=1
#PBS -A YL

Index=${input_idx}
echo  ${Index} > tmp_${Index}.table

```
qsub -v input_idx=10 test.sh


## top process full path

```
top -c
```
