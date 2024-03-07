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

Dieser Schritt kann übersprungen werden, wenn keine Zuordnung zu den EÜR-Posten erfolgen soll. In dem Fall darf im Import die Spalte "Benutzerdefinierte Kontengruppe" nicht zugeordnet werden!

Zunächst müssen die für die Einnahmen-Überschuss-Rechnung genutzten benutzerdefinierten Kontengruppen hinzugefügt werden. Diese Zuordnung ist aus dem Datev Kontenrahmen PDF aus der Spalte "Ergebnisposten" übernommen. Leider habe ich keine Funktion für einen automatischen Import dieser Kontengruppen gefunden. Deshalb müssen diese von manuell zu Dolibarr hinzugefügt werden. Hinzugefügt werden Kontengruppen in Dolibarr unter Buchhaltung - Einstellungen - Benutzerdefinierte Kontengruppen.

Die benötigten Kontengruppen befinden sich in der Datei [kontengruppen.csv](kontengruppen.csv) oder in der Excel-Tabelle im zweiten Tab.

_HINWEIS: Die Reihenfolge der Kontengruppen bzw. der Position ist wichtig! Die Formeln funktionieren nur, wenn sie in der Position hinter den zu berechnende Feldern stehen_

### 2. Import in Dolibarr

Unter Buchhaltung - Einstellungen - Allgemein sollte die Funktion "Beibehalten der Nullen am Ende eines Buchungskontos ("1200")" aktiviert werden.

Der Import des Kontenplan funktioniert in Dolibarr über Tools - Import-Assistent - Neuer Import - Buchhaltung (erweitert) / Kontenplan. Dort unter csv die Datei [dolibarr-skr49.csv](dolibarr-skr49.csv) hochladen. Im 4. Schritt müssen jeweils die Spalten der Tabelle den (in der Frontend-Übersetzung) gleichnamigen Datenbankfeldern zugeordnet werden. Die Spalte "EUeR Zuordung" braucht keine Zuordnung. Wenn das Hinzufügen der benutzerdefinierten Kontengruppen übersprungen wurden, darf diese Spalte nicht zugeordnet werden, sonst kommt es zu einem Problem beim Import.

Wenn du den Kontenplan erneut importierst, wähle in Schritt 5 einen oder mehrere Werte in der Option "Schlüssel (Spalte), der zum Aktualisieren der vorhandenen Daten von verwendet wird" aus.

Standardmäßig sind alle Konten deaktiviert! Die benötigten Konten müssen also zuerst aktiviert werden, bevor sie genutzt werden können.

### 3. EÜR Auswertung

Unter Buchhaltung - Berichte - Einnahmen/Ausgaben - Benutzerdefinierte Gruppierung kannst du jetzt eine Auswertung nach den Posten der Einnahmen-Überschuss-Rechnung sehen!

Es gibt leider keine Möglichkeit für den Export dieser Daten, du musst die Ergebnisse also wieder von Hand kopieren, um sie z.B. als PDF weitergeben oder beim Finanzamt einreichen zu können.
