# xprofile

This repo contains one or more disk images in the proprietary XPROFILE format for use with various vintage systems.

Files might be compressed with tar/gzip.

For more information on the X/PROFILE visit [http://sigmasevensystems.com](http://sigmasevensystems.com). I am not affiliated with Sigma Seven Systems or the X/PROFILE.

> **_WARNING:_**  Be careful with the direction used with dd - 'if' (in) vs 'of' (out). If you're not careful you can destroy a disk you didn't intend to overwrite.

## Copying from Disk to Image

To copy an XPROFILE disk to image, perform the following:

1. Run `diskutil list` from the Terminal app to find the path of the XPROFILE compact flash card:

```
➜  ~ diskutil list
/dev/disk0 (internal, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *1.0 TB     disk0
   1:                        EFI EFI                     314.6 MB   disk0s1
   2:                 Apple_APFS Container disk1         1.0 TB     disk0s2

/dev/disk1 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +1.0 TB     disk1
                                 Physical Store disk0s2
   1:                APFS Volume Macintosh HD            15.7 GB    disk1s1
   2:              APFS Snapshot com.apple.os.update-... 15.7 GB    disk1s1s1
   3:                APFS Volume Macintosh HD - Data     967.9 GB   disk1s2
   4:                APFS Volume Preboot                 430.4 MB   disk1s3
   5:                APFS Volume Recovery                1.1 GB     disk1s4
   6:                APFS Volume VM                      3.2 GB     disk1s5

/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *256.9 MB   disk2
   1:                 DOS_FAT_12 XPROFILE                68.6 KB    disk2s1
                    (free space)                         256.8 MB   -
```

We are interested in `/dev/disk2`, as the name `XPROFILE` demonstrates an existing XPROFILE disk.

2. Using `dd`, copy the disk to file:

```
sudo dd if=/dev/disk2 of=/Users/<username>/<image name>
```

Example:

```
sudo dd if=/dev/disk2 of=/Users/arcanebyte/xprofile-system71.dd
```

You may be able to substitute $HOME for the path:

```
sudo dd if=/dev/disk2 of=$HOME/system71.img
```

## Copying from Image to Disk

To copy from image (file) to disk, insert a compact flash card of the same size or larger.

1. Run `diskutil list` from the Terminal app to find the path of the XPROFILE compact flash card:

```
➜  ~ diskutil list
...
/dev/disk2 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:     FDisk_partition_scheme                        *256.9 MB   disk2
   1:                 DOS_FAT_32 UNTITLED                256.9 MB   disk2s1
```

The compact flash card is located at `/dev/disk2`. name `UNTITLED` might reflect a new or formatted disk. What's important is that the size is the same or larger.

2. Unmount the disk (but don't eject):

```
diskutil unmountDisk /dev/disk2
```

3. Using `dd`, copy the image file to disk:

```
sudo dd if=/Users/<username>/<image name> of=/dev/disk2
```

Example:

```
sudo dd if=/Users/arcanebyte/xprofile-system71.dd of=/dev/disk2
```


