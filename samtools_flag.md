## minimap2

### 1. f4 and F4

1st of all, '-f 4' means to include all unmapped reads which equals to ('total' - 'mapped'); then '-F 4' means to only include reads with none of the unmapped reads that will result to ('mapped' - 'supplementary' - 'sccondary') while 'flagstat' only contain 'mapped', 'supplementary', and 'sccondary', which is a little bit of contrary to common sense that **NONE unmapped means mapped**. Furthermore, '-G' will EXCLUDE reads with all of the FLAGs result to ('mapped' - 'supplementary' - 'sccondary') as well. Finally, adding the result of '-f 4' and '-F 4' will reslut to the total reads of actural fastq but not the total reads from 'flagstat'. Besides, the reslut of flagstat is a little bit of diffrent from input fastq file.

I think thers may be some FLAG conflit from minimap2 and normal SAM files. 
