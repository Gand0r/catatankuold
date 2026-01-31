* * *
| [Home](https://gand0r.github.io/home) | [Catatan](https://gand0r.github.io/catatanku) | [Tentang](https://gand0r.github.io/about) |
* * *

### Catatanku

* * *

<details><summary>Cara Mengganti Dns dengan menggunakan wmic via command line</summary><br>
   <li><data value="1">wmic nicconfig where (IPEnabled=TRUE) call SetDNSServerSearchOrder ()</data></li>
   <li><data value="2">wmic nicconfig where (IPEnabled=TRUE) call SetDNSServerSearchOrder ("8.8.8.8", "8.8.4.4")</data></li>
</details>

* * *

<details><summary>Install Nodejs dari Binary di Ubuntu 20.04</summary><br>
   Node 18.x
   <li>curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - </li>
   <li>sudo apt-get install -y nodejs </li>
   <br>
   Node 17.x
   <li>curl -fsSL https://deb.nodesource.com/setup_17.x | sudo -E bash - </li>
   <li>sudo apt-get install -y nodejs </li>
   <br>
   Node 16.x
   <li>curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash - </li>
   <li>sudo apt-get install -y nodejs </li>
   <br>
   versi LTS
   <li>curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash - </li>
   <li>sudo apt-get install -y nodejs </li>
   <br>
   versi Terbaru
   <li>curl -fsSL https://deb.nodesource.com/setup_current.x | sudo -E bash - </li>
   <li>sudo apt-get install -y nodejs </li>
</details>

* * *

<details><summary>Setting IP pada Ubuntu Server 22.04</summary><br>
   <li>ip link</li>
   <li>cd /etc/netplan</li>
   <li>sudo nano /etc/netplan/installer-config.yaml</li>
   <li>edit file yaml seperti contoh di bawah ini, untuk addresss dan gateway sesuaikan dengan jaringan anda</li>
   
   ```
   network:
   version: 2
   renderer: networkd
   ethernets:
    enp5s0:
       dhcp4: no
       addresses:
         - "172.11.11.15/24"
       gateway4: "192.168.75.254"
       nameservers:
           addresses: [1.1.1.1,8.8.8.8]
   ```
   
   <li>sudo netplan apply</li>
   <li>selesai, setelah itu cek ping ke gateway atau ip Lokal</li>
</details>

* * *

<details><summary>SETUP WDCP</summary><br>

```
/interface ethernet
set [ find default-name=ether1 ] name=eth1-EDC
set [ find default-name=ether2 ] arp=reply-only
/interface wireless
set [ find default-name=wlan1 ] antenna-gain=0 band=2ghz-b/g/n country=no_country_set \
    default-authentication=no disabled=no frequency=auto frequency-mode=manual-txpower mode=\
    ap-bridge name=WDCP security-profile=wdcp ssid=WDCP station-roaming=enabled
/ip firewall filter
add action=accept chain=input comment=winbox dst-port=8296 protocol=tcp
add action=accept chain=forward comment=winbox dst-port=8296 protocol=tcp
add chain=forward comment=portwdcp1 dst-port=9400 protocol=tcp
add chain=forward comment=portwdcp2 protocol=tcp src-port=9400
add action=accept chain=output out-interface=bridge1 protocol=tcp src-port=8278
add action=accept chain=output out-interface=bridge1 protocol=tcp src-port=7790
add action=accept chain=output out-interface=bridge1 protocol=tcp src-port=7789
add action=accept chain=output out-interface=bridge1 protocol=tcp src-port=3828
add action=accept chain=output out-interface=bridge1 protocol=tcp src-port=2325
add action=accept chain=output out-interface=bridge1 protocol=icmp
add action=accept chain=output dst-address=192.168.0.0/16 out-interface=bridge1
add action=accept chain=output dst-address=172.31.31.1 out-interface=bridge1
add action=accept chain=output dst-address=172.24.0.0/16 out-interface=bridge1
add action=accept chain=output dst-address=172.20.0.0/16 out-interface=bridge1
add action=accept chain=output dst-address=10.254.253.253 out-interface=bridge1
add action=accept chain=output dst-address=10.254.253.250 out-interface=bridge1
add action=accept chain=output dst-address=10.64.0.0/16 out-interface=bridge1
add action=accept chain=input dst-port=8278 in-interface=bridge1 protocol=tcp
add action=accept chain=input dst-port=7790 in-interface=bridge1 protocol=tcp
add action=accept chain=input dst-port=7789 in-interface=bridge1 protocol=tcp
add action=accept chain=input dst-port=3828 in-interface=bridge1 protocol=tcp
add action=accept chain=input dst-port=2325 in-interface=bridge1 protocol=tcp
add action=accept chain=input in-interface=bridge1 protocol=icmp
add action=accept chain=input in-interface=bridge1 src-address=192.168.0.0/16
add action=accept chain=input in-interface=bridge1 src-address=172.31.31.1
add action=accept chain=input in-interface=bridge1 src-address=172.24.0.0/16
add action=accept chain=input in-interface=bridge1 src-address=172.20.0.0/16
add action=accept chain=input in-interface=bridge1 src-address=10.254.253.253
add action=accept chain=input in-interface=bridge1 src-address=10.254.253.250
add action=accept chain=input in-interface=bridge1 src-address=10.64.0.0/16
add action=accept chain=forward dst-address=10.254.253.253 out-interface=bridge1
add action=accept chain=forward dst-address=10.254.253.250 out-interface=bridge1
add action=drop chain=forward
add action=drop chain=forward in-interface=bridge1
add action=drop chain=input in-interface=bridge1
add action=drop chain=output out-interface=bridge1
add action=drop chain=forward protocol=udp src-port=135-139,445
add action=drop chain=forward dst-port=135-139,445 protocol=udp
add action=drop chain=forward protocol=tcp src-port=135-139,445
add action=drop chain=forward dst-port=135-139,445 protocol=tcp
add action=drop chain=forward comment=vnc dst-port=5900 protocol=tcp
add action=drop chain=forward comment=vnc dst-port=5800 protocol=tcp
add action=drop chain=forward comment=RDP dst-port=3389 protocol=tcp
/ip firewall nat
add action=masquerade chain=srcnat out-interface=bridge1 src-address=10.0.0.0/24
add action=masquerade chain=srcnat src-address=172.31.41.0/24
add action=masquerade chain=srcnat

..

..

..

console clear-history
```
</details>

* * *

<a href='https://ko-fi.com/M4M3AGKQC' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://cdn.ko-fi.com/cdn/kofi1.png?v=3' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>
