---
title: dbus
date: 2025-12-15 09:52:15
categories:
tags:
---


**root使用dbus**

```
#/usr/share/dbus-1/system.d/org.itchub.scan.conf

<!DOCTYPE busconfig PUBLIC
 "-//freedesktop//DTD D-BUS Bus Configuration 1.0//EN"
 "http://www.freedesktop.org/standards/dbus/1.0/busconfig.dtd">
<busconfig>

  <policy user="root">
    <allow own="org.chub.scan"/>
  </policy>

  <policy context="default">
    <allow send_interface="org.chub.ClamScanService"/>
    <allow send_destination="org.chub.scan"/>
  </policy>

</busconfig>
```

