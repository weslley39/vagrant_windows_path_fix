Vagrant Windows Path Fix
============

There are few known limitations when using Vagrant on Windows machine. One of these limitations is the lack of symbolic links support on synced folder.

Windows limits path names length, and this little thing causes many problems when working with deep folder trees structures, like we usually have in nodejs projects.

Most of the time, you can get away with ‘npm —no-bin-link’ solution. However, you need more robust solution if you are using complex tools such as Grunt or Yeoman.

In this post, I’ll show you the proper way to add symbolic links support to your Vagrant machine.

First, you need to add this code snippet inside your Vagrantfile

```
config.vm.provider "virtualbox" do |v|
    v.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
end
```

VirtualBox disables symbolic links for security reasons. In order to pass this restriction, you need to boot up the Vagrant machine in Administrator mode.

OBG: U NEDD TO RUN THE CMD OR POWERSHELL AS ADMINISTRATOR TO WORK

After that, boot up the Vagrant machine normally with ‘vagrant up’ command. Wait until the machine boots up, SSH to the machine and try to create symbolic link in the synced folder.

##But, what this shit do sr.?

It will create a folder inside your VMs home folder (which I hope is not mapped to a windows host folder) called "win_host_fixes", and when you run this script, it will create a new folder with a random name and a symlink in your project folder with the name that you provide.

###Usage
```bash
# On your project folder, type this to create the link
sh win_host_fix.sh SYMLINK_NAME
EX: sh win_host_fix.sh node_modules
EX: sh win_host_fix.sh bower_components
