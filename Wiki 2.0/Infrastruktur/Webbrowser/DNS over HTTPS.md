---
aliases:
  - " "
  - DoH
---
DNS over HTTPS (kurz DoH) bezeichnet ein Verfahren, bei dem Anfragen des Domain Name System (DNS) über das verschlüsselte HTTPS geschickt werden. Die Namensauflösung wird damit in denselben Kanal verlegt, über den auch normale Webinhalte übertragen werden. Ziel ist unter anderem, es Dritten zu erschweren, DNS-Anfragen mitzulesen oder zu verändern, etwa im Rahmen eines Man-in-the-Middle-Angriffs.

Standardisiert wurde DoH im Oktober 2018 durch die Internet Engineering Task Force (IETF) als RFC 8484. Es ist damit einer von mehreren Ansätzen, DNS-Anfragen zu verschlüsseln; dazu gehören auch DNS over TLS (DoT) sowie das auf QUIC basierende DNS over QUIC (DoQ). Mit RFC 9230 wurde später Oblivious DNS over HTTPS (ODoH) eingeführt, bei dem ein vorgeschalteter Proxy verhindern soll, dass ein einzelner Server gleichzeitig die IP-Adresse des Clients und den Inhalt seiner DNS-Anfragen kennt.

DNS over HTTPS wird inzwischen von verschiedenen öffentlichen DNS-Resolvern angeboten und von mehreren Webbrowsern und Betriebssystemen unterstützt. Befürworter sehen darin vor allem einen Gewinn für den Schutz der Privatsphäre bei der Namensauflösung, Kritiker verweisen dagegen auf mögliche Nachteile für Netzwerksicherheit, betriebliche Überwachung, Inhaltsfilter und auf eine zunehmende Konzentration von DNS-Diensten bei wenigen großen Anbietern.