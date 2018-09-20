# Toilet

TOIlet - display large colourful characters

This is pretty long, I'm writing a tutorial and notes for you as I go.

## Installation and Configuration

Open the Terminator.

### Install toilet

	sudo apt install toilet

Remember `sudo` commands will ask you for your password to confirm you have permission to install something.

Let's try it out.

	toilet Brian

This should output:

                                   
	 mmmmm           "
	 #    #  m mm  mmm     mmm   m mm
	 #mmmm"  #"  "   #    "   #  #"  # 
	 #    #  #       #    m"""#  #   # 
	 #mmmm"  #     mm#mm  "mm"#  #   # 

Cool, but there are more fonts out there to mess with, let's get them.

### Change Permissions

First we'll need to change the permissions of the fonts directory. The current owner and group are `root` and `root`.

You can verify by using `ls -l`. The `ls` command is used to **list directory contents**. The `-l` part is called a *switch* which tells the program to do special or alternate things. More specifically `-l` is for *use a long listing format*.

	ls -l /usr/share

As a quick side note, you can get more information about any command in linux by using the `man` or manual command. This will show you a description of the command and an explanation of all of the *switches*, for example for `ls`, to quit the man page just press the `q` key.

	man ls

This will display everything in the `/usr/share` path, you can scroll up until you find `figlet` on the far right, it is in alphabetical order, so it should be pretty straight forward.

It should look like:

	drwxr-xr-x    2 root root 20480 Sep 20 12:53 figlet
	^             ^ ^    ^    ^     ^            ^
	|             | |    |    |     |            |
	|             | |    |    |     |            +-> name of the path or file
	|             | |    |    |     +--------------> modification date
	|             | |    |    +--------------------> file size or size of the contents
	|             | |    +-------------------------> owners group
	|             | +------------------------------> owners name
	|             +--------------------------------> number of links
	+----------------------------------------------> permissions of the path or file
	

We want to change the owners name and owners group to your name and your group.

To see your name and group you can do:

	whoami
	groups

This should output whatever your username is and the groups you are part of.

Mine for example outputs these, yours will be slightly different.

	pjobson
	pjobson sudo www-data docker
	
Yours probably shows something like this, though I can't remember what we made your username so instead of `brian` it may show something else.

	brian
	brian sudo

The easiest way to change the ownership of your path is:

	sudo chown -R $(whoami).$(whoami) /usr/share/figlet

This command tells linux to use the `sudo` command for higher level privileges to `chown` or change ownership with `-R` for recursive, we're changing the permissions of the `/usr/share/figlet` path AND all of the contents inside of it.

Now we can do `ls` again, this time we'll narrow down the result set so we don't see all of the other paths.

	ls -l /usr/share | grep figlet

It should show something like where `brian` is your user name and group.

	drwxr-xr-x    2 brian brian 20480 Sep 20 12:53 figlet

### Get Some Fonts

First change directory to `Downloads`. The `cd` command is for changing directory.

	cd ~/Downloads

Next download the font archive with `wget`. The `wget` command is for downloading files directly to the terminal without using a browser.

Let's test to see if `wget` is installed.

	wget

If you get an error message:

	Command 'wget' not found, but can be installed with:

	sudo apt install wget

You can follow the instructions and do:

	sudo apt install wget

If you don't get an error it'll show this informational message:

	wget: missing URL
	Usage: wget [OPTION]... [URL]...

	Try `wget --help' for more options.

From here you can do that or use the `man` command for more information about `wget`.

Now you can actually use `wget` to download the zip file.

	wget https://github.com/xero/figlet-fonts/archive/master.zip

This will output a status message, the end of it should show something like this.

	2018-09-20 13:38:15 (5.04 MB/s) - ‘master.zip’ saved [1464644]

Let's see if it actually downloaded.

	ls -l master.zip

This should show something like:

	-rw-rw-r-- 1 brian brian 1464644 Sep 20 13:38 master.zip

This shows the file exists on your computer and is `1464644` bytes.

Let's unzip it.

	unzip master.zip

This will show that it is `inflating` a bunch of files the output should look like this.

	  inflating: figlet-fonts-master/usaflag.flf
	  inflating: figlet-fonts-master/ushebrew.flc
	  inflating: figlet-fonts-master/uskata.flc
	  inflating: figlet-fonts-master/utf8.flc
	  inflating: figlet-fonts-master/wetletter.flf
	  inflating: figlet-fonts-master/wideterm.tlf 

Let's check them out:

	ls -l figlet-fonts-master/

This will show a list of all of the fonts which were extracted into the `figlet-fonts-master/` path. It should show something like this:

	total 5364
	-rwxr-xr-x 1 pjobson pjobson  1520 Sep 21  2017  1Row.flf
	-rwxr-xr-x 1 pjobson pjobson 13134 Sep 21  2017  3D-ASCII.flf
	-rwxr-xr-x 1 pjobson pjobson 23107 Sep 21  2017  3d_diagonal.flf
	-rwxr-xr-x 1 pjobson pjobson 25475 Sep 21  2017  3D Diagonal.flf
	-rwxr-xr-x 1 pjobson pjobson 14219 Sep 21  2017  3d.flf
	-rwxr-xr-x 1 pjobson pjobson  8762 Sep 21  2017  3-D.flf
	-rwxr-xr-x 1 pjobson pjobson  3949 Sep 21  2017  3x5.flf
	-rwxr-xr-x 1 pjobson pjobson  4374 Sep 21  2017  4Max.flf
	...many more entries...

### Permisions Fix

Unfortunately the permissions on some of the font files are incorrect, many of them are set to executable, which they shouldn't be. I'm not going to get too far into this right now, we'll get into this more on a later date.

To fix them use `chmod` with the `-x` switch, you can run `man` on `chmod` to learn more about it.

	chmod -x figlet-fonts-master/*

### Copy the Fonts

In order to get toilet to see the fonts, we need to put them into the `/usr/share/figlet` path.

	cp -v figlet-fonts-master/* /usr/share/figlet

This should show something like this:

	'figlet-fonts-master/1Row.flf' -> '/usr/share/figlet/1Row.flf'
	'figlet-fonts-master/3D-ASCII.flf' -> '/usr/share/figlet/3D-ASCII.flf'
	'figlet-fonts-master/3d_diagonal.flf' -> '/usr/share/figlet/3d_diagonal.flf'
	'figlet-fonts-master/3D Diagonal.flf' -> '/usr/share/figlet/3D Diagonal.flf'
	'figlet-fonts-master/3d.flf' -> '/usr/share/figlet/3d.flf'
	'figlet-fonts-master/3-D.flf' -> '/usr/share/figlet/3-D.flf'
	...many more entries...

### Test Toilet

Let's try using a different font. Again we're using the `toilet` command with the `-f` switch to specify a different font, this one called `mono9`.

	toilet -f mono9 Brian

This should output.

                                   
	 ▄▄▄▄▄           ▀
	 █    █  ▄ ▄▄  ▄▄▄     ▄▄▄   ▄ ▄▄
	 █▄▄▄▄▀  █▀  ▀   █    ▀   █  █▀  █ 
	 █    █  █       █    ▄▀▀▀█  █   █ 
	 █▄▄▄▄▀  █     ▄▄█▄▄  ▀▄▄▀█  █   █ 
                                   
### Font List

Let's dump all of the fonts to a text file on your desktop. We're going to use `ls` again, but this time we're going to do a thing called piping. here the `>` character is going to send the output to this text file `~/Desktop/toilet.fonts.txt`.

	ls /usr/share/figlet > ~/Desktop/toilet.fonts.txt

Now you can click on the **Z** button and open a text editor and then open the `toilet.fonts.txt` file on your desktop.

You'll see a bunch of files which end with `flf` or `flc` you can use any of them with `toilet`, you can leave off the extension. Try some of these, or whatever you want!

	toilet -f 3x5 Brian
	toilet -f Alligator Brian
	toilet -f bigmono12 Brian
	toilet -f eftifont Brian

