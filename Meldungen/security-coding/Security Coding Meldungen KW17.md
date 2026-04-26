# **Strategischer Analysebericht zur Security-Coding-Landschaft: Kalenderwoche 17, 2026**

Der vorliegende Bericht liefert eine aktuelle Analyse der wichtigsten Sicherheitsereignisse der Kalenderwoche 17 des Jahres 2026\. Diese Zusammenfassung wurde speziell für das wöchentliche Reporting im Ordner "Security Coding" in der Drive-Ablage erstellt.

## **Top 10 Meldungen der Kalenderwoche 17**

| Rang | Zusammenfassung | Stichwörter | Quelle |
| :---- | :---- | :---- | :---- |
| 1 | Eine kritische Sicherheitslücke in den SAP-Lösungen Business Planning and Consolidation und Business Warehouse erfordert die Aufmerksamkeit von Systemadmins. Eine unzureichende Berechtigungsprüfung kann zu SQL-Injection führen. \[1\] | SAP, SQL-Injection, Business Warehouse | Security-Insider NEWS |
| 2 | Unsichere Verschlüsselung (CVE-2025-8095) bei OpenEdge ermöglicht Cyberangriffe. Mit OECH1-kodierte Anmeldedaten gelten als kompromittiert, weshalb die Methode ausgetauscht wurde. \[2\] | OpenEdge, Verschlüsselung, CVE-2025-8095 | Security-Insider NEWS |
| 3 | Die CISA meldet die Ausnutzung von 17 und 14 Jahre alten Schwachstellen in EoL-Versionen von Office und Excel (Visual Basic for Applications). Ein Update auf unterstützte Versionen wird dringend empfohlen. \[3\] | Excel, Office, EoL, CISA | Security-Insider NEWS |
| 4 | CISA warnt vor Angriffen auf Microsoft Exchange und Windows CLFS. Es handelt sich um alte Sicherheitslücken, die Remote Code Execution (RCE) und Elevation of Privilege ermöglichen. \[4\] | Microsoft Exchange, Windows CLFS, RCE | Security-Insider NEWS |
| 5 | Eine kritische FortiClient-Schwachstelle (SQL-Injection) in FortiClient EMS von Fortinet wird in Angriffen genutzt. Die CISA drängt auf sofortiges Schließen der Lücke. \[5\] | Fortinet, FortiClient EMS, SQL-Injection | Security-Insider NEWS |
| 6 | Im Chrome Web Store wurden 108 bösartige Erweiterungen hochgeladen, die Google- und Telegram-Daten von über 20.000 Nutzern stehlen. \[1\] | Chrome, Extensions, Datendiebstahl | Security-Insider NEWS |
| 7 | Shiny Hunters erpressen Rockstar Games nach einem Breach bei Snowflake-Zulieferer Anodot, bei dem Drittanbieter-Tokens gestohlen wurden. \[2\] | Shiny Hunters, Snowflake, Supply Chain | Security-Insider NEWS |
| 8 | Ein Datendiebstahl bei der europäischen Gym-Kette Basic-Fit betrifft sensible Informationen und Zahlungsdaten von über einer Million Mitgliedern. \[3\] | Data Breach, Basic-Fit, Zahlungsdaten | Security-Insider NEWS |
| 9 | Ein überprivilegierter KI-Agent, versteckt in einer Pickle-Datei in Google Cloud Vertex AI, kann Remote Code in Kundenprojekten ausführen. \[4\] | Vertex AI, KI-Agent, GCP, RCE | Security-Insider NEWS |
| 10 | Phishing-Angriff auf Booking.com: Unbekannte erbeuten Buchungsdetails (Namen, E-Mails, Telefonnummern) von Nutzern und setzen Buchungs-PINs zurück. \[4\] | Booking.com, Phishing, Datendiebstahl | Security-Insider NEWS |

## **Detaillierte Analyse der Kritischen Lücken**

### **Kritische Schwachstellen in Unternehmenssoftware**

Die KW17 war geprägt von gravierenden Sicherheitslücken in weit verbreiteten Enterprise-Lösungen. Besonders hervorzuheben ist die SQL-Injection-Lücke in den SAP-Lösungen Business Planning and Consolidation sowie Business Warehouse. Durch unzureichende Berechtigungsprüfungen können Angreifer diese Systeme kompromittieren \[1\]. Darüber hinaus drängt die CISA auf das sofortige Schließen einer kritischen SQL-Injection-Schwachstelle in Fortinet's FortiClient EMS, da diese bereits in freier Wildbahn aktiv ausgenutzt wird \[5\].

### **Ausnutzung veralteter Systeme (End-of-Life)**

Ein anhaltendes Problem in der Security-Landschaft ist die Nutzung von End-of-Life-Software. Die CISA meldet, dass zwei 17 und 14 Jahre alte Schwachstellen in veralteten Versionen von Microsoft Office und Excel (speziell im Bereich Visual Basic for Applications) hochgradig für Cyberangriffe anfällig sind und aktiv ausgenutzt werden \[3\]. Zusätzlich warnt die CISA vor der Ausnutzung von älteren Lücken in Microsoft Exchange und dem Windows Common Log File System (CLFS), welche Remote Code Execution und die Ausweitung von Privilegien ermöglichen \[4\].

### **Künstliche Intelligenz als Angriffsvektor**

Der Missbrauch von KI-Technologien nimmt weiter zu. Unit42 entdeckte in der Google Cloud Platform (GCP) Vertex AI eine gefährliche Schwachstelle. Über eine manipulierte Pickle-Datei konnte ein überprivilegierter KI-Agent eingeschleust werden. Dieser ist in der Lage, Service-Accounts zu nutzen, um Remote Code auszuführen und Daten aus fremden Kundenprojekten zu stehlen \[4\].

### **Supply-Chain-Angriffe und Datendiebstahl**

In KW17 setzten sich die massiven Supply-Chain-Probleme fort. Die Hackergruppe Shiny Hunters kompromittierte Rockstar Games, nachdem sie Drittanbieter-Tokens durch einen Angriff auf den Snowflake-Zulieferer Anodot erbeutet hatte \[2\]. Zeitgleich sorgten gezielte Phishing-Kampagnen bei Booking.com dafür, dass Reise- und Kontaktdaten abflossen \[4\]. Auch die Fitnesskette Basic-Fit wurde Opfer eines massiven Vorfalls, bei dem die Daten von über einer Million Kunden, einschließlich Bankverbindungen, gestohlen wurden \[3\].

## **Referenzen**

1. Newsletter "Kritische SQL-Injection bei SAP" (Security-Insider NEWS, 24.04.2026)  
2. Newsletter "Unsichere Verschlüsselung bei OpenEdge ermöglicht Cyberangriffe" (Security-Insider NEWS, 23.04.2026)  
3. Newsletter "17 Jahre alte Excel-Schwachstelle wird sehr wahrscheinlich ausgenutzt" (Security-Insider NEWS, 22.04.2026)  
4. Newsletter "CISA warnt vor Angriffen auf Microsoft Exchange und Windows CLFS" (Security-Insider NEWS, 21.04.2026)  
5. Newsletter "Kritische FortiClient-Schwachstelle wird in Angriffen genutzt" (Security-Insider NEWS, 20.04.2026)