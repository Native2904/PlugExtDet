# StreamList

**Versteckte NTFS-Datenströme in Total Commander sehen, bearbeiten und aufräumen**

Version 0.4.3 · Dateisystem-Plugin (WFX) für 32 und 64 Bit · Autor: Native2904 · Lizenz: MIT

Diese Anleitung ist zum Durchlesen gedacht. Sie beginnt bei der Frage, was Datenströme überhaupt sind, und führt Schritt für Schritt bis zum Aufräumen tausender Dateien auf einmal. Jedes Kapitel baut auf dem vorigen auf.

---

## 1. Was ist ein Datenstrom?

Eine Datei auf einem NTFS-Laufwerk hat einen Inhalt – den Text eines Briefes, die Bilddaten eines Fotos. Was kaum jemand weiß: Sie kann zusätzlich weitere, unsichtbare Inhalte tragen. Man nennt sie **alternative Datenströme** (englisch *Alternate Data Streams*, kurz ADS) oder einfach **Streams**.

Man kann sich das wie Notizzettel vorstellen, die hinten an eine Datei geheftet sind. Der Explorer zeigt sie nicht an, die Dateigröße enthält sie nicht – aber sie reisen mit der Datei mit, solange sie auf NTFS bleibt.

Höchstwahrscheinlich haben Sie solche Zettel schon zu Tausenden, ohne es zu wissen. Jeder Browser heftet an jeden Download einen Stream namens `Zone.Identifier`. Darin steht, dass die Datei aus dem Internet stammt, und oft auch, von welcher Adresse. Genau deshalb fragt Windows beim Öffnen: *„Diese Datei stammt von einem anderen Computer …“*

Sie können das selbst sichtbar machen. Öffnen Sie eine Eingabeaufforderung in Ihrem Download-Ordner und tippen Sie `dir /r`:

| Größe | Name |
|---:|---|
| 4.858.880 | `alrext.exe` |
| 52 | `alrext.exe:Zone.Identifier:$DATA` |
| 87.552 | `ChmLib.dll` |
| 52 | `ChmLib.dll:Zone.Identifier:$DATA` |

Die Zeilen mit dem Doppelpunkt im Namen sind die Streams. `alrext.exe` hat also einen 52 Byte kleinen Zettel namens `Zone.Identifier`.

Mit StreamList werden diese Zettel in Total Commander zu ganz normalen Einträgen, die Sie ansehen, bearbeiten, anlegen und löschen können.

---

## 2. Installation

Sie brauchen Total Commander ab Version 7.5 und Windows 7 oder neuer. Öffnen Sie das Archiv in Total Commander, bestätigen Sie die Installation, und StreamList erscheint in der **Netzwerkumgebung**.

Everything wird nicht vorausgesetzt. Ohne Everything funktioniert alles in den Kapiteln 3 bis 7. Kapitel 8 zeigt, was zusätzlich möglich wird, wenn Everything 1.5 läuft.

---

## 3. Der erste Blick

Öffnen Sie in der Netzwerkumgebung den Eintrag **StreamList**. Sie sehen Ihre NTFS-Laufwerke und, falls Everything läuft, zwei Listen mit Lupen-Symbol:

| Name | Streams |
|---|---:|
| 🔍 `! Alle Dateien mit Streams` | |
| 🔍 `! Downloads (Zone.Identifier)` | |
| 📁 `C` | ∑ 58.448 |
| 📁 `D` | ∑ 312 |

Die Listen heben wir uns für Kapitel 8 auf. Gehen Sie zunächst mit Enter in ein Laufwerk und dann in einen Ordner, zum Beispiel Ihren Download-Ordner.

---

## 4. Ein Ordner aus der Sicht von StreamList

Ein Ordner sieht in StreamList fast so aus wie im normalen Panel – mit einem wichtigen Unterschied: **Auch Dateien erscheinen als Ordner.** Das ist Absicht, denn jede Datei kann ja Streams enthalten, und mit Enter öffnen Sie diese.

Damit Sie nicht raten müssen, in welche Dateien sich ein Blick lohnt, zeigt StreamList beim Betreten eine eigene Spaltenansicht:

| Name | Größe | Streams | Inhalt | Herkunft |
|---|---:|---:|---|---|
| 📁 `Tools` | `<DIR>` | ∑ 31 | | |
| 📦 `setup.zip` | 2.418.330 | 1 | Zone.Identifier | https://example.org/… |
| 📄 `notiz.txt` | 1.204 | 2 | Comment, eins | |
| 📄 `bild.png` | 88.112 | | | |

So lesen Sie die Spalten:

- **Streams** zeigt bei einer Datei, wie viele Zettel an ihr hängen. Bei einem Ordner steht ein `∑` davor: Das ist die Zahl der Dateien mit Streams *darunter* – über alle Unterordner hinweg. Diese Summen gibt es nur mit Everything (Kapitel 8).
- **Inhalt** nennt die Namen der Streams.
- **Herkunft** zeigt bei heruntergeladenen Dateien die Adresse, von der sie stammen.

Eine leere Zeile wie bei `bild.png` bedeutet einfach: keine Streams.

---

## 5. In eine Datei hinein

Drücken Sie Enter auf `notiz.txt`. Jetzt sehen Sie nicht mehr Dateien, sondern die Streams dieser einen Datei:

| Name | Größe | Inhalt |
|---|---:|---|
| 📄 `Comment` | 7 | Rechnung |
| 📄 `eins` | 16 | erster Stream |

Die Spalte **Inhalt** zeigt hier die erste Textzeile jedes Streams. Binäre Streams erscheinen als Hex, etwa `4D 5A 90 00 …`.

Ab hier arbeiten Sie mit den gewohnten Total-Commander-Tasten:

| Taste | Wirkung |
|---|---|
| F3 | Stream ansehen |
| F4 | Stream bearbeiten – beim Speichern wird er in die Datei zurückgeschrieben |
| F5 | Streams herauskopieren; sie werden zu normalen Dateien |
| F5 aus dem anderen Panel | normale Dateien hineinkopieren; sie werden zu Streams |
| F7 | neuen, leeren Stream anlegen |
| Umsch+F6 | Stream umbenennen |
| F6 | Streams herausverschieben |
| F8 | Streams löschen |
| Alt+Enter | Infos zum Stream |

Mit Rücktaste kommen Sie wieder in den Ordner. Probieren Sie ruhig alles aus – das nächste Kapitel erklärt, warum dabei nichts kaputtgehen kann.

---

## 6. Was StreamList niemals tut

Ein Werkzeug, das Dateien wie Ordner zeigt, wirft eine berechtigte Frage auf: Was passiert, wenn ich in der Ordneransicht auf eine Datei F8 drücke?

**Nichts.** StreamList zeigt nur einen Hinweis. Die Regeln dahinter:

- **Echte Dateien und Ordner werden nie gelöscht, umbenannt, verschoben oder angelegt.** StreamList ändert ausschließlich Streams.
- **Streams lassen sich nur dort löschen, wo sie ausdrücklich als Streams aufgelistet sind:** in der Stream-Ansicht einer Datei (Kapitel 5) oder in einer Streamliste (Kapitel 8). Ein F8 auf einen ganzen Ordner kann also nicht versehentlich tausende Streams darunter entfernen.
- **Das Datum einer Datei bleibt erhalten**, wenn Sie ihre Streams ändern. Ohne diese Vorsorge würde jede bearbeitete Notiz das Änderungsdatum der Datei verschieben.
- **Schreibgeschützte Dateien** bleiben unangetastet. StreamList sagt Ihnen, warum es nicht geht.

---

## 7. Schneller hinein

Der Weg über die Netzwerkumgebung ist zum Kennenlernen gut, im Alltag aber umständlich. Mit zwei eigenen Befehlen springen Sie direkt hinein. Tragen Sie sie in die Datei `usercmd.ini` ein (sie liegt neben Ihrer `wincmd.ini`):

```ini
[em_StreamList]
cmd=cd
param=\\\StreamList\%P%N

[em_StreamListDir]
cmd=cd
param=\\\StreamList\%P
```

Legen Sie dann unter *Konfiguration → Einstellungen → Diverses* je einen Hotkey darauf, oder setzen Sie die Befehle auf Buttons.

| Befehl | Im normalen Panel ausgelöst auf … | Ergebnis |
|---|---|---|
| `em_StreamList` | einer Datei | Sie landen direkt in ihren Streams |
| `em_StreamListDir` | irgendeiner Stelle | Sie landen im aktuellen Ordner, aus Sicht von StreamList |

Benutzen Sie die Befehle im normalen Panel. Innerhalb von StreamList genügt Enter.

---

## 8. Mit Everything: die ganze Platte auf einen Blick

Bis hierher arbeitet StreamList Ordner für Ordner. Mit **Everything 1.5** von voidtools kommt der Überblick über die ganze Festplatte dazu – in wenigen Sekunden.

### Einmalige Vorbereitung

Everything muss die Streams in seinen Index aufnehmen. Fügen Sie in Everything sinngemäß unter *Extras → Optionen → Indizes → Eigenschaften* diese beiden Eigenschaften hinzu:

- **Namen alternativer Datenströme**
- **Anzahl alternativer Datenströme**

Lassen Sie Everything danach einmal fertig indizieren. Fehlt diese Vorbereitung, meldet sich StreamList mit genau dieser Anleitung, statt minutenlang zu suchen.

### Die Summen in der Spalte Streams

Ab jetzt erscheinen die `∑`-Werte aus Kapitel 4. Sie verraten, in welche Richtung es sich zu klicken lohnt:

| Name | Streams |
|---|---:|
| 📁 `Users` | ∑ 32.192 |
| 📁 `mingw64` | ∑ 23.044 |
| 📁 `tcmd` | ∑ 961 |
| 📁 `Windows` | ∑ 46 |
| 📁 `Program Files` | |

Ein großer Wert bei einem entpackten Werkzeug wie `mingw64` ist typisch: Windows vererbt die Download-Markierung eines ZIP-Archivs beim Entpacken an jede einzelne Datei darin.

### Die Listen im Hauptverzeichnis

Jetzt werden die beiden Lupen-Einträge aus Kapitel 3 interessant. Sie zeigen Treffer der ganzen Platte in einer flachen Liste – ohne Ordner-Wanderung:

**`! Alle Dateien mit Streams`** listet jede Datei, die mindestens einen Stream trägt. Enter führt wie gewohnt in ihre Streams.

**`! Downloads (Zone.Identifier)`** ist anders gebaut. Hier *ist* jeder Eintrag der `Zone.Identifier` einer Datei:

| Name | Inhalt | Ordner |
|---|---|---|
| 📄 `__init__.py` | …\winlibs.zip | C:\mingw64\lib\python3.9\asyncio |
| 📄 `__init__ [2].py` | …\winlibs.zip | C:\mingw64\lib\python3.9\collections |
| 📦 `setup.zip` | https://example.org/… | C:\Users\…\Downloads |

Die Spalte **Ordner** zeigt, wo die Datei liegt. Gleichnamige Dateien aus verschiedenen Ordnern bekommen einen Zusatz wie `[2]`.

### Download-Markierungen aufräumen

Weil jeder Eintrag in dieser Liste ein Stream ist, wirken die Tasten direkt: F3 zeigt die Markierung, und **F8 entfernt sie** – auch für hunderte markierte Einträge auf einmal, mit Total Commanders üblicher Rückfrage und Fortschrittsanzeige.

Was genau dabei passiert:

| | vorher | nachher |
|---|---|---|
| Datei | `alrext.exe` | `alrext.exe` – unverändert, gleiches Datum |
| Stream | `alrext.exe:Zone.Identifier` | entfernt |
| Windows | warnt beim Öffnen | warnt nicht mehr |

Überlegen Sie vorher kurz, welche Dateien Sie entsperren: Bei eigenen Werkzeugen ist die Warnung nur lästig, bei Dateien aus fragwürdigen Quellen ist sie erwünscht.

---

## 9. Eigene Listen

Die beiden Listen sind nur Voreinstellungen. In der Datei `StreamList.ini` neben dem Plugin können Sie bis zu zwanzig eigene Suchen festlegen. Jede besteht aus einem Namen, einer Everything-Suche und optional einem Streamnamen:

```ini
[Search1]
Name=! Kommentare (ntfs_diz)
Query=alternate-data-stream-names:comment
Stream=Comment
```

Die Zeile `Stream=` entscheidet über die Art der Liste:

| `Stream=` | Art der Liste | Ein Eintrag ist … | Enter / F8 |
|---|---|---|---|
| leer | Dateiliste | eine Datei | Enter öffnet ihre Streams, F8 ist gesperrt |
| gesetzt | Streamliste | genau dieser Stream der Datei | F3/F4/F8 wirken direkt auf den Stream |

Sobald Sie eine eigene `[Search1]` anlegen, ersetzen Ihre Listen die beiden Voreinstellungen.

---

## 10. Eigene Spaltenansichten

Die Standardansicht genügt für den Alltag. Wer sich eigene Ansichten baut, findet die Felder von StreamList unter dem Plugin-Namen:

| Feld | zeigt … |
|---|---|
| `StreamCount` | Anzahl der Streams, bei Ordnern die ∑-Summe |
| `StreamNames` | Namen der Streams einer Datei |
| `Content` | je nach Ebene das Nützlichste: Namen, erste Zeile oder Herkunft |
| `Origin` | Download-Adresse (HostUrl) |
| `Referrer` | verweisende Seite oder Archiv (ReferrerUrl) |
| `Zone` | Internet, Intranet, Lokal … |
| `Preview` | erste Textzeile eines Streams |
| `Kind` | Text, Binär, Zoneninfo oder Leer |
| `Folder` | Ordner einer Datei in den Everything-Listen |

Auch **Inhalts-Plugins anderer Autoren** funktionieren in der Ordneransicht, etwa ntfs_diz oder xytags. StreamList teilt Total Commander dafür mit, welche echte Datei hinter jedem Eintrag steht.

Haben Sie eine Version vor 0.4.1 benutzt? Dann heißt das Anzahl-Feld in Ihrer alten Ansicht noch `Streams` und bleibt leer. Löschen Sie die Ansicht, oder wählen Sie die Spalte neu.

---

## 11. Zusammenarbeit mit ntfs_diz

Das Plugin ntfs_diz speichert Dateikommentare als Streams – jedes Kommentarfeld in einem eigenen Stream mit dem Feldnamen, also `Comment`, `Comment1` und so weiter. In StreamList sehen Sie diese Kommentare deshalb als ganz normale Streams und können sie mit F3 lesen und mit F4 bearbeiten. Beide Plugins lassen sich problemlos nebeneinander verwenden.

---

## 12. Einstellungen

Alle Einstellungen stehen in `StreamList.ini` neben dem Plugin:

| Abschnitt | Eintrag | Bedeutung |
|---|---|---|
| `[Options]` | `KeepTime=1` | Datum der Datei beim Ändern von Streams erhalten |
| `[Settings]` | `Language=auto` | Sprache von Total Commander, sonst `eng`, `deu`, `rus`, `ukr`, `dan` |
| `[Everything]` | `Timeout=30000` | so lange (ms) höchstens auf Everything warten |
| | `MaxResults=100000` | höchstens so viele Einträge pro Liste |
| | `Exclude=!\$Recycle.Bin\` | an jede Suche angehängt; blendet den Papierkorb aus |
| | `FolderQuery=…` | was für die ∑-Summen gezählt wird; leer schaltet sie ab |
| | `FolderCacheMinutes=5` | so lange werden die Summen wiederverwendet |
| `[Search1]` … `[Search20]` | | eigene Listen (Kapitel 9) |

Ein Tipp zu `FolderQuery`: Wenn Dropbox an jede synchronisierte Datei einen eigenen Stream hängt, übertönen diese alles andere. Mit `FolderQuery=alternate-data-stream-names:zone.identifier` zählen die Summen nur noch Download-Markierungen.

---

## 13. Gut zu wissen

- Streams gehen verloren, wenn eine Datei auf FAT32 oder exFAT kopiert, per E-Mail verschickt oder zu vielen Cloud-Diensten hochgeladen wird.
- Kopiert Total Commander keine Streams mit, hilft ein Blick auf die Option `CopyStreams` in der Hilfe von Total Commander.
- Netz- und CD-Laufwerke erscheinen nicht im Hauptverzeichnis, lassen sich aber über `em_StreamListDir` öffnen.
- Streams von Ordnern selbst werden noch nicht angezeigt.
- In Streamlisten steht bei der Größe 0, weil Everything nur die Größe der Datei liefert, nicht die des Streams.
- Die ∑-Summen kommen aus Everything, die Zahl bei einer einzelnen Datei liest StreamList direkt von der Platte. Direkt nach einer Änderung können beide kurz voneinander abweichen.

---

## 14. Wenn etwas nicht stimmt

Für die Fehlersuche gibt es eine **Debug-Version** des Plugins. Sie schreibt eine Datei `StreamList_debug.log` neben das Plugin. Die erste Zeile verrät sofort, welche Plugin-Datei und welche Einstellungsdatei Total Commander tatsächlich verwendet:

```text
=== DEBUG-Build geladen: C:\…\StreamList.wfx64 | INI: C:\…\StreamList.ini (vorhanden) | Log=1 ===
```

Schicken Sie dieses Log zusammen mit einer kurzen Beschreibung über das Total-Commander-Forum oder GitHub. Die normale Version enthält keinerlei Log-Code und bleibt dadurch schlank.

---

## 15. Übersetzungen

`StreamList.lng` enthält Englisch, Deutsch, Russisch, Ukrainisch und Dänisch. Weitere Sprachen sind willkommen – bitte über das Total-Commander-Forum oder GitHub.

---

## 16. Versionen

| Version | Änderungen |
|---|---|
| 0.4.3 | Debug- und Release-Version getrennt; Debug-Version meldet geladene Dateien |
| 0.4.0 | ∑-Summen für Ordner und Laufwerke; Feld `StreamCount` |
| 0.3.0 | Everything-Listen für die ganze Platte; Streamlisten mit direktem F3/F4/F8; Spalte Ordner |
| 0.2.0 | eigene Spalten und Standardansicht; Inhalts-Plugins anderer Autoren; Symbol |
| 0.1.1 | Hotkey funktioniert auch innerhalb des Plugins |
| 0.1.0 | erste Version: Streams auflisten, ansehen, bearbeiten, anlegen, umbenennen, löschen |
