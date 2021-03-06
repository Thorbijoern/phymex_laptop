## PhyMex installation
PhyMex (die Software für die PhyBox) kann bei der Uni-Bayreut (link) runter geladen werden bzw. von einer CD genommen werden.
Wenn man die Dateien als Zip von der Uni-Bayreut heruntergeladen hat entpackt man sie am bestem mit "unzip phybox.zip".
Man wechselt mit cd in das verzeichnis mit den installationsdateien (z.B. "cd ~/Downloads/phybox").
Im Terminal:

    wine Phymex_Setup.exe

Dies wird das setup-programm für phymex starten.
Wenn das Setup-Programm gestartet ist folgt man einfach dem Prozess und benutzt die Standarteinstellungen.
Das am Ende noch offene kleine Fenster (Entpacker) kann man einfach schließen.

Fehler die wine im Terminal ausgegeben hat, z.B. der folgende, kann man einfach ignorieren.
debug/errors vom installer:

    err:ddeml:WDML_CreateString Unknown code page 850

nun um PhyMex zu starten:
Im Terminal:

    wine ~/.wine/drive_c/PHYMEX/PHYMEX1.EXE

debug/errors von phymex:

    err:dc:CreateDCW no device found for (null)
    err:dc:CreateDCW no driver found for L",,"


serial-port stuff:
- installed setserial (im nachhinnein nicht benötigt)
- muss vlt. zu gruppen hinzugefügt werden
- wine sollte in ~/.wine/dosdevices COM's und/oder LPT's erstellen (user muss erst in gruppe sein)

gruppen vlt. sys oder dialout

    groups phybox #zeigt gruppen von user phybox an

unter ~/.wine/dosdevices/ sollten die serial-ports sein
unter /dev/ sind ttyS0 bis ttyS3 für uns interessant (/dev/ttyS0)

unter root: (su -)

    usermod -a -G sys phybox # gruppe 'sys' dem user phybox hinzufügen, danach neu anmelden
    cut -d: -f1 /etc/group #alle gruppen im system anzeigen
    find /sys/ -name 'tty:ttyS*' #hatte null output
    dmesg | egrep --color "serial|ttyS" #output:
Output:

    [    0.574142] 00:03: ttyS0 at I/O 0x3f8 (irq = 4, base_baud = 115200) is a 16550A
    [ 3558.210088] serial 00:03: disabled
    [ 3559.356524] serial 00:03: activated

`apt install setserial` (ist nicht installiert)
 > benötigt? eher nicht
`setserial -g /dev/ttyS[0123]` zeigt config der seriellen-ports
Output:

    /dev/ttyS0, UART: 16550A, Port: 0x03f8, IRQ: 4
    /dev/ttyS1, UART: unknown, Port: 0x02f8, IRQ: 3
    /dev/ttyS2, UART: unknown, Port: 0x03e8, IRQ: 4
    /dev/ttyS3, UART: unknown, Port: 0x02e8, IRQ: 3

um von gruppe wieder zu entfernen müssen alle angegeben werden denen der user noch angehören soll (nicht ganz richtig; es gibt andere commands, bei denen es ):

    usermod -G phybox,cdrom,floppy,audio,dip,video,plugdev,netdev,bluetooth,lpadmin,scanner phybox
sys hat nicht geklappt, nun dialout testen

    usermod -a -g dialout phybox

`ls -la /dev/tty*` alle tty devices im system (eher nutzlos)
`ls -la /dev/ttyS*` alle seriellen tty devices (die die wir wollen)

programme für serial, die ich iwo fand (oder auch nicht; aber nicht installert waren):
 - hal
 - cu
 - screen
 - tmux
 - minicom
 - tip


### Lösung:
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
habe noch gruppen wieder weg genommen
neustarten um gruppen upzudaten
nach neustart `wine -V` > konfigurationsupdate... pakete fehlen, erstmal ignoriert
Output:

    000f:fixme:service:scmdatabase_autostart_services Auto-start service L"MountMgr" failed to start: 2
    Could not load wine-gecko. HTML rendering will be disabled.
    Could not load wine-gecko. HTML rendering will be disabled.
    wine: configuration in '/home/phybox/.wine' has been updated.
    wine: cannot find L"C:\\windows\\system32\\-V.exe"

es fehlt wohl mono (wine-mono)? und wine-gecko, nicht mit apt auffindbar
neue errors... bei programmstart
habe mit wineboot das konfigurationsupdate noch mal durchgeführt:

    wineboot -u
Output:

    0034:fixme:urlmon:InternetBindInfo_GetBindString not supported string type 20
    0034:fixme:ntdll:NtLockFile I/O completion on lock not implemented yet
    0034:err:mscoree:LoadLibraryShim error reading registry key for installroot
    0034:err:mscoree:LoadLibraryShim error reading registry key for installroot
    0034:err:mscoree:LoadLibraryShim error reading registry key for installroot
    0034:err:mscoree:LoadLibraryShim error reading registry key for installroot
    0034:fixme:msi:internal_ui_handler internal UI not implemented for message 0x0b000000 (UI level = 1)
    0034:fixme:msi:internal_ui_handler internal UI not implemented for message 0x0b000000 (UI level = 1)
    003b:fixme:urlmon:InternetBindInfo_GetBindString not supported string type 20
    003b:fixme:ntdll:NtLockFile I/O completion on lock not implemented yet
    003b:fixme:msi:internal_ui_handler internal UI not implemented for message 0x0b000000 (UI level = 1)
    003b:fixme:msi:internal_ui_handler internal UI not implemented for message 0x0b000000 (UI level = 1)
    002f:err:winediag:SECUR32_initNTLMSP ntlm_auth was not found or is outdated. Make sure that ntlm_auth >= 3.0.25 is in your path. Usually, you can find it in the winbind package of your distribution.
    002f:fixme:dwmapi:DwmIsCompositionEnabled 0x6dbd1518
    0040:fixme:iphlpapi:NotifyIpInterfaceChange (family 0, callback 0x69ebd3de, context 0x8d6590, init_notify 0, handle 0x11cfa20): stub
    0054:fixme:urlmon:InternetBindInfo_GetBindString not supported string type 20
    0054:fixme:ntdll:NtLockFile I/O completion on lock not implemented yet
    0054:fixme:msi:internal_ui_handler internal UI not implemented for message 0x0b000000 (UI level = 1)
    0054:fixme:msi:internal_ui_handler internal UI not implemented for message 0x0b000000 (UI level = 1)
    0052:err:winediag:SECUR32_initNTLMSP ntlm_auth was not found or is outdated. Make sure that ntlm_auth >= 3.0.25 is in your path. Usually, you can find it in the winbind package of your distribution.
    0052:fixme:dwmapi:DwmIsCompositionEnabled 0x6d5d3018
    0059:fixme:iphlpapi:NotifyIpInterfaceChange (family 0, callback 0x6a0cb608, context 0x958878, init_notify 0, handle 0x119fc88): stub
    wine: configuration in '/home/phybox/.wine' has been updated.

`wineboot -u` noch mal ausgefürt
Output:

    0030:err:winediag:SECUR32_initNTLMSP ntlm_auth was not found or is outdated. Make sure that ntlm_auth >= 3.0.25 is in your path. Usually, you can find it in the winbind package of your distribution.
    0030:fixme:dwmapi:DwmIsCompositionEnabled 0x6dbd1518
    0034:fixme:iphlpapi:NotifyIpInterfaceChange (family 0, callback 0x69ebd3de, context 0x8d6590, init_notify 0, handle 0x11cfa20): stub
    0044:err:winediag:SECUR32_initNTLMSP ntlm_auth was not found or is outdated. Make sure that ntlm_auth >= 3.0.25 is in your path. Usually, you can find it in the winbind package of your distribution.
    0044:fixme:dwmapi:DwmIsCompositionEnabled 0x6d5d3018
    0046:fixme:iphlpapi:NotifyIpInterfaceChange (family 0, callback 0x6a0cb608, context 0x958878, init_notify 0, handle 0x119fc88): stub
    wine: configuration in '/home/phybox/.wine' has been updated.

serielle ports in wine check!

phymex starten:

    wine ~/.wine/drive_c/PHYMEX/PHYMEX1.EXE

to capture serial data:
 noch nichts gefunden


## TEST AN PHYBOX
erfolgreich nachdem ich dem user phybox die gruppen sys und dialout gegeben habe
`usermod -a -G sys,dialout phybox`
> schauen was gruppen (sys, dialout) machen/für rechte haben
