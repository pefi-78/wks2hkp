
Wir wollen mit der Schlüsselverteilung eine breite Userbasis erreichen. Deshalb wollen wir unseren Usern einen WKS anbieten, da dieser von GnuPG in Version >= 2.1.15 native unterstützt wird. Optimal wäre eine Implementierung direkt  im OX-Server, dies wird momentan aber aus div. technischen Gründen abgelehnt (ORTS-Ticket: 2016081110000907)

Deshalb wollen wir diese Funktion mit einem Proxy auf den ox-internen/vorhandenen HKP-Server nachbilden.  Damit wir keine doppelte Datenhaltung haben, soll der WKS2HKP-Proxy eine WKS-Anfrage auf die entsprechende HKP-Anfrage mappen. 

```
von: https://example.org/.well-known/openpgpkey/hu/iy9q119eutrkn8s1mk4r39qejnbu3n5q
nach: http://keys.example.org:11371/pks/lookup?op=get&search=0x99242560
```
HashBSP's aus Doku/Draft: 
*  iy9q119eutrkn8s1mk4r39qejnbu3n5q  -> "Joe.Doe@Example.ORG"
*  bxzcxpxk8h87z1k7bzk86xn5aj47intu -> “key-submission”

WKS: https://wiki.gnupg.org/WKS
DRAFT: https://tools.ietf.org/id/draft-koch-openpgp-webkey-service-02.txt
HKP: https://tools.ietf.org/html/draft-shaw-openpgp-hkp-00

Ablauf:
* mittels einfacher manuell eingerichteter HTTP-Weiterleitung testen ob die GnuPG-Version über diese den PGP-Schlüssel beziehen kann
* Konzept erarbeiten und Dokumentieren
* Schnittstellen für den Zugriff auf PGP-Keys der UserAccounts bei OX anfragen (Daten: Mailadresse und KeyID)
* Tests und Feinspezifikation schreiben
* TestSystem mit ReferenzServer einrichten 
* Implementieren des Proxys
* Testen und Ausrollen

Fragen:
* soll der Proxy auch bei einem echten HKP-Server funktionieren? (der OX-HKP verhält sich anders als ein echter, was die Aufgabe einfacher macht)
* sollen mehrere virtuelle Domains unterstützen? ([~p.fischer] was hat das für Konsequenzen?)
* sollen mehrere HKP-Server unterstützt werden?
  * Lastverteilung (gleiche Datenbasis)
  * je virtueller Domain ein/mehrere  HKP-Server (unterschiedliche Daten)

* Probleme mit OX
  * DOS-Protektion von OX -> wie ist das Verhalten, was wenn der Trigger ausgelöst wird


* Sonstige Anmerkungen 
  * die Hash's der local-parts der WKS-API aus dem Draft stimmen nicht mit der Implementierung überein.
