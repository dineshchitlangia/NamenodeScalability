# Dealing with Heap

Each namespace object in NN is ~ 150 bytes
Assuming, Block Size 128 MB consider the following scenarios:
* 128 MB File = 2 objects, 1 inode + 1 block ~ 300 bytes
* But 128   1 MB files =256 objects, 128 inodes + 128 blocks ~ 38.4K bytes

As you can see, the number of inode+blocks dictate how much heap memory will Namenode use  inorder to keep all that metadata in-memory.

**1 GB for every million blocks is a very conservative rule of thumb.**
