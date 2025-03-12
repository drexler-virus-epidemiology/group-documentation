# Easily mount remote file systems via sshf

At the BIH HPC, they maintain two distinct storage tiers, each designed to serve different data storage needs:

-   **Cephfs-1 (Tier 1)**: Hot storage located at `/data/cephfs-1`, SSD-based, designated for files that require frequent random access, user home directories, and scratch space.
-   **Cephfs-2 (Tier 2)**: Warm storage located at `/data/cephfs-2`, HDD-based, offering slower performance but greater capacity, intended for permanent storage.

Within these tiers, we have defined specific spaces based on their intended use:

-   **User Home**: Small, persistent, and secure storage intended for configuration files and the Miniconda structural directory (excluding environment installations).
-   **Work**: Larger, persistent storage for frequently accessed files or databases, pipeline code, scripts, and software, including conda environment installations. There are both "user" and "group" work spaces, which share the same quota.
-   **Scratch**: Non-persistent storage for files related to ongoing analysis. Files in this space are automatically deleted after two weeks. Like the work space, there are both "user" and "group" scratch spaces that share the same quota.
-   **Group Storage**: Similar in purpose to group work but located in Tier 2. This space is intended for large, commonly used databases and the storage of group results. 

### Attention!

- **We recommend strong caution on using work and scratch folders as these spaces are shared with the whole group!**

- **Avoid data redundancy!**

- **After finishing the analysis, download, save and erase data from the cluster!**

## Folder locations

Below, we show the location of each of these folders at the cluster's filesystem. These locations are written as UNIX paths.

In this case, `<user>` is a BIH HPC user account, usually ending with + "_c", such as`muca10_c`, and `<doe>` is our working group name, in our case it is `drexler`.

|Folder|Location|Symlink|Capacity|
|--|--|--|--|
|User Home|`/data/cephfs-1/home/users/<user>`||1 GB|
|Group Work|`/data/cephfs-1/work/groups/<group>`||1TB|
|User Work|`/data/cephfs-1/work/groups/<doe>/users/<user>`|`~/work`|^|
|User Scratch|`/data/cephfs-1/scratch/groups/<doe>/users/<user>`|`~/scratch`|10 TB|
|Group Scratch|`/data/cephfs-1/scratch/groups/<group>`||^|
|Group Storage|`/data/cephfs-2/unmirrored/groups/<group>`||10 TB|

^ = contained withing the group, shared quota.

## How to easily connect our Unix or WSL machines

Here we provide a bash script to make file transfer easy as a copy & paste or a drag & drop operation. The idea is build a function we can call when mounting the remote folder locally is needed and another to unmount, in case we are note using it anymore or to recover from a network issue.

Note that for this to work we need to be registered as a BIH HPC user and have already submitted a SSH key to Charite portal. See https://hpc-docs.cubi.bihealth.org/connecting/submit-key/charite/. 

For that, first we define a folder structure like the one bellow:

```
cluster
├── group
│   ├── scratch
│   └── work
|   └── storage
└── user
    ├── home
    ├── scratch
    └── work
```

This `cluster` folder could be placed in our home directory. If you decide to place it somewhere else, **mandatorily** change the location in `home_dir` variable defined below in the script.

In this function we use 4 variables that **must be changed** for each user:
- `home_dir`: location of the folder structure shown as above;
-  `username`: your personal cluster user name;
- `groupname`: your research group name;
- `servername`: the address for the cluster's file transfer node. It could be either  `"hpc-transfer-1.cubi.bihealth.org"` or `"hpc-transfer-2.cubi.bihealth.org"`

```bash
function mount_BIH_HPC() {
    local username="muca10_c"  # use your own
    local groupname="drexler"  # your ag
    local home_dir="/home/muca10/cluster" #my choice
    local servername="hpc-transfer-2.cubi.bihealth.org" #cluster information

    # Create the local directory if it doesn't exist
    if [ ! -d "$home_dir" ]; then
        mkdir -p "$home_dir"
        mkdir -p "$home_dir/group"
        mkdir -p "$home_dir/group/work"
        mkdir -p "$home_dir/group/scratch"
        mkdir -p "$home_dir/group/storage"
        mkdir -p "$home_dir/user"
        mkdir -p "$home_dir/user/home"
        mkdir -p "$home_dir/user/work"
        mkdir -p "$home_dir/user/scratch"
    fi

    options="auto_cache,reconnect,no_readahead,kernel_cache,Ciphers=aes128-ctr" #,Compression=no"

    # Mount the remote file system using SSHFS
    sshfs "$username"@"$servername":"/data/cephfs-1/home/users/$username" "$home_dir/user/home"
    sshfs "$username"@"$servername":"/data/cephfs-1/work/groups/$groupname/users/$username/" "$home_dir/user/work"
    sshfs "$username"@"$servername":"/data/cephfs-1/scratch/groups/$groupname/users/$username" "$home_dir/user/scratch"
    sshfs "$username"@"$servername":"/data/cephfs-1/work/groups/$groupname" "$home_dir/group/work"
    sshfs "$username"@"$servername":"/data/cephfs-1/scratch/groups/$groupname" "$home_dir/group/scratch"
    sshfs "$username"@"$servername":"/data/cephfs-2/unmirrored/groups/$groupname" "$home_dir/group/storage"

    # Check if the mount was successful
    echo ""
    echo ""
    if [ $? -eq 0 ]; then
        echo "Remote file system mounted successfully, Darwin would be praised!"
    else
        echo "Failed to mount the remote file system, It could be a duck!"
    fi
    echo ""
    echo ""
}

function unmount_BIH_HPC() {
    local home_dir="/home/murilo/cluster" #same as above

    sudo echo "Umounting BIH HPC folders!"

    # Mount the remote file system using SSHFS
    sudo umount -l "$home_dir/user/home"
    sudo umount -l "$home_dir/user/work"
    sudo umount -l "$home_dir/user/scratch"
    sudo umount -l "$home_dir/group/work"
    sudo umount -l "$home_dir/group/scratch"
    sudo umount -l "$home_dir/group/storage"

    echo ""
    echo ""
    # Check if the mount was successful
    if [ $? -eq 0 ]; then
        echo "Remote file system umounted successfully, Darwin would be praised!"
    else
        echo "Failed to umount the remote file system, It could be a duck!"
    fi
    echo ""
    echo ""
}
```

One could copy and paste this function through the **local** terminal or save it on the terminal's profile file `~/.bashrc`. Once this function is available, just type:

```bash
mount_BIH_HPC
```
or 

```bash
unmount_BIH_HPC
```

## References

This content was built using information provided by our BIH HPC Documentation. Detailed explanations of storage access instructions and good practices can be find in:
- [BIH HPC Docs - Storage and Volumes: Locations](https://hpc-docs.cubi.bihealth.org/storage/storage-locations/)

To learn more on how to customize your shell and add functions and aliases in your profile, just check:
- [Bashrc Customization Guide – How to Add Aliases, Use Functions, and More](https://www.freecodecamp.org/news/bashrc-customization-guide/)


