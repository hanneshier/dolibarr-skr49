# dolibarr-kontenplan

Deutscher SKR49 Kontenplan inkl. EÜR-Auswertung für den Import in Dolibarr.

**WICHTIG:** Die Zuordnung für die Kontengruppen ist noch nicht für alle Konten abgeschlossen! Dazu mehr unter ["Kontengruppen zuordnen"](#kontengruppen-zuordnen).

Siehe die [Diskussion](https://forum.dolibarr.de/forum/t/dolibarr-als-vereinsverwaltung/6548) im deutschen Dolibarr Forum für mehr Infos zum Kontenplan und Dolibarr als Vereinsverwaltung.

## How to

Der [Kontenplan](dolibarr-skr49.csv) muss in Dolibarr importiert werden. Außerdem müssen von Hand die [benutzerdefiniert Kontengruppen](kontengruppen.csv) angelegt werden.

### 0. Kontengruppen den EÜR Posten zuordnen

Es sind bis jetzt noch nicht alle Konten den entsprechenden Posten der Einnahmen-Überschuss-Rechnung zugeordnet. Die Zuordnung findet sich im öffentlich zugänglichen Datev-PDF vor jedem Konto. Die manuelle übertragung in den Kontenplan kann auf zwei Arten passieren:

- in der Excel-Tabelle können in der Spalte "EUeR Zuordung" aus dem Dropdown die EÜR Posten ausgewählt werden. Dann wird jeweils die richtige Referenz in die Spalte "benutzerdefinierte Kontengruppe" eingetragen. Die Excel Tabelle muss als csv exportiert werden und kann anschließend in Dolibarr importiert werden.
- Die Zuordnung kann auch nach und beim Aktivieren neuer Konten in Dolibarr erfolgen. Dafür beim Aktivieren neuer Konten unter Buchhaltung - Einstellungen - Kontenplan das übergeordnete Konto hinzufügen. Wenn dies vergessen wird, ist die EÜR Auswertung nicht korrekt!

Und bitte teilt doch eure Zuordnungen wieder als pull-request auf Github! Oder als mail an hannes at innwerk dot org, dann stelle ich die bei Github ein. So können wir die Zuordnung als Community vervollständigen.

### 1. (Optional) benutzerdefinierte Kontengruppen hinzufügen

Dieser Schritt kann übersprungen werden, wenn keine Zuordnung zu den EÜR-Posten erfolgen soll. In dem Fall darf im Import des Kontenplans die Spalte "Benutzerdefinierte Kontengruppe" nicht zugeordnet werden!

Zunächst müssen die für die Einnahmen-Überschuss-Rechnung genutzten benutzerdefinierten Kontengruppen hinzugefügt werden. Diese Zuordnung ist aus dem Datev Kontenrahmen PDF aus der Spalte "Ergebnisposten" übernommen.

Leider steht in Dolibarr keine Funktion für einen automatischen Import dieser Kontengruppen bereit. Deshalb müssen diese entweder manuell zu Dolibarr hinzugefügt werden oder alternativ per Import direkt in die zugrundeliegende Datenbank geschrieben werden.

#### A) Manuelle Einträge per Dolibarr-Webinterface
Hinzugefügt werden Kontengruppen in Dolibarr unter Buchhaltung - Einstellungen - Benutzerdefinierte Gruppen.
Die benötigten Kontengruppen befinden sich in der Datei [kontengruppen.csv](kontengruppen.csv) oder in der Excel-Tabelle im zweiten Tab.

#### B) CSV-Import in die Datenbank
Per phpMyAdmin oder einem zu eurer Umgebung passenden DB-Management wird die Tabelle *"llx_c_accounting_category"* geöffnet.  
Per "**Importieren**"-Funktion wird die Datei [dolibarr-skr49_kontengruppen.csv](dolibarr-skr49_kontengruppen.csv) ausgewählt. Um die Kopfzeile zu überspringen, wird bei "Diese Anzahl Abfragen (für SQL) überspringen, beginnend von der ersten:" der Wert auf **"1"** gesetzt. Die Spalten sind getrennt mit **";"**.

_HINWEIS: Die Reihenfolge der Kontengruppen bzw. der Position ist wichtig! Die Formeln funktionieren nur, wenn sie in der Position hinter den zu berechnende Feldern stehen._

### 2. Import in Dolibarr

Unter Buchhaltung - Einstellungen - Allgemein sollte die Funktion "Beibehalten der Nullen am Ende eines Buchungskontos ("1200")" aktiviert werden.

Der Import des Kontenplan funktioniert in Dolibarr über Tools - Import-Assistent - Neuer Import - Buchhaltung (erweitert) / Kontenplan. Wenn die Option bzw. die Verknüpfung auf ein _"Übergeordnetes Konto"_ genutzt werden soll, ist ein zweistufiger Import notwendig.
Im Schritt 2 "csv" wählen und unter Schritt 3 die Datei [dolibarr-skr49.csv](dolibarr-skr49.csv) hochladen. Im 4. Schritt müssen jeweils die Spalten der Tabelle den (in der Frontend-Übersetzung) gleichnamigen Datenbankfeldern zugeordnet werden:
* Die Spalte "EUeR Zuordung" braucht keine Zuordnung.
* Wenn das Hinzufügen der benutzerdefinierten Kontengruppen übersprungen wurde, darf diese Spalte nicht zugeordnet werden, sonst kommt es zu einem Problem beim Import.
* Beim **erstmaligen** Import darf die Spalte "Übergeordnetes Konto" **nicht** zugeordnet werden. (Ansonsten Fehlermeldung: "Das Feld D : ’ xxxx ’ ist keine AccountingAccount Referenz")

Um die Verknüpfungen der Konten zu "Übergeordneten Konten" herzustellen, ist ein zweiter identischer Importvorgang durchzuführen, bei dem schließlich die Zuordnung der Spalte "Übergeordnetes Konto" durchgeführt wird.
Wenn du den Kontenplan erneut importierst, wähle in Schritt 5 einen oder mehrere Werte in der Option "Schlüssel (Spalte), der zum Aktualisieren der vorhandenen Daten von verwendet wird" aus. Das sind üblicher Weise „Kontenplan“ und „Buchungskonto“.

Standardmäßig sind alle Konten deaktiviert! Die benötigten Konten müssen also zuerst aktiviert werden, bevor sie genutzt werden können.

### 3. EÜR Auswertung

Unter Buchhaltung - Berichte - Einnahmen/Ausgaben - Benutzerdefinierte Gruppierung kannst du jetzt eine Auswertung nach den Posten der Einnahmen-Überschuss-Rechnung sehen!

Es gibt leider keine Möglichkeit für den Export dieser Daten, du musst die Ergebnisse also wieder von Hand kopieren, um sie z.B. als PDF weitergeben oder beim Finanzamt einreichen zu können.
