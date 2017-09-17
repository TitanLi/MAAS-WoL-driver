# Ubuntu MAAS 2.2 Wake on LAN Driver Patch
The WoL driver has indeed been removed from MAAS 2.0, but WoL functionality is still needed.\
The previous fix [Using Wake on LAN with MAAS 2.x](https://stgraber.org/2017/04/02/using-wake-on-lan-with-maas-2-x/), can't solve on MASS 2.2+
https://github.com/kairen/MAAS-WoL-driver uses wakeonlan program which does not work with my current setup using intel NUCs.  so the WoL etherwake Driver is provided here.


Requirement:
* Install the etherwake  package
* Install MAAS


etherwake uses udp which requires it to run as root. The fix is to allow maas to become root.

run `sudo visudo`

add the line

`maas ALL= NOPASSWD: /usr/sbin/etherwake`

where /usr/sbin/etherwake is the result of `which etherwake`

Patch the WoL etherwake driver into MAAS as follows:

```sh
$ sudo apt-get install -y etherwake
$ PATCH_DIR="/usr/lib/python3/dist-packages/provisioningserver/"
$ sudo patch -p1 -d ${PATCH_DIR} < maas-etherwake.diff 
$ sudo systemctl restart maas-rackd.service maas-regiond.service
```

