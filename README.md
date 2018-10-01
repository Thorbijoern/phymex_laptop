# phymex_laptop
In dieser Datei beschreibe ich die Schritte die ich zum aufsetzen eines Laptops vorgenommen habe.

Als Basis dient die GNU/Linux Distribution Debian 9 alias "stretch".
Die Hardware ist ein Fujitsu Esprimo Mobile D9510.

## Known Limitations/Problems
 - Der integrierte WLAN (und Bluetooth) Adapter benötigt nicht-freie Treiber, die ich nicht finden konnte.
 - Ich konnte leider das Problem in Phymex nicht lösen, bei dem im Vollbild-Modus eine graue Leiste am rechten Rand Steuerelemente überdeckt.
 - Microsoft PowerPoint läuft scheinbar nicht in Wine (getestet mit: Wine [Version]).


## Vorbereitung
Es war noch ein unaktiviertes (keine Lizens) Windows 7 auf dem Laptop zu Testzwecken oder so installiert.
Ich habe geschaut ob noch Dateien oder andere wichtige Sachen darauf sind, aber habe nicht gefunden und fuhr mit der Installation fort.


## Linux Installation
Sprache, Tastatur: Deutsch-Deutschland  
Zeitzohne: Mitteleuropa/GMT (UTC+1:00), Berlin  
installierte Standartpakete  
Name neuer Nutzer: phybox


### Nach der Linux Installation
Ich habe mit Gnome Software sämtliche vorinstallierten Spiele entfernt.
Ich habe in Firefox ein paar vorgeschlagene Webseiten entfernt, da es sich bei diesen u.a. um Online-Shops handelte.
Sie erschienen mir nicht besonders didaktisch wertvoll und können zur Not wieder manuell hinzugefügt werden.

am netzwerk, unter root:
 1. `apt update && apt upgrade` Paket Datenbank aktualisieren und Paket-Updates installieren
 2. `apt install vim sudo`      vim und sudo installieren
 3. `usermod -aG sudo phybox`   User phybox zur sudo Gruppe hinzufügen
 4. überprüfen mit `groups phybox` (sudo muss mit aufgelistet sein)
 5. aus- und wieder einloggen bzw. reboot zum übernehmen der Änderung


## Wine Installation
Nach der Installation von dem Linux habe ich wine (übersetzt Windows API calls für POSIX-Systeme) installiert:
Im Terminal:

    sudo apt update
    sudo apt install wine

    sudo dpkg --add-architecture i386 && apt-get update && apt-get install wine32

nachdem das abgeschlossen ist, kann man nun die gewünschten Programme in Wine installieren

wine ist in den debian paketquellen irre veraltet (1.8.7)
siehe https://wiki.winehq.org/Debian
(eigene wine.list)
vor dem apt update muss folgendes installiert werden:
`apt install apt-transport-https`
falls man noch altes wine installiert hat sollte man folgendes ausführen:
 1. `apt remove wine` alte wine version entfernen
 2. `apt autoremove` unbenötigte dependencies entwfernen
 > am besten nutzt man apt und nicht apt get
am besten installiert man die stable (wine-stable) version mit den vorgeschlagenen paketen (--install-recommends)

    apt install wine-stable --install-recommends
nun hat man neuste wine version, es sollte alles gehen
 

## Microsoft Office installation:
Um Office zu installieren musste ich den Ordner ~/.wine löschen (mit `rm -rf ~/.wine`), weil Office nur unter win32 in wine läuft.
Mit einem 64bit wineprefix gibt es Probleme. (der wineprefix ist der ordner ~/.wine)
hierzu siehe:  
https://appdb.winehq.org/objectManager.php?sClass=version&ild=4992 unter HOWTO, sowie
https://wiki.winehq.org/FAQ#How_do_I_create_a_32_bit_wineprefix_on_a_64_bi_system.3F

Um das 32bit wineprefix zu erstellen führt man folgendes aus (nachdem der alte ~/.wine Ordner gelöscht wurde):
`WINEARCH=win32 WINEPREFIX=~/.wine winecfg`
Dies wird nun die wine Konfiguration sowie den wineprefix erstellen.
Das Fenster, dass sich öffnet, kann man dann erstmal schließen
Um nun Office zu installieren geht man in das Verzeichnis mit den Installationsdateien und führt 'wine setup.exe' aus.
Man folg nun einfach dem Installer wie auch auf Windows.
Die Programme sind nun unter `"~/.wine/drive_c/Program Files (x86)/Microsoft Office/Office12/"` als WINWORD.exe, EXCEL.exe und POWERPNT.exe installiert und könnten mit wineconsole ausgeführt werden.
Nach der Installation von Office kann Phymex installiert werden.

### Tests:
Kurze Tests haben ergeben, dass Excel und Word scheinbar ziemlich problemlos laufen, aber Powerpoint startet leider nicht.

Mit `wine winemenubuilder` lassen sich Einträge in das gnome application menu erstellen.


## Phymex Installation
PhyMex (die Software für die PhyBox) kann bei der Uni-Bayreut ([link](http://daten.didaktikchemie.uni-bayreuth.de/experimente/chembox/0_download/phybox.zip)) runter geladen werden bzw. von einer CD genommen werden.
Wenn man die Dateien als Zip von der Uni-Bayreut heruntergeladen hat entpackt man sie am bestem mit `unzip phybox.zip`.
Man wechselt mit cd in das verzeichnis mit den installationsdateien (z.B. "cd ~/Downloads/phybox").
Im Terminal:

    wine Phymex_Setup.exe

Dies wird das setup-programm für phymex starten.
Wenn das Setup-Programm gestartet ist folgt man einfach dem Prozess und benutzt die Standarteinstellungen.
Das am Ende noch offene kleine Fenster (Entpacker) kann man einfach schließen.

Fehler die wine im Terminal ausgegeben hat kann man einfach ignorieren.  
nun um PhyMex zu starten:

    wine ~/.wine/drive_c/PHYMEX/PHYMEX1.EXE


### Phymex wine Einstellungen / Weitere Einstellungen für Phymex
Der User phybox benötigt die Gruppen sys und dialout, der folgende Befehl fügt den User zu den Gruppen hinzu:  
`usermod -a -G sys,dialout phybox`
> schauen was gruppen (sys, dialout) machen/für rechte haben

Nach dem reinstall in dem win32 wineprefix habe ich versucht ob das Hinzufügen der Phymex Executables (unter "....PHYMEX") und das setzten von anderen Windows Versionen für diese (Standart auf Windows 7 gelassen, hatte Windows 2000, 98 und 95 für Phymex) einen Unterschied bezüglich des grauen Bandes macht, was sich bei Vollbild über die Steuerelemente auf der rechten Seite schiebt, aber leider hatte das nicht funktioniert und ich habe sie wieder entfernt (der Standart wird wieder genutzt).


## Weitere Einstellungen
### Einstellungen
Einstellungen in der "Einstellungen"-GUI:
Maus und Tastfeld:
 - Tastfeld
    - Bildlauf am Rand: An
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
Fenster
 - Knöpfe der Titelleiste
    - Maximieren: An
    - Minimieren: An
Obere Leiste
 - Anwendungsmenü anzeigen: An
 - Uhr
    - :ballot_box_with_check: Datum zeigen
    - :black_square_button: Sekunden zeigen
