# {{page-title}}
[https://demis.rki.de/fhir/StructureDefinition/NotificationDiseaseCORD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/notificationdiseasecord)

Das **[NotificationDiseaseCORD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/notificationdiseasecord)**-Profil leiten sich aus dem [NotificationDisease](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/notificationdisease)-Profil ab.

Innerhalb des Profils erfolgt der Zuschnitt der Erkrankungsmeldung auf einen ganz konkreten Meldetatbestand (hier: Diphtherie). Über die in Instanzen dieses Profils verpflichtend zu befüllenden section-Entries wird auf insgesamt drei Ressourcen verwiesen:

- eine meldetatbestandsspezifisch ausgestaltete Condition-Ressource (hier gemäß [DiseaseCORD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseasecord)), die u.a. Angaben zur Diagnose und zu Symptomen/Manifestationen enthält,
- einen ausgefüllten allgemeinen Fragebogen (hier gemäß [DiseaseInformationCommon](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformationcommon)), der u.a. Angaben zur Hospitalisierung, zur Exposition, zur Beauftragung eines Labors usw. enthalten kann und
- einen ausgefüllten meldetatbestandsspezifischen Fragebogen (hier gemäß [DiseaseInformationCORD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformationcord&category=Profile&sortBy=RankScore_desc)), der u.a. Angaben zur Immunisierung der betroffenen Person aber auch zum möglichen Infektionsumfeld referenziert bzw. enthält.

**Profilansicht**
{{tree:https://demis.rki.de/fhir/StructureDefinition/NotificationDiseaseCORD, hybrid}}

**Beispiel**
{{xml:ExampleResources/example-composition-notificationdiseasecord-01.xml}}
