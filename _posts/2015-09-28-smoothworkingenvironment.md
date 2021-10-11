---
layout: post
title:  "A Smooth Working Environment"
date:   2015-09-28 14:02:17
categories: website blog jekyll github
permalink: /posts/smooth-working-environment
image: "https://github.com/oliverangelil/oliverangelil.github.io/blob/master/photos/blog2_laptop.jpg?raw=true"

---

### Motivation
Three things prompted me to write this blog post. 1) A [blog post](https://drclimate.wordpress.com/2014/05/01/whats-in-your-bag/) by Damien Irving - a tech savvy PhD student from Melbourne, in which he lists his software "toolbox" for his climate research, 2) a recent task Nadja has been busy with consisting of restructuring the CMIP archives on [raijin](http://nci.org.au/systems-services/peak-system/raijin/) such that they are more orderly like the ones on the ETHZ and NCAR servers, helping scientists in this part of the world work more efficiently, and 3) a few hours I spent with a new PhD student at CCRC, during which I showed her some of the tips and "tricks" discussed in this post, which have helped me, and now her, immensely. My point is, a slick working environment is key. This post focuses on point 3) — configuring your PC to make life a lot easier in the long run for a climate scientist that reads in data, writes code, generates plots, and accesses servers, regularly.

### The Beginning
It has taken me years (mostly divided between taking courses and doing research) to develop a slick working environment for my climate research. Interestingly, the most valuable skills I acquired for climate research weren't taught to me from content in my university degrees, despite having Honours (University of Cape Town) and Masters degrees (ETH Zürich) in Atmospheric and Climate Science. It all began 5 years ago just before starting my Honours degree at the University of Cape Town. I was looking for a holiday job, and considering I decided to continue studying climate science, I thought an internship as a "young researcher" might be a wise idea. On campus, I walked amongst the climate science rooms knocking on the doors of senior scientists, asking "Any chance I could work for you until semester starts?" Everyone turned me down, except for a guy whose hair resembled that of a lead guitarist's from a rock band (he has since gone to the barber), and an email address starting with "stoned@"... this was going to be interesting.

### The Struggle
The next day I was back in his office with a 200GB external hard-drive, onto which he began to transfer hundreds of NetCDF files from his PC — I would later learn that large model ensembles were a key component of the [method](blame-humans) I would use to attribute extreme weather to human activity. He explained the field and method to me, and in fear he would realise I actually had no idea what on earth he was talking about, I nodded while he continued to use strange words like "threshold", "return periods", "attribution", "ensembles", "natural variability", and "distributions". The meeting ended with him suggesting I start off reading in the NetCDF files with a programming language and plot 95th percentiles of rainfall over southern Africa, as well as read through a manuscript he and some colleagues were about to submit to Nature. I had little idea what "programming" meant (let alone understood the manuscript), but the word scared the hell out of me because my elder, nerdier brother studying physics had recently started doing whatever "programming" entailed. I tried three programming languages in two months before I eventually stuck with MATLAB, and it took about 2 months after that to read in the files and plot return period curves — partly because MATLAB had no built-in NetCDF reader at the time, forcing me to find and install a function online that did the job. The entire process took ages, was extremely frustrating, unrewarding, and put me on the verge of quitting the internship entirely. Luckily I didn't — a decision that has paid off.

### Half supervisor-trained, half self-trained
Mr. "stoned@" ended up either supervising or co-supervising my Honours, Masters, and (now) PhD. His patience and helpfulness were invaluable, however because he used a relatively old-school programming language (IDL), our conversations covered mostly the scientific methods of my work. I was on my own when it came to the software/working environment. What follows is a list of tools and tips (for a programming climate scientist) I would have loved to be aware of 5 years ago, and that no course work at my universities covered (even in the umpteen courses I took pertaining to climate science). Some of these may have relatively large learning curves and thus pertain to the "younger" scientists who are not yet completely set in their ways (e.g. choice of programming language), and some of these are so easy to implement, that anyone who uses the command line could immediately benefit (e.g. aliases).

### The Meat
**Programming Language**

Selecting a programming language for data analysis is a tough and important choice. In my opinion we should consider the following before deciding:

1. How popular is the language within my field? — this helps gauge how much help one could get from colleagues.
2. How popular is the language within other fields / how flexible is the language? — this helps gauge how much online documentation there may be. Additionally, you may one day decide to switch careers or write code not pertaining to your field.
3. How intuitive/readable/concise is the syntax?
4. Is it proprietary or free?
5. <strike>What is my supervisor using?</strike> This is hardly relevant in my opinion. You may only be near your supervisor for a few years and he/she probably doesn't have the time to help a newbie learn how to program.
6. <strike>How fast is the language?</strike> PCs are so fast nowadays, that unless you plan to develop/contribute to, for example, a model consisting of thousands of lines of code, this is not particularly relevant.

A quick and dirty tally of the scores for the four questions using trinomial results (with 1 being the lowest/worst and 3 the highest/best):
  <table class="table-bordered">
		    <thead>
		      <tr>
			<th></th>
			<th>&emsp;&emsp;1) FieldPop&emsp;&emsp;</th>
			<th>&emsp;&emsp;2) Pop&emsp;&emsp;</th>
			<th>&emsp;&emsp;3) Read/Int/Concise&emsp;&emsp;</th>
			<th>&emsp;&emsp;4) Free&emsp;&emsp;</th>
			<th>&emsp;&emsp;Total&emsp;&emsp;</th>
		      </tr>
		    </thead>
		    <tbody>
		      <tr>
			<th>IDL </th>
			<td>2</td>
			<td>2</td>
			<td>3</td>
			<td>1</td>
			<th>8</th>
		      </tr>
		      <tr>
			<th>NCL</th>
			<td>3</td>
			<td>1</td>
			<td>2</td>
			<td>3</td>
			<th>9</th>
		      </tr>
		      <tr>
			<th>MATLAB</th>
			<td>2</td>
			<td>2</td>
			<td>3</td>
			<td>1</td>
			<th>8</th>
		      </tr>
		      <tr>
			<th>Python</th>
			<td>3</td>
			<td>3</td>
			<td>3</td>
			<td>3</td>
			<th>12</th>
		      </tr>
		      <tr>
			<th>R</th>
			<td>3</td>
			<td>3</td>
			<td>2</td>
			<td>3</td>
			<th>11</th>
		      </tr>
		      <tr>
			<th>Fortran</th>
			<td>2</td>
			<td>2</td>
			<td>1</td>
			<td>3</td>
			<th>8</th>
		      </tr>
		      <tr>
			<th>C</th>
			<td>1</td>
			<td>3</td>
			<td>1</td>
			<td>3</td>
			<th>8</th>
		      </tr>
		    </tbody>
</table><br>

Based on these questions, amongst the languages commonly used for climate research I would rate Python highest followed shortly by R. Of course one could argue these scores are completely subjective. To at least convince you further that python is way up there as one of the recommended languages for this field, read [this](https://drclimate.wordpress.com/2013/06/11/picking-the-right-programming-language/), and [this](http://journals.ametsoc.org/doi/abs/10.1175/BAMS-D-12-00148.1), and maybe [this](http://readwrite.com/2012/06/05/5-ways-to-tell-which-programming-lanugages-are-most-popular/) (the latter pertaints to programming in general) for information compiled by people far more knowledgeable than myself. Of course the choice of language isn't at all the be-all and end-all to a stellar climate science career (I'm sure you know successful scientists that use the "lesser rated" languages in my table) — but making a great first pick can only help!

I switched from MATLAB to Python earlier this year and was surprised by how similar the languages are. If you decide to work with Python locally, I (and [this](http://www.ccrc.unsw.edu.au/ccrc-team/students/ned-haughton) guy and [this](https://twitter.com/drclimate) guy) recommend downloading the [Anaconda](https://www.continuum.io/downloads) distribution by Continuum Analytics. If you need help getting started with Python (as a climate scientist) read [this](https://drclimate.wordpress.com/2013/10/03/getting-started-with-python/). If you need help plotting data on a map see [this](http://matplotlib.org/basemap/users/examples.html).

**Bash**

If you're going to be learning a programming language, you should be able to manage (and are strongly advised) to navigate around the command line. [James Goldie](http://www.ccrc.unsw.edu.au/ccrc-team/students/james-goldie) wrote a great bash tutorial [here](http://rensa.co/writing/shell-tut/). Once you know the basics of bash, you can start using really useful tools (see below), and your workflow won't be confined to the limitations of GUIs (Graphical User Interfaces). GUIs can be really slow when working off servers in far away places with slow internet connections — when I was ssh'ing from Cape Town to the ETHZ server the only way to work with poor bandwidth was in the terminal. I used the no window ("-nw") versions of matlab and emacs — what a life saver!

**Understanding and Visualising Your Data**

One thing we have more or less converged on in this field is data-storage. Most climate data is stored in NetCDF format. For this reason, command line tools to visualise, understand, and manipulate your data have emerged. Understanding and visualising your data is typically done with the "ncdump" and "ncview" commands. If you've ssh'd to a server type "module load netcdf" or "module load ncview" to have them available. Or even better, add them to your .bash_profile file so they are always automatically available without having to type the "module load" commands (more on adding stuff to your .bash_profile later). Commands (with their arguments/option) I commonly use are...

Print the contents/variables in your NetCDF file:

`ncdump -h file.nc`
	
Print the type of NetCDF file (NetCDF4 or NetCDF3/Classic). One use of this could be to know what functions to use to read in your data (in whatever programming language you have decided to use), as some functions only read in one type:

`ncdump -k file.nc`

Print the contents of a variable in your file (I like to know what the latitude and longitude vectors look like):

`ncdump -v lat file.nc`

Replace "lat" with "latitude" or whatever your variable name might be (`ncdump -h file.nc` will tell you).

View the contents of you file:

`view file.nc`

If this does not work on a server, you have probably forgotten the "-X" or "-Y" arguments that go with the "ssh" command. This allows for graphical interfaces when working remotely.

**Climate Operators**

I often do quick NetCDF manipulation before reading the newly generated NetCDF files into Python to do more complicated analysis and/or to generate plots. Two popular command line tools to manipulate data in NetCDF have also thankfully emerged: [CDO](https://code.mpimet.mpg.de/projects/cdo) and [NCO](http://nco.sourceforge.net/). They are quite similar (except for a few things either one can do that the other can't). I use CDO to generate new NetCDF files by manipulating data in already available ones. Keep in mind many arguments can be used in combination — CDO will execute each argument from right to left. There are [endless](https://code.mpimet.mpg.de/projects/cdo/embedded/cdo_refcard.pdf) commands to use, but a few useful commands I commonly use are...

Print all time-steps in the file to screen (you can replace "showdate" with "showyear"/"showmon", etc):

`cdo showdate file.nc`

Print the annual average surface temperature of the spatial domain. In my work the domain is usually global, if you have regional data this will still work. Replace the "tas" to whatever the surface temperature variable name is ("ncdump -h" to find out):

`cdo output -yearmean -fldmean -selvar,tas file.nc`

Create a new NetCDF file with rainfall data converted from kg m-2 s-1 to mm/day, and extract only the years of interest (you can see what units your data is stored in with "ncdump -h"):

`cdo -mulc,86400 -selyear,1979/2000 -selvar,pr file.nc pr_new.nc`

Remap a logical mask from one grid resolution to another (in this example, 80 x 60 grid cells), using nearest neighbour interpolation:

`cdo -remapnn,r80x60 mask.nc mask_new.nc`

Remap climate variable fields from one grid resolution to another, using first-order conservative remapping:

`cdo -remapcon,r80x60 -selvar,pr file.nc pr_remapped.nc`

Merge many NetCDF files that all start with the same beginning:

`cdo -cat pr* pr_concatenated.nc`

**Aliases**

I'm surprised so few people use aliases since they only take a few seconds to implement and they'll be around to use until you get a new computer or until you no longer have access to a server. A typical scenario where an alias would help would be: person X ssh's into a server everyday multiple times a day and always has to type `ssh z1234567@monsoon.ccrc.unsw.edu.au`.

Make an alias for this on your local computer buy adding the following line to your ".bash_profile" file in your home directory (if it doesn't exist then create it with "touch .bash_profile"):

`alias ssh_m = "ssh z1234567@monsoon.ccrc.unsw.edu.au"`

Open up a new terminal window. Now all you have to do to ssh is type "ssh_m". If your computer is not shared or you trust others in your work environment, you can use passwordless ssh-keys (next section), so ssh'ing is almost instant — IT people tend to use this but it may or may not not be recommended for muggles without magical powers. If you find you tend to navigate to the same directory every day then another alias is in order. Open up your .bash_profile on the relevant machine (local or server), and add something like:

`alias scripts = "cd /home/z1234567/phd/projects/project1/scripts/"`. I usually store my data in a very different location from my scripts so I also have a "data" alias.

Another little one I use is the following because I find the "ls" command alone lacks some important info. "l" lists the output, "t" sorts output chronologically, "r" reverses the order, "h" changes units to "human readable":

`alias ll = "ls -ltrh"`

**SSH-Keys**

If you'd like to speed up the ssh login process so no password is needed or so you can set up cronjobs (scripts that automatically execute at regular intervals) to, for example, automate backing-up of important work you're doing on your local machine (see next section), then follow these instructions. In the command line/terminal on your local machine enter the following:

`cd ~/.ssh; ssh-keygen -t rsa -f id_rsa -P ""`

Copy the newly created "id_rsa.pub" file to the server:
`scp id_rsa.pub username@server.net:~/`

ssh to the server as you normally would, and append contents in "id_rsa.pub" to a file named "authorized_keys" in the ".ssh" directory:

`cd; cat id_rsa.pub >> .ssh/authorized_keys; rm id_rsa.pub`

Now ssh'ing is as easy as it can get: just enter your ssh alias and you're in!

**Automated Data Backup**

Backing up your files is indisputably important. Some people like to use version control software like git or svn. However if you're like me: you don't tend to work collaboratively and the backup part of version control is what you really care about, then this is for you. If you went as far as getting ssh-keys set up, you can automate the backup (to a server) of important files on your PC by setting up a cronjob. Before you do this you should find out how much space is available for you on servers you have access to (you may only be able to backup scripts, documents, and figures — usually the most important stuff anyways!). To view currently active cronjobs on your PC type "crontab -l" in the command line. If you've never made a cronjob you probably won't see much. To add the cronjob, type "crontab -e" and add the following lines:

`MAILTO=""
30 */2 * * * date >> ~/.cron.log && rsync --stats -hai --delete /path/your/important/documents/ username@server.net:/path/to/your/backed-up/documents/ >> ~/.cron.log 2>&1 && echo ============================= >> ~/.cron.log`

The first time this runs it may take a while, afterwhich it'll only sync the files that have changed, been added, or been deleted. This job will run at "30" minutes past the hour, every two hours ("*/2"). Make sure to first create the documents directory on the server and to get the path name right (if not, the "delete" argument can be dangerous)! To view a log of all files that have been synced, do a "more" on the ".cron.log" file. Once this is setup you don't have to do anything — it just runs in the background. I have found this setup extremely helpful. Detailed info and examples pertaining to cronjobs can be found here.

Thanks for reading! If you'd like to add anything or if you have any questions, there's a comment box below!
