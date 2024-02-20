# {{page-title}} 
[https://demis.rki.de/fhir/StructureDefinition/DiseaseInformationCommon](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformationcommon) 

Das **[DiseaseInformationCommon](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformationcommon)**-Profil nimmt eine Anpassung der allgemeinen FHIR [QuestionnaireResponse](http://www.hl7.org/fhir/questionnaireresponse.html)-Definition vor.

{{render:image-profilehierarchydiseaseinformationcommon}}  

Primär beziehen sich diese Anpassungen auf Streichungen/Festlegungen bezüglich der die Ressource näher beschreibenden Inhalte, wie z.B. questionnaire, encounter oder author. Zusätzlich erfolgt eine Bindung des questionnaire-Elementes an einen fest definierten Wert, der den zugrundeliegenden Fragebogen (hier: [DiseaseQuestionsCommon](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/questionnaire/diseasequestionscommon)) referenziert. Diese Questionnaire-Ressource wird neben der hier vorgestellten StructureDefinition für die Validierung der jeweiligen Ressource herangezogen. Dort werden auch die (fachlichen) Inhalte detailliert dargestellt, welche über die hier profilierte [QuestionnaireResponse](http://www.hl7.org/fhir/questionnaireresponse.html) transportiert werden können/müssen.

**Profilansicht**
{{tree:https://demis.rki.de/fhir/StructureDefinition/DiseaseInformationCommon, hybrid}}

**Beispiel** - Minimalsatz an Antworten (mit Angabe zur Hospitalisierung)
{{xml:ExampleResources/example-questionnaireresponse-diseaseinformationcommon-00.xml}}

**Beispiel** - Erweiterter Antwortsatz
{{xml:ExampleResources/example-questionnaireresponse-diseaseinformationcommon-01.xml}}