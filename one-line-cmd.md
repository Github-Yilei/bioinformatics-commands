# System and Memory

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
		
# combine files

cat file1 file2

paste file1 file2

