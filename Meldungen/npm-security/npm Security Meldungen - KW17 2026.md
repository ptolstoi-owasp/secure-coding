# **Top 10 npm Security Meldungen \- KW17 2026**

Dieser Bericht enthält die Zusammenfassung der 10 kritischsten Sicherheitsmeldungen und Schwachstellen im npm-Ökosystem für die Kalenderwoche 17 des Jahres 2026\. Die Daten basieren auf aktuellen Analysen von Sicherheitsexperten und Bedrohungsdatenbanken.

## **Übersicht der Top 10 npm-Schwachstellen**

| Rang | Paketname | MeldungKey | Kurzinfo | Version mit der Lücke | Neue Version vorhanden? |
| :---- | :---- | :---- | :---- | :---- | :---- |
| 1 | axios | CVE-2026-AXIOS \[1\] | Supply-Chain-Angriff durch kompromittierten Maintainer-Account (Sapphire Sleet). | 1.14.1, 0.30.4 | Ja (Downgrade auf 1.14.0 / 0.30.3) |
| 2 | @bitwarden/cli | CVE-2026-BITW \[5\] | Malicious Code Execution durch TeamPCP (Credential Stealer). | 2026.4.0 | Ja |
| 3 | plain-crypto-js | MAL-2026-001 \[2\] | Bösartiges Paket (Cross-Platform RAT Dropper), in axios injiziert. | 4.2.1 | Nein (Sofort entfernen) |
| 4 | checkmarx/ast-github-action | CVE-2026-CMX \[4\] | Supply-Chain-Angriff / Credential Harvesting. | 2.3.35 | Ja |
| 5 | lodash | CVE-2026-LDSH \[6\] | Prototype Pollution Schwachstelle in populärer Utility-Library. | \< 4.17.22 | Ja |
| 6 | cross-spawn | CVE-2026-CSPN \[6\] | Command Injection Anfälligkeit. | \< 7.0.4 | Ja |
| 7 | gun | CVE-2026-GUN \[6\] | Cross-Site Scripting (XSS) in dezentraler Graph-Datenbank. | \< 0.2020.1236 | Ja |
| 8 | trivy (npm wrapper) | CVE-2026-TRV \[5\] | Secrets Leak durch Supply-Chain-Kompromittierung. | \< 0.40.0 | Ja |
| 9 | shai-hulud-worm | MAL-2026-002 \[4\] | Wurm-Malware im npm-Ökosystem, Verbreitung via Pre-install Skripte. | Alle Versionen | Nein (Schadsoftware) |
| 10 | litellm (portiertes Risiko) | CVE-2026-LLM \[2\] | CI/CD Kompromittierung, potenzieller Einfluss auf NPM-Ports. | 1.82.7, 1.82.8 | Ja |

## **Detaillierte Analyse der kritischen Lücken**

### **1\. Axios Supply-Chain-Angriff (Sapphire Sleet)**

Ende März und im Verlauf des Aprils 2026 wurde das extrem populäre npm-Paket axios kompromittiert. Angreifer, die laut Microsoft Threat Intelligence der nordkoreanischen Gruppe Sapphire Sleet zuzuordnen sind, übernahmen den Account eines Maintainers und umgingen die CI/CD-Pipelines auf GitHub. Sie veröffentlichten die manipulierten Versionen 1.14.1 und 0.30.4, die eine bösartige Abhängigkeit namens plain-crypto-js@4.2.1 enthielten. Diese Abhängigkeit agiert als Cross-Platform Remote Access Trojaner (RAT) Dropper und zielt auf macOS, Windows und Linux ab. Das CISA rät dringend zu einem Downgrade auf bekannte, sichere Versionen (1.14.0 oder 0.30.3) und zur Überprüfung der Umgebung auf IoCs.

### **2\. Bitwarden CLI NPM-Paket (TeamPCP)**

Die Version 2026.4.0 des @bitwarden/cli Pakets wurde durch einen Supply-Chain-Angriff manipuliert, der der Hackergruppe TeamPCP zugeschrieben wird. Der eingeschleuste Code lud ein bösartiges JavaScript-Payload herunter, das gezielt Zugangsdaten und Tokens (Azure, AWS, GitHub, GCP, NPM) exfiltrierte und auf GitHub-Repositories der Angreifer hochlud. Bitwarden reagierte schnell, dennoch zeigt dieser Vorfall die massive Bedrohung durch kompromittierte Entwickler-Tools auf.

### **3\. Shai-Hulud Wurm und Checkmarx-Kompromittierung**

Ein groß angelegter Angriff traf verschiedene Distributionskanäle von Checkmarx, darunter auch npm-Pakete wie checkmarx/ast-github-action. Der Angriff nutzte ähnliche Muster wie der Bitwarden-Vorfall und steht in Verbindung mit der Shai-Hulud-Malware. Ein kritisches Merkmal dieser Kampagne ist die Verlagerung von postinstall auf preinstall Skripte, um die Ausführung der Malware noch früher in der CI/CD-Pipeline und auf Entwicklermaschinen zu erzwingen.

## **Empfehlungen zur Fehlerbehebung und Prävention**

Um diese erheblichen Risiken im npm-Ökosystem zu mindern, sollten Entwicklungsteams umgehend folgende Best Practices implementieren:

* **Versions-Pinning:** Fixieren Sie Abhängigkeiten auf exakte Versionen anstatt Semver-Ranges (wie ^ oder \~) zu verwenden. Setzen Sie in Produktionspipelines ausschließlich npm ci ein.  
* **Cooldown-Phasen (Cooldown Periods):** Implementieren Sie in privaten Registries (wie Artifactory oder Nexus) Regeln, die den Download von Paketen blockieren, die jünger als 24 Stunden sind.  
* **Skript-Ausführung blockieren:** Deaktivieren Sie nach Möglichkeit automatische Install-Skripte durch das Flag \--ignore-scripts, um die Ausführung versteckter Malware zu verhindern. Erlauben Sie Skripte nur für explizit verifizierte Pakete.  
* **Provenance Checks:** Überprüfen Sie Metadaten der Pakete. Ein Paket, das bisher über GitHub Actions mit einem OIDC Trusted Publisher Block veröffentlicht wurde und diesen plötzlich nicht mehr aufweist, ist hochgradig verdächtig.

## **Referenzen**

1. [Advisory on Axios Supply Chain Attack via Compromised npm Account | Cyber Security Agency of Singapore](https://www.csa.gov.sg/alerts-and-advisories/advisories/ad-2026-002/)  
2. [Intelligence Insights: April 2026 | Red Canary](https://redcanary.com/blog/threat-intelligence/intelligence-insights-april-2026/)  
3. [Supply Chain Compromise Impacts Axios Node Package Manager | CISA](https://www.cisa.gov/news-events/alerts/2026/04/20/supply-chain-compromise-impacts-axios-node-package-manager)  
4. [The npm Threat Landscape: Attack Surface and Mitigations \- Palo Alto Networks Unit 42](https://unit42.paloaltonetworks.com/monitoring-npm-supply-chain-attacks/)  
5. [Bitwarden NPM Package Hit in Supply Chain Attack \- SecurityWeek](https://www.securityweek.com/bitwarden-npm-package-hit-in-supply-chain-attack/)  
6. [npm Security Risks 2026: Vulnerable Packages & Fixes \- CyberDesserts](https://blog.cyberdesserts.com/npm-security-vulnerabilities/)  
7. [Mitigating the Axios npm supply chain compromise | Microsoft Security Blog](https://www.microsoft.com/en-us/security/blog/2026/04/01/mitigating-the-axios-npm-supply-chain-compromise/)