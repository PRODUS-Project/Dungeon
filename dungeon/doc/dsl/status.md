---
title: "Ziele und Zustand der aktuellen DungeonDSL-Implementierung"
---

Dieses Dokument dokumentiert den aktuellen Zustand der DungeonDSL-Implementierung, also alle
geplanten Features und deren aktueller Realisierungsstand.

Mögliche Zustände:

- geplant, kein klares Konzept vorhanden: 💭
- geplant, Konzept vorhanden: 💡
- implementiert, nicht getestet ☑
- implementiert, getestet: ✅

## High Level Konzepte

| Feature                   | Was und warum?                                                                                                                                                                                                                                                                                                          | Zustand                           |
|---------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------|
| Taskdefinition            | Die zentrale Definition für Aufgaben im Dungeon und wird vermutlich für die meisten Lehrpersonen das sein, was sie am häufigsten erstellen und verwenden                                                                                                                                                                | 💡                                |
| Level-Konfiguration       | Die primäre Verbindung zwischen der DungeonDSL-Datei und dem Dungeon; konfiguriert, welche Aufgaben in einem Level dargestellt werden                                                                                                                                                                                   | 💡                                |
| Task-Organisation         | Bietet die Möglichkeit, mehrere Aufgaben in Beziehung zu setzen, um sequentielle und parallele Aufgaben und Aufgabenverschachtelung zu realisieren; wird höchstwahrscheinlich auf Petri-Netzen basieren                                                                                                                 | 💡                                |
| Level-Organisation        | Ein ähnliches Konzept wie für Task-Organisation, allerdings auf Level-Ebene; hiermit soll konfiguriert werden, welche Aufgaben / Level geladen werden, wenn ein Level abgeschlossen wurde                                                                                                                               | 💡                                |
| Entitätstyp-Definition    | Definition von Entitätstypen (benannte Zusammenstellung aus mehreren Komponenten)                                                                                                                                                                                                                                       | ✅ (als `entity_type` Definition) |
| Entitätstyp-Konfiguration | Anpassung der Werte der Komponenten einer Entitätstyp-Definition, um aus einem Entitätstypen konkrete Ausprägungen zu erstellen. Es ist bereits möglich, aus `entity_type` Definitionen konkrete Instanzen im Dungeon zu erzeugen, die für alle nicht vorgegebenen Werte in den Komponenten die Default-Werte verwenden | 💡                                |
| Event-Handler Funktion    | Bietet die Möglichkeit, auf bestimmte Events aus dem Dungeon-Kontext oder dem Kontext einer einzelnen Entität zu reagieren. Wichtig, um (aufgabenbezogenes) Verhalten zu definieren, was von dem Default-Verhalten abweicht.                                                                                            | 💡                                |
| Task-Builder Methode      | Methoden, die vom Dungeon aufgerufen werden, um eine Taskdefinition in ein konkretes Szenario zu übersetzen. Erzeugen eine Menge Entitäten, definieren und verknüpfen deren Event-Handler Methoden und geben sie an den Dungeon zurück                                                                                  | 💡                                |
| Bewertungskonfiguration   | Die Bewertung einer Aufgabe soll über die DSL konfigurierbar sein, um bspw. festzulegen, wann und welche Daten als Antwort für eine Aufgabe geloggt werden, wie Fehlversuche in die Bewertung eingehen, etc.                                                                                                            | 💭                                |

## DungeonDSL “Ökosystem”

| Feature                            | Was und warum?                                                                                                                                                                                                                                                                                                   | Zustand |
|------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|
| Typechecking                       | Typechecking ist essentiell, um die richtige Verwendung aller Datentypen in der DungeonDSL sicherzustellen                                                                                                                                                                                                       | 💡      |
| Typebuilding                       | Bspw. die Komponentendefinitionen auf der Java-Seite des Dungeons müssen der DSL verfügbar gemacht werden, damit in der DSL Entitätsdefinitionen mit Komponenten definiert werden könenn.                                                                                                                        | ✅      |
| Error-Handling/-Recovery           | Fehler in der Nutzung der DSL werden passieren. Ein konkretes Konzept für die Behandlung dieser Fehler ist daher erforderlich.                                                                                                                                                                                   | 💭      |
| Error-Messages                     | Zu der Fehlerbehandlung gehört auch die Kommunikation der Fehler an die Nutzenden, damit diese eine Chance haben, die Fehler zu beheben und zu verstehen. Dafür muss ein Kommunikationskanal festgelegt werden und festgelegt werden, wie die Fehlermeldungen aussehen sollen, bzw. welchen Inhalt sie brauchen. | 💭      |
| Funktionsschnittstelle zum Dungeon | Event-Handler Funktionen setzen eine Kommunikation zwischen DSL-Interpreter und Dungeon voraus; die sollte klar definiert werden.                                                                                                                                                                                | 💡      |

## Sprachkonzepte

| Feature                                           | Was und warum?                                                                                                                                                                                                                                                                                               | Zustand |
|---------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|
| Property-Bag für Entitätsdefinitionen             | Es kann hilfreich sein, wenn einer Entitätsdefinition Eigenschaften zugewiesen werden können, die unabhängig von den Komponenten sind. So könnte bspw. die Verknüpfung einer Entität mit einer Aufgabe realisiert werden.                                                                                    | 💡      |
| Funktionsdefinitionen                             | Funktionen lagern Logik aus, nehmen Parameter an und geben einen Rückgabewert zurück. Wichtig für Event-Handler Methoden                                                                                                                                                                                     | ☑       |
| Funktionsaufrufe                                  | Funktionen müssen aufgerufen werden können                                                                                                                                                                                                                                                                   | ✅      |
| Handling von Funktions-Rückgabewert               | Es soll möglich sein, aus einer Funktion einen Wert zurückzugeben.                                                                                                                                                                                                                                           | 💡      |
| Funktionsreferenzen                               | Es soll möglich sein, Funktionsreferenzen als Event-Handler in Entitäten zu verknüpfen.                                                                                                                                                                                                                      | 💭      |
| Variablendefinitionen                             | Um eine gewisse Flexibilität bei der Implementierung von Logik in Funktionen zu geben, sollen Variablen erstellt werden können, sowohl von primitiven, als auch von komplexen Datentypen.                                                                                                                    | 💡      |
| Objektdefinitionen                                | Definition eines Objekts eines komplexen Datentyps                                                                                                                                                                                                                                                           | ✅      |
| Member-Zugriff für Objekte mit komplexem Datentyp | Definition eines Objekts                                                                                                                                                                                                                                                                                     | ✅      |
| Inline Objektdefinitionen                         | Beim einer Eigenschaftszuweisung soll es möglich sein, direkt in der Zeile der Zuweisung auch das zuzuweisende Objekt zu erstellen                                                                                                                                                                           | ☑       |
| Single-Argument Type Adapter                      | Datentypen, die per DSL instanziierbar sein sollen, die allerdings nicht mit den normalen [Typebuilding](typebuilding.md) Mechanismen (`@DSLType`, `@DSLTypeMember`), müssen adaptiert werden. Single-Argument Type Adapter bezieht sich auf die Datentypen, die mit einem Parameter erstellt werden können. | ✅      |
| Aggregate Type Adapter                            | Wie Single-Argument Type Adapter, nur für komplexe Datentypen, die mehr als einen Parameter zur Erstellung benötigen                                                                                                                                                                                         | ☑       |
| Graphdefinition per dot                           | Graphen können genutzt werden, um die Generierung des Dungeons zu beeinflussen, daher sollten sie per DSL definierbar sein                                                                                                                                                                                   | ✅      |
| Petri-Netz                                        | Die Task-Organisation soll per Petri-Netzen umgesetzt werden, daher muss die DSL die Möglichkeit bieten, Petri-Netze darzustellen. Ob und wie dies durch eine dot-Definition erreicht werden kann, muss noch erarbeitet werden.                                                                              | 💭      |
| Attributierung von dot Knoten und Kanten          | Perspektivisch könnten Attribute von Knoten und Kanten in einer dot-Definition genutzt werden, um eine Beziehung zu Taskdefinitionen herzustellen, daher sollte unsere dot-Umgebung die Attributierung von Knoten und Kanten unterstützen                                                                    | 💭      |
| Objekt-Kappselung (Object encapsulation)          | Der DSL-Interpreter muss während der Laufzeit auf Objekte des Dungeons zugreifen (bspw. in Event-Handler Funktionen). Daher müssen die Java-Objekte gekappselt werden, sodass sie im DSL-Kontext verwendbar sind.                                                                                            | ☑       |
| Include-Mechanismus für Entitätsdefinitionen      | Es soll eine Standardbibliothek geben, in der Entitätsdefinition vorhanden sind. Diese müssen in eine DungeonDSL Beschreibung inkludiert werden können.                                                                                                                                                      | 💭      |
| Include-Mechanismus für Funktionsdefinitionen     | Es soll eine Standardbibliothek geben, in der Funktionsdefinitionen vorhanden sind. Diese müssen in eine DungeonDSL Beschreibung inkludiert werden können.                                                                                                                                                   | 💭      |
| Enum-Variant Binding                              | Einer Eigenschaft eines Objekts sollte ein Enum-Variante zugewiesen werden können, bspw. für die Wahl eines [Aufgabentyps](../task_concept.md#aufgabentypen) könnte das nützlich sein.                                                                                                                       | 💭      |
| Arrays/Listen                                     | Task-Builder Methoden sollen eine Menge an Entitäten an den Dungeon zurückgeben, daher muss die DSL in der Lage sein, Mengen von Objekten abzubilden.                                                                                                                                                        | 💭      |
| Kontrollflussmechanismen                          | Um in Event-Handlern flexibler auf bestimmte Ereignisse reagieren zu können, sollen Kontrollflussmechanismen (if,else,while) umgesetzt werden                                                                                                                                                                | 💭      |