# {{page-title}} 

Die folgende Seite beschreibt EINE mögliche Umsetzung der "COVID-19-Hospitalisierungsmeldung" unter Nutzung der im Aufnahmedatensatz gemäß §301 Absatz 3 SGBV V enthaltenen Informationen. An dieser Stelle sei explizit darauf hingewiesen, dass die im Aufnahmedatensatz enthaltenen Informationen nur einen Teil des Informationsmodells der vollständigen Erkrankungsmeldung bedienen können. Im Sinne einer schnellen Umsetzbarkeit und Verfügbarkeit der elektronischen Meldung kann mit dieser Einschränkung in der Übergangszeit dennoch "gelebt" werden.

**Hinweis:** Die Beschreibung ist bisher noch nicht mit allen relevanten Stakeholdern abgestimmt und stellt somit lediglich einen Diskussionsstand dar.
 
Das folgende Objektmodell stellt ein "Minimum Viable Dataset" für eine Hospitalisierungsmeldung überblicksartig in grafischer Form dar:

{{render:image-example301}}

Auf folgende Besonderheiten sei in dieser spezifischen Meldungsausgestaltung hingewiesen:

- Da im Aufnahmedatensatz lediglich die IK-Nummer des Krankenhauses eingebettet ist, kann kein Rückschluss auf den genauen Krankenhausstandort, in dem der Versicherte untergebracht ist, gezogen werden. Aus diesem Grund wird die betroffene Person in der Meldung lediglich als [NotifiedPerson](https://simplifier.net/demis/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/notifiedperson) abgebildet, die jedoch auf KEINE [NotifiedPersonFacility](https://simplifier.net/demis/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/notifiedpersonfacility) verweist.
- Die [NotifierFacility](https://simplifier.net/demis/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/notifierfacility) (meldende Einrichtung) muss mit Angaben befüllt werden, die sich aus der IK-Nummer des Krankenhauses ableiten lassen. Hierzu könnte eine statische Konfiguration in der sendenden Software hinterlegt werden.
- Aus dem ausgefüllten allgemeinen Fragebogen ([DiseaseInformationCommon](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformationcommon)) heraus wird lediglich auf ein Hospitalisierung ([Hospitalization](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/hospitalization)) verwiesen. Andere Elemente des Fragebogens werden ausschließlich mit den minimal erforderlichen Informationen belegt, was sich primär darin manifestiert, dass Antworten zu Fragen automatisch als "nicht erhoben" (Default-Antwort) gekennzeichnet werden.
- Aus der Hospitalisierungs-Ressource ([Hospitalization](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/hospitalization)) heraus wird wiederum über das Feld serviceProvider auf die [NotifierFacility](https://simplifier.net/demis/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/notifierfacility) verwiesen. Hierbei ist zu erwähnen, dass dies natürlich nicht zwingend den Krankenhausstandort, an dem die betroffene Person behandelt wird, widerspiegelt. Diese Information ist im Aufnahmedatensatz leider nicht enthalten.
- Für den ausgefüllten COVID-spezifischen Fragebogen ([DiseaseInformationCVDD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformationcvdd)) wird ein identisches Vorgehen gewählt: Alle verpflichtend zu beantwortenden Fragen werden mit der Default-Antwort belegt.

## Mögliche Mappings
Der folgende Abschnitt beschreibt mögliche Mappings zwischen den Feldern des Aufnahmedatensatzes gemäß §301 Absatz 3 SGB V und den Inhalten der oben skizzierten Hospitalisierungsmeldungsvariante.  

**Abbildung von AUFN.FKT auf NotifierFacility**
{{render:image-301mappingaufnfkt}}

Wie bereits oben beschrieben, ist es erforderlich ein automatisches Mapping von der IK-Nummer des Absenders auf Angaben zur meldenden Einrichtung (NotifierFacility) innerhalb des Systems zu realisieren, da sich die entsprechenden Angaben (Name, Anschrift, Kontaktdaten der Organisation etc.) nicht im Aufnahmedatensatz befinden.

**Abbildung von AUFN.FKT + AUFN.INV auf NotificationDiseaseCVDD (Meldungs-Id)**
{{render:image-301mappingaufnfktinv}}

**Diskussion:** DEMIS sieht die Nutzung einer Meldungs-Id vor, um ein sauberes Lifecyclemanagement für Meldungen (Korrektur-/Folgemeldungen) zu ermöglichen. Aus diesem Grund würde es sich u.U. empfehlen, auf Basis von unveränderlichen Informationen zum Krankenhausfall (z.B. IK-Nummer + KH-internes Kennzeichen des Versicherten) eine UUID zu generieren, die als Meldungs-Id verwendet werden kann. Meldungen auf Basis von Entlassdatensätzen könnten dann dieselbe ID generieren. Als Generierungsalgorithmus würde sich hier ggf. eine name-based UUID anbieten.

**Abbildung von AUFN.NAD auf NotifiedPerson**
{{render:image-301mappingaufnnad}}

AUFN.NAD enthält alle relevanten Informationen zur betroffenen Person (einschließlich der Privatanschrift der Versicherten). Diese Informationen könnten sehr leicht auf die Inhalte der [NotifiedPerson](https://simplifier.net/demis/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/notifiedperson)-Ressource gemappt werden. Ein detaillierteres Mapping der Einzelangaben auf Unterelement der Ressource ergibt sich aus den entsprechenden Profildetails.

**Abbildung von AUFN.AUF + AUFN.EAD auf Hospitalization + DiseaseCVDD**
{{render:image-301mappingaufnaufead}}
Aus AUFN.AUF heraus können Informationen der [Hospitalization](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/hospitalization)- und der [DiseaseCVDD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseasecvdd)-Ressource befüllt werden. Dies sind:
- **Encounter.serviceType** - erlaubt Angaben darüber, ob eine Person intensivmedizinisch behandelt wird oder auf einer "normalen" Station liegt
- **Encounter.period** - erlaubt die Angabe des Startdatums der Hospitalisierung
- **Condition.recordedDate** - erlaubt die Angabe zum Tag der Diagnoseerstellung

Aus AUFN.EAD heraus kann bestimmt werden, ob es sich überhaupt um eine meldepflichtige Diagnose handelt. Die für COVID-19 spezifischen ICD10 Codes können auf den Meldetatbestand "cvdd" gemappt werden. Dies könnte auch der Trigger sein, um zu entscheiden, ob durch das System überhaupt eine Meldung erstellt und versendet wird oder nicht.


## Offene Punkte

- Wie bereits angesprochen, kann allein auf Grundlage der IK-Nummer kein Rückschluss auf den tatsächlichen Krankenhausstandort gezogen werden. Diese Information würde sich erst im Entlassdatensatz wiederfinden. Deshalb muss eine Reihe von Kompromissen bezüglich der Datenqualität eingegangen werden. Gibt es ggf. einen Weg, die Standort-Id auch im Aufnahmedatensatz nutzbar zu machen (andere Datenflüsse etc.)? Ist diese Information zum Zeitpunkt der Aufnahme überhaupt schon im KIS?