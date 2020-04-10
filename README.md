
# Overview
**OneShot** performs [Pixie Dust attack](https://forums.kali.org/showthread.php?24286-WPS-Pixie-Dust-Attack-Offline-WPS-Attack) without having to switch to monitor mode.
# Features
 - [Pixie Dust attack](https://forums.kali.org/showthread.php?24286-WPS-Pixie-Dust-Attack-Offline-WPS-Attack);
 - integrated [3WiFi offline WPS PIN generator](https://3wifi.stascorp.com/wpspin);
 - [online WPS bruteforce](https://sviehb.files.wordpress.com/2011/12/viehboeck_wps.pdf);
 - Wi-Fi scanner with highlighting based on iw;
# Requirements
 - Python 3.6 and above;
 - [Wpa supplicant](https://www.w1.fi/wpa_supplicant/);
 - [Pixiewps](https://github.com/wiire-a/pixiewps);
 - [iw](https://wireless.wiki.kernel.org/en/users/documentation/iw).
# Setup
## Debian/Ubuntu
**Installing requirements**
 ```
 sudo apt install -y python3 wpasupplicant iw wget
 ```
**Installing Pixiewps**

***Ubuntu 18.04 and above or Debian 10 and above***
 ```
 sudo apt install -y pixiewps
 ```
 
***Other versions***
 ```
 sudo apt install -y build-essential unzip
 wget https://github.com/wiire-a/pixiewps/archive/master.zip && unzip master.zip
 cd pixiewps*/
 make
 sudo make install
 ```
**Getting OneShot**
 ```
 cd ~
 wget https://raw.githubusercontent.com/drygdryg/OneShot/master/oneshot.py
 ```
Optional: getting a list of vulnerable to pixie dust devices for highlighting in scan results:
 ```
 wget https://raw.githubusercontent.com/drygdryg/OneShot/master/vulnwsc.txt
 ```
## Arch Linux
**Installing requirements**
 ```
 sudo pacman -S wpa_supplicant pixiewps wget python
 ```
**Getting OneShot**
 ```
 wget https://raw.githubusercontent.com/drygdryg/OneShot/master/oneshot.py
 ```
Optional: getting a list of vulnerable to pixie dust devices for highlighting in scan results:
 ```
 wget https://raw.githubusercontent.com/drygdryg/OneShot/master/vulnwsc.txt
 ```
## Alpine Linux
It can also be used to run on Android devices using [Linux Deploy](https://play.google.com/store/apps/details?id=ru.meefik.linuxdeploy)

**Installing requirements**  
Adding the testing repository:
 ```
 sudo sh -c 'echo "http://dl-cdn.alpinelinux.org/alpine/edge/testing/" >> /etc/apk/repositories'
 ```
 ```
 sudo apk add python3 wpa_supplicant pixiewps iw
 ```
 **Getting OneShot**
 ```
 sudo wget https://raw.githubusercontent.com/drygdryg/OneShot/master/oneshot.py
 ```
Optional: getting a list of vulnerable to pixie dust devices for highlighting in scan results:
 ```
 sudo wget https://raw.githubusercontent.com/drygdryg/OneShot/master/vulnwsc.txt
 ```
## [Termux](https://play.google.com/store/apps/details?id=com.termux)
Please note that root access is required.  

**Building wpa_supplicant**
 ```
 pkg install wget build-essential pkg-config libnl openssl
 wget https://www.w1.fi/releases/wpa_supplicant-2.9.tar.gz && tar xvf wpa_supplicant-2.9.tar.gz
 cd wpa_supplicant-2.9/wpa_supplicant/
 cp defconfig .config
 ```
Open `.config` and comment following lines:
 ```
 CONFIG_DRIVER_WEXT=y
 CONFIG_SAE=y
 CONFIG_CTRL_IFACE_DBUS_NEW=y
 CONFIG_DEBUG_FILE=y
 CONFIG_DEBUG_SYSLOG=y
 ```
 ```
 make
 cp wpa_supplicant $PREFIX/bin/
 chmod +x $PREFIX/bin/wpa_supplicant
 ```
**Building Pixiewps**
 ```
 wget https://github.com/wiire-a/pixiewps/archive/master.zip && unzip master.zip
 cd pixiewps*/
 make && make install
 ```
**Installing OneShot**
 ```
 pkg install root-repo
 pkg install tsu python iw
 ```
 ```
 wget https://raw.githubusercontent.com/drygdryg/OneShot/master/oneshot.py
 ```
Optional: getting a list of vulnerable to pixie dust devices for highlighting in scan results:
 ```
 wget https://raw.githubusercontent.com/drygdryg/OneShot/master/vulnwsc.txt
 ```
Running:
 ```
 tsudo python oneshot.py -i wlan0 -K
 ```

# Usage
```
 oneshot.py <arguments>
 Required arguments:
     -i, --interface=<wlan0>  : Name of the interface to use

 Optional arguments:
     -b, --bssid=<mac>        : BSSID of the target AP
     -p, --pin=<wps pin>      : Use the specified pin (arbitrary string or 4/8 digit pin)
     -K, --pixie-dust         : Run Pixie Dust attack
     -B, --bruteforce         : Run online bruteforce attack

 Advanced arguments:
     -d, --delay=<n>          : Set the delay between pin attempts [0]
     -w, --write              : Write AP credentials to the file on success
     -F, --pixie-force        : Run Pixiewps with --force option (bruteforce full range)
     -X, --show-pixie-cmd     : Alway print Pixiewps command
     --vuln-list=<filename>   : Use custom file with vulnerable devices list ['vulnwsc.txt']
     --iface-down             : Down network interface when the work is finished
     -v, --verbose            : Verbose output
 ```

## Usage examples
Start Pixie Dust attack on a specified BSSID:
 ```
 sudo python3 oneshot.py -i wlan0 -b 00:90:4C:C1:AC:21 -K
 ```
Show avaliable networks and start Pixie Dust attack on a specified network:
 ```
 sudo python3 oneshot.py -i wlan0 -K
 ```
Launch online WPS bruteforce with the specified first half of the PIN:
 ```
 sudo python3 oneshot.py -i wlan0 -b 00:90:4C:C1:AC:21 -B -p 1234
 ```
## Troubleshooting
#### "RTNETLINK answers: Operation not possible due to RF-kill"
 Just run:
```sudo rfkill unblock wifi```
#### "Device or resource busy (-16)"
 Try disabling Wi-Fi in the system settings and kill the Network manager. Alternatively, you can try running OneShot with ```--iface-down``` argument.
# Acknowledgements
## Special Thanks
* `rofl0r` for initial implementation;
* `Monohrom` for testing, help in catching bugs, some ideas;
* `Wiire` for developing Pixiewps.
