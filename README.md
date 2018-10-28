# phymex_laptop
Diese Dokumentation ist unter der XXXXX Lizenz auf Github veröffentlicht: https://github.com/Thorbijoern/phymex_laptop

## Aufgabe / Zielsetzung
In diesem Dokument beschreibe ich die Schritte, die ich zum Aufsetzen eines Laptops vorgenommen habe.  
Das Laptop soll dem Physik-Unterricht dienen und soll, u.a. wegen Sicherheitsbedenken, ein anderes Laptop mit Windows XP ersetzen.  
Dieses Laptop wird hauptsächlich in Verbindung mit Phymex genutzt. Dies ist eine Software die es ermöglicht, mit Hilfe der zugehörigen Phybox, Messdaten von z.B. Experimenten aufzunehmen und diese grafisch darzustellen. Die Datenübertragung zwischen Phymex, bzw. dem Computer auf dem Phymex läuft, und der Phybox geschieht durch eine serielle Schnittstelle, in diesem Fall eine RS-232 Schnittstelle über einen 9-poligen D-Sub COM-Port an dem Laptop.  
Weitere Informationen kann man dem folgenden von der Uni-Bayreuth veröffentlichten Dokument entnehmen:  
[http://daten.didaktikchemie.uni-bayreuth.de/experimente/chembox/0_download/phybox.pdf]

Um die Nutzung dem Fachlehrer etwas einfacher zu machen wird zusätzlich Microsoft Office 2007 installiert, damit schon bestehende Dokumente problemlos genutzt werden können (Zusätzlich zu MS Office steht noch LibreOffice zur Verfügung).
Zudem hat sich der Fachlehrer einen Webbrowser gewünscht. Dafür wird Firefox genutzt, welcher schon bei der Debian Desktopoberfläche/Gnome vorinstalliert ist.  
Eine Windows-Domain/Active Directory integration war für den Fachlehrer nicht nötig.

Als Hardware stand ein Fujitsu Esprimo Mobile D9510 zur Verfügung und die Basis bildet die GNU/Linux Distribution Debian 9 alias "stretch".

## Ergebnis
### Anmerkungen/Known Limitations/Problems
 - Der integrierte WLAN (und Bluetooth) Adapter benötigt nicht-freie Treiber, die ich nicht finden konnte. Dies ist jedoch kaum ein Problem, da die Schule eh kein WLAN zur Verfügung stellt.
 - PlayOnLinux bietet eine einfachere Möglichkeit Programme in wine zu installieren und zu verwalten, hat aber auch Probleme und Limitationen.


### Erfolge
Office und Phymex funktionieren erfolgreich und ich konnte sogar das Problem mit Phymex beheben, dass sich bei maximiertem Fenster eine Leiste über Steuerelemente geschoben hat und man diese dann nicht mehr nutzen konnte.  
Ich habe einiges zur Nutzung von wine lernen können und konnte mein Wissen zu Linux im allgemeinen etwas vertiefen und erweitern.


## Vorbereitung
Es war noch ein unaktiviertes (keine Lizens) Windows 7 auf dem Laptop zu Testzwecken oder so installiert.
Ich habe geschaut ob noch Dateien oder andere wichtige Sachen darauf sind, aber habe nicht gefunden und fuhr mit der Installation fort.


## Evaluation von Phymex auf Linux
In diesem Kapitel beschreibe ich die Evaluation, die ich mit dem Laptop, wine und Phymex unternommen hatte. Diese Evaluation war nur ein erster Test um sicher zu gehen, dass die Nutzung von Phymex auf Linux auch wirklich möglich ist.
Dieses Kapitel ist ziemlich unabhängig von den anderen, da ich nach der Evaluation einiges verändert und sogar den Laptop komplett neu aufgesetzt hatte.
Wenn man das Laptop neu aufsetzen möchte kann man dieses Kapitel getrost ignorieren.

Die Linux Installation hatte ich ähnlich vorgenommen wie in folgenden Kapiteln beschrieben.

Aufgefallen war mir der Versionsunterschied als ich Nachforschungen angestellt habe, warum wine nicht unter ~/.wine/dosdevices die verschiedenen seriellen Geräte anlegt. Ich hatte, einer Antwort im Internet folgend, das kleine Tool setserial installiert, was mir aber nicht half und somit im nachhinein überflüssig ist. Eines der Probleme war, dass der Benutzer, unter dem wine gestartet wird, also der, der es startet, muss die Berechtigungen erhalten um auf die seriellen Schnittstellen zugreifen zu können. Das Berechtigungskonzept von Linux löst das mit verschiedenen Gruppen, darunter die Gruppen sys und dialout. Ich habe den Benutzer beiden Gruppen hinzugefügt, aber sys wird nicht benötigt. Das zweite Problem bestand darin, dass ältere Versionen von wine die seriellen Geräte nicht automatisch anlegten, dadurch bemerkte ich, dass ich eine alte Version von wine nutzte.
wine hatte ich vorerst nur über die Debian Paketquellen installiert was sich dann nach einigen Tests als Problem rausstellte, da es in den Debian Paketquellen sehr veraltet war. Die letzte Version verfügbar in den Debian Paketquellen war 1.8.7, aber die aktuelle stabile Version (zum Zeitpunkt des Schreibens/Testens) war wine 3.0.3.
Ich hatte dann die aktuelle Version installiert.

Nachdem ich die aktuelle Version von wine nutzte und den Benutzer zu den Gruppen hinzugefügt hatte, hatte ich zusammen mit meinem Lehrer Phymex auf wine mit der Phybox erfolgreich getestet.


## Linux
### Installation
Für die Installation hatte ich eine aktuelle Debian ISO (9.5.0 amd64 netinstall) von debian.org runter geladen und auf einen USB-Stick geflasht. Am Laptop habe ich dann von dem USB-Stick gestartet und die Installation mit Hilfe der grafischen Oberfläche (Graphical Install) gewählt.

Folgend sind alle Einstallungen, die ich während der Installation vorgenommen habe:
Sprache: German - Deutsch  
Standort: Deutschland  
Tastatur: Deutsch  

An diesem Punkt erscheint wahrscheinlich eine Meldung, die fragt, ob die fehlenden WLAN-Treiber von einem anderen Medium gelesen werden sollten. Ich habe Nein ausgewählt.

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
Mit`usermod -aG sudo,dialout phybox` fügt man den User phybox zu den Gruppen sudo und dialout hinzu. 
Der User phybox benötigt die Gruppe sudo um den Befehl sudo nutzen zu dürfen und dialout ermöglicht es dem User vollen und direkten Zugriff auf die Seriellen Ports zu haben, siehe https://wiki.debian.org/SystemGroups.
 > wine benötigt später die Gruppe dialout um Zugriff auf die Seriellen Ports zu bekommen, sodass Phymex funktioniert.  
 > Mehr zu Seriellen Ports mit wine: https://wiki.winehq.org/wine_User%27s_Guide#Serial_and_Parallel_Ports

Überprüfen kann man das nun mit `groups phybox` (sudo und dialout müssen mit aufgelistet sein).
Mit `exit` kann man root wieder verlassen.
Man muss sich nun mit phybox aus- und wieder einloggen bzw. den Computer neu starten damit die Gruppen-Änderungen übernommen werden.
Nachdem die Spiele entfernt wurden kann man auch noch `sudo apt autoremove` ausführen um nicht mehr benötigte Pakete zu entfernen.

Für das Schulnetzwerk wird ein Proxy benötigt. Falls man sich im Schulnetzwerk befindet oder man die Installation abgeschlossen hat wird im folgenden beschrieben, was man setzen muss:  
In der "Einstellungen"-GUI, unter Netzwerk, unter Netzwerk-Proxy wählt man "Automatisch" als Methode aus und gibt dann die Konfigurationsadresse (welche man von einem anderen Schulrechner bekommen kann) ein.


## wine
### Installation
Nach der Installation von dem Linux habe ich wine (übersetzt Windows API calls für POSIX-Systeme) installiert:

wine ist in den Debian Paketquellen irre veraltet, wie ich in meinem ersten Test herausgefunden habe.
Auf https://wiki.winehq.org/Debian ist eine vollständige Anleitung (Englisch) zum installieren von wine auf Debian.
Hier nur eine kurze Anleitung mit den Schritten, die ich unternahm:

`sudo dpkg --add-architecture i386` um 32 bit Pakete zu erlauben,  
den Key, der benutzt wird um die Pakete zu signieren runterladen `wget -nc https://dl.winehq.org/wine-builds/Release.key` und hinzufügen `sudo apt-key add Release.key`  
eine wine.list erstellen `sudo nano /etc/apt/sources.list.d/wine.list` und in diese `deb https://dl.winehq.org/wine-builds/debian/ stretch main` schreiben  
 > `stretch` gilt nur für Debian 9.x, da diese Verion den Alias "stretch" hat, für andere Versionen kann man den Alias in der Einstellungen-GUI unter Details in Klammern bei "Basissystem" finden.
Vor dem apt update muss das Paket apt-transport-https installiert werden: `sudo apt install apt-transport-https`
Nun kann man `sudo apt update` ausführen.
falls man noch altes wine installiert hat sollte man folgendes ausführen:
 1. `sudo apt remove wine` alte wine Version entfernen (falls nötig)
 2. `sudo apt autoremove` unbenötigte Pakete entfernen
 > apt ist neuer und simpler als apt-get und wurde deswegen hier genutzt.

Am besten installiert man die stable (winehq-stable) Version mit den vorgeschlagenen Paketen (--install-recommends):

    sudo apt install -y --install-recommends winehq-stable
Nun hat man die neuste, stabile wine Version und kann die gewünschten Programme in wine installieren.

Für Office (und andere wine-Funktionen) sollte man noch winbind (Teil von samba) mit `sudo apt install -y winbind` installieren.

Zum Schluss sollte man noch einen normalen wineprefix mit `winecfg` erstellen. Wenn man winecfg ausführt wird ein neues Verzeichnis ~/.wine erstellt und darin die Konfiguration gespeichert.
winecfg wird wahrscheinlich danach fragen, ob man wine-mono und wine-gecko installieren möchte (falls diese noch nicht installiert sind) und man sollte sie installieren.
Das winecfg Fenster ("wine-Konfiguration") kann man dann erstmal schließen.
Mit `rm Release.key` kann man die Key-Datei von wine löschen.


## Microsoft Office
### Installation:
Die im Folgenden beschriebenen Schritte wurde nur für Office 2007 getestet, für andere Office Versionen muss man andere Schritte durchführen.

Damit Office vollständig funktioniert sollte man winbind (Teil von samba) zusätzlich installiert haben (wurde schon bei der wine Installation gemacht).
Weil Office nur unter win32 in wine läuft, es also mit einem 64bit wineprefix Probleme gibt und um Probleme mit anderen Programmen zu vermeiden muss man einen neuen, separaten wineprefix erstellen. (der wineprefix ist z.B. der Ordner ~/.wine)  
Hierzu siehe:  
https://appdb.winehq.org/objectManager.php?sClass=version&iId=4992 unter HOWTO, sowie  
https://wiki.winehq.org/FAQ#How_do_I_create_a_32_bit_wineprefix_on_a_64_bi_system.3F

Um das 32bit wineprefix zu erstellen führt man folgendes aus:

    WINEARCH=win32 WINEPREFIX=~/.wine_office winecfg
Dies wird nun den neuen wineprefix für Office mit einer Konfiguration erstellen.
> Möchte man Office unter Windows XP laufen haben wählt man in dem Fenster (winecfg, "wine-Konfiguration"), welches sich öffnet, nun in der Liste im "Anwendungen"-Register die Standardeinstellungen aus und setzt die Windows-Version auf Windows XP. Ansonsten kann man es auf Windows 7 belassen.

Da wir nun einen neuen wineprefix erstellt haben, muss dieser jedes mal angegeben werden, wenn man diesen oder in ihm installierte Programme nutzen möchte.  
Mehr informationen zu wineprefix findet man hier:  
https://wiki.winehq.org/wine_User%27s_Guide#WINEPREFIX  
https://wiki.winehq.org/FAQ#wineprefixes

Um nun Office zu installieren geht man in das Verzeichnis mit den Installationsdateien und führt `WINEPREFIX=~/.wine_office wine SETUP.EXE` aus oder gibt den kompletten Pfad an, z.B. `WINEPREFIX=~/.wine_office wine /media/phybox/USB-STICK/Office2007/SETUP.EXE`
Man folgt nun einfach dem Installer wie auch auf Windows.
> Wenn man sich zuvor entschieden hat Office unter Windows XP zu installieren, installiert man am besten auch das Service Pack 2.
> Um das Service Pack zu installieren geht man ähnlich vor wie mit Office selbst. Man führt den Installer mit wine aus, z.B. `WINEPREFIX=~/.wine_office wine /media/phybox/USB-STICK/Office2007_SP2/office2007sp2-kb953195-fullfile-de-de.exe`, und folgt diesem.

Nachdem Office nun installiert ist muss man nun winecfg (mit `WINEPREFIX=~/.wine_office winecfg`) erneut starten. In dem Register "Bibliotheken" fügt man eine neue Überschreibung für riched20 hinzu. Man tippt `riched20` in das Feld "Neue Überscheibung für:" ein bzw. wählt es aus und klickt "Hinzufügen". Es sollte nun `riched20 (Native, Buildin)` in der Liste stehen. Ist dies nicht der Fall wählt man riched20 aus, klickt auf "Bearbeiten" und stellt sicher, dass "Native dann Buildin" ausgewählt ist.
Diese Überschreibung konfiguriert wine so, dass Office seine eigene riched20.dll nutzen kann.

Die Programme sind nun unter `"~/.wine_office/drive_c/Program Files/Microsoft Office/Office12/"` als WINWORD.EXE, EXCEL.EXE und POWERPNT.EXE installiert und könnten mit `wine` ausgeführt werden (man muss den wineprefix angeben).

Man sollte Word ein mal starten, bevor man Office anfängt wirklich zu nutzen. Dafür kann man den Befehl `WINEPREFIX=~/.wine_office wine "~/.wine_office/drive_c/Program Files/Microsoft Office/Office12/WINWORD.EXE"` nutzen.  
Windows Update gibt es in Wine keines, weswegen Automatische Updates nicht funktionieren.

### Tests:
Während der Tests habe ich je eine Word, Excel und Powerpoint Datei erstellt und in den Dateieigenschaften von Debian/Gnome eingestellt, diese standartmäßig mit dem jeweiligen Office-Program zu öffnen.


## Phymex
### Installation
Phymex (die Software für die Phybox) kann bei der Uni-Bayreuth ([link](http://daten.didaktikchemie.uni-bayreuth.de/experimente/chembox/0_download/phybox.zip)) runter geladen werden bzw. von einer CD genommen werden.
Wenn man die Dateien als Zip von der Uni-Bayreuth heruntergeladen hat entpackt man sie im Terminal mit `unzip phybox.zip` oder im Datei-Browser mit Rechtsklick und Klick auf "Hier entpacken".

Phymex funktioniert einfach so unter wine und hat keine besonderen Anforderungen, wie ich nach mehreren Versuchen mit verschiedenen Einstellungen festgestellt habe. Es kann daher in den standard wineprefix installiert werden. Wenn das Verzeichnis bzw.der wineprefix ~/.wine noch nicht besteht kann man einfach `winecfg`, ohne das setzen von Variablen, aufrufen.
 > Es ändert zwar scheinbar nichts an der Funktionalität aber aufgrund des alters von Phymex könnte man es auch mit `WINEARCH=win32 winecfg` einen 32bit wineprefix erstellen, in winecfg die Windows-Version auf XP, 98 oder 95 stellen und es dann in diesen installieren.
Man wechselt mit cd in das Verzeichnis mit den Installationsdateien (z.B. `cd ~/Downloads/phybox`) und führt `wine Phymex_Setup.exe` aus. Dies wird das setup-Programm für Phymex starten.
Wenn das Setup-Programm gestartet ist folgt man einfach dem Prozess.
Ich habe Deutsch als Sprache gewählt und den Standard Installationspfad genutzt.
Das am Ende noch offene kleine Fenster (Entpacker) kann man einfach schließen, die Warnung die beim Schließen angezeigt wird kann ignoriert werden.
Fehler (hauptsächlich "fixme"-Anmerkungen) die wine im Terminal ausgegeben hat kann man einfach ignorieren.  
Phymex kann man nun mit `wine ~/.wine/drive_c/PHYMEX/PHYMEX1.EXE` starten.


### Phymex wine Einstellungen
Nach einem Reinstall in einem win32 wineprefix habe ich versucht ob das Hinzufügen der Phymex Executables (unter "~/.wine/drive_c/PHYMEX") und das setzten von anderen Windows Versionen für diese (Standart auf Windows 7 gelassen, hatte Windows 2000, 98 und 95 für Phymex) einen Unterschied bezüglich des grauen Bandes macht, was sich bei Vollbild über die Steuerelemente auf der rechten Seite schiebt, aber leider hatte das nicht funktioniert und ich habe sie wieder entfernt (der Standart wird wieder genutzt).

Was jedoch funktioniert hat war im Grafik-Register von winecfg den virtuellen Bildschirm aktivieren und die Desktop-Größe auf 1339 x 803 (836 ohne Window list) zu setzen. Die Desktop-Größe habe ich durch Versuche herausgefunden und funktioniert am besten mit Phymex und der Auflösung von dem Laptop. Die zusätzliche Fenster-Leiste oben bekommt man leider durch deaktivieren der Optionen "Erlaube dem Fenstermanager die Fenster zu dekorieren" bzw. "Erlaube dem Fenstermanager die Fenster zu kontrollieren" nicht weg.


## Weitere Einstellungen
### Einstellungen
Einstellungen in der "Einstellungen"-GUI:
Maus und Tastfeld:
 - Tastfeld
    - Bildlauf am Rand: An
    - Drücken zum Klicken: An
Hintergrund:
 - anderen Hintergrund und Sperrbildschirm auswählen


### Gnome-Tweaks Einstellungen
Gnome-Tweaks oder "Optimierungswerkzeug" ist eine kleine GUI Anwendung um Einstellungen der Gnome Desktop-Environment anzupassen.
Ich habe folgende Einstellungen vorgenommen:
Arbeitsoberfläche
 - Symbole auf Arbeitsfläche: An
    - ☑️ Persönlicher Ordner
    - 🔲 Netzwerk Server
    - ☑️ Papierkorb
    - ☑️ Eingebundene Datenträger
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
    - ☑️ Datum zeigen
    - 🔲 Sekunden zeigen


### Desktop Einträge
Mit dem in wine integrierten Tool `winemenubuilder` (man braucht es nicht manuell ausführen) lassen sich Einträge in das Gnome Application Menu erstellen. Jedoch funktioniert dies nur automatisch für die Microsoft Office Programme, da diese von dem Installer in die wine-/Windows-Registry eingetragen wurden. Für Phymex müssen manuell Einträge erstellt werden.

Gnome folgt den Standarts von freedesktop.org (ehemals X Desktop Group, kurz XDG) und folgt für Desktop Einträge bzw. Einträge ins Appliction Menu der [Desktop Menu Specification](https://specifications.freedesktop.org/menu-spec/menu-spec-latest.html) und der [Desktop Entry Specification](https://specifications.freedesktop.org/desktop-entry-spec/desktop-entry-spec-latest.html).

Die Menü Einträge für Office sollten von wine automatisch erstellt werden.
winemenubuilder erstellt u.a. in den folgenden Ordnern Einträge:
 - `~/.config/menus/applications-merged/` .menu einträge
 - `~/.local/share/desktop-directories/` .directory einträge
 - `~/.local/share/applications` .desktop einträge
 - `~/.local/share/icons` icons für die programme
Die Einträge, die wine vornimmt, sind nur für den jeweiligen Benutzer, genau so wie die wineprefixe.

Im folgenden sind die Schritte, die man durchführen muss knapp erklärt. Wo bloß ein Pfad mit einem Codesnipsel angegeben ist erstellt man diese Datei mit dem Codesnipsel als Inhalt, z.B. mit dem Editor nano. Möchte man in den folgenden Schritten die Änderungen sehen, muss man sich u.U. abmelden und neu anmelden.

Erstellen von ein paar Dateien:
~/.local/share/desktop-directories/Microsoft\ Office.directory

    [Desktop Entry]
    Type=Directory
    Name=Microsoft Office
    Icon=folder

~/.local/share/desktop-directories/Phymex.directory

    [Desktop Entry]
    Type=Directory
    Name=Phymex
    Icon=folder

~/.local/share/desktop-directories/wine-Programs-Phymex.directory

    [Desktop Entry]
    Type=Directory
    Name=Phymex
    Icon=folder


Einen Ordner erstellen: `mkdir ~/.local/share/applications/wine/Programs/Phymex/`  
und den Desktop-Eintrag für Phymex erstellen:  
~/.local/share/applications/wine/Programs/Phymex/Phymex.desktop

    [Desktop Entry]
    Type=Application
    Version=1.1
    Name=Phymex
    Comment=Phybox/Phymex: Das universelle, PC-gesteuerte Meß- und Datenerfassungs-System
    Icon=Phymex
    Exec=env WINEPREFIX="/home/phybox/.wine" wine /home/phybox/.wine/dosdevices/c:/PHYMEX/PHYMEX1.EXE
    #MimeType=application/octet-stream;application/x-php;
    Categories=Education;Science;DataVisualization;Physics;
    Keywords=Phybox;Physik;Experimente;
    StartupNotify=true
    StartupWMClass=phymex1.exe

Den Comment habe ich aus dem deutschen Prospekt zu der Phybox entnommen, welcher von der Uni-Bayreuth veröffentlich wurde.  
Der MimeType kann aus den Eigenschaften von Phymex-Dateien gelesen werden, ist aber sehr mehrdeutig und deswegen auskommentiert.  
Die Categories wurden aus einer vordefinierten Liste in der Desktop Menu Specification gewählt.  
Die Keywords wurden frei gewählt und sind haupsächlich dazu da, dass man das Programm in der Suche im Application Menu besser findet.  
Ein Icon kann aus der Exe-Datei extrahiert oder manuell gezeichnet werden.
Um einen Icon für Phymex zu extrahieren, installiert man icoutils mit `sudo apt install icoutils`. Man extrahiert dann den Icon mit `wrestool -x --output=. -t14 ~/.wine/drive_c/PHYMEX/PHYMEX1.EXE` und konvertiert in mit `icotool -x PHYMEX1.EXE_14_1.ico` zu einer PNG-Datei. Um diesen Schritt abzuschließen verschiebt man nun noch die PNG: `mv PHYMEX1.EXE_14_1_1_32x32x4.png ~/.local/share/icons/hicolor/32x32/apps/Phymex.png`.

Damit Phymex später in dem "Mit anderer Anwednung öffnen"-Menü erscheint erstellt man am besten einen weiteren Desktop-Eintrag:
~/.local/share/applications/wine-extension-php_phi_phx.desktop

    [Desktop Entry]
    Type=Application
    Name=Phymex
    Icon=Phymex
    MimeType=application/octet-stream;application/x-php;
    Exec=env WINEPREFIX="/home/phybox/.wine" wine start /unix /home/phybox/.wine/dosdevices/c:/PHYMEX/PHYMEX1.EXE Z:%f
    NoDisplay=true
    StartupNotify=true

Phymex erstellt beim speichern drei Dateien, \*.PHI und \*.PHX von dem Mime-Typ application/octet-stream und eine \*.PHP Datei mit Mime-Typ application/x-php.

 > Bezüglich `Z:%f` fand ich den folgenden Forum-Thread nützlich: https://ubuntuforums.org/showthread.php?t=749684&s=e452398ab46f430a5280865d0f848f0a

 > Ich habe mit verschiedenen Wegen rumexperimentiert, dass man auch per Drag-and-Drop Dateien mit dem Desktop-Icon anzeigen kann.
 > Mein vielversprechendster Versuch war der folgende:
 > `bash -c env WINEPREFIX="/home/phybox/.wine"; file=%f; if [[ -z $file ]] ; then wine /home/phybox/.wine/dosdevices/c:/PHYMEX/PHYMEX1.EXE ; else wine /home/phybox/.wine/dosdevices/c:/PHYMEX/PHYMEX1.EXE Z:$file ; fi`
 > Im Terminal funktionierte dies, nur nicht in dem Desktop-Eintrag. Man könnte es jedoch in eine separate Shell-Datei schreiben und nur den bash Befehl mit der Shell-Datei angeben.


In diesem Schritt werden zwei Ordner und mehrere Softlinks zu Desktop-Einträgen angelegt, damit die Dateien nicht mehrmals existieren (falls man sie später editieren möchte):  
`mkdir ~/.local/share/applications/Phymex`

`mkdir ~/.local/share/applications/Microsoft\ Office`

`ln -s~/.local/share/applications/wine/Programs/Phymex/Phymex.desktop ~/.local/share/applications/Phymex/Phymex.desktop`

`ln -s~/.local/share/applications/wine/Programs/Microsoft\ Office/Microsoft\ Office\ Word\ 2007.desktop ~/.local/share/applications/Microsoft\ Office/Microsoft\ Office\ Word\ 2007.desktop`

`ln -s~/.local/share/applications/wine/Programs/Microsoft\ Office/Microsoft\ Office\ Excel\ 2007.desktop ~/.local/share/applications/Microsoft\ Office/Microsoft\ Office\ Excel\ 2007.desktop`

`ln -s~/.local/share/applications/wine/Programs/Microsoft\ Office/Microsoft\ Office\ PowerPoint\ 2007.desktop ~/.local/share/applications/Microsoft\ Office/Microsoft\ Office\ PowerPoint\ 2007.desktop`


Weitere Dateien anlegen:  
~/.config/menus/applications-merged/wine-Programs-Phymex-Phymex.menu

    <!DOCTYPE Menu PUBLIC "-//freedesktop//DTD Menu 1.0//EN"
    "http://www.freedesktop.org/standards/menu-spec/menu-1.0.dtd">
    <Menu>
      <Name>Applications</Name>
      <Menu>
        <Name>wine-wine</Name>
        <Directory>wine-wine.directory</Directory>
        <Menu>
          <Name>wine-Programs</Name>
          <Directory>wine-Programs.directory</Directory>
          <Menu>
            <Name>wine-Programs-Phymex</Name>
            <Directory>wine-Programs-Phymex.directory</Name>
            <Include>
              <Filename>wine-Programs-Phymex-Phymex.desktop</Filename>
            </Include>
          </Menu>
        </Menu>
      </Menu>
    </Menu>

~/.config/menus/applications-merged/Phymex-Phymex.menu

    <!DOCTYPE Menu PUBLIC "-//freedesktop//DTD Menu 1.0//EN"
    "http://www.freedesktop.org/standards/menu-spec/menu-1.0.dtd">
    <Menu>
      <Name>Applications</Name>
      <Menu>
        <Name>Phymex</Name>
        <Directory>Phymex.directory</Directory>
        <Include>
          <Filename>Phymex-Phymex.desktop</Filename>
        </Include>
      </Menu>
    </Menu>

~/.config/menus/applications-merged/Microsoft\ Office-Microsoft\ Office.menu

    <!DOCTYPE Menu PUBLIC "-//freedesktop//DTD Menu 1.0//EN"
    "http://www.freedesktop.org/standards/menu-spec/menu-1.0.dtd">
    <Menu>
      <Name>Applications</Name>
      <Menu>
        <Name>Microsoft Office</Name>
        <Directory>Microsoft Office.directory</Directory>
        <Include>
          <Filename>Microsoft Office-Microsoft Office Word 2007.desktop</Filename>
        </Include>
        <Include>
          <Filename>Microsoft Office-Microsoft Office Excel 2007.desktop</Filename>
        </Include>
        <Include>
          <Filename>Microsoft Office-Microsoft Office PowerPoint 2007.desktop</Filename>
        </Include>
      </Menu>
    </Menu>


Um die Berechtigungen der .desktop Datei von Firefox ändern zu können, müssen wir sie kopieren und den Besitzer auf Phybox ändern:  
`sudo cp /usr/share/applications/firefox-esr.desktop ~/.local/share/applications/`  
`sudo chown phybox:phybox ~/.local/share/applications/firefox-esr.desktop`  
Zum Schluss legt man noch mit den folgenden Befehlen Softlinks auf dem Schreibtisch/Desktop an:  
`ln -s ~/.local/share/applications/Microsoft\ Office/* ~/Schreibtisch/`  
`ln -s ~/.local/share/applications/Phymex/Phymex.desktop ~/Schreibtisch/`  
`ln -s ~/.local/share/applications/firefox-esr ~/Schreibtisch/`  
und macht diese ausführbar:
`chmod ug+x ~/Schreibtisch/*`  
Um das abzuschließen sollte man auf den Schreibtisch klicken, F5 drücken und alle Programme über ihre Links auf dem Schreibtisch ein mal starten und mit "Vertrauen und ausführen" bestätigen.

Zu guter Letzt habe Ich noch einen Desktop-Eintrag als Link in dem ~/Dokumente Ordner erstellt:  
"~/Dokumente/Dokumentation zu diesem Laptop.desktop"
    [DesktopEntry]
    Type=Link
    Version=1.1
    Name=Dokumentation zu diesem Laptop
    Comment=Dies ist ein Link zu der Dokumentation zum Setup dieses Laptops auf Github.
    URL=https://github.com/Thorbijoern/phymex_laptop


### Nachbereitungen
`apt remove icoutils
Mit `sudo apt autoremove` überflüssig gewordene Programme deinstallieren.

Ich habe in Firefox noch verschiedene nützliche Webseiten zur Phybox (siehe Weitere Informationen) als Lesezeichen in einen Ordner Namens `Phybox/Phymex` und auch noch ein Lesezeichen zu diese Dokumentation in dem Ordner für die Lesezeichen-Symbolleiste hinzugefügt.

## Weitere Ideen
 - man könnte versuchen Phymex, sowie die Phybox und die seriell gesendeten Daten zu reverse-engineeren (serielle Daten aufnehmen/mitschneiden) und eine Open-Source/Open-Hardware Phybox entwerfen, sowie ein platform-unabhängiges Open-Source Programm wie Phymex schreiben.

## Weitere Informationen zu Phymex/Phybox
Weitere Informationen findet man unter http://didaktikchemie.uni-bayreuth.de/de/teachers/05chembox/index.html bzw. http://daten.didaktikchemie.uni-bayreuth.de/experimente/chembox/ und verschiedenen veröffentlichten Dokumenten unter http://daten.didaktikchemie.uni-bayreuth.de/experimente/chembox/0_download/.

 - http://daten.didaktikchemie.uni-bayreuth.de/experimente/chembox/dokumentation.htm
 - http://daten.didaktikchemie.uni-bayreuth.de/experimente/chembox/0_download/phybox.pdf
 - http://www.chembox.hetz.de/news.htm
 - http://www.chembox.hetz.de/fragen.htm
 - http://www.chembox.hetz.de/chembox/h2-kin.htm
