This document describes the schema of the ngram dataset and how to get it.  

There are two data sources, the first is a list of files, verdicts, and tags, the second is the files and features.

-----------------------------------------

The file "ngramVerdictTags" is a list of ~4.6 million files, their verdicts, and their malware family tags.  Each tab seperated line has the following:

A SHA256 value identifying the file
A verdict, where 0 means benign, 1 means malicious, and 2 means questionable
Optionally, a number of tags with popular malware family names

If there is a discrepancy between verdicts between the "ngramVerdictTags" and the feature files, the "ngramVerdictTags" file is more correct.

------------------------------------------
The second dataset is the files and features.  It's ~4.6 million samples with approximately 100,000 features per sample.  The dataset is 25TB in size, compressed into ~25K ~1GB files.  The files are https://wiki.apache.org/hadoop/SequenceFile, if you load them in hadoop you can unpack them manually by using hadoop fs -text, although you should increase your memory first.  The recommended way to deal with them is to write native Hadoop code in Java.

Example of increasing memory to access with -text:
$ hadoop fs -text /data/ds/applicationplatform/billy/app_39/results_53223.seq | wc -l

18/12/05 01:06:00 INFO zlib.ZlibFactory: Successfully loaded & initialized native-zlib library
18/12/05 01:06:00 INFO compress.CodecPool: Got brand-new decompressor [.deflate]
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space

$ export HADOOP_HEAPSIZE=32000

$ hadoop fs -text /data/ds/applicationplatform/billy/app_39/results_53223.seq | wc -l

18/12/05 01:08:40 INFO zlib.ZlibFactory: Successfully loaded & initialized native-zlib library
18/12/05 01:08:40 INFO compress.CodecPool: Got brand-new decompressor [.deflate]

66

The actual unpacked data is a JSON string that looks like the following:
1       {"update_date": "1970-01-01 00:00:00", "malware": 1, "create_date": "2017-02-09 05:34:54", "finish_date": "2017-02-09 05:40:07", "ngrams": "{\"747948b0552cfe\": 1, \"ada36d13fb9d19\": 4}", "update_date_epoch": 0, "create_date_epoch": 1486618494, "sha256": "1bfb37d2e87407a0a235d1ae5b2afbf290d8943e16a610752ae101da870b38d0", "finish_date_epoch": 1486618807}
(Except with 100K ngrams, rather than 2, and 4.6M lines rather than 1)

------------------------------------------

To get these datasets, you need to set up a secure file transfer protocal (SFTP) that we can log into and send the 25TB of files to you.  Please email whewlett@paloaltonetworks [.] com and we will follow up to send you the data.

