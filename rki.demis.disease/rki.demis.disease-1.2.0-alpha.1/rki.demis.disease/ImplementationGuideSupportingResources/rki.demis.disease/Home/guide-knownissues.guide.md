# {{page-title}}

Die folgende Seite gibt einen Überblick über bekannte Probleme mit den beschriebenen Profilen bzw. den von diesen verwendeten Dependencies:

## Meldevorgangs-Id / Meldungs-Id - Widersprüche zur HL7 FHIR Spezifikation bzgl. der Verwendung von UUIDs innerhalb von Identifiern

HL7 definiert im FHIR Standard Vorgaben bezüglich der Nutzung global eindeutiger Identifier wie UUIDs oder OIDs innerhalb des Datentyps "Identifier" (https://www.hl7.org/fhir/datatypes.html#Identifier). DEMIS nutzt UUIDs im Zusammenhang mit der Gewährleistung einer eindeutigen Referenzierbarkeit von Meldevorgängen und Meldungen. Die derzeitige Ausgestaltung innerhalb der definierten Profile widerspricht den Vorgaben des Standards.

Der Standard definiert folgende Anforderung:
_"If the identifier value itself is naturally a globally unique URI (e.g. an OID, a UUID, or a URI with no trailing local part), then the system SHALL be "urn:ietf:rfc:3986", and the URI is in the value (OIDs and UUIDs using urn:oid: and urn:uuid: - see note on the V3 mapping and the examples). Naturally globally unique identifiers are those for which no system has been assigned and where the value of the identifier is reasonably expected to not be re-used. Typically, these are absolute URIs of some kind."_

In DEMIS werden Meldevorgangs- und Meldungs-Id jedoch wie folgt als Identifier abgebildet:

Beispiel Melungs-Id:

```xml
<Composition xmlns="http://hl7.org/fhir">
  ...
  <identifier>
    <system value="https://demis.rki.de/fhir/NamingSystem/NotificationId"/>
    <value value="c13cd356-f147-5901-859d-31e6b2772465"/>
  </identifier>
  ...
</Composition>
```

Der entsprechende Fehler blieb über mehrere Jahre unentdeckt und wurde innerhalb dieser Zeit in vielen IT-Systemen "umgesetzt". Eine zeitnahe Korrektur ist derzeit nicht möglich, da sowohl Systeme der Melder (z.B. LIS), Systeme der Gesundheitsämter (z.B. SurvNet) und das DEMIS Backend selbst von entsprechenden Anpassungen betroffen wären und in diesem Zusammenhang die Gefahr bestünde, dass Meldungen im System nicht abgesetzt oder nicht verarbeitet werden könnten.

Derzeit gehen wir davon aus, dass erst in eng aufeinander abgestimmten, zukünftigen Major Releases der betroffenen Profilpakete (Labormeldungen, Arztmeldungen, Bettenbelegungsstatistik) eine Korrektur erfolgen kann. Darüber hinaus wird an Lösungsstrategien gearbeitet, die potentiell auch ein fließende Identifier-Migration ermöglichen. Bis zum Abschluss der entsprechenden Arbeiten muss der Fehler leider ignoriert/akzeptiert werden. Auch derzeit laufenden Integrationsarbeiten müssen den Fehler in einer ersten Iteration "umsetzen".



