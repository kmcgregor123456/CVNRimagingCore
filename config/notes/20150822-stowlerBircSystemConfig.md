# stowler BIRC system config: pano and rama on Saturday 20150822

_More config and testing TBD but this is what I completed today._

Contents
=================

  * [pano\.birc\.emory\.edu](#panobircemoryedu)
    * [MELODIC group ICA: inspected test results from known\-good UNCLEANED sample data](#melodic-group-ica-inspected-test-results-from-known-good-uncleaned-sample-data)
    * [unlockable console problems continue](#unlockable-console-problems-continue)
  * [rama\.birc\.emory\.edu](#ramabircemoryedu)
    * [installed nvidia support, rebooted](#installed-nvidia-support-rebooted)
    * [configured quad monitors](#configured-quad-monitors)
    * [installed matlab](#installed-matlab)
    * [unlockable console: initial troubleshooting](#unlockable-console-initial-troubleshooting)


# pano.birc.emory.edu

_Continuing config and testing on pano, which seems to be getting increasingly unstable._

## MELODIC group ICA: inspected test results from known-good UNCLEANED sample data

_TBD: double-check html reports_

Yesterday's group ICA run completed around 7:30 this morning (~13.5 hours):

```bash
Melodic Started at Fri Aug 21 17:52:38 EDT 2015 :
330M    /tmp/melFromFeeds-groupICA-inputsUncleaned-temporalCat-structBBR-standard2mmNonlinear.gica
...but melodic not yet finished as of Sat Aug 22 07:23:04 EDT 2015. Will check again in 20 seconds...

Finished at Sat Aug 22 07:23:16 EDT 2015
```

Looks valid:

```bash
[02:05:56]-[stowler-local]-at-[pano]-in-[/data/panolocal/tempStowler]
$ find . -type f -printf '%T@ %p\n' | sort -n | tail -5 | cut -f2- -d" " | xargs ls -al
-rw-rw-r-- 1 stowler-local stowler-local   3467074 Aug 22 04:35 ./fixThresh16/melFromFeeds-struct6dof-standardNone.ica/filtered_func_data_melFromFeeds-groupICA-inputsUncleaned-temporalCat-structBBR-standard2mmNonlinear.ica/reg_standard/bg_image.nii.gz
-rw-rw-r-- 1 stowler-local stowler-local 243188192 Aug 22 04:36 ./fixThresh16/melFromFeeds-struct6dof-standardNone.ica/filtered_func_data_melFromFeeds-groupICA-inputsUncleaned-temporalCat-structBBR-standard2mmNonlinear.ica/reg_standard/filtered_func_data.nii.gz
-rw-rw-r-- 1 stowler-local stowler-local     10801 Aug 22 04:36 ./fixThresh16/melFromFeeds-struct6dof-standardNone.ica/filtered_func_data_melFromFeeds-groupICA-inputsUncleaned-temporalCat-structBBR-standard2mmNonlinear.ica/reg_standard/mask.nii.gz
-rw-rw-r-- 1 stowler-local stowler-local    533672 Aug 22 04:35 ./fixThresh16/melFromFeeds-struct6dof-standardNone.ica/filtered_func_data_melFromFeeds-groupICA-inputsUncleaned-temporalCat-structBBR-standard2mmNonlinear.ica/reg_standard/standard.nii.gz
-rw-rw-r-- 1 stowler-local stowler-local     12409 Aug 22 04:36 ./fixThresh16/melFromFeeds-struct6dof-standardNone.ica/filtered_func_data_melFromFeeds-groupICA-inputsUncleaned-temporalCat-structBBR-standard2mmNonlinear.ica/report_log.html

[02:06:17]-[stowler-local]-at-[pano]-in-[/data/panolocal/tempStowler]
$ du -sh fixThresh*
4.7G    fixThresh16
4.7G    fixThresh18
4.7G    fixThresh20

[02:06:58]-[stowler-local]-at-[pano]-in-[/data/panolocal/tempStowler]
$ du -sh fixThresh*/* | head -n 5
384M    fixThresh16/melFromFeeds-struct12dof-standard2mmLinear.ica
415M    fixThresh16/melFromFeeds-struct12dof-standard2mmNonlinear.ica
381M    fixThresh16/melFromFeeds-struct12dof-standardNone.ica
384M    fixThresh16/melFromFeeds-struct6dof-standard2mmLinear.ica
414M    fixThresh16/melFromFeeds-struct6dof-standard2mmNonlinear.ica
```

Moved the output from /tmp to external drive in preparation for reboot:

```bash
[02:09:03]-[stowler-local]-at-[pano]-in-[/tmp/melFromFeeds-groupICA-inputsUncleaned-temporalCat-structBBR-standard2mmNonlinear.gica]
$ du -sh /tmp/melFromFeeds-groupICA-inputsUncleaned-temporalCat-structBBR-standard2mmNonlinear.gica
334M    /tmp/melFromFeeds-groupICA-inputsUncleaned-temporalCat-structBBR-standard2mmNonlinear.gica

[02:11:00]-[stowler-local]-at-[pano]-in-[~]
$ mv /tmp/melFromFeeds-groupICA-inputsUncleaned-temporalCat-structBBR-standard2mmNonlinear.gica /data/panolocal/tempStowler/resultsGroupICA/

[02:11:29]-[stowler-local]-at-[pano]-in-[~]
$ sync

[02:11:34]-[stowler-local]-at-[pano]-in-[~]
$
```

Archived the input data, now altered by the gica:
```bash
[02:13:11]-[stowler-local]-at-[pano]-in-[/data/panolocal/tempStowler]
$ tar -cvf melFromFeeds-afterFixAndGroupIcaOfUncleaned.tar fixThresh*

[02:18:47]-[stowler-local]-at-[pano]-in-[/data/panolocal/tempStowler]
$ rm -fr fixThresh*
```

Backed up external drive to hippoback:
```bash
stowler-local@hippoback:/data/backup/Atlanta/stowlerxfer082115$ du -sh data
23G     data
stowler-local@hippoback:/data/backup/Atlanta/stowlerxfer082115$ rsync -avR --progress stowler-local@pano.birc.emory.edu:/data/panolocal .
Ubuntu 14.04.3 LTS pano.birc.emory.edu ssh-pty

Password:
receiving incremental file list
data/panolocal/tempStowler/
data/panolocal/tempStowler/melFromFeeds-afterFixAndGroupIcaOfUncleaned.tar
   176783360   1%   33.76MB/s    0:07:07

...snip...


sent 12393 bytes  received 15298383483 bytes  32935190.26 bytes/sec
total size is 35823966971  speedup is 2.34
stowler-local@hippoback:/data/backup/Atlanta/stowlerxfer082115$ du -sh data
37G     data
```

## unlockable console problems continue

Pano's console still becomes unlockable intermittently but frequently. Opened [ticket](https://github.com/CVNRneuroimaging/infrastructure/issues/138#issuecomment-133730061), excerpted:

> Keith and Rob @kmcgregor123456 @rrmm : please check pano's logs and zabbix plots. I left WMB last night with perfectly functional lightdm showing on the console and now I'm back in WMB for GUI-intensive work, staring at a blank and unresponsive console including unresponsive capslock light.

...then:

> Keith and Rob @kmcgregor123456 @rrmm : I spoke to Keith on the phone, and he advised that I reboot pano (instead of bouncing runlevel) as the log pollution is a fair trade-off for having a functional console quickly. Rob/Keith please check pano's logs and plots between 6pm yesterday (the 21st) and this reboot:

```bash
[01:57:35]-[stowler-local]-at-[pano]-in-[~]
$ df -h
Filesystem                             Size  Used Avail Use% Mounted on
/dev/sda1                              412G   84G  307G  22% /
none                                   4.0K     0  4.0K   0% /sys/fs/cgroup
udev                                    24G  4.0K   24G   1% /dev
tmpfs                                  4.8G  1.6M  4.8G   1% /run
none                                   5.0M     0  5.0M   0% /run/lock
none                                    24G   72K   24G   1% /run/shm
none                                   100M   12K  100M   1% /run/user
/dev/sdb1                              1.8T   34G  1.7T   2% /data/panolocal
hippoback.birc.emory.edu:/data/backup   32T   15T   15T  51% /data/backup
corpus.birc.emory.edu:/export/users    1.8T  304G  1.4T  18% /net

[01:58:06]-[stowler-local]-at-[pano]-in-[~]
$ uptime
 13:59:15 up 20:25,  1 user,  load average: 0.00, 0.01, 0.05

# ...performed backups before reboot, then:

[02:34:48]-[stowler-local]-at-[pano]-in-[~]
$ sudo shutdown -r now
[sudo] password for stowler-local:
```

Tried a few single- and dual-monitor configs...didn't help.

# rama.birc.emory.edu

Giverpt's increasing instability it's now EXTRA important to get lightdm rama configured and working.

## installed nvidia support, rebooted

TBD: see full stdout in separate file...

```bash
[03:30:54]-[stowler-local]-at-[rama]-in-[~]
$ sudo apt-get update

[03:31:10]-[stowler-local]-at-[rama]-in-[~]
$ sudo apt-get dist-upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages will be upgraded:
  apport apport-gtk firefox firefox-locale-en openssh-client openssh-server
  openssh-sftp-server python-numexpr python3-apport python3-problem-report
  r-base r-base-core r-base-dev r-base-html r-cran-foreign r-cran-nlme
  r-cran-spatial r-recommended ssh
19 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Need to get 65.6 MB of archives.
After this operation, 135 kB of additional disk space will be used.
Do you want to continue? [Y/n] Y

[03:40:38]-[stowler-local]-at-[rama]-in-[~]
$ sudo apt-get install nvidia-common nvidia-settings nvidia-current-updates
Reading package lists... Done
Building dependency tree
Reading state information... Done
The following extra packages will be installed:
  dkms lib32gcc1 libc6-i386 libcuda1-304-updates libvdpau1 nvidia-304-updates
  nvidia-opencl-icd-304-updates screen-resolution-extra
Suggested packages:
  nvidia-vdpau-driver vdpau-driver
The following NEW packages will be installed:
  dkms lib32gcc1 libc6-i386 libcuda1-304-updates libvdpau1 nvidia-304-updates
  nvidia-common nvidia-current-updates nvidia-opencl-icd-304-updates
  nvidia-settings screen-resolution-extra
0 upgraded, 11 newly installed, 0 to remove and 0 not upgraded.
Need to get 50.9 MB of archives.
After this operation, 223 MB of additional disk space will be used.
Do you want to continue? [Y/n] Y
```
## configured quad monitors

Used GUI "NIVDIA X Server Settings" and saved to /etc/X11/xorg.conf (TBD: link to file)


## installed matlab

Used GUI to install to `/opt/MATLAB/R2015a`, with symlinks in `/usr/local/bin`:

```
>> ver
----------------------------------------------------------------------------------------------------
MATLAB Version: 8.5.0.197613 (R2015a)
MATLAB License Number: 300274
Operating System: Linux 3.19.0-26-generic #28~14.04.1-Ubuntu SMP Wed Aug 12 14:09:17 UTC 2015 x86_64
Java Version: Java 1.7.0_60-b19 with Oracle Corporation Java HotSpot(TM) 64-Bit Server VM mixed mode
----------------------------------------------------------------------------------------------------
MATLAB                                                Version 8.5        (R2015a)
Bioinformatics Toolbox                                Version 4.5.1      (R2015a)
Image Processing Toolbox                              Version 9.2        (R2015a)
MATLAB Coder                                          Version 2.8        (R2015a)
MATLAB Compiler                                       Version 6.0        (R2015a)
MATLAB Compiler SDK                                   Version 6.0        (R2015a)
Parallel Computing Toolbox                            Version 6.6        (R2015a)
Signal Processing Toolbox                             Version 7.0        (R2015a)
Statistics and Machine Learning Toolbox               Version 10.0       (R2015a)
>> 
```

## unlockable console: initial troubleshooting

Today is the first day I've worked with rama's console. Showing same symptoms as pano: disappearing lightdm, lightdm without a login box. Intermittent but frequent.

Looks like this is a known ubuntu 14.04 problem, with existing [bugreports](https://bugs.launchpad.net/ubuntu/+source/unity/+bug/1311316). 

Tried single-monitor and dual-monitor setups in a few configurations, but didn't help. Opening a new [ticket](https://github.com/CVNRneuroimaging/infrastructure/issues/142) for Keith and Rob.
