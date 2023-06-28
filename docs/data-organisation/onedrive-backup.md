# Backup files on OneDrive

## What

This document describes how to use `rclone` to backup data to the Charité
OneDrive. 

## Why

Having a backup of data is a good idea to avoid loss of work and data in case
local hardware has issues.

## How

### Installation

Make sure `rclone` is installed or install it using your package manager (e. g.
`apt`).

```
# One way to install rclone 
apt install rclone

# Check the local installation
rclone --version
```

`Rclone` uses an interactive installation tool to configure the access to a
remote storage location like the OneDrive used at the Charité. The
configuration assistant will guide through the proces.

### Configuration

```
# Run the configuration assisstant and follow the steps
rclone config
```

Answer the questions as follows:

* Select `New remote`
* Set a name, e. g. `MyOneDrive`
* Select `Microsoft OneDrive` (`31` on rclone v1.62.2)
* Leave `client_id` and `client_secret` blank (press Enter) 
* Select `region>` 1 (Microsoft Global Cloud)
* Select `n` to not edit the advanced configuration
* Select `y` to automatically authenticate using a browser. Login using your
  Charité credentials
* Select `config_type>` 1 (OneDrive Personal or Business)
* Select `config_driveid>` 1 (OneDrive business)
* If the drive `https://charitede-my.sharepoint.com/` was found, select `y` to
  confirm the selection
* Select `y` to confirm the configuration
* Select `q` to quit the configuration assisstant

### Manual synchronization

The OneDrive is now correctly configured and can be used with `rclone`. Using
the name we choose, e. g. `MyOneDrive`, we can query or mount the remote
storage and synchronize it to local directories. To mount the drive, create a
new folder and start a process using `rclone`

```
# Create a new directory to mount the remote location at
mkdir ~/OneDrive

# While this process runs, the folder will stay accessible locally
rclone --vfs-cache-mode writes mount "MyOneDrive":  ~/OneDrive
```

To synchronize the local source (the data to be backuped) and the remote
destination, use `rclone sync`. Replace `<SOME/FOLDER>` and
`<SOME/REMOTE/FOLDER>` with an actual location on both you local machine and
the remote storage location.

```
rclone sync ~/<SOME/FOLDER> OneDriveBackup:<SOME/REMOTE/FOLDER>
```

### Automatic synchronization

Having a fully automatic backup solution requires more effort but is important
to make regular backups as convenient as possible. 

!!! note "Automatic Backups"
    This section should describe on how to easily configure automatic,
    scheduled backups using rclone. I have not yet found a good solution, but
    will update this section.

## Results

Have a remote backup of important data in case of data loss on your local
machine. Remember: **No backup? No pity!**

## References

* `Rclone` documentation for [OneDrive](https://rclone.org/onedrive/)
* Configuration of continuous and immediate file sync can be found [here](https://blog.rymcg.tech/blog/linux/rclone_sync/)
