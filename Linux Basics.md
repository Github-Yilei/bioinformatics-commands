## Quotation

1. Single quotation(' '): in single quotation marks, all characters, including special characters (`$`, `’`, `‘`, and `\`) are interpreted as characters themselves and become ordinary characters.

2. Back quotation(` `): the string in the back quote will be interpreted as a shell command to execute.

3. Double quotation(" "): `$`, `‘`, `’`, `!`, and `\` in double quotation marks will be evaluated before display.

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
