# {{page-title}}
[https://demis.rki.de/fhir/StructureDefinition/DiseaseInformationGFVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformationgfvd)

Das **[DiseaseInformationGFVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformationcommon)**-Profil ist eine Spezialisierung des [DiseaseInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformation)-Profils.

Der Hauptunterschied zum Elternprofil liegt in der Bindung des questionnaire-Elementes an einen fest definierten Wert, der den zugrundeliegenden Fragebogen referenziert. In diesem Fall handelt es sich um die Questionnaire-Ressource [DiseaseQuestionsGFVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/questionnaire/diseasequestionsgfvd). Diese Ressource wird neben der hier vorgestellten StructureDefinition für die Validierung der jeweiligen QuestionnaireResponse herangezogen. Dort werden auch die (fachlichen) Inhalte detailliert dargestellt, welche über die hier profilierte QuestionnaireResponse transportiert werden können/müssen.

**Profilansicht**
{{tree:https://demis.rki.de/fhir/StructureDefinition/DiseaseInformationGFVD, hybrid}}

**Beispiel**
{{xml:ExampleResources/example-questionnaireresponse-diseaseinformationgfvd-01.xml}}
