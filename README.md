# NZBGet Build
This recipe is for setting up NzbGet.

Network Prerequisites are:
- [x] Layer 2 Network Switches
- [x] Network Gateway is `192.168.1.5`
- [x] Network DNS server is `192.168.1.5` (Note: your Gateway hardware should enable you to a configure DNS server(s), like a UniFi USG Gateway, so set the following: primary DNS `192.168.1.254` which will be your PiHole server IP address; and, secondary DNS `1.1.1.1` which is a backup Cloudfare DNS server in the event your PiHole server 192.168.1.254 fails or os down)
- [x] Network DHCP server is `192.168.1.5`

Other Prerequisites are:
- [x] Synology NAS, or linux variant of a NAS, is fully configured as per [SYNOBUILD](https://github.com/ahuacate/synobuild#synobuild).
- [x] Proxmox node fully configured as per [PROXMOX-NODE BUILDING](https://github.com/ahuacate/proxmox-node/blob/master/README.md#proxmox-node-building).
- [x] pfSense is fully configured as per [HAProxy in pfSense](https://github.com/ahuacate/proxmox-reverseproxy/blob/master/README.md#haproxy-in-pfsense)
- [x] NZBGet LXC with NZBGet SW installed as per [NZBget LXC - Ubuntu 18.04](https://github.com/ahuacate/proxmox-lxc-media/blob/master/README.md#300-nzbget-lxc---ubuntu-1804).

Tasks to be performed are:
- [ ] 1.00 Basic NZBGet Preferences
- [ ] 2.00 NZBGet Security Preferences
- [ ] 3.00 Add your News-Servers
- [ ] 4.00 Create & Restore NzbGet Backups
- [ ] 00.00 Patches & Fixes

## 1.00 Basic NZBGet Preferences
All your preferences are stored in the `/opt/nzbget/nzbget.conf` file. 

Your basic NZBGet settings should've been configured when you installed NZBGet as shown [HERE](https://github.com/ahuacate/proxmox-lxc-media/blob/master/README.md#308-edit-nzbget-configuration-file---ubuntu-1804).

The basic settings are:
*  download location changed to your ZFS typhoon-share downloads folder /mnt/downloads/nzbget;
*  NZBGet daemon username changed to run under media not root;
*  create and add labels sonarr-series, radarr-movies, lidarr-music and lazylibrarian;
*  create a RPC username and password.

## 2.00 NZBGet Security Preferences
Browse to http://192.168.30.112:6789 to edit the NZBGet security preferences. Your NZBget default login details are (login:nzbget, password:tegbzn6789).

In your web browser click on the `Settings Tab` > `Security Tab` and set the following:

| Security | Value | Notes
| :---  | :---: | :---
| ControlIP | 0.0.0.0
| ControlPort | 6789
| ControlUsername | `storm` | *Add the username `storm`*
| ControlPassword | ********* | *Add a complex password and record it i.e oTL&9qe/9Y&RV*
| RestrictedUsername | `client` | *Add the username `client`*
| RestrictedPassword | ********* | *Add a complex password and record it i.e oTL&9qe/9Y&RV*
| AddUsername | rpcaccess | *The username `rpcaccess` should be preset. If not, add it*
| AddPassword | Ut#)>3'o&RVmRj>] | *The password `Ut#)>3'o&RVmRj>]` should be preset. If not, add it*
| FormAuth | No
| SecureControl | No
| SecurePort | 6791
| SecureCert | blank
| SecureKey | blank
| AuthorizedIP | 127.0.0.1
| CertCheck | Yes
| UpdateCheck | Stable
| DaemonUsername | `media` | *The daemon name `media` should be preset. If not, add it*
| UMask | 1000

And click `Save all changes`.

## 3.00 Add your News-Servers
Browse to http://192.168.30.112:6789 to edit the NZBGet News-Servers preferences. Your NZBget default login details are username `storm` and the password you set in the above step 2.00.

In your web browser click on the `Settings Tab` > `News-Servers Tab` > `Add another Server` and add in your Usenet Server accounts. Remember to click `Save all changes` at the bottom of the page.

## 4.00 Create & Restore NZBGet Backups

NZBGet has a built in backup service.

After installing NZBGet on your machine you may want to reuse the old config file in order to skip the full reconfiguration. For that purpose NZBGet web-interface provides two functions:

*  Backup Settings;
*  Restore Settings.

### 4.01 Backup settings
Browse to http://192.168.30.112:6789 to edit the NZBGet preferences.
In your web browser click on `Settings Tab` > `SYSTEM Tab` > `Backup Settings` to create a backup of your current configuration. We recommend you store this file on your NAS share at `\\192.168.1.10\proxmox\backup\nzbget`. This file can be used later to restore settings on this or another machine. For restore purposes you can also use the configuration file (nzbget.conf) instead of backup file. This is helpful if the old setup doesn’t work anymore and you can’t access web-interface to create a backup of settings.

### 4.02 Restore settings
Browse to http://192.168.30.112:6789 to edit the NZBGet preferences.
In your web browser click on `Settings Tab` > `SYSTEM Tab` > `Restore Settings` and browse to your NZBGet settings backup file (default naming like `nzbget-20191008-162540.conf`). You can restore either all settings or the settings from selected configuration sections. For example during restore process you can choose to restore only settings in the section “CATEGORIES” but not touch other settings.

## 00.00 Patches & Fixes
Nothing yet.
