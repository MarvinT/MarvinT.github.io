---
title: Re-enable network discovery on Windows 10
toc: true
toc_label: "Network Windows 10"
---

So it turns out windows 10 has deprecated HomeGroups in update 1803 to to encourage paying them to put your data on their computers...

### To re-enable it:
1. Open the Windows Services Management Console (services.msc).
2. In the list of services, look for the Function Discovery Resource Publication service. It must be disabled.
3. Change the service startup type from Manual to Automatic (Delayed Start). 
4. restart or right click on the service and start it

I found the instructions from [here](http://woshub.com/network-computers-not-showing-windows-10/).