# Windows Server AD Home Lab: Setup & Benutzerautomatisierung mit PowerShell

<img width="595" height="525" alt="Diagramm" src="https://github.com/user-attachments/assets/23d71936-8384-4a45-9c75-9fa28fd50e52" />




### Beschreibung 

Ich habe in diesem Projekt eine komplette Microsoft-Domänenumgebung aufgebaut und verwaltet. Dazu gehörten die Einrichtung eines Microsoft Servers mit Active Directory, die Bereitstellung eines Domain Controllers sowie die strukturierte Planung der Systemarchitektur und Domänenverwaltung.
Zur Automatisierung habe ich PowerShell-Skripte genutzt, um die Benutzeranlage effizient und fehlerarm abzuwickeln. Die Umgebung lief in VMware; Windows 11 Enterprise und Microsoft Server 2022 wurden erfolgreich integriert und betrieben.
Das Ergebnis: eine sauber orchestrierte, praxisnahe Infrastruktur, die meine Fähigkeiten in Systemadministration, Automatisierung und dem Einsatz aktueller Technologien belegt


### Lernziele 

- Grundprinzipien von Active Directory verstehen und dessen Rolle in der Netzwerkadministration einordnen.

- Virtuelle Maschinen mit Virtualisierungssoftware (z. B. VMware) erstellen, konfigurieren und verwalten.

- Einen Domain Controller in einer Netzwerkumgebung aufsetzen und betreiben.

- Troubleshooting-Kompetenzen ausbauen, um Setup- und Konfigurationsprobleme systematisch zu beheben.

- PowerShell sicher einsetzen, um administrative Aufgaben im Windows-Umfeld zu automatisieren.

- Prinzipien der Domänenkonnektivität und Authentifizierung verstehen und praktisch anwenden (z. B. Anmelden von Benutzerkonten in der Domäne).


### Technologien & Anforderungen

- Oracle Virtual Box: [Downloads – Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads)

![Image](https://github.com/user-attachments/assets/2bb8d257-bd0d-46d9-bb68-3079eb42915b)

- Windows Server 2022: [Windows Server 2022 | Microsoft Evaluation Center](https://www.microsoft.com/de-de/evalcenter/download-windows-server-2022)

![Image](https://github.com/user-attachments/assets/2d339092-157a-4347-8a0e-10a63398a865)


- Windows 11 Enterprise: [Windows 11 Enterprise | Microsoft Evaluation Center](https://www.microsoft.com/de-de/evalcenter/download-windows-11-enterprise)

![Image](https://github.com/user-attachments/assets/e7b71f88-4fc0-466b-9e21-f03a0bfea916)





### 1. Anleitung: Erstellung einer virtuellen Maschine für einen Domain Controller

**Anleitung:** Erstellung einer virtuellen Maschine für einen Domain Controller
Diese Anleitung beschreibt die schrittweise Einrichtung einer virtuellen Maschine (VM) in Oracle VirtualBox, optimiert für die Installation eines Windows Server als Domain Controller (DC).

**1.1 Grundlegende Erstellung der virtuellen Maschine:**

- [ ] Starten Sie Oracle VirtualBox und klicken Sie auf die Schaltfläche "Neu".

- [ ] Vergeben Sie im VM Name einen aussagekräftigen Namen für die VM (z. B. "DC").

- [ ] Wählen Sie das ISO-Abbild des Windows Server-Betriebssystems direkt aus, indem Sie auf das Ordnersymbol klicken, die entsprechende Datei auswählen und mit "Öffnen" bestätigen.

- [ ] Deaktivieren Sie die Option "Mit unbeaufsichtigter Installation fortfahren", um eine manuelle Installation durchführen zu können.

**1.2 Konfiguration der virtuellen Hardware:**
_Weisen Sie der VM die folgenden Hardware-Ressourcen zu:_

- [ ] Arbeitsspeicher (RAM): Weisen Sie mindestens 4096 MB zu. Dies ist ein empfohlener Wert für einen Domain Controller unter Windows Server.

- [ ] Prozessoren (CPUs): Weisen Sie 4 virtuelle CPUs (vCPUs) zu.

- [ ] Festplatte: Erstellen Sie eine neue virtuelle Festplatte.

- [ ] Größe: Vergeben Sie eine Größe von mindestens 20 GB. Passen Sie diesen Wert bei Bedarf an die geplanten Dienste und Daten an.

- [ ] Schließen Sie den Vorgang mit "Fertigstellen" ab.

**1.3 Erweiterte Einstellungen anpassen:**
_Nach der Erstellung der VM sind noch wichtige ergänzende Einstellungen vorzunehmen:_

- [ ] Wählen Sie die neue VM aus und klicken Sie auf "Ändern".

- [ ] Navigieren Sie zum Menüpunkt "Allgemein" und wählen Sie den Reiter "Eigenschaften".

- [ ] Konfigurieren Sie die folgenden Optionen für eine optimierte Interaktion:

- Gemeinsame Zwischenablage: Stellen Sie diese auf "Bidirektional".

- Drag & Drop: Stellen Sie auch dies auf "Bidirektional".

**1.4 Netzwerkkonfiguration für den Domain Controller :**
__Für_ die Funktion als Domain Controller ist eine spezielle Netzwerkkonfiguration erforderlich, die eine Isolation vom Host-Netzwerk und eine Kommunikation mit anderen VMs ermöglicht._

- [ ] Navigieren Sie in den Einstellungen der VM zu "Netzwerk".

- [ ] Wechseln Sie zum Reiter "Adapter 2".

- [ ] Aktivieren Sie diesen Netzwerkadapter.

- [ ] Wählen Sie unter "Angeschlossen an" die Option "Internes Netzwerk".

- __(Ziel:_ Dies erstellt ein vom Host-Rechner isoliertes Netzwerk, über das die VMs untereinander kommunizieren können.)_

Die virtuelle Maschine ist nun korrekt konfiguriert und kann gestartet werden, um mit der Installation des Windows Server-Betriebssystems zu beginnen.

https://github.com/user-attachments/assets/11ae303d-129b-45ac-a1c3-4598e1cf06e7

### 1.5 Installationsschritte – Windows Server 2022

Nach der Erstellung und Grundkonfiguration der VM starten wir die DC‑VM. Nach dem Bootvorgang wählen wir Sprache, Uhrzeit/Zeitzone und Tastatur und bestätigen mit Weiter. Anschließend wählen wir Windows Server 2022 Standard Evaluation (Desktopdarstellung) und fahren mit Weiter fort. Wir akzeptieren die Lizenzbedingungen und wählen als Installationsart „Benutzerdefiniert: nur Microsoft Server‑Betriebssystem installieren“. Danach wählen wir das Ziellaufwerk aus und klicken auf Weiter.

Die Installation läuft automatisch und kann einige Minuten dauern. Nach dem Neustart legen wir ein Kennwort für das lokale Administratorkonto „Administrator“ fest und schließen mit Fertigstellen ab. Im Anschluss melden wir uns mit Administrator und dem soeben gesetzten Kennwort an.

Für ein verbessertes Benutzererlebnis binden wir in VirtualBox über Geräte → Gasterweiterungen einlegen… die Guest Additions ein. In der VM öffnen wir den Windows‑Explorer, navigieren zu Dieser PC → VirtualBox Guest Additions, starten VBoxWindowsAdditions‑amd64.exe, folgen dem Assistenten und führen nach Abschluss einen Neustart durch.

https://github.com/user-attachments/assets/a5262cab-623c-4e5a-9b09-95a93197e280

 ### 2. Netzwerkkonfiguration (Internet & internes LAN)
_In diesem Schritt richten wir die IP-Adressierung für unseren Server ein. Das System verfügt über zwei Netzwerkkarten:_

- [ ] Internet: für den Zugriff auf das Heimnetz/Internet

- [ ] Intern: für das interne Lab-Netz

**2.1 Adapter benennen**

- [ ] Einstellungen öffnen → Netzwerk & Internet → Adapteroptionen ändern.

- [ ] Den Adapter mit Internetzugang in „Internet“ umbenennen (ehem. Ethernet).

- [ ] Den zweiten Adapter in „Intern“ umbenennen (ehem. Ethernet 2).

_Hinweis: Der Adapter „Internet“ erhält in der Regel automatisch per DHCP eine Adresse vom Heimrouter. Hier ist keine manuelle Konfiguration nötig._

**2.2 Statische IP für das interne Netz setzen**

- [ ] Rechtsklick auf „Intern“ → Eigenschaften.

- [ ] Internetprotokoll, Version 4 (TCP/IPv4) markieren → Eigenschaften.

- [ ] Folgende IP-Adresse verwenden auswählen und eintragen:

- IP-Adresse: 172.16.0.1

- Subnetzmaske: 255.255.255.0

- Standardgateway: (leer lassen, falls das interne Netz isoliert ist)

- [ ] Folgende DNS-Serveradressen verwenden → Bevorzugter DNS-Server: 172.16.0.1

- [ ] Mit OK bestätigen und das Fenster schließen.

**2.3 Server umbenennen (für den Domain Controller)**

- [ ] Start → System → Diesen PC umbenennen.

- [ ] Als Namen DC vergeben (für Domain Controller).

- [ ] Mit Weiter bestätigen und den Server neu starten.

_Nach dem Neustart ist der Server für die spätere AD-DS-Rolle vorbereitet (interne, statische IP + eigener DNS-Eintrag)._

https://github.com/user-attachments/assets/a849311b-a363-4e8e-a27d-c5b59f247a13



### 3. Active Directory-Domänendienste (AD DS) installieren

_In diesem Schritt installieren wir die Rolle Active Directory-Domänendienste (AD DS), um anschließend eine neue Domäne zu erstellen._

**3.1 Installation über Server-Manager** 

- [ ] Server-Manager öffnen → Rollen und Features hinzufügen.
Bevor Sie beginnen → Weiter

- [ ] Installationstyp: Rollenbasierte oder feature basierte Installation → Weiter

- [ ] Serverauswahl: Gewünschten Server auswählen (i. d. R. der DC ) → Weiter

- [ ] Serverrollen: Active Directory-Domänendienste aktivieren

- Hinweis bestätigen: Features hinzufügen

- Weiter durch Features und AD DS Infoseite

- [ ] Bestätigung: Installieren

- [ ] Warten, bis die Installation abgeschlossen ist, dann Schließen.

**3.2 Server zum Domänencontroller hochstufen**

- [ ] Im Server-Manager oben auf das Flaggen-Symbol mit gelbem Hinweis klicken.

- [ ] **„Server zu einem Domänencontroller heraufstufen“** auswählen.

- [ ] **Bereitstellungskonfiguration**:

-    Neue Gesamtstruktur hinzufügen.

- Stammdomänenname angeben, z. B. `mydomain.com` (Beispielname).

- [ ] Domänencontroller-Optionen:

- Funktionsebene (Standard belassen).

- DNS-Server installieren aktiviert lassen.

-  Verzeichnisdienst-Wiederherstellungskennwort (DSRM) festlegen (sich).

- [ ] Weitere Optionen/ NetBIOS-Name: Automatisch vorgeschlagenen Namen prüfen.

- [ ] Pfade(Datenbank, Protokolle, SYSVOL): Standardpfade sind für Lab-Umgebungen ausreichend.

- [ ] Überprüfung → Installieren.

_Der Server wird nach der Hochstufung **automatisch neu gestartet**. Danach steht die neue Domäne bereit und der Server fungiert als **Domänencontroller** (inkl. DNS)._

https://github.com/user-attachments/assets/e95bb618-4cc2-4637-bd19-9ee5fd1bdf51

### 4. Domänen-Admin Konto anlegen und anmelden
_Ziel: Nach der AD-DS-Installation mit DOMAIN\Administrator anmelden, ein eigenes Domänen-Admin Konto erstellen, es der Gruppe Domänen-Admins zuweisen und sich damit anmelden._
 
**4.1 Anmeldung als integrierter Administrator**

- [ ] Am DC anmelden: MYDOMAIN\Administrator (oder mydomain.com\Administrator).

- [ ] Nach der DC-Promotion erscheint das Format Domäne\Benutzer auf dem Anmeldebildschirm.

**4.2 Active Directory-Konsole öffnen**

- [ ] Startmenü → Windows-Verwaltungsprogramme → Active Directory-Benutzer und -Computer)

**4.3 OU für Admins erstellen**

- [ ] In ADUC auf die Domäne mydomain.com rechtsklicken → Neu → Organisationseinheit.

- [ ] Name: z. B. Admins.

- [ ] Optional (für Lab): Häkchen „Objekt vor zufälligem Löschen schützen“ entfernen.


**4.4 Dediziertes Admin-Benutzerkonto anlegen**

- [ ] In der OU Admins → Rechtsklick → Neu → Benutzer.

- [ ] Angaben (Beispiel):

- Vollständiger Name: (Bielibig)

- Benutzeranmeldename : a-kmustermann@mydomain.com („a-“ als Präfix für Adminkonten)

- Benutzeranmeldename (Vorgängerversion / sAMAccountName): a-kmustermann

**Kennwort setzen:**

- [ ] Für Labs: „Benutzer muss Kennwort bei der nächsten Anmeldung ändern“ deaktivieren.

- [ ] Für Labs bequem: „Kennwort läuft nie ab“ aktivieren. (In Produktion nicht empfohlen.)

### Fertig stellen.
_Namenskonvention: Häufig: a-<Erstinitial><Nachname> (z. B. a-kmustermann) als sichtbares Admin-Präfix._


**4.5 Konto zu „Domänen-Admins“ hinzufügen**

- [ ] Benutzer rechtsklicken → Eigenschaften →Mitglied von → Hinzufügen…

- [ ] Domänen-Admins eingeben → Namen überprüfen → OK → Übernehmen → OK.


### 4.6 Abmelden und mit neuem Admin Konto anmelden

- [ ] Am DC Abmelden.

- [ ] Strg+Alt+Entf → Anderer Benutzer.

- [ ] Anmeldung als:

- Benutzername

- Kennwort eingeben → Anmelden.

https://github.com/user-attachments/assets/e429c539-4f5e-4176-8097-96a9d96295c0
 

### 5. RRAS/NAT einrichten (Remotezugriff + NAT) für Internetzugang im Lab

_Ziel: Der Domänencontroller fungiert als NAT-Gateway, damit Clients im internen Lab-Netz (z. B. 172.16.0.0/24) über die öffentliche NIC (Heimrouter/DHCP) ins Internet gelangen._
 

**5.1 Rolle installieren** 

- [ ] Server-Manager → Rollen und Features hinzufügen

- [ ] Assistent:

- Bevor Sie beginnen → Weiter

- Installationstyp: Rollenbasierte oder featurebasierte Installation → Weiter

- Serverauswahl: Zielserver wählen → Weiter

- Serverrollen: Remotezugriff aktivieren → Weiter

- Rollendienste: Routing anhaken (weitere Häkchen, die automatisch gesetzt werden, übernehmen) → Weiter

- Bestätigung → Installieren

- [ ] Nach Abschluss Schließen.

https://github.com/user-attachments/assets/dd5d9607-2c7f-4a56-96c4-f0f109524206


### 6. RRAS/NAT konfigurieren (Routing und RAS)

_In diesem Schritt aktivieren und konfigurieren wir **Routing und RAS (RRAS)**, damit interne Clients über den Domänencontroller per **NAT (Network Address Translation)** ins Internet gelangen._

**6.1 Routing und RAS-Konsole öffnen**

- [ ] 1. Server-Manager → Tools → Routing und RAS.

- [ ] 2. In der Konsole den lokalen Server (z. B. `DC`) mit Rechtsklick auswählen.

- [ ] 3. „ Routing und RAS Konfigurieren und Aktivieren“ auswählen.

_Hinweis: In seltenen Fällen lädt die Konsole fehlerhaft und zeigt keine Schnittstellen an. Falls das passiert, Konsole schließen und erneut öffnen. In manchen Fällen hilft auch ein Neustart des Servers._

**6.2 Assistent zur Konfiguration starten**

- [ ] 1. Assistent öffnet sich → Weiter.

- [ ] 2. Netzwerkadressübersetzung (NAT) / Option sinngemäß „NAT aktivieren, sodass interne Clients das Internet über eine einzelne öffentliche Adresse nutzen können“ auswählen.

- [ ] 3. Weiter.


**6.3 Öffentliche Netzwerkschnittstelle auswählen**

_Der Assistent fragt nun, welche Schnittstelle als „öffentliche“ Verbindung ins Internet dienen soll._

- Hier sollte die Netzwerkkarte ausgewählt werden, die wir zuvor z. B. „Internet“ genannt haben (die Karte mit Zugriff auf das Heimnetz/Internet).

- Diese öffentliche Schnittstelle wird für NAT verwendet.

- Die interne Schnittstelle (z. B. **„Intern“**, IP `172.16.0.1`) bleibt das private Netz.

**Vorgehen:**

- [ ] 1. Öffentliche Schnittstelle (z. B. Internet) markieren.

- [ ] 2. Option aktivieren wie „Diese Schnittstelle stellt die Verbindung mit dem Internet her und soll NAT für private Clients bereitstellen“.

- [ ] 3. Weiter→ Fertig stellen.


**6.4 Dienst aktiv**

Nach Abschluss:

- RRAS wird gestartet.

- In der Routing und RAS Konsole sollte der Server jetzt mit einem grünen Symbol angezeigt werden.

- Unter IPv4 → NAT ist die öffentliche Schnittstelle eingetragen, mit einem Pfeilsymbol nach oben (aktiv).

Das bedeutet:

- Der Domänencontroller übersetzt jetzt Verbindungen vom internen Lab-Netz ins Internet.

- Interne Clients können über das Standardgateway `172.16.0.1` ins Internet, obwohl sie selbst nur im privaten Adressraum (z. B. `172.16.0.x/24`) sind.

https://github.com/user-attachments/assets/686456b3-3da3-4fd8-a5d8-b90fc3ca2eff


### 7. DHCP-Server auf dem Domänencontroller einrichten

_Ziel: Windows-10/11-Clients im privaten Lab-Netz automatisch mit IP-Adressen versorgen, sodass sie über NAT/RRAS ins Internet gelangen._

**7.1 Rolle „DHCP-Server“ installieren** 

- [ ] Server-Manager → Rollen und Features hinzufügen

- [ ] Assistent:

- Bevor Sie beginnen → Weiter
- Installationstyp: Rollenbasiert → Weiter
- Serverauswahl: DC auswählen (z. B. dc.mydomain.com) → Weiter
- Serverrollen: DHCP-Server aktivieren → Features hinzufügen bestätigen → Weiter
- Bestätigung → Installieren

- [ ] Nach Abschluss Schließen.

https://github.com/user-attachments/assets/6144f274-050b-4b3c-948c-f6b9f2321109


### 8. DHCP-Scope anlegen und DHCP-Server autorisieren

_Ziel: Einen IPv4-Scope erstellen, damit Clients im Lab automatisch IP-Adressen sowie Gateway und DNS erhalten._

**8.1 DHCP-Konsole öffnen**

**Server-Manager → Tools → DHCP.**
_Falls der Server rot/„inaktiv“ angezeigt wird, wird die Autorisierung später durchgeführt._

**8.2 Neuen IPv4-Scope erstellen**

- [ ] In der linken Baumstruktur IPv4 → Rechtsklick Neuer Bereich….

- [ ] Name: z. B. LAN 172.16.0.100-200.
- [ ] Startadresse: 172.16.0.100
- Endadresse: 172.16.0.200
- Länge: 24
- Subnetzmaske: 255.255.255.0
- [ ] Ausschlüsse (optional): Weiter →
- [ ] Lease-Dauer: Standard 7 Tage (für Labs ausreichend).
- [ ] Ja, DHCP-Optionen jetzt konfigurieren → Weiter.

**Wichtige Scope-Optionen**

- 003 Router (Gateway): 172.16.0.1 hinzufügen → Weiter
- 006 DNS-Server: 172.16.0.1 (DC/DNS)
- 015 DNS-Domänenname: mydomain.com (anpassen)
- WINS (044/046) i. d. R. nicht erforderlich.

- [ ] Bereich aktivieren → Fertig stellen.


**8.3 DHCP-Server autorisieren (falls nötig)**

- [ ] In der DHCP-Konsole oben den Servernamen rechtsklicken → Autorisieren.
- [ ] Kurz warten, dann Aktualisieren.
- IPv4 sollte grün sein, der Scope sichtbar/aktiv.

https://github.com/user-attachments/assets/28b07ba5-f0ff-48af-9093-244d8dbd8fc1


### 9. AD-Benutzer massenhaft per PowerShell anlegen (aus Textdatei)

Hier habe ich einen Ordner erstellt und „Skript“ benannt. Danach habe ich ein Textdokument mit 100 zufälligen Namen erstellt, die wir mithilfe unseres Skripts als Benutzerkonten anlegen.

Anschließend öffnen wir über die Windows-Startfläche Windows PowerShell ISE und führen es als Administrator aus. Wir fügen unser PowerShell-Skript ein, geben unten Set-ExecutionPolicy Unrestricted ein und navigieren zu dem Speicherort, an dem sich unsere Textdatei mit den Namen befindet. Abschließend drücken wir die grüne Play-Taste und führen das Skript aus – jetzt werden die Benutzerkonten erstellt.

Hinweis: Namen ohne Umlaute verwenden, damit keine falschen Zeichen in den Anmeldenamen erscheinen.

Zur Überprüfung, ob die Konten erstellt wurden: Windows-Startfläche → Windows-Verwaltungsprogramme → Active Directory-Benutzer und -Computer (ADUC) → mydomain.com → Users. Hier sehen wir alle angelegten Konten.

Um ein bestimmtes Konto zu suchen: In ADUC auf mydomain.com rechtsklicken → Suchen… → Name eingeben → Jetzt suchen. Das entsprechende Konto wird angezeigt.

https://github.com/user-attachments/assets/f3a8d1f9-ab86-45de-b99f-bb2f252a3f60


### 10. Windows 11 Enterprise-Client als virtuelle Maschine einrichten

_Im nächsten Schritt erstellen wir unseren Windows-11-Enterprise-Client als virtuelle Maschine und orientieren uns dabei an der Vorgehensweise, die wir bereits für den Domänencontroller verwendet haben._

https://github.com/user-attachments/assets/4d69253c-9816-4aff-90d3-b920c55b1e40


### 11. Windows-Client prüfen, umbenennen und der Domäne beitreten

**11.1 Netzwerk prüfen**

- [ ] Am frisch erstellten Windows-11-Client anmelden.
- [ ] CMD öffnen und ausführen: ipconfig/all

- Ziel der Prüfung:
- Der Client hat eine IP aus dem Lab-Subnetz (z. B. 172.16.0.x).
- Standardgateway zeigt auf den DC/NAT (z. B. 172.16.0.1).
- DNS-Server ist der DC (z. B. 172.16.0.1).
- _Damit stellen wir sicher, dass DHCP, Gateway und DNS korrekt zugewiesen wurden._

**11.2 Systemeigenschaften öffnen und Rechnername anpassen**

- [ ] Windows-Taste → Ausführen → sysdm.cpl → OK.
- [ ] Register Computername → Ändern… → Computername gemäß Namensstandard setzen (z. B. Nutzer-1). 

**11.3 Domänenbeitritt durchführen**

- [ ] Im selben Dialog Mitglied von → Domäne aktivieren.
- [ ] Domänenname eingeben (z. B. mydomain.com) → OK.
- [ ] Anmeldedaten eingeben: Ein Konto mit Join-Berechtigung verwenden
- [ ] Bestätigung abwarten → Neustart wird verlangt → Jetzt neu starten.

**11.4 Funktion nach Neustart verifizieren**

- [ ] Nach dem Reboot am Anmeldebildschirm „Andere Benutzer“ wählen und mit einem Domänenkonto anmelden
- [ ] (z. B. MYDOMAIN\username oder username@mydomain.com).
- [ ] CMD öffnen und testen: ping mydomain.com
- [ ] Zweck: Prüft DNS-Auflösung der Domäne sowie Netzwerk-Erreichbarkeit des Domänencontrollers.

https://github.com/user-attachments/assets/77d59c46-6956-476e-b4d8-59ca3b0a7680


### 12. DHCP-Lease prüfen und Domain-Mitgliedschaft verifizieren

**12.1 DHCP-Lease des Clients anzeigen**

- [ ] Domänencontroller → Server-Manager → Tools → DHCP öffnen.
- [ ] Domänencontroller ausklappen → IPv4 → <Ihr Scope> → Address Leases (Adressleases).
- [ ] Im rechten Fenster sehen Sie die Lease Ihres Windows-Clients:

- IP-Adresse, MAC/Client-ID, Hostname, Lease-Beginn/Ende. 
_Damit ist bestätigt, dass der Client beim DHCP-Server automatisch eine Adresse angefordert und erhalten hat._

**12.2 Domain-Join des Clients in AD prüfen**

- [ ] Start→ Windows-Verwaltungsprogramme→ Active Directory-Benutzer und -Computer (ADUC) öffnen.
- [ ] Domäne ausklappen → Computers.
- [ ] Hier sollte das Computerobjekt des Clients sichtbar sein – dies bestätigt die Mitgliedschaft in der Domäne.

**Angelegte Benutzerkonten sichten**

- [ ] In ADUC zur gewünschten OU (z. B. Users, Admins o. Ä.) navigieren.
- [ ] Die mit dem PowerShell-Skript erstellten Benutzerkonten sind hier gelistet.

https://github.com/user-attachments/assets/3cd8f1f6-97dc-4321-8980-243c73a9131d
