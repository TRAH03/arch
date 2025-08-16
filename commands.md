# Here is a cheatsheet of basic terminal commands because I forgor

## Navigation
```
pwd        # Print Working Directory (shows where you are)
ls         # List files in the current directory
ls -l      # Long listing (permissions, size, date, etc.)
ls -a      # Show hidden files (those starting with .)
cd /path   # Change directory
cd ..      # Go up one directory
cd ~       # Go to your home directory
```
## Create and Destroy
```
mkdir myfolder        # Make a new directory
touch file.txt        # Create an empty file
cp file.txt copy.txt  # Copy a file
mv file.txt newname   # Move or rename a file
rm file.txt           # Delete a file
rm -r myfolder        # Delete a folder and its contents
head file.txt         # Show first 10 lines
tail file.txt         # Show last 10 lines
tail -f file.txt      # Follow file output (useful for logs)
wc -l file.txt        # Count lines in a file

```
## View and Edit
```
grep "text" file      # Search for text in a file
cat file.txt          # Print file contents
less file.txt         # Scroll through file (q to quit)
nano file.txt         # Simple text editor
du -sh *              # Show disk usage of files/folders in current dir
df -h                 # Show disk space usage of mounted filesystems
file filename         # Show file type info
find . -name foo      # Search for files by name
```
## System Monitoring
```
uname -r          # Show linux kernel version
top               # process viewer included with linux
btop              # top but better needs to be installed first
kill PID          # Kill a process by PID
killall name      # Kill processes by name
uptime            # Show system uptime and load
free -h           # Show memory usage
```
## Networking
```
ip a              # Show network interfaces and IPs
ping example.com  # Test connectivity
curl example.com  # Fetch a webpage needs to be installed
wget URL          # Download a file needs to be installed
ss -tuln          # Show listening ports
```
## Permissions
```
chmod +x file            # Make a file executable
chmod 644 file           # Set read/write for owner, read for others
chown user:group file    # Change file ownership
sudo !!                  # Rerun last command with sudo
```
## Health Check
```
systemctl --failed          # check for failed services
systemctl --user --failed   # check user services
sudo touch /forcefsck       # check for filesystem errors on next boot
 mkinitcpio -P              # check for kernel/intramfs issues
```
## Arch Pacman Syntax
```
sudo pacman -S                      # Install a package
sudo pacman -R                      # Remove a package
sudo pacman -Rns                    # Remove a package and its dependencies that are not required by other packages
sudo pacman -Syu                    # Update the system (sync repositories and upgrade packages)
sudo pacman -Syyu                   # Force a full sync and upgrade
pacman -Ss                          # Search for a package in the repositories
pacman -Qs                          # Search for an installed package
pacman -Q                           # List all installed packages
pacman -Qe                          # List explicitly installed packages (not dependencies)
pacman -Qdt                         # Lists packages installed as dependencies but no longer needed
pacman -Qi                          # Show information about a specific package
sudo pacman -Rns $(pacman -Qdtq)    # Remove unused packages (orphans)
udo pacman -Sc                      # Clear the package cache
```
