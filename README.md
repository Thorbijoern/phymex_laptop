# phymex_laptop
## Aufgabe / Zielsetzung
In diesem Dokument beschreibe ich die Schritte die ich zum Aufsetzen eines Laptops vorgenommen habe.  
Das Laptop soll dem Physik-Unterricht dienen und soll ein anderes Laptop mit Windows XP ersetzen.
Dieses Laptop wird hauptsächlich in Verbindung mit PhyMex genutzt, eine Software die es ermöglicht 

Als Basis dient die GNU/Linux Distribution Debian 9 alias "stretch".
Die Hardware ist ein Fujitsu Esprimo Mobile D9510.

## Ergebnis
### Known Limitations/Problems
 - Der integrierte WLAN (und Bluetooth) Adapter benötigt nicht-freie Treiber, die ich nicht finden konnte.
 - Ich konnte leider das Problem in Phymex nicht lösen, bei dem im Vollbild-Modus eine graue Leiste am rechten Rand Steuerelemente überdeckt.


### Erfolge
 - Office läuft
 - Phymex läuft


## Vorbereitung
Es war noch ein unaktiviertes (keine Lizens) Windows 7 auf dem Laptop zu Testzwecken oder so installiert.
Ich habe geschaut ob noch Dateien oder andere wichtige Sachen darauf sind, aber habe nicht gefunden und fuhr mit der Installation fort.


## Linux
### Installation
Graphical Install  
Sprache: German - Deutsch  
Standort: Deutschland  
Tastatur: Deutsch  
Rechnername (Hostname): phybox-laptop  
Domainname:

root Nutzer
 1. Passwort wählen und wiederholen

neuer Nutzer
 1. vollständiger Name: phybox
 2. Benutzername: phybox
 3. Passwort wählen und wiederholen

Festplatte partitionieren
 1. "Geführt - vollständige Festplatte verwenden"
 2. Festplatte auswählen
 3. alle Dateien auf eine Partition
 4. Partitionierung beenden und Änderungen übernehmen
 5. ja, Festplatte Partitionieren

Grundsystem wird installiert...  
andere CD/DVD einlesen: Nein

APT konfigurieren
 1. Land des Spiegelservers: Deutschland
 2. Spiegelserver: debian.inf.tu-dresden.de
 3. kein Proxyserver (im Schulnetzwerk benötigt  man einen)

weitere Software wird installiert...  
nicht an Paketverwendungserfassung teilnehmen

Softwareauswahl (tasksel)
 - Debian desktop environment
 - Druckserver
 - Standard-Systemwerkzeuge

ausgewählte Software wird installiert...
GRUB
 - GRUB-Bootloader in den MBR installieren: Ja
 - Festplatte auswählen


### Nach der Linux Installation
Ich habe mit Gnome Software sämtliche vorinstallierten Spiele entfernt und habe in Firefox ein paar vorgeschlagene Webseiten ("wichtige Seiten" im neuen Tab) entfernt, bei denen es sich u.a. um Online-Shops handelte.
Sie erschienen mir nicht besonders didaktisch wertvoll und können zur Not wieder manuell hinzugefügt werden, außerdem sparen die deinstallierten Spiele etwas Speicherplatz, wenn auch nicht viel.

Terminal öffnen
Zuerst muss man mit `su -` zu root wechseln.
Danach führt man am besten erst mal `apt update && apt upgrade -y` aus, was die Paket Datenbank aktualisiert und Paket-Updates installiert.
Mit `apt install sudo` installier man nun sudo um später nicht immer root nehmen zu müssen.
Mit`usermod -aG sudo,dialout phybox` (oder anderen Befehlen) fügt man den User phybox zu den Gruppen sudo und dialout hinzu. 
Der User phybox benötigt die Gruppe sudo um den Befehl sudo nutzen zu dürfen und dialout ermöglicht es dem User vollen und direkten Zugriff auf die Seriellen Ports zu haben, siehe https://wiki.debian.org/SystemGroups.
 > wine benötigt später die Gruppe dialout um Zugriff auf die Seriellen Ports zu bekommen, sodass PhyMex funktioniert.  
 > Mehr zu Seriellen Ports mit wine: https://wiki.winehq.org/Wine_User%27s_Guide#Serial_and_Parallel_Ports

Überprüfen kann man das nun mit `groups phybox` (sudo und dialout müssen mit aufgelistet sein).
Mit `exit` kann man root wieder verlassen.
Man muss sich nun mit phybox aus- und wieder einloggen bzw. den Computer neu starten damit die Gruppen-Änderungen übernommen werden.
Nachdem die Spiele entfernt wurden kann man auch noch `sudo apt autoremove` ausführen um nicht mehr benötigte Pakete zu entfernen.

Für das Schulnetzwerk wird ein Proxy benötigt. Falls man sich im Schulnetzwerk befindet oder man die Installation abgeschlossen hat wird im folgenden beschrieben, was man setzen muss:  
In der "Einstellungen"-GUI, unter Netzwerk, unter Netzwerk-Proxy wählt man "Automatisch" als Methode aus und gibt dann die Konfigurationsadresse (welche man von einem anderen Schulrechner bekommen kann) ein.


## Erster Test von Phymex
In diesem Kapitel beschreibe ich den ersten Test, den ich mit dem Laptop, wine und PhyMex unternommen hatte.
Dieses Kapitel ist ziemlich unabhängig von den anderen, da ich nach diesen Tests vieles verändert hatte und sogar den Laptop komplett neu aufgesetzt hatte.
Wenn man das Laptop neu aufsetzen möchte kann man dieses Kapitel getrost ignorieren.

Wine hatte ich vorerst nur über die Debian Paketquellen installiert was sich dann nach einigen Tests als Problem rausstellte, da es in den Debian Paketquellen sehr veraltet war. Die letzte Version verfügbar in den Debian Paketquellen war 1.8.7, aber die aktuelle stabile Version (beim Zeitpunkt des Schreibens/Testens) war wine 3.0.3.


## Wine
### Installation
Nach der Installation von dem Linux habe ich wine (übersetzt Windows API calls für POSIX-Systeme) installiert:

Wine ist in den Debian Paketquellen irre veraltet, wie ich in meinem ersten Test herausgefunden habe.
Auf https://wiki.winehq.org/Debian ist eine vollständige Anleitung (Englisch) zum installieren von wine auf Debian.
Hier nur eine kurze Anleitung mit den Schritten, die ich unternahm:

`sudo dpkg --add-architecture i386` um 32 bit Pakete zu erlauben  
den Key, der benutzt wird um die Pakete zu signieren runterladen `wget -nc https://dl.winehq.org/wine-builds/Release.key` und hinzufügen `sudo apt-key add Release.key`  
eine wine.list erstellen `sudo nano /etc/apt/sources.list.d/wine.list` und in diese `deb https://dl.winehq.org/wine-builds/debian/ stretch main` schreiben  
 > `stretch` gilt nur für Debian 9.x, da diese Verion den Alias "stretch" hat, für andere Versionen kann man den Alias in der Einstellungen-GUI unter Details in Klammern bei "Basissystem" finden.
Vor dem apt update muss das Paket apt-transport-https installiert werden: `sudo apt install apt-transport-https`
Nun kann man `sudo apt update` ausführen.
falls man noch altes wine installiert hat sollte man folgendes ausführen:
 1. `sudo apt remove wine` alte wine Version entfernen
 2. `sudo apt autoremove` unbenötigte Pakete entfernen
 > apt ist neuer und simpler als apt-get und wurde deswegen hier genutzt.

Am besten installiert man die stable (winehq-stable) Version mit den vorgeschlagenen Paketen (--install-recommends):

    sudo apt install --install-recommends winehq-stable
Nun hat man die neuste, stabile wine Version und kann die gewünschten Programme in wine installieren.

Für Office (und andere wine-Funktionen) sollte man noch winbind (Teil von samba) mit `sudo apt install winbind` installieren.

Zum Schluss sollte man noch einen normalen wineprefix mit `winecfg` erstellen. Wenn man winecfg ausführt wird ein neues Verzeichnis ~/.wine erstellt und darin die Konfiguration gespeichert.
winecfg wird wahrscheinlich danach fragen, ob man wine-mono und wine-gecko installieren möhcte (falls diese noch nicht installiert sind) und man sollte sie installieren.
Das Fenster "Wine-Konfiguration kann man dann erstmal schließen.


## Microsoft Office
### Installation:
Die im Folgenden beschriebenen Schritte wurde nur für Office 2007 getestet, für andere Office Versionen muss man andere Schritte durchführen.

Damit Office vollständig funktioniert sollte man winbind (Teil von samba) zusätzlich installiert haben (wurde schon bei der wine Installation gemacht).
Weil Office nur unter win32 in wine läuft und es mit einem 64bit wineprefix Probleme gibt, muss man einen neuen wineprefix erstellen. (der wineprefix ist z.B. der Ordner ~/.wine)
hierzu siehe:  
https://appdb.winehq.org/objectManager.php?sClass=version&iId=4992 unter HOWTO, sowie
https://wiki.winehq.org/FAQ#How_do_I_create_a_32_bit_wineprefix_on_a_64_bi_system.3F

Um das 32bit wineprefix zu erstellen führt man folgendes aus:
`WINEARCH=win32 WINEPREFIX=~/.wine_office winecfg`
Dies wird nun den neuen wineprefix für Office mit einer Konfiguraion erstellen.
In dem Fenster (winecfg, "Wine-Konfiguration"), welches sich öffnet, wählt man nun in der Liste im "Anwendungen"-Register die Standardeinstellungen aus und setzt die Windows-Version auf Windows XP.

Da wir nun einen neuen wineprefix erstellt haben, muss dieser jedes mal angegeben werden, wenn man diesen oder in ihm installiere Programme nutzen möchte.  
Mehr informationen zu wineprefix findet man hier:  
https://wiki.winehq.org/Wine_User%27s_Guide#WINEPREFIX  
https://wiki.winehq.org/FAQ#Wineprefixes

Am besten installiert man Office mit dem Service Pack 2.
Um nun Office zu installieren geht man in das Verzeichnis mit den Installationsdateien und führt `WINEPREFIX=~/.wine_office wine setup.exe` aus oder gibt den kompletten Pfad an, z.B. `WINEPREFIX=~/.wine_office wine /media/phybox/USB-STICK/Office2007/SETUP.EXE`
Man folgt nun einfach dem Installer wie auch auf Windows.
Um das Service Pack zu installieren geht man ähnlich vor. Man führt den Installer mit wine aus, z.B. `WINEPREFIX=~/.wine_office wine /media/phybox/USB-STICK/Office2007_SP2/office2007sp2-kb953195-fullfile-de-de.exe`, und folgt diesem.

Nachdem beides installiert ist muss man nun winecfg (mit `WINEPREFIX=~/.wine_office winecfg`) erneut starten. In dem Register "Bibliotheken" fügt man eine neue Überschreibung für riched20 hinzu. Man tippt `riched20` in das Feld "Neue Überscheibung für:" ein bzw. wählt es aus und klickt "Hinzufügen". Es sollte nun `riched20 (Native, Buildin)` in der Liste stehen, ist dies nicht der Fall wählt man riched20 aus, klickt auf "Bearbeiten" und stellt sicher, dass "Native dann Buildin" ausgewählt ist.
Diese Überschreibung Konfiguriert wine so, dass Office seine eigene riched20.dll nutzen kann.

Die Programme sind nun unter `"~/.wine_office/drive_c/Program Files (x86)/Microsoft Office/Office12/"` als WINWORD.exe, EXCEL.exe und POWERPNT.exe installiert und könnten mit wine, wineconsole oder wine start ausgeführt werden (man muss den wineprefix angeben).

### Tests:
Mit `wine winemenubuilder` lassen sich Einträge in das gnome application menu erstellen.

Während der Tests habe ich auch je eine Word und Excel Datei erstellt und in den Dateieigenschaften eingestellt, diese standartmäßig mit dem jeweiligen Office-Program zu öffnen.


## Phymex
### Installation
PhyMex (die Software für die PhyBox) kann bei der Uni-Bayreut ([link](http://daten.didaktikchemie.uni-bayreuth.de/experimente/chembox/0_download/phybox.zip)) runter geladen werden bzw. von einer CD genommen werden.
Wenn man die Dateien als Zip von der Uni-Bayreut heruntergeladen hat entpackt man sie im Terminal mit `unzip phybox.zip` oder im Datei-Browser mit Rechtsklick und Klick auf "Hier entpacken".

PhyMex funktioniert einfach so unter wine und hat keine besonderen Anforderungen, es kann daher in den standard wineprefix installiert werden. Das Verzeichnis bzw.der wineprefix ~/.wine sollte schon bestehen.
Man wechselt mit cd in das Verzeichnis mit den Installationsdateien (z.B. "cd ~/Downloads/phybox") und führt `wine Phymex_Setup.exe` aus. Dies wird das setup-programm für Phymex starten.
Wenn das Setup-Programm gestartet ist folgt man einfach dem Prozess.
Ich habe Deutsch als Sprache gewählt und den Standard Installationspfad genutzt.
Das am Ende noch offene kleine Fenster (Entpacker) kann man einfach schließen.
Fehler die wine im Terminal ausgegeben hat kann man einfach ignorieren.  
Phymex kann man nun mit `wine ~/.wine/drive_c/PHYMEX/PHYMEX1.EXE` starten.


### Phymex wine Einstellungen
Nach dem Reinstall in dem win32 wineprefix habe ich versucht ob das Hinzufügen der Phymex Executables (unter "~/.wine/drive_c/PHYMEX") und das setzten von anderen Windows Versionen für diese (Standart auf Windows 7 gelassen, hatte Windows 2000, 98 und 95 für Phymex) einen Unterschied bezüglich des grauen Bandes macht, was sich bei Vollbild über die Steuerelemente auf der rechten Seite schiebt, aber leider hatte das nicht funktioniert und ich habe sie wieder entfernt (der Standart wird wieder genutzt).

Was jedoch funktioniert hat war im Grafik-Register von winecfg den virtuellen Bildschirm aktivieren und die Desktop-Größe auf 1339 x 836 zu setzen. Die Desktop-Größe habe ich durch Versuche herausgefunden und funktionieren am besten mit PhyMex und der Auflösung von dem Laptop. Die Fenster-Leiste oben bekommt man leider durch deaktivieren der Optionen "Erlaube dem Fenstermanager die Fenster zu dekorieren" bzw. "Erlaube dem Fenstermanager die Fenster zu kontrollieren" nicht weg.


## Weitere Einstellungen
### Einstellungen
Einstellungen in der "Einstellungen"-GUI:
Maus und Tastfeld:
 - Tastfeld
    - Bildlauf am Rand: An
Hintergrund:
 - anderen Hintergrund und Sperrbildschirm auswählen

### Gnome-Tweaks Einstellungen
Gnome-Tweaks oder "Optimierungswerkzeug" ist eine kleine GUI Anwendung um Einstellungen der Gnome Desktop-Environment anzupassen.
Ich habe folgende Einstellungen vorgenommen:
Arbeitsoberfläche
 - Symbole auf Arbeitsfläche: An
    - :ballot_box_with_check: Persönlicher Ordner
    - :black_square_button: Netzwerk Server
    - :ballot_box_with_check: Papierkorb
    - :ballot_box_with_check: Eingebundene Datenträger
Erweiterungen
 - Applications menu: An
 - Window list: An
Fenster
 - Knöpfe der Titelleiste
    - Maximieren: An
    - Minimieren: An
Obere Leiste
 - Anwendungsmenü anzeigen: An
 - Uhr
    - :ballot_box_with_check: Datum zeigen
    - :black_square_button: Sekunden zeigen


### Nachbereitungen
Mit `sudo apt autoremove` überflüssig gewordene Programme deinstallieren

## Weitere Ideen
 - man könnte versuchen PhyMex, sowie die PhyBox und die Seriell gesendeten Daten zu reverse-engineeren (serielle Daten aufnehmen/mitschneiden) und eine Open-Source/Open-Hardware PhyBox entwerfen, sowie ein platform-unabhängiges Open-Source Programm wie PhyMex schreiben.
