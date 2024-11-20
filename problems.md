...include the following:

### Freeing Disk Space:
1. **Remove Unused Snap Packages**:
   Snap packages can consume significant space. To remove unused snaps:
   ```bash
   sudo snap remove <snap-package-name>
   ```
   To list installed snaps:
   ```bash
   snap list
   ```

2. **Clean APT Cache**:
   If you're using `apt`, clear the cache to free up space:
   ```bash
   sudo apt autoremove
   sudo apt clean
   ```

3. **Check for Large Files**:
   Use the `du` command to identify large files or directories:
   ```bash
   sudo du -ah / | sort -rh | head -n 20
   ```

4. **Remove Old Kernels**:
   If you have multiple Linux kernel versions installed, remove older ones:
   ```bash
   sudo apt autoremove --purge
   ```

---

### Steps to Adjust Swap Space:
If you find enough space, follow these steps to create a larger swap file:

1. **Disable Current Swap**:
   ```bash
   sudo swapoff /swapfile
   ```

2. **Remove the Old Swap File**:
   ```bash
   sudo rm /swapfile
   ```

3. **Create a New Swap File** (e.g., 4 GB):
   ```bash
   sudo fallocate -l 4G /swapfile
   sudo chmod 600 /swapfile
   sudo mkswap /swapfile
   sudo swapon /swapfile
   ```

4. **Verify Swap**:
   ```bash
   swapon --show
   ```

5. **Make Swap Permanent**:
   Edit the `/etc/fstab` file:
   ```bash
   sudo nano /etc/fstab
   ```
   Add this line:
   ```
   /swapfile none swap sw 0 0
   ```
   Save and exit.

---

### Monitor Memory and Swap Usage
Use the `watch` command to monitor memory and swap usage while running the model:
```bash
watch -n 1 free -h
```

---

### Alternative: Optimize Model or Offload Processing
If memory issues persist despite larger swap space, consider:
1. **Using a Smaller Model**: Check if the tool provides lightweight versions.
2. **Running on a More Powerful Machine**: Offload the task to a machine with more RAM (e.g., a cloud server).
3. **Configure Model Settings**: Adjust parameters (like batch size) to reduce memory usage.

Let me know if you'd like help with any specific step!
<!-- # **Memory Error Troubleshooting**
yellowflash@yellowflash-Latitude-7490:~$ ollama run llama3
Error: model requires more system memory (5.9 GiB) than is available (5.5 GiB) but i have 8 gb raam in pc
ChatGPT said:
ChatGPT
It seems that the error you're encountering is related to the system memory available for running the model. The error message indicates that the model requires 5.9 GiB of RAM, but only 5.5 GiB is available to the process.

## **Initial Error Message**
- **Command**: `ollama run llama3`
- **Error**: Model requires more system memory (5.9 GiB) than available (5.5 GiB)
Despite having 8 GB of total RAM in your PC, the issue could be due to several factors:

## **Possible Causes**
1. **Available RAM for the Process**: Other applications may be using RAM, reducing available memory for the model.
2. **Memory Fragmentation**: Insufficient contiguous memory blocks.
3. **System Configuration**: Hardware or software memory limits.
Available RAM for the Process: Your operating system and other running applications might be using a significant portion of the RAM, leaving less for ollama run llama3. Even though your system has 8 GB, the available RAM for the model might be less.

## **Troubleshooting Steps**
- **Close Unnecessary Applications**: Free up RAM by closing unused applications.
- **Increase Swap Space**: Add virtual memory.
Memory Fragmentation: The available 5.5 GB of RAM might be fragmented in a way that the system is unable to allocate the required contiguous memory block for the model.

### **To Increase Swap Space on Ubuntu**
```bash
System Configuration: Some systems might have hardware restrictions, such as the amount of memory allocated for certain processes, especially if you are running in a virtualized environment or with limits imposed by your system configuration.

Troubleshooting Steps:
Close Unnecessary Applications: Close any unnecessary applications or processes that might be consuming RAM. Check your task manager or system monitor to see which processes are using the most memory.

Increase Swap Space: You can try increasing the swap space (virtual memory) on your system. This will allow your system to use disk space as additional memory, though this will be slower than using physical RAM.

To increase swap space on Ubuntu:

Open a terminal and run:
bash
Copy code
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
```
- **Check for Memory Leaks**: Verify if there are known memory issues with the software.
- **Check Model Requirements**: Ensure compatibility with your system.
- **Use a Lighter Model**: Opt for smaller models if available.
This will add an additional 4 GB of swap space, but be mindful that using swap space can affect performance.
Check for Memory Leaks: Sometimes, programs have memory leaks that cause them to consume more RAM than they should. You could check if there are any known issues with the ollama tool or the llama3 model related to memory consumption.

## **Current System Status**
- **RAM**: 7.6 GiB total, 3.4 GiB available
- **Swap**: 2.0 GiB total, minimally used
Check Model Requirements: Make sure the version of the llama3 model you're trying to use is compatible with your system. Some versions might be optimized for more powerful systems or might require additional configurations for lower-memory environments.

## **Additional Steps**
1. **Verify Current Swap Configuration**:
   ```bash
   swapon --show
   ```
2. **Check for Existing Swap**:
   ```bash
   lsblk
   ```
3. **Disable Existing Swap** (if necessary):
   ```bash
   sudo swapoff -a
   ```
4. **Recreate Swap File**:
Use a Lighter Model: If possible, you could try using a smaller version of the model that requires less memory, if ollama provides that option.

Check for System Memory Limits: Ensure that there are no system-level limits on memory usage. You can check if there are any cgroup or ulimit settings limiting the amount of memory that a process can use.

You can check your available RAM with:

bash
Copy code
free -h
This will help you confirm if the system is indeed running out of memory and how much is being used.

Let me know if you need further guidance on any of these steps!



You said:
yellowflash@yellowflash-Latitude-7490:~$ sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
[sudo] password for yellowflash: 
fallocate: fallocate failed: Text file busy
mkswap: error: /swapfile is mounted; will not make swapspace
swapon: /swapfile: swapon failed: Device or resource busy
yellowflash@yellowflash-Latitude-7490:~$ 
ChatGPT said:
ChatGPT
It seems that you're encountering an issue because the swap file you are trying to create is already in use or mounted. This can happen if your system already has a swap partition or file in use.

Here are the steps you can take to troubleshoot and resolve this:

1. Check Current Swap Configuration
First, check if swap space is already enabled and how much is in use. Run the following command:

bash
Copy code
swapon --show
This will display the swap devices or files currently in use.

2. Check if Swap is Already Enabled via Partition
If your system already has a swap partition or swap file, you can confirm it by looking at the system’s swap space.

To see all partitions and their usage, run:

bash
Copy code
lsblk
Look for a partition marked as swap.

If you already have a swap partition, you might not need to add another one, but if it is insufficient, you could consider increasing its size or adding more swap space.

3. Disable Existing Swap (If Necessary)
If there is a swap file that is mounted and you want to replace it with a new one (for example, the one you're trying to create), you need to disable the existing swap first.

To disable the existing swap:

bash
Copy code
sudo swapoff -a
Then, you can proceed with creating a new swap file as before:

bash
Copy code
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
This should allow you to create a new swap file.

4. Check for the "Text file busy" Error
The "Text file busy" error you encountered when trying fallocate suggests that there might be an issue with the swap file already being in use or there might be some file lock on it. If you're unable to create a new swap file, try rebooting your system to clear any locks or conflicts and attempt the swap creation process again.

5. Verify Swap Is Active
After adding the new swap file, verify that the swap is active by running:

bash
Copy code
swapon --show
You should see your new swap file listed.

6. Make Swap Permanent
To make the swap file permanent (i.e., it will be automatically used after a reboot), add it to /etc/fstab.

Edit /etc/fstab with a text editor (e.g., nano):

bash
Copy code
sudo nano /etc/fstab
Add the following line at the end of the file:

bash
Copy code
/swapfile none swap sw 0 0
Save and exit the editor (in nano, it's Ctrl + X to exit, then press Y to confirm saving).

7. Re-enable Swap After Reboot
After adding the swap to /etc/fstab, it should automatically be used after the next reboot. However, if you need it immediately, you can use:

bash
Copy code
sudo swapon /swapfile
Next Steps
Once the swap space is properly configured, try running your ollama run llama3 command again. If the issue was related to memory, adding more swap space should resolve it.

Let me know if you encounter any further issues!



You said:
yellowflash@yellowflash-Latitude-7490:~$ free -h
               total        used        free      shared  buff/cache   available
Mem:           7.6Gi       4.2Gi       212Mi       727Mi       4.2Gi       3.4Gi
Swap:          2.0Gi       256Ki       2.0Gi
yellowflash@yellowflash-Latitude-7490:~$ 
ChatGPT said:
ChatGPT
It looks like your system already has 2 GB of swap space, and it's only using a small amount of it (256 KiB), while you have 3.4 GiB of available memory remaining. This suggests that your system is not currently using the swap space heavily.

Given that you have enough memory (3.4 GiB available) and swap space (2 GiB), it's surprising that you're getting the error about needing more memory (5.9 GiB) for the model to run.

Here are a few possible explanations and steps to address the issue:

1. Memory Fragmentation or Allocation Issues
Even though you have 3.4 GiB of available memory, memory fragmentation or other processes might be preventing the allocation of a contiguous block of memory large enough for the model.

To mitigate this:

Close unnecessary applications: Close other apps or processes that might be consuming memory to free up more resources for the model.
Try restarting your system: This will help clear any memory fragmentation or issues with processes holding onto memory.
2. Limitations of the Model or Application
The model you're running might be trying to allocate more memory than is available due to how it is designed. Some models or applications may also have their own internal memory limits or inefficiencies, causing them to require more than what’s available.

To address this:

Check if the model supports lower memory configurations: Some models might have optimized versions or smaller variants that require less memory.
Try running smaller models: If possible, test with a smaller model or a different configuration of llama3 that requires less memory.
3. Increase Swap Space (Optional)
If you believe more memory is needed, you can further increase your swap space, though this will likely slow things down since disk space is slower than RAM.

Here’s how to extend your swap file:

Create a larger swap file (e.g., 4 GiB or more):

bash
Copy code
sudo fallocate -l 4G /swapfile2
sudo chmod 600 /swapfile2
sudo mkswap /swapfile2
sudo swapon /swapfile2
Check swap space with:

bash
Copy code
swapon --show
Optional: Add this new swap file to /etc/fstab to make it permanent.

4. Check Process Memory Usage
You might want to check how much memory other processes are consuming. Run:

bash
Copy code
top
This will show you the processes using the most memory. You can also use htop for a more detailed and interactive view if it's installed.

5. Check for Model-Specific Configuration
Some models have specific configurations for system memory, like chunk sizes or parameters that control memory usage. Check the documentation for ollama or llama3 to see if there are any settings to reduce memory consumption.

6. Testing the Swap Usage
If you're still encountering issues, it might be worth testing whether the model is using the swap space properly.

Run the model with your current swap space (2 GiB).
Monitor memory usage and swap usage using:
bash
Copy code
watch -n 1 free -h
This will show you real-time memory and swap usage while the model is running.

7. Recheck the Error
After making the above adjustments, try running the model again and check if the error persists. If the issue is about needing more memory, the increased swap or freeing up system memory should resolve it.

Let me know if you need further assistance or if any of these steps help you resolve the issue!



You said:
yellowflash@yellowflash-Latitude-7490:~$ swapon --show
NAME      TYPE SIZE USED PRIO
/swapfile file   2G 256K   -2
ChatGPT said:
ChatGPT
The output from swapon --show indicates that you have a 2 GB swap file (/swapfile) set up, but it is currently only using a very small amount (256 KiB) of swap space. This suggests that your system hasn't yet needed to use much swap, possibly because your available physical memory (3.4 GiB) is sufficient for the processes running.

So why are you still getting the memory error?
It could be that even though you have swap space available, the model or application you're running (ollama run llama3) is attempting to allocate a large contiguous block of memory that exceeds what's available in physical RAM, and possibly also in swap.

Possible next steps to troubleshoot:
1. Free Up System Memory
Even though you have 3.4 GiB of available memory, it's important to ensure that other processes are not consuming more memory than necessary. You can:

Close unnecessary applications to free up RAM.
Use top or htop to check memory usage and identify any processes consuming large amounts of memory.
2. Use More Swap
While it's less optimal than using physical RAM, you can extend the swap space to give the model more room to run. To add more swap, follow these steps:

Create a larger swap file (e.g., 4 GiB):

bash
Copy code
sudo fallocate -l 4G /swapfile2
sudo chmod 600 /swapfile2
sudo mkswap /swapfile2
sudo swapon /swapfile2
Check that the swap is active:

bash
Copy code
swapon --show
Verify available swap space. After adding the new swap file, you should see both /swapfile and /swapfile2 listed with the total size available for swap.

(Optional) If you'd like to make the new swap file permanent, add it to your /etc/fstab:

Edit the /etc/fstab file:
bash
Copy code
sudo nano /etc/fstab
Add the following line at the end of the file:
bash
Copy code
/swapfile2 none swap sw 0 0
Save and exit.
3. Reduce Model Memory Usage (If Possible)
Check if there are any options in ollama or llama3 to reduce the memory footprint of the model. Some models have settings for reducing batch sizes or memory usage.

4. Check System Logs for Errors
You might want to check your system logs for more detailed error messages regarding memory allocation. Look in the dmesg log:

bash
Copy code
dmesg | grep -i "out of memory"
This might give you more information on why memory allocation is failing.

5. Consider Using a Lighter Version of the Model
If llama3 is too large for your system's available memory, see if you can use a smaller variant of the model (e.g., a 7B or 13B version if you're using a larger model like 70B) or another model that's optimized for lower memory usage.

Summary
You currently have 2 GiB of swap space, but it's barely used.
Consider increasing swap space to provide more room for memory-heavy processes.
Free up as much system memory as possible by closing unnecessary applications.
Check if the model you're running has options for reducing memory usage.
Let me know if you'd like further assistance on any of these steps!



You said:
yellowflash@yellowflash-Latitude-7490:~$ swapon --show
NAME      TYPE SIZE USED PRIO
/swapfile file   2G 256K   -2
yellowflash@yellowflash-Latitude-7490:~$ lsblk
NAME        MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0         7:0    0     4K  1 loop /snap/bare/5
loop1         7:1    0 316.5M  1 loop /snap/code/174
loop2         7:2    0 316.5M  1 loop /snap/code/176
loop3         7:3    0 104.2M  1 loop /snap/core/17200
loop4         7:4    0  55.4M  1 loop /snap/core18/2846
loop5         7:5    0  55.7M  1 loop /snap/core18/2829
loop6         7:6    0    64M  1 loop /snap/core20/2379
loop7         7:7    0  63.7M  1 loop /snap/core20/2434
loop8         7:8    0  74.2M  1 loop /snap/core22/1621
loop9         7:9    0  73.9M  1 loop /snap/core22/1663
loop10        7:10   0  66.2M  1 loop /snap/core24/609
loop11        7:11   0  66.2M  1 loop /snap/core24/490
loop12        7:12   0 115.5M  1 loop /snap/discord/214
loop13        7:13   0 110.5M  1 loop /snap/discord/216
loop14        7:14   0   273M  1 loop /snap/firefox/5273
loop15        7:15   0 273.6M  1 loop /snap/firefox/5239
loop16        7:16   0  11.1M  1 loop /snap/firmware-updater/147
loop17        7:17   0 164.8M  1 loop /snap/gnome-3-28-1804/198
loop18        7:18   0 349.7M  1 loop /snap/gnome-3-38-2004/143
loop19        7:19   0   140K  1 loop /snap/gtk2-common-themes/13
loop20        7:20   0 505.1M  1 loop /snap/gnome-42-2204/176
loop21        7:21   0   497M  1 loop /snap/gnome-42-2204/141
loop22        7:22   0  91.7M  1 loop /snap/gtk-common-themes/1535
loop23        7:23   0 443.2M  1 loop /snap/kf5-5-111-qt-5-15-11-core22/7
loop24        7:24   0  11.1M  1 loop /snap/ngrok/214
loop25        7:25   0  11.1M  1 loop /snap/ngrok/226
loop26        7:26   0 182.4M  1 loop /snap/postman/289
loop27        7:27   0  12.9M  1 loop /snap/snap-store/1113
loop28        7:28   0 182.6M  1 loop /snap/postman/291
loop29        7:29   0  12.2M  1 loop /snap/snap-store/1216
loop30        7:30   0  40.4M  1 loop /snap/snapd/20671
loop31        7:31   0  38.8M  1 loop /snap/snapd/21759
loop32        7:32   0   564K  1 loop /snap/snapd-desktop-integration/247
loop33        7:33   0   568K  1 loop /snap/snapd-desktop-integration/253
loop34        7:34   0 182.2M  1 loop /snap/spotify/79
loop35        7:35   0 184.5M  1 loop /snap/spotify/80
loop36        7:36   0 447.4M  1 loop /snap/telegram-desktop/6321
loop37        7:37   0 447.4M  1 loop /snap/telegram-desktop/6336
loop38        7:38   0 150.3M  1 loop /snap/thunderbird/562
loop39        7:39   0 150.3M  1 loop /snap/thunderbird/571
loop40        7:40   0 321.1M  1 loop /snap/vlc/3777
nvme0n1     259:0    0 238.5G  0 disk 
├─nvme0n1p1 259:1    0   512M  0 part /boot/efi
└─nvme0n1p2 259:2    0   238G  0 part /var/snap/firefox/common/host-hunspell
                                      /
yellowflash@yellowflash-Latitude-7490:~$ 
ChatGPT said:
ChatGPT
The output from lsblk shows that you don't have a dedicated swap partition but rather a swap file located on your primary disk (/swapfile). The disk nvme0n1 is your main storage device, with the root partition mounted as /.

This suggests that there might not be enough space on the disk to create a significantly larger swap file, or your current swap file may not be large enough for your needs. Your root partition (/) appears to be using most of the available space on your disk, with only around 238 GB total space, and a portion of that is used for the Firefox snap (/var/snap/firefox/common/host-hunspell).

Here are a few potential steps you can take to address the memory issue:

1. Verify Available Disk Space
To check if there is sufficient free space on your disk to create a larger swap file, you can run:

bash
Copy code
df -h
This will show you the available space on your filesystem and let you know if you're running low on disk space.

2. Freeing Up Disk Space
If your disk is nearly full, consider cleaning up unnecessary files or removing unused snap packages. Some steps to help free up space:

Clean up old snap versions: Snap stores old versions of packages, which can take up significant space. You can remove old versions with the following command:

bash
Copy code
sudo snap list --all
sudo snap remove <package-name> --revision <revision-number>
Clean the apt cache: This can free up some space used by old package versions.

bash
Copy code
sudo apt clean
Remove unused packages:

bash
Copy code
sudo apt autoremove
3. Resize or Increase the Swap File
Once you've verified there's enough disk space, you can create a larger swap file (e.g., 4 GB or more). Here's how to resize the swap file:

Turn off the current swap file:

bash
Copy code
sudo swapoff /swapfile
Resize the swap file (e.g., to 4 GB):

bash
Copy code
sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
Enable the new swap file:

bash
Copy code
sudo swapon /swapfile
Check the new swap size:

bash
Copy code
swapon --show
4. Consider Using a Larger External Drive for Swap
If you run into space limitations on your primary drive, you could consider adding a swap file on an external drive (e.g., USB, external SSD). You would need to mount the external drive, create a swap file on it, and enable it.

5. Check System Logs for Memory Issues
Since the model is still requiring more memory than your system can provide, checking logs for out-of-memory errors might help. Run the following to see if there are any memory-related errors:

bash
Copy code
dmesg | grep -i "out of memory"
This can provide insights into whether the kernel is killing processes due to memory limits.

Conclusion
Check disk space with df -h to ensure you have enough free space to create a larger swap file.
Clean up disk space if needed by removing old snaps, unnecessary packages, and clearing the apt cache.
Resize the swap file to a larger size (e.g., 4 GB) if there's enough disk space available.
Monitor swap usage using swapon --show to ensure the new swap file is active and in use.
Let me know if you run into any issues, and I can guide you through more troubleshooting steps!



You said:
yellowflash@yellowflash-Latitude-7490:~$ sudo fallocate -l 4G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
mkswap: /swapfile: warning: wiping old swap signature.
Setting up swapspace version 1, size = 4 GiB (4294963200 bytes)
no label, UUID=17144a9a-9892-44e9-8917-8c53542a775d
yellowflash@yellowflash-Latitude-7490:~$ 
ChatGPT said:
ChatGPT
Great! It looks like you've successfully resized your swap file to 4 GB, and the new swap space has been set up. The warning message (wiping old swap signature) is expected because the old swap file was replaced with the new one.

Now, let's make sure the system recognizes and activates the new swap space properly. Here's how you can verify and ensure everything is working:

1. Enable the New Swap File
You may need to turn on the new swap file, especially if you haven't done so yet. To activate the swap:

bash
Copy code
sudo swapon /swapfile
2. Check the Swap Usage
Now, verify that the swap is active and showing the correct size:

bash
Copy code
swapon --show
You should see the swap file /swapfile with a size of 4 GiB listed, and it should be in use.

3. Verify Disk Space and Swap Usage
You can also use free -h to verify the overall memory and swap usage:

bash
Copy code
free -h
This should show that you now have 4 GiB of swap space available. The output should look something like:

makefile
Copy code
Swap:           4.0Gi       0B      4.0Gi
4. Make Swap Permanent
To ensure the swap file is used automatically at every boot, you'll need to add it to your /etc/fstab. Here’s how to do it:

Open the /etc/fstab file with a text editor (e.g., nano):

bash
Copy code
sudo nano /etc/fstab
Add the following line at the end of the file:

bash
Copy code
/swapfile none swap sw 0 0
Save and exit (Ctrl + X, then Y to confirm, and Enter to exit).

This will ensure that the swap file is activated automatically on reboot.

5. Monitor the Model Execution
Now that you have increased your swap space, try running your model again:

bash
Copy code
ollama run llama3
If memory was the issue, this should resolve it. You can monitor the memory and swap usage while the model is running with:

bash
Copy code
watch -n 1 free -h
This command will update the memory usage every second, allowing you to see how much memory and swap is being used in real time.

Let me know how it goes! If you're still encountering issues, we can dive deeper into other possibilities.



You said:
yellowflash@yellowflash-Latitude-7490:~$ swapon --show
NAME      TYPE SIZE USED PRIO
/swapfile file   4G   0B   -2
yellowflash@yellowflash-Latitude-7490:~$ \
ChatGPT said:
ChatGPT
It looks like the swap file has been successfully resized to 4 GB, but it is not being actively used (USED 0B). This might happen if the system hasn't yet needed to use swap because there is still enough available physical RAM.

Here's what you can do next to ensure everything is functioning properly:

1. Verify Swap Activation
First, let's make sure the swap file is properly activated. Try running the following command to manually activate the swap space again:

bash
Copy code
sudo swapon /swapfile
Then, verify the swap again with:

bash
Copy code
swapon --show
You should see that /swapfile is now actively being used.

2. Check System Memory Usage
If your system still has sufficient free memory, it might not need to use the swap space right away. You can check memory and swap usage by running:

bash
Copy code
free -h
If it shows a significant amount of free memory (RAM), the system might not be swapping because it doesn't need to. To test swap usage, you can force memory consumption to trigger swap usage.

3. Force Memory Usage (Testing Swap)
If you want to simulate a high memory load and test if the swap file is working, you can run a memory-intensive command like this:

bash
Copy code
stress --vm 2 --vm-bytes 3G --timeout 30s
This will allocate 3 GB of memory for 30 seconds, which should force the system to start using the swap space. If swap is used, you should see it in free -h or swapon --show.

If you don't have the stress tool installed, you can install it using:

bash
Copy code
sudo apt install stress
Alternatively, if you're running memory-heavy tasks or models, you can also monitor swap usage while running those tasks.

4. Check System Logs for Swap Issues
If the swap is still not being used, there may be some issue with swap configuration or memory settings. You can check the system logs for any relevant messages:

bash
Copy code
dmesg | grep -i swap
This will show you if there are any warnings or errors related to swap usage.

Conclusion
Activate the swap file manually using sudo swapon /swapfile.
Monitor memory usage with free -h and see if swap is needed.
Force memory usage to test swap functionality using the stress tool.
Check system logs (dmesg | grep -i swap) for any related errors or warnings.
Let me know if you continue to encounter issues!







 -->
