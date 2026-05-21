Cloud
- Gründe für Cloud
- Gründe gegen Cloud
- Unterschiede Cloud VS On-Premise (Datacenter)
- Was macht eine Cloud Native Anwendung aus

Container & Images
- Idee für Container & Images Konzept (Warum?)
	+ "Runs on my Machine" Image liefert Umgebung für Anwendung
	+ Reproduzierbarkeit
	+ Kapselung von Arbeitslasten
	+ Stärkere Abtrennung von Umgebungen (Sicherheit, Stabilität)
	+ Schnellere Entwicklung/Deployments
- Herausforderungen beim Erstellen eines Image (was gilt es zu beachten)
	+ Größe des Image (bspw. Doku und ungenutzt Software entfernen)
	+ Zugriff auf außenliegende Inhalte (bspw. Dateisystem)
- Wie können Container verteilt werden?
	+ Orchestrierungstools wie [[Kubernetes]], Docker Swarm
	+ Verwaltungstools wie Docker Compose
- Container-Runtimes 
	+ cri-o, containerd, docker

CI/CD
- Warum CI/CD?
	+ Rapid Deployment
	+ Fail Fast, Fail Often
	+ Kurzer Feedback-Loop durch häufige Deployments
- Was kann Teil einer Pipeline sein?
	+ Syntax-Check => Schnelles Erkennen von Fehlern
	+ Linting => Frühes Erkennen von Anti-Pattern
	+ Checks im Security Kontext (Passwort-Leaks, bekannte Vulnerabilities)
	+ Build
	+ Tests (Statische Tests, Unit-Test)
	+ Deployment

Infrastructure-As-Code / Konfigurationsverwaltung / GitOps
- Warum Verwaltung von Konfiguration mit Tools und Speicherung in git?
	+ Reproduzierbarkeit
	+ Einfachere Zusammenarbeit
	+ Dokumentation
	+ Einfachere Verteilung und Wiederverwendung von Konfiguration