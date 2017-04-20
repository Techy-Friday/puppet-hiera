# puppet-hiera
Puppet Hiera

Voor de techy Friday van 21 april dienen de volgende voorbereidingen uitgevoerd te worden
Dit moet je echt vooraf doen zodat we vrijdag middag gelijk van start kunnen
Wacht niet te lang met de voorbereiding zodat we vooraf nog kunnen helpen als het niet lukt. Vrijdag middag hebben wij geen tijd meer om te helpen met de setup

Info omgeving:  
* Local Netwerk (no internet access)
* SSID puppet   (2.4Ghz)
*         puppet5 (5Ghz)
* Netwerk 192.168.1.0/24
* Gateway/DHCP Server 192.168.1.1
* Puppet master 1: 192.168.1.3
* (Er komt ook een switch zodat je bekabeld kan aansluiten op het LAN en je via de Wifi toch nog naar internet kan)

Voorbereiding: 
Om lokaal op je laptop een vm te starten voor de praktijk werkzaamheden maken we gebruik van virtual box (https://www.virtualbox.org/) en vagrant(https://www.vagrantup.com/).
Install oracle vm virtual box 
  http://download.virtualbox.org/virtualbox/5.1.18/VirtualBox-5.1.18-114002-Win.exe
  (installeren is voldoende vagrant regelt de configuraties)
Install vagrant
  https://www.vagrantup.com/downloads.html
Installeren is voldoende de rest komt vanuit de meegeleverde Vagrantfile

Kopieer de bijgevoegde Vagrantfile naar een lokale folder bijvoorbeeld: c:\vagrant\puppet
Let op hij moet Vagrantfile heten en beginnen met de hoofdletter V
Edit bovenin de Vagrantfile de variable NAME met jou voornaam om unieke server namen te generen
Je moet nu 3 verschillende vm’s aanmaken. Centos7, Ubunutu 16.04 en Windows 2012R2
Open een cmd box en ga in de folder staan
```
c:
cd \vagrant\puppet
```
Geeft het onderstaande commando om een vm te starten
`vagrant up centos7`
De box wordt nu eenmalig gedownload vanaf de vagrant website en de vm wordt aangemaakt in virtualbox en gestart. Er worden wat aanvullende packages geinstalleerd
Je kan nu aanloggen met
`vagrant ssh centos7`  (Er moet wel een ssh client in het path gevonden kunnen worden)
aanloggen gebeurt met de user vagrant op basis van een ssl certificaat. 

Je kan ook met putty verbinden naar localhost met port 2222 of 2232 (zie Vagrantfile voor de forwarded poort) Het private certificaat staat in
`C:\Vagrant\puppet\.vagrant\machines\<centos7|ubuntu>\virtualbox\private_key`
Voor gebruik met putty moet je deze converteren mbv puttygen naar een ppk formaat
Herhaal dit voor de ubuntu en de win vm’s
Voor de ubuntu box:
```
vagrant up ubuntu
vagrant ssh ubuntu
```
Voor de windows box:
```
vagrant up win
vagrant rdp win
```
aanmelden kan met user vagrant en wachtwoord vagrant

Voor de ubuntu box:
```
vagrant up ubuntu
```
Indien je onderstaande error melding krijgt bij de Ubuntu box moet je nog even extra de provision stap uitvoeren
`/sbin/ifdown eth1 2> /dev/null`
Stdout from the command:
Stderr from the command:
mesg: ttyname failed: Inappropriate ioctl for device
Provision starten met:
`vagrant provision ubuntu`
Hierna is de puppetagent geinstalleerd en ka je kijken of je kan aanmelden met:
`vagrant ssh ubuntu`

De puppetagent is al klaar voor gebruik. Start nog geen puppetrun anders worden er certificaten aangemaakt waarmee je dan later niet kan aanmelden bij de puppetmaster.
Controleer of je je aan kan melden op de vagrant vm’s met ssh of rdp.
Als dat lukt is de voorbereiding zover klaar.
(Je kan ook alle vm’s in 1 keer aanmaken met `vagrant up` zonder vm naam in dat geval zal vagrant alle vm’s in de Vagrantfile aanmaken)

