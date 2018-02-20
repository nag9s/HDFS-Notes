https://stackoverflow.com/questions/29604823/change-block-size-of-existing-files-in-hadoop?rq=1



Consider a hadoop cluster where the default block size is 64MB in`hdfs-site.xml`. However, later on the team decides to change this to 128MB. Here are my questions for the above scenario?

1. Will this change require restart of the cluster or it will be taken up automatically and all new files will have the default block size of 128MB?
2. What will happen to the existing files which have block size of 64M? Will the change in the configuration apply to existing files automatically? If it will be automatically done, then when will this be done - as soon as the change is done or when the cluster is started? If not automatically done, then how to manually do this block change?



Will this change require restart of the cluster or it will be taken up automatically and all new files will have the default block size of 128MB

A restart of the cluster will be required for this property change to take effect.

> What will happen to the existing files which have block size of 64M? Will the change in the configuration apply to existing files automatically?

Existing blocks will not change their block size.

> If not automatically done, then how to manually do this block change?

To change the existing files you can use distcp. It will copy over the files with the new block size. However, you will have to manually delete the old files with the older block size. Here's a command that you can use

```
hadoop distcp -Ddfs.block.size=XX /path/to/old/files /path/to
ew/files/with/larger/block/sizes.
```

As mentioned[here](https://stackoverflow.com/a/28590163/3831557)for your point:

1. Whenever you change a configuration, you need to restart the NameNode and DataNodes in order for them to change their behavior.
2. No, it will not. It will keep the old block size on the old files. In order for it to take the new block change, you need to rewrite the data. You can either do a hadoop fs -cp or a distcp on your data. The new copy will have the new block size and you can delete your old data.

check link for more information.

