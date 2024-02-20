# {{page-title}}
[https://demis.rki.de/fhir/StructureDefinition/DiseaseInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformation)  

Das vorliegende **[DiseaseInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformation)**-Profil nimmt eine Anpassung der allgemeinen FHIR [QuestionnaireResponse](http://www.hl7.org/fhir/questionnaireresponse.html)-Definition vor.   

Primär beziehen sich diese Anpassungen auf Streichungen/Festlegungen bezüglich der die Ressource näher beschreibenden Inhalte, wie z.B. questionnaire, encounter oder author. Dieses Profil ist abstrakt. Instanzen dürfen/können daher nicht gebildet werden. Verwendung innerhalb der Meldung finden die von diesem Profil abgeleiteten Profile, z.B. [DiseaseInformationCVDD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseinformationcvdd).  Derzeit sind 13 Kindprofile definiert. Mit der Unterstützung weiterer Meldetatbestände gemäß §6 Absatz 1 IfSG werden zusätzliche Kindprofile hinzukommen.

{{render:image-profilehierarchydiseaseinformationxyzd}}

**Profilansicht**
{{tree:https://demis.rki.de/fhir/StructureDefinition/DiseaseInformation, hybrid}}
