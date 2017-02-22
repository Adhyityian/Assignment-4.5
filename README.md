# Assignment-4.5

1.Explain what is checksum and the importance of checksum and how hadoop performs checksum
            1)Checksum is a small data from a digital data taken for the purpose of detecting errors which may have occured during transmission or storage.
           2) It provides data integrity but not data authenticity.
           3)Tranmissions always causes errors such as missing of files or duplication.So the data received is not credible.
           4)Checksum is used to detect the errors.           
           5)HDFS checksums all data written to it and by default verifies checksums when reading data.
           6)A separate checksum is created for every dfs.bytes-per-checksum bytes of data. 
           
 2.Explain the anatomy of file write to HDFS
 
1)An instance of object of distributed file system is grabbed.
2)A new file in a filesystem is created by doing a remote procedure call to namenode. * An instance of object of distributed file system is grabbed.
3)A new file in a filesystem is created by doing a remote procedure call to namenode.
4)Client creates, calls create() on DistributedFileSystem.   
5)DistributedFileSystem contacts namenode to create a new file in the filesystem’s namespace,
with no blocks associated with it.
6)Client signals write() method on FSDataOutputStream.
7)DFSoutputStream helps in communication  between namenode and datanodes.
8)DFSOutputStream splits into packets when client writes data and a data queue is maintained.
9)Data Streamer consumes data queue which asks namenode to allocate new blocks.
10)Closing a file in HDFS performs an implicit hflush().
11)After a successful return from hflush(), HDFS guarantees that the data written up to that point in the file has reached all the datanodes in the write pipeline and is visible to all new readers.

3.Explain how HDFS handles failures during file write
1)When the file write fails,first the pipeline is closed and the packets in the acknowledge queue are added to the data queue.
2)The current block on the good datanodes is given a new identity, which is communicated to the namenode.
3)Under replication of the blocks is noted by the namenode,and further replica is created on another node.
4)The failed datanode is removed from the pipeline, and a new pipeline is constructed from the two good datanodes. 
5)The block will be asynchronously replicated across the cluster until its target replication factor is reached
6)As long as dfs.replication.min replicas are written, the write will succeed, and the block will be asynchronously replicated across the cluster until its target replication factor is reached.
