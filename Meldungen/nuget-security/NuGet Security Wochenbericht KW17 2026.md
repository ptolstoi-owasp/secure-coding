# **NuGet Security Wochenbericht \- KW17 / 2026**

Dieser Bericht enthält eine Zusammenfassung der Top 10 Sicherheitsmeldungen für NuGet-Pakete der Kalenderwoche 17 (April 2026). Nachfolgend finden Sie eine Übersichtstabelle sowie detaillierte Analysen zu den als kritisch eingestuften Schwachstellen.

## **Top 10 NuGet Sicherheitsmeldungen**

| Rang | Paketname | MeldungKey | Kurzinfo | Version mit der Lücke | Neue Version vorhanden   |
| :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | NuGetGallery | CVE-2026-39399 | Arbitrary Blob Overwrite / RCE via .nuspec | Ungepatchte Commits | Ja |
| 2 | Microsoft.Native.Quic.MsQuic.OpenSSL | CVE-2026-32179 | Remote Elevation of Privilege | Vor April 2026 | Ja |
| 3 | HotChocolate.Language | CVE-2026-40324 | Stack Overflow via Deeply Nested GraphQL | \< Aktuelle Version | Ja |
| 4 | Microsoft.AspNetCore.DataProtection | CVE-2026-40372 | ASP.NET Core Elevation of Privilege (Cookie Forgery) | 10.0.0 \- 10.0.6 | Ja (10.0.7) |
| 5 | System.Security.Cryptography.Xml | CVE-2026-26171 | .NET Denial of Service | div. .NET Versionen | Ja |
| 6 | System.Security.Cryptography.Xml | CVE-2026-33116 | .NET Denial of Service Vulnerability | div. .NET Versionen | Ja |
| 7 | OpenTelemetry.Resources.AWS | CVE-2026-41173 | Unbounded HTTP response body reads | Vor April 2026 | Ja |
| 8 | OpenTelemetry.Api | CVE-2026-40894 | Excessive memory allocation in OTel headers | Vor April 2026 | Ja |
| 9 | MailKit | GHSA-9j88-vvj5-vhgr | STARTTLS Response Injection | Vor April 2026 | Ja |
| 10 | Microsoft.NetCore.App.Runtime.linux-arm | CVE-2026-32178 | .NET Spoofing Vulnerability | \< 9.0.10, \< 8.0.21 | Ja |

## **Detaillierte Analyse der kritischen Lücken**

### **1\. NuGetGallery (CVE-2026-39399) \- CVSS 9.6 (Critical)**

**Beschreibung:** NuGetGallery, das Backend-System für nuget.org, ist von einer kritischen Schwachstelle beim Verarbeiten von .nuspec-Dateien innerhalb von NuGet-Paketen betroffen. Ein Angreifer kann eine manipulierte Nuspec-Datei mit bösartigen Metadaten bereitstellen.  
**Auswirkungen:** Dies führt zu "Cross Package Metadata Injection", was potenziell Remote Code Execution (RCE) und das Überschreiben beliebiger Blobs zur Folge haben kann. Die Schwachstelle lässt sich über URI-Fragment-Injection ausnutzen.  
**Handlungsempfehlung:** Das System muss unverzüglich auf den neuesten Patch-Stand aktualisiert werden, der verbesserte Input-Validierungen beinhaltet.

### **2\. Microsoft.Native.Quic.MsQuic.OpenSSL (CVE-2026-32179) \- Critical**

**Beschreibung:** In der MsQuic OpenSSL-Komponente wurde eine schwerwiegende Schwachstelle entdeckt, die eine Remote Elevation of Privilege ermöglicht.  
**Auswirkungen:** Angreifer können über das Netzwerk erhöhte Rechte erlangen, was eine vollständige Kompromittierung des betroffenen Dienstes nach sich ziehen kann.  
**Handlungsempfehlung:** Ein sofortiges Update auf die neueste, von Microsoft bereitgestellte Version des Pakets wird dringend empfohlen.

### **3\. HotChocolate.Language (CVE-2026-40324) \- Critical**

**Beschreibung:** Die GraphQL-Plattform ChilliCream weist eine Schwachstelle im Utf8GraphQLParser auf. Durch extrem tief verschachtelte GraphQL-Dokumente kann ein Stack Overflow provoziert werden.  
**Auswirkungen:** Dies führt zu einem Absturz der Anwendung (Denial of Service) und stellt eine erhebliche Gefahr für die Verfügbarkeit von GraphQL-Endpunkten dar.  
**Handlungsempfehlung:** Aktualisieren Sie das HotChocolate.Language-Paket auf die neueste gepatchte Version, in welcher der Parser gegen extrem tiefe Rekursionen abgesichert ist.

### **4\. Microsoft.AspNetCore.DataProtection (CVE-2026-40372) \- CVSS 9.1 (High)**

**Beschreibung:** Ein Regressionsfehler in den ASP.NET Core Data Protection Paketen (Versionen 10.0.0 bis 10.0.6) führt dazu, dass der MAC (Message Authentication Code) fehlerhaft validiert wird. Dies entspricht einer Art "Padding Oracle Attack".  
**Auswirkungen:** Angreifer können Authentifizierungs-Cookies und andere geschützte Payloads fälschen, was zu einer Elevation of Privilege (EoP) führt. Ein unautorisierter Nutzer könnte sich so als privilegierter Administrator ausgeben.  
**Handlungsempfehlung:** Ein sofortiges Update auf Version 10.0.7 ist zwingend erforderlich. Sofern bereits gefälschte Tokens im Umlauf sind, muss der DataProtection-Schlüsselring (Key Ring) rotiert werden, um alle bestehenden Sessions zu invalidieren.

## **Referenzen**

1. \[1\] CVE-2026-39399: NuGetGallery Arbitrary Blob Overwrite  
2. \[2\] CVE-2026-32179: Microsoft.Native.Quic.MsQuic.OpenSSL  
3. \[3\] CVE-2026-40324: ChilliCream GraphQL Platform Stack Overflow  
4. \[4\] [CVE-2026-40372: Microsoft Security Advisory, ASP.NET Core Elevation of Privilege](https://github.com/dotnet/announcements/issues/395)  
5. \[5\] [CVE-2026-26171: .NET Denial of Service Vulnerability](https://github.com/advisories/GHSA-w3x6-4m5h-cxqf)  
6. \[6\] CVE-2026-33116: .NET Denial of Service Vulnerability  
7. \[7\] CVE-2026-41173: OpenTelemetry.Resources.AWS Unbounded Reads  
8. \[8\] CVE-2026-40894: OpenTelemetry.Api Excessive memory allocation  
9. \[9\] GHSA-9j88-vvj5-vhgr: MailKit STARTTLS Response Injection  
10. \[10\] [CVE-2026-32178: .NET Spoofing Vulnerability](https://github.com/advisories/GHSA-vmwf-m9c5-3jvc)