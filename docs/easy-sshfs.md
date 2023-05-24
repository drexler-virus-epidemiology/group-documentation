# Connecting  file systems via sshf

In our BIH HPC, we have a well defined storage and data structure for different kinds of entities, such as `users` (our cluster user, normally it is the Charite's username + "_c") or `group` ("ag" + "-" or "_" + P.I. name). Each entity has tree kinds of storage: i) home, for the smallest configuration files, ii) work, for long-persistent storage, and iii) scratch, for large and non-persistent files.

Above, we show the location of each of these folders at the cluster's filesystem. These locations are written in a UNIX based fashion.

## User folders

In this case, `$NAME` is a charite account + "_c", such as`muca10_c`.

| Folder | Location | Description |
| --- | --- | --- |
| Home | /fast/users/$NAME | small, persistent, and safe storage, e.g., for documents and configuration files (default quota of 1GB). |
| Work | /fast/users/$NAME/work | larger and persistent storage, e.g., for your large data files (default quota of 1TB). |
| Scratch | /fast/users/$USER/scratch | large and non-persistent storage, e.g., for temporary files, files are automatically deleted after 2 weeks (default quota of 100TB; deletion not implemented yet). |

## Group folders

In our case, `$GROUP` is `ag-drexler`.
        
| Folder | Location | description |
| --- | --- | --- |
| Work | /fast/work/groups/$GROUP | Persistent files from the group |
| Scratch | /fast/scratch/groups/$GROUP | Large and non-persistent files from the group |

## How to easily connect our Linux or WSL machines

To make file transfer easy as a copy & paste or a drag & drop operation, here we provide an interesting and useful way to do so. The idea is build a function to call every time we need to mount the remote folder locally. For that, first we define a folder structure like the on showed bellow:

```
cluster
├── group
│   ├── scratch
│   └── work
└── user
    ├── home
    ├── scratch
    └── work
```

This folder could be placed in our home directory. If you decide to place it somewhere else, **mandatorily** change the location from `home_dir` variable defined above.

In this function we use 4 variables that **MUST be changed** for each user:

- `home_dir`: location of the folder structure shown as above;
-  `username`: your personal cluster user name;
- `groupname`: your research group name;
- `servername`: the address for the file transfer machine located at cluster. It could be either  `"hpc-transfer-1.cubi.bihealth.org"` or `"hpc-transfer-1.cubi.bihealth.org"`

```bash
function mount_BIH_HPC() {
    local username="muca10_c"  # use your own
    local groupname="ag-drexler"  # your ag
    local home_dir="/home/muca10/cluster" #my choice
    local servername="hpc-transfer-1.cubi.bihealth.org" #cluster information
    
    # Create the local directory if it doesn't exist
    if [ ! -d "$home_dir" ]; then
        mkdir -p "$home_dir"
        mkdir -p "$home_dir/group"
        mkdir -p "$home_dir/group/work"
        mkdir -p "$home_dir/group/scratch"
        mkdir -p "$home_dir/user"
        mkdir -p "$home_dir/user/home"
        mkdir -p "$home_dir/user/work"
        mkdir -p "$home_dir/user/scratch"
    fi

    # Mount the remote file system using SSHFS
    sshfs "$username"@"$servername":"/fast/users/$username" "$home_dir/user/home"
    sshfs "$username"@"$servername":"/fast/work/users/$username" "$home_dir/user/work"
    sshfs "$username"@"$servername":"/fast/scratch/users/$username" "$home_dir/user/scratch"
    sshfs "$username"@"$servername":"/fast/work/groups/$groupname" "$home_dir/group/work"
    sshfs "$username"@"$servername":"/fast/scratch/groups/$groupname" "$home_dir/group/scratch"

    # Check if the mount was successful
    if [ $? -eq 0 ]; then
        echo "Remote file system mounted successfully, Darwin would be praised!"
        echo ""
        echo ""
    else
        echo "Failed to mount the remote file system, It could be a duck!"
        echo ""
        echo ""
    fi
}
```

One could copy and paste this function through the **local** terminal or save it on the terminal's profile preference file. Once this function is available, just type:

```bash
mount_BIH_HPC
```
## References

All the built content here used as basis the information provided by our BIH HPC Docs. Detailed explanations of storage access instructions and good practices could be found in:

- [BIH HPC Docs - Storage and Volumes: Locations](https://bihealth.github.io/bih-cluster/storage/storage-locations/)
- [BIH HPC Docs - Nodes and Storage Volumes](https://bihealth.github.io/bih-cluster/overview/storage/)

To learn how to customize your shell and add function and aliases in your profile, just check:

- [Bashrc Customization Guide – How to Add Aliases, Use Functions, and More](https://www.freecodecamp.org/news/bashrc-customization-guide/)

