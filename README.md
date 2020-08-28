#Programs
###A quick way of backing up a list of programs is to run this:

```shell
dpkg --get-selections > ~/Package.list
sudo cp -R /etc/apt/sources.list* ~/
sudo apt-key exportall > ~/Repo.keys
```


###It will back them up in a format that dpkg can read* for after your reinstall, like this:
```shell
sudo apt-key add ~/Repo.keys
sudo cp -R ~/sources.list* /etc/apt/
sudo apt-get update
sudo apt-get install dselect
sudo dselect update
sudo dpkg --set-selections < ~/Package.list
sudo apt-get dselect-upgrade -y
```

* You may have to update dpkg's list of available packages or it will just ignore your selections (see this debian bug for more info). You should do this before sudo dpkg --set-selections < ~/Package.list, like this:
```shell
apt-cache dumpavail > ~/temp_avail
sudo dpkg --merge-avail ~/temp_avail
rm ~/temp_avail
```
Settings and Personal Data
Before you reinstall, you should probably back up the settings from some of your programs, this can easily be done by grabbing folders from /etc and all the content from your user directory (not just the stuff you can see in nautilus!):
```shell
rsync --progress /home/`whoami` /path/to/user/profile/backup/here
```

After you reinstall, you can restore it with:

```shell
rsync --progress /path/to/user/profile/backup/here /home/`whoami`
```

So all together as a pseudo-bash script.
This assumes there is only one user on the machine (remove /'whoami' otherwise) and that you used the same username on both installs (modify dest. of rsync otherwise).
```shell
dpkg --get-selections > ~/Package.list
sudo cp -R /etc/apt/sources.list* ~/
sudo apt-key exportall > ~/Repo.keys
rsync --progress /home/`whoami` /path/to/user/profile/backup/here
```
##  Reinstall now
```
rsync --progress /path/to/user/profile/backup/here /home/`whoami`
sudo apt-key add ~/Repo.keys
sudo cp -R ~/sources.list* /etc/apt/
sudo apt-get update
sudo apt-get install dselect
sudo dpkg --set-selections < ~/Package.list
sudo dselect
```
