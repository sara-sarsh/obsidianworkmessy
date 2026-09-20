
to add common as a package editable:
cd /opt/evck
source opt/evck/.venv/bin/activate
pip install -e .

you need to have internet


---
- This is how I replace the whole common package from my local place to the orangepi board, and this instruction will be used to write the Installer bash script and these are templates notes:
	- Checking the permission:
	- ls -ld /path/to/directory
	While you *can* definitely use SCP for this, there is actually a much better tool for this exact scenario: **rsync**.
SCP (Secure Copy Protocol) only copies files. To do what you want with SCP, you would have to SSH into the Orange Pi, delete the old files manually, and then push the new files over.
rsync can do all of that in a single, safe command. It will sync your local folder to the remote folder, automatically delete anything on the Orange Pi that isn't on your local machine, and **it will not change the permissions of the target directory itself**.
Here is how to do it both ways so you can choose what you are most comfortable with.
### Method 1: The rsync Way (Recommended)
You will run this command from your **local machine's terminal** (not while logged into the Orange Pi).
```bash
rsync -av --delete /path/to/local/files/ orangepi@orangepizero2w:/opt/evck/target_folder/

```

> [!warning] کامند
> این دستور رو استفاده کردم و خیلی خوب بود.
> 


rsync -av --delete --exclude='.venv' --exclude='logs' "/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/" orangepi@192.168.43.189:/opt/evck/

/Users/sara/Documents/Personal/Work/Tesla/github-evckcli/evck160c- decentralized/opt/evck/common

**Why this is the best method:**
 * **--delete**: This is the magic flag. It tells rsync to delete any files inside /opt/evck/target_folder/ that do not exist in your local folder. This perfectly replaces the contents.
 * **-a (Archive)**: Preserves file permissions, times, and symbolic links for the files being transferred.
 * **-v (Verbose)**: Shows you exactly what files are being deleted and copied in real-time.
 * **The Trailing Slash (/)**: Notice the slash at the end of /path/to/local/files/. This is **crucial**. It tells rsync to copy the *contents* of the folder, not the folder itself. This ensures your remote directory's drwxr-xr-x permissions remain completely untouched.
### Method 2: The scp Way (The Two-Step Method)
If you don't have rsync installed or prefer SCP, you have to do it in two steps to get the "replacement" effect.
**Step 1: Empty the remote directory**
First, SSH into your Orange Pi and remove everything *inside* the target folder.
```bash
ssh orangepi@orangepizero2w
rm -rf /opt/evck/target_folder/*

```
*(Be very careful with the rm -rf command to ensure you include the /* at the end, so you delete the contents and not the folder itself!)*
**Step 2: SCP the new files**
Log out of the Orange Pi (or open a new terminal tab on your local machine) and push the new files over:
```bash
scp -r /path/to/local/files/* orangepi@orangepizero2w:/opt/evck/target_folder/

```
 * **-r**: Recursively copies all files and subdirectories.
 * Again, using /* ensures you are just moving the contents, keeping your remote directory's permissions safe.
Which specific directory from your /opt/evck/ list are you looking to update first, and do you want me to help write out the exact command for your specific file paths?
