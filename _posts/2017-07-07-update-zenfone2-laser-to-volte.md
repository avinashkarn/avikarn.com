---
layout: post
title: Update your ZenFone 2 Laser (ZE550KL) to VoLTE (Version WW-21.40.1220.2153) & Fix /asdf /recovery Errors without ROOT
tags: [zenfone, laser, volte, 4G, andriod update, asdf recovery, ZE550KL, ADB, recovery, TWRP. without root]
image: http://www.anudit.in/img/zenfone_volte/zenfone_preview.png
share-img: http://www.anudit.in/img/zenfone_volte/zenfone_preview.png
---

A concise guide to get VoLTE update on your Zenfone 2 Laser (ZE550KL) to unlock "True 4G" or "Rich Calling" (for Jio) or HD calling capabilities. The hardware of your Zenfone 2 phone is capable of hosting a LTE Cat4 network with maximum download and upload speed of 100-150 Mbps, so why not use your device well and get the most out of it with a simple update?

<div class="ads">
<script type="text/javascript">
  ( function() {
    if (window.CHITIKA === undefined) { window.CHITIKA = { 'units' : [] }; };
    var unit = {"calltype":"async[2]","publisher":"anuditverma","width":320,"height":50,"sid":"Chitika Default"};
    var placement_id = window.CHITIKA.units.length;
    window.CHITIKA.units.push(unit);
    document.write('<div id="chitikaAdBlock-' + placement_id + '"></div>');
}());
</script>
<script type="text/javascript" src="//cdn.chitika.net/getads.js" async></script>
</div>

<h3>The Current Scenario:</h3>
There is a notification on your Zenfone, "An update is available" or you have checked for latest OTA (Over-The-Air) update manually. Then you downloaded the update file, phone asks you to restart in order to further continue with the installation of the update. The phone restarts itself and boots into stock recovery mode. BUT! you might end with:

<div class="ads">
<div class="typed-js-hide">
<script type="text/javascript">
  ( function() {
    if (window.CHITIKA === undefined) { window.CHITIKA = { 'units' : [] }; };
    var unit = {"calltype":"async[2]","publisher":"anuditverma","width":550,"height":250,"sid":"Chitika Default"};
    var placement_id = window.CHITIKA.units.length;
    window.CHITIKA.units.push(unit);
    document.write('<div id="chitikaAdBlock-' + placement_id + '"></div>');
}());
</script>
<script type="text/javascript" src="//cdn.chitika.net/getads.js" async></script>
</div>
</div>


```
Supported API:3
E:failed to mount /asdf (Invalid argument)
E:failed to mount /asdf
E:failed to set up expected mounts for install; aborting
E:failed to mount /asdf (Invalid argument)
E:failed to mount /asdf (Invalid argument)
E:failed to mount /asdf (Invalid argument)
E:Can't mount /asdf/recovery/last_log
E:Can't open /asdf/recovery/last_log
E:Can't mount /asdf/recovery/last_log(Invalid argument)
E:Can't mount /asdf/recovery/last_log
E:Can't open /asdf/recovery/last_log
```

<center><img src="/img/zenfone_volte/errors.jpg" height="500" width="278"></center>

Now you can see system is unable to mount or open /asdf partition for formatting. This is the main reason which is causing those errors when you try to perform factory resets or flashing (writing updates to memory) OTA updates. This may have really pissed you off, so how to solve this? This may be the one of the reason you ended up on this page, so let's dive in and start with the process, shall we?

I was on the same scenario few months back when I published this post, I decided to wait for the OEM (Original Equipment Manufacturer) ASUS in this case, to provide an update with a patch for this, but nothing seemed to work out. Then after searching and experimenting with various methods/steps/guides/tutorials on the Internet, I devised my own simple steps which have worked really well for me.

I am going to mention two methods, go with __method one__ first, if it fails proceed with __method two__. So without taking much time, let start:

NOTE: __Before proceeding please make a backup of your personal data, you may lost your data while performing the following steps. Also ensure the battery is above 50% (just to be safe.)__

Disclaimer: __You might brick your phone or it might be completely unusable, FOLLOW THESE AT YOUR OWN RISK! Any problems you encounter or global nuclear wars you cause while following these steps are your own responsibility. If something goes wrong (such as a power outage or loose cable) in the middle of the flashing, it will soft-brick your phone. If you don't know how to deal with such things, take your phone to a proper service centre instead of following this post.__


<h3>1st Method (Using Stock Recovery)</h3>

<h4>Download the Firmware Update (Stock ROM):</h4>

* You can download the update file directly on your __sd-card__ (observe __NOT internal storage__) or you can transfer the downloaded file from your PC as per convenience.
* Make sure to place the downloaded file i.e. __UL-Z00L-WW-21.40.1220.2153-user.zip__ on the upper most directory of the sd-card (__NOT in any folder__).
* Go to [ASUS Zenfone 2 Laser Support site](https://www.asus.com/in/Phone/ZenFone-2-Laser-ZE550KL/HelpDesk_Download/) for downloading the Firmware update.
* On __Driver & Tools__ tab, select OS: __Android__ and then under __Firmware__ download from __Global__ link.


<h4>Prepare Your Device:</h4> 
* Power off your Device.
* Now hold down __Volume Down__ Key and __Power Key__ simultaneously, wait for 3 seconds.
* Device would boot into stock __Android System Recovery Mode,__ now go to (Use Volume buttons to navigate Up/Down and Power Button to select) __wipe data/factory reset__ and select yes, then back on recovery menu go to __wipe cache partition__

<h4>Installing The Update:</h4> 

* Go to __apply update from sdcard.__
* Navigate to __UL-Z00L-WW-21.40.1220.2153-user.zip__ file, and press __Power Button__ to select the file.

NOTE: __If you are still getting the /asdf /recovery errors, then follow Method 2, if not then continue.__

* Wait for the process to complete, it may take around 15-20 minutes to complete, have patience.
* If you get the success message after the system has been patched unconditionally process, then __wipe the data/factory__ and __cache partition__ again to ensure complete clean up.

Then select __reboot system now,__ wait for the boot process to complete, it may again take 15-20 minutes because for the first boot system optimises the apps and builds the cache. If you don't have patience, then of course you can hard reboot, by long pressing the Power button and restarting the device. (NOT Recommended)

<h3>2nd Method (Clearing ASDF partition)</h3>

This method intends to __solve /asdf related errors,__ we will use a custom recovery called __TWRP__ other than stock recovery to __wipe asdf and system__ partitions which was not possible with stock recovery.

<h4>Requirements</h4>
* Install [ADB Drivers](http://adbdriver.com/downloads/) on your PC.
* Install [Minimal ADB & Fastboot 1.4.3](https://drive.google.com/uc?export=download&id=0B0MKgCbUM0itNVB1elljU2NPR0k) again on your PC.
* Go to this [Cloud Drive](https://mega.nz/#F!lodnzDhK!H5ChxausDTyko1qGlnF7dw!Fg9FwLoa) and select the directory with your existing build version, download only __boot.img__ and __recovery.img__ from it and place these two files in __adb folder__ (This folder will be generated after adb & fastboot installation.)
* Download [TWRP Recovery](https://dl.twrp.me/Z00L/) for ASUS ZenFone 2 Laser (Code Name: Z00L) and copy this too in __adb folder.__

<h4>Have The Firmware Update (Stock ROM) Downloaded, if not follow these steps:</h4>

* You can download the update file directly on your __sd-card__ (observe __NOT internal storage__) or you can transfer the downloaded file from your PC as per convenience.
* Make sure to place the downloaded file i.e. __UL-Z00L-WW-21.40.1220.2153-user.zip__ on the upper most directory of the sd-card (__NOT in any folder__).
* Go to [ASUS Zenfone 2 Laser Support site](https://www.asus.com/in/Phone/ZenFone-2-Laser-ZE550KL/HelpDesk_Download/) for downloading the update.
* On __Driver & Tools__ tab, select OS: __Android__ and then under __Firmware__ download from __Global__ link.

<h4>Installing TWRP with ADB and Fastboot</h4>
* First enable Developer Mode, Go To __Setting>About>Software Information>Build Number__ (Click 7 times) then “Your Are Developer Now” message will appear.
* Now enable __USB Debugging__ from __Developer Mode__ from Setting and  connect your phone to PC via USB cable.
* Now open __Command Prompt (cmd)__ in the adb folder by holding __Shift__ and __Right clicking__ simultaneously, you will see option __Open Command Prompt Here__.
* On on cmd, type __adb devices__ and press enter, look for a pop on your device for allowing debugging authorisation for your PC, select __always__ and click __Yes.__
* Type __adb reboot bootloader__ on the cmd, you will notice that your phone will get into __Fastboot Mode!!__
* Now we will flash the TWRP recovery that will help us wipe asdf partition, type __fastboot devices__ to show the list of connected devices, currently your phone should be visible with a serial number.
* Type command __fastboot flash recovery twrp-3.1.1-0-Z00L.img__, allow it to FINISH, then disconnect your phone and remove the battery.
* Now with the key combination, press __Volume Down__ and __Power Button__ together, you will be in TWRP recovery mode.

<h4>Wiping ASDF Cache and Flashing Stock Recovery Over TWRP</h4>

<center><img src="/img/zenfone_volte/twrp_menu.jpg" height="500" width="278"></center>

* On TWRP recovery, navigate to __Wipe__, now select __Dalvik/ART cache, System, Data, Internal storage, Cache, ASDF.__
* Swipe right to Wipe.
* Since we __cannot use TWRP for flashing the desired stock firmware,__ we will flash Stock Recovery in the phone.
* Disconnect your phone from the PC, unplug the USB cable and remove the battery to turn the phone off, then install the battery again, now boot into the __Fastboot Mode!!__ , this time use __Volume Up__ and __Power Button__ together.
* Connect your phone to PC, again open cmd in the adb folder, now type __fastboot flash recovery recovery.img__ let it FINNISH, similary type __fastboot flash boot boot.img__ again let this FINNISH too.

<div class="ads">
<div class="typed-js-hide">
<script type="text/javascript">
  ( function() {
    if (window.CHITIKA === undefined) { window.CHITIKA = { 'units' : [] }; };
    var unit = {"calltype":"async[2]","publisher":"anuditverma","width":728,"height":90,"sid":"Chitika Default"};
    var placement_id = window.CHITIKA.units.length;
    window.CHITIKA.units.push(unit);
    document.write('<div id="chitikaAdBlock-' + placement_id + '"></div>');
}());
</script>
<script type="text/javascript" src="//cdn.chitika.net/getads.js" async></script>
</div>
</div>

<h4>Apply Update From sdcard</h4>

* Now again disconnect your phone, remove battery and now boot into stock recovery that we have just installed, (__Volume Down + Power Button__).
* Go to __apply update from sdcard.__
* Navigate to __UL-Z00L-WW-21.40.1220.2153-user.zip__ file, and press __Power Button__ to select the file.
* Wait for the process to complete, it may take around 15-20 minutes to complete, have patience.
* You should be able to get the success message after the system has been patched unconditionally process, then __wipe the data/factory__ and __cache partition__ again to ensure complete clean up.

Then select __reboot system now,__ wait for the boot process to complete, it may again take 15-20 minutes because for the first boot system optimises the apps and builds the cache. If you don't have patience, then of course you can hard reboot, by long pressing the Power button and restarting the device. (NOT Recommended)

<center><h3>ASUS Zenfone 2 Laser (ZE550KL) after updating:</h3></center>

<center><img src="/img/zenfone_volte/zenfone_collage.jpg"></center>

NOTE: __Test your phone with a 4G enabled network (SIM) to test VoLTE capabilities (for example I used Jio SIM to test VoLTE) and make sure VoLTE option is enabled in Network Settings. This will also help you see a small VoLTE icon in the status area.__

I hope you have successfully installed the new update __UL-Z00L-WW-21.40.1220.2153-user.zip__ on your __ASUS Zenfone 2 Laser (ZE550KL)__ device, if not comment below your queries or questions, I will be glad to help you.

<h3>Useful Links:</h3>

* Also Stock ROMs available for Zenfone Selfie [here.](https://mega.nz/#F!Ao1RjT7D!hQ5x60r02WveB9-Hl32B4g!V0tUXT6a)
* Read more about TRWP, an awesome open source community project [here.](https://twrp.me/about/)
* Your ASUS Zenfone 2 Laser (ZE550KL) [device specification.](https://www.asus.com/in/Phone/ZenFone-2-Laser-ZE550KL/specifications/)

Thank you for reading.