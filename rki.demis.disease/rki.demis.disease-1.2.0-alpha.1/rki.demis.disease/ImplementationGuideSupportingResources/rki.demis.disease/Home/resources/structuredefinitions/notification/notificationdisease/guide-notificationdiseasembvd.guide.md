# {{page-title}}
[https://demis.rki.de/fhir/StructureDefinition/NotificationDiseaseMBVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/notificationdiseasembvd)

Das **[NotificationDiseaseMBVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/notificationdiseasembvd)**-Profil leiten sich aus dem [NotificationDisease](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/notificationdisease)-Profil ab.

Innerhalb des Profils erfolgt der Zuschnitt der Erkrankungsmeldung auf einen ganz konkreten Meldetatbestand (hier: Marburgfieber). Über die in Instanzen dieses Profils verpflichtend zu befüllenden section-Entries wird auf insgesamt drei Ressourcen verwiesen:

- eine meldetatbestandsspezifisch ausgestaltete Condition-Ressource (hier gemäß [DiseaseMBVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseasembvd)), die u.a. Angaben zur Diagnose und zu Symptomen/Manifestationen enthält,
- einen ausgefüllten allgemeinen Fragebogen (hier gemäß [DiseaseInformationCommon](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformationcommon)), der u.a. Angaben zur Hospitalisierung, zur Exposition, zur Beauftragung eines Labors usw. enthalten kann und
- einen ausgefüllten meldetatbestandsspezifischen Fragebogen (hier gemäß [DiseaseInformationMBVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformationmbvd&category=Profile&sortBy=RankScore_desc)), der u.a. Angaben zur Immunisierung der betroffenen Person aber auch zum möglichen Infektionsumfeld referenziert bzw. enthält.

**Profilansicht**
{{tree:https://demis.rki.de/fhir/StructureDefinition/NotificationDiseaseMBVD, hybrid}}

**Beispiel**
{{xml:ExampleResources/example-composition-notificationdiseasembvd-01.xml}}
