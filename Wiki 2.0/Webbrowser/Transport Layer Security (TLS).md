---
aliases:
  - " "
  - TLS
---
https://developer.mozilla.org/de/docs/Web/Security/Defenses/Transport_Layer_Security

Transport Layer Security (TLS) ist ein Protokoll, das es einem Client ermöglicht, sicher mit einem Server über ein nicht vertrauenswürdiges Netzwerk zu kommunizieren. Am bekanntesten ist seine Verwendung zur Sicherung von HTTP-Verbindungen im Web: Das resultierende Protokoll wird [HTTPS](https://developer.mozilla.org/de/docs/Glossary/HTTPS) genannt.

TLS sichert eine Netzwerkverbindung auf drei Arten:

- **Verschlüsselung**: Die zwischen Client und Server ausgetauschten Daten werden während der Übertragung verschlüsselt, sodass sie von Angreifern nicht gelesen werden können.
- **Integrität**: Ein Angreifer kann Daten nicht heimlich ändern (ohne dass es bemerkt wird), während sie zwischen Client und Server übertragen werden.
- **Authentifizierung**: Client und Server können sich gegenseitig nachweisen, dass sie die Entität sind, die sie vorgeben zu sein. Im Web authentifizieren sich in der Regel Server gegenüber Clients, aber Clients authentifizieren sich normalerweise nicht gegenüber Servern.

Insbesondere ist HTTPS die Verteidigung gegen einen [Angriff eines Mittelsmannes (MITM)](https://developer.mozilla.org/de/docs/Web/Security/Attacks/MITM), bei dem sich der Angreifer zwischen den Browser des Benutzers und den Server, mit dem er sich verbindet, einfügt und den ausgetauschten Datenverkehr lesen und ändern kann.

Browser betrachten Seiten, die über HTTPS geliefert werden, als Bereitstellung eines [sicheren Kontexts](https://developer.mozilla.org/de/docs/Web/Security/Defenses/Secure_Contexts). Viele leistungsstarke Web-APIs stehen nur Code zur Verfügung, der in einem sicheren Kontext ausgeführt wird.

**Alle Websites sollten alle ihre Seiten und Subressourcen über HTTPS bereitstellen und eine Serverauthentifizierung implementieren.**


## TLS-Handschlag

Wenn ein Client sich mit einem Server über TLS verbindet, legt ein initialer _Handschlag_ die Sicherheitsparameter für das Protokoll fest:

- Client und Server einigen sich auf die zu verwendende TLS-Version. Die aktuelle Version von TLS ist 1.3 ([RFC 8446](https://datatracker.ietf.org/doc/html/rfc8446)), und dies ist die am weitesten verbreitete Version. TLS 1.2 wird noch von einigen Websites verwendet, TLS 1.1 und 1.0 sollten nicht mehr verwendet werden.
- Client und Server einigen sich auf die zu verwendende [Cipher Suite](https://developer.mozilla.org/de/docs/Glossary/Cipher_suite): Diese definiert die Algorithmen, die sie für Schlüsselvereinbarung, Authentifizierung, Verschlüsselung und Nachrichtenauthentifizierung verwenden werden.
- Optional authentifizieren sich Client und Server gegenseitig. Die Client-Authentifizierung, bei der der Client dem Server seine Identität nachweist, ist im Web selten, abgesehen von einigen spezialisierten Anwendungen. Die Serverauthentifizierung, bei der der Server dem Client seine Identität nachweist, ist jedoch ein grundlegender Bestandteil der Websicherheit.
- Client und Server einigen sich auf einen [geheimen Schlüssel](https://developer.mozilla.org/de/docs/Glossary/Symmetric-key_cryptography), den sie zum Verschlüsseln und Entschlüsseln von Nachrichten verwenden werden.

Nach dem Handschlag verwenden Client und Server den geheimen Schlüssel, um alle Nachrichten zu verschlüsseln und zu entschlüsseln, einschließlich HTTP-Headern und -Körpern.