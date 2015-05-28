Windows-*nix VM Long Path Names Workaround
============

Windows limits path names length, and this little thing causes many problems when working with deep folder trees structures, like we usually have in nodejs projects.

Quoting this [article](http://msdn.microsoft.com/en-us/library/aa365247(VS.85).aspx#maxpath)

>Maximum Path Length Limitation
>In the Windows API (with some exceptions discussed in the following paragraphs), the maximum length for a path is MAX_PATH, which is defined as 260 characters. A local path is structured in the following order: drive letter, colon, backslash, name components separated by backslashes, and a terminating null character. For example, the maximum path on drive D is "D:\some 256-character path string<NUL>" where "<NUL>" represents the invisible terminating null character for the current system codepage. (The characters < > are used here for visual clarity and cannot be part of a valid path string.)

When you work inside a VM, you can have synced folder and symlinks, and with the brilliant solution posted [here](https://github.com/fideloper/Vaprobash/issues/183#issuecomment-36632374) by [jgoux](https://github.com/jgoux), we can create a folder that exists only inside the *nix VM and is linked to a folder inside your project (i.e node_modules for a nodejs project).

So, I created this little shell script that do all this stuff for you.

It will create a folder inside your VMs home folder (which I hope is not mapped to a windows host folder) called "win_host_fixes", and when you run this script, it will create a new folder with a random name and a symlink in your project folder with the name that you provide.

###Usage
```bash
# On your project folder, type this to create the link
sh win_host_fix.sh SYMLINK_NAME
