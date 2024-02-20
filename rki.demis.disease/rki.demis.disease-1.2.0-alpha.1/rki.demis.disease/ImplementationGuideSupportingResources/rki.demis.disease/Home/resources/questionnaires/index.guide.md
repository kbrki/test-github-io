# {{page-title}}

Fragebögen im Kontext dieses Implementierungsleitfadens definieren, strukturieren und bündeln Fragen, die durch die im Sinne des IfSG meldepflichtige Person beantwortet werden sollen: 

- Fragen, die meldetatbestandsübergreifend von Interesse sind, werden in einem allgemeinen Fragebogen (https://demis.rki.de/fhir/Questionnaire/DiseaseQuestionsCommon) zusammengefasst. Dieser Fragebogen ist für sämtliche Meldungen gemäß §6 Absatz 1, 2 IfSG auszufüllen und somit als spezifische QuestionnaireResponse integraler Bestandteil einer jeden Meldung. 
- Meldetatbestandsspezifische Informationsbedarfe werden hingegen in meldetatbestandsspezifischen Fragebögen zusammengestellt. Für jeden Meldetatbestand gibt es somit neben dem allgemeinen Fragebogen noch einen zusätzlich meldetatbestandsspezifischen Fragebogen (z.B. https://demis.rki.de/fhir/Questionnaire/DiseaseQuestionsCVDD), der ausgefüllt werden muss und sich als weitere QuestionnaireResponse in der Meldung manifestiert.

**Hinweis:** Ein [Questionnaire](http://www.hl7.org/fhir/questionnaire.html) (Fragebogen) im Sinne von HL7 FHIR ist ein sogenanntes "Definitional Artifact". Er definiert daher lediglich einen **auszufüllenden** Fragebogen. Der **ausgefüllte** Fragebogen selbst wird hingegen über den Ressourcentyp [QuestionnaireResponse](http://www.hl7.org/fhir/questionnaireresponse.html) dargestellt. DEMIS-Meldung enthalten daher grundsätzlich [QuestionnaireResponse](http://www.hl7.org/fhir/questionnaireresponse.html)-Ressourcen. Diese werden im Backend jedoch gegen den zugehörigen [Questionnaire](http://www.hl7.org/fhir/questionnaire.html) validiert.