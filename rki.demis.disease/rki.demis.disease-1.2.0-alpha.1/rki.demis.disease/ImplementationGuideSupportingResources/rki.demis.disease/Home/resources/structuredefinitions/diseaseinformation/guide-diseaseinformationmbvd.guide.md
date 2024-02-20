# {{page-title}}
[https://demis.rki.de/fhir/StructureDefinition/DiseaseInformationMBVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformationmbvd)

Das **[DiseaseInformationMBVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformationcommon)**-Profil ist eine Spezialisierung des [DiseaseInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformation)-Profils.

Der Hauptunterschied zum Elternprofil liegt in der Bindung des questionnaire-Elementes an einen fest definierten Wert, der den zugrundeliegenden Fragebogen referenziert. In diesem Fall handelt es sich um die Questionnaire-Ressource [DiseaseQuestionsMBVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/questionnaire/diseasequestionsmbvd). Diese Ressource wird neben der hier vorgestellten StructureDefinition für die Validierung der jeweiligen QuestionnaireResponse herangezogen. Dort werden auch die (fachlichen) Inhalte detailliert dargestellt, welche über die hier profilierte QuestionnaireResponse transportiert werden können/müssen.

**Profilansicht**
{{tree:https://demis.rki.de/fhir/StructureDefinition/DiseaseInformationMBVD, hybrid}}

**Beispiel**
{{xml:ExampleResources/example-questionnaireresponse-diseaseinformationmbvd-01.xml}}
