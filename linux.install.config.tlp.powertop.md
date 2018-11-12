
# How to set Linux Power Managment Settings 

Author: Carlos Felipe C. Alves
email: carlos.fas@gmail.com

## GENERAL COMMENTS

A guide that uses TLP to act as a power saving service that also covers frequency scaling settings and uses PowerTOP to monitor and set just specific settings WHEN the context or day requires. TLP has a lot of specific settings and its default option is best fitted for almost all users.

Most of the configuration here is based in my personal choice based on internet references, so feel free to try another one. Furthermore, I used here specific settings to avoid data corruption on SSD drive.

Linux Info

>Linux Mint 19 Cinnamon
>Kernel 4.15.0-38-generic
> Processor Intel© Core™ i7-7500U CPU @ 2.70GHz × 2

## Brief Description

1. Install TLP
   1.2. Remove Scaling Governor Of Linux Distribution
   1.3. Check Status And Start If Not Running
2. Install PowerTOP
3. Additional TLP Configuration
   3.1. List `scaling_available_governors` availables 
   3.2. Backup Standard TLP config and edit `tlp` file
   3.3. Personal Config

## 1. Install TLP

~~~
sudo apt install tlp
~~~

### 1.2. Remove Scaling Governor Of Linux Distribution 
```
sudo update-rc.d -f ondemand remove
```

### 1.3 Check Status And Start If Not Running
~~~
sudo systemctl status tlp
sudo systemctl start tlp
~~~

### 2. Install PowerTOP

~~~
sudo apt install powertop
~~~

## 3. Additional TLP Configuration

To most of the users, the default configuration is good enough to have a optimal performance with a good power saving, but if you want to chance there are some suggestions below.

### 3.1.  List `scaling_available_governors` availables 

~~~
sudo tlp-stat -p
 --- TLP 1.1 --------------------------------------------

+++ Processor
CPU model      = Intel(R) Core(TM) i7-7500U CPU @ 2.70GHz

/sys/devices/system/cpu/cpu0/cpufreq/scaling_driver    = intel_pstate
/sys/devices/system/cpu/cpu0/cpufreq/scaling_governor  = powersave
/sys/devices/system/cpu/cpu0/cpufreq/scaling_available_governors = performance powersave
/sys/devices/system/cpu/cpu0/cpufreq/scaling_min_freq  =   400000 [kHz]
/sys/devices/system/cpu/cpu0/cpufreq/scaling_max_freq  =  2700000 [kHz]
/sys/devices/system/cpu/cpu0/cpufreq/energy_performance_preference = performance
/sys/devices/system/cpu/cpu0/cpufreq/energy_performance_available_preferences = default performance balance_performance balance_power power 

~~~

Take note!
>scaling_available_governors = 
    performance 
    powersave
>energy_performance_available_preferences = 
	default 
	performance 
	balance_performance 
    balance_power 
    power 

### 3.2.  Backup Standard TLP config and edit `tlp` file
~~~
sudo cp /etc/default/tlp  /etc/default/tlp_old
sudo nano /etc/default/tlp
~~~

### 3.3. Personal Config

Chnages made on `/etc/default/tlp` file

~~~
 CPU_SCALING_GOVERNOR_ON_AC=performance
 CPU_SCALING_GOVERNOR_ON_BAT=powersave
 CPU_MIN_PERF_ON_AC=25
 CPU_MAX_PERF_ON_AC=100
 CPU_MIN_PERF_ON_BAT=5
 CPU_MAX_PERF_ON_BAT=80
 DISK_APM_LEVEL_ON_AC="255 255"
 DISK_APM_LEVEL_ON_BAT="255 255"
 DEVICES_TO_DISABLE_ON_STARTUP="bluetooth wwan"
 STOP_CHARGE_THRESH_BAT0=80
~~~

Restart TLP

~~~
sudo systemctl restart tlp
~~~

### General Configuration 

Check for running tlp : `sudo systemctl status tlp `
System info : `sudo tlp-stat -s `
PCI info : `sudo tlp-pcilist `
USB info : `sudo tlp-usblist `
Show config : : `sudo tlp-stat -c` 
Show everything : `sudo tlp-stat `
Show thermal info : `sudo tlp-stat -t `
Show processor info : `sudo tlp-stat -p `
Show battery info : `sudo tlp-stat -b `
Show refreshing battery info : `watch sudo tlp-stat -b`


## Aditional Notes

### TLP - Conflict with your distribution's settings

Some distributions use dedicated scripts to select a scaling governor during system start.

Solution: disable the scripts as follows:
Ubuntu
`sudo update-rc.d -f ondemand remove`

Revert change with:
`sudo update-rc.d ondemand defaults `



## References

- https://www.youtube.com/watch?v=Ku0491LfhR4
- https://www.youtube.com/watch?v=RWyOn1ThEnc
- https://petermolnar.net/hard-drive-spindown-clicking-noise/
- https://www.reddit.com/r/linuxquestions/comments/71ozjm/is_it_safe_to_increase_ssd_apm_level_to_254/
- https://askubuntu.com/questions/805627/which-is-more-effective-tlp-or-cpufreq
- https://mintguide.org/tools/484-install-tlp-linux-advanced-power-management-for-laptops.html


