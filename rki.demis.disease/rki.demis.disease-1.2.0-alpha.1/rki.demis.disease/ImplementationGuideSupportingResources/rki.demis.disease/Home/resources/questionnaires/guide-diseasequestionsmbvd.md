# {{page-title}}
[https://demis.rki.de/fhir/Questionnaire/DiseaseQuestionsMBVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/Questionnaire/DiseaseQuestionsMBVD)

Für Marburgfieber gibt es eine Reihe meldetatbestandsspezifischer Angaben, die über den vorliegenden Questionnaire bzw. die zugehörige QuestionnaireResponse erfasst werden müssen. Dazu gehören aus logischer Sicht die folgenden Inhalte:

{{render:image-DiseaseQuestionsmbvd}}

## Inhaltliche Erläuterung

Die folgenden Abschnitte beschreiben die innerhalb des Marburgfieber-spezifischen Fragebogens abgebildeten logischen Informationsbedarfe im Detail und geben Hinweise wie diese entsprechend über einen ausgefüllten Fragebogen (QuestionnaireResponse) anforderungsgerecht zu befriedigen sind. In diesem Zusammenhang sei noch einmal explizit auf die offiziellen HL7 FHIR Spezifikationen ([Questionnaire](https://www.hl7.org/fhir/questionnaire.html) + [QuestionnaireResponse](https://www.hl7.org/fhir/questionnaireresponse.html)) verwiesen. Diese vermitteln zwingend erforderliches Hintergrundwissen zu diesem Themenbereich.

Der hier diskutierte Fragebogen unterteilt sich in verschiedene "Items" und Unter-"Items", die verschiedene Fragen und Fragenbereiche repräsentieren. Diese Items/Unter-Items weisen dabei einen sehr engen Bezug zur weiter oben dargestellten Grafik auf und werden im Folgenden als Orientierungshilfe dienen. Die Referenzierung einzelnen Items erfolgt dabei grundsätzlich über sogenannte "linkIds". Diese "linkIds" finden sich entsprechend auch in der zugehörigen QuestionnaireResponse wieder.

### Zugehörigkeit zum Ausbruch

#### "outbreak" (Kann der gemeldete Fall einem Ausbruch zugeordnet werden?)

Über dieses Item wird gemäß § 9 Abs. 1 Nr. 1 Buchst. k IfSG gefragt, ob die betroffene Person einem Ausbruch zuzuordnen ist. Als Antwortmöglichkeiten sind die in https://demis.rki.de/fhir/ValueSet/yesOrNoOrNoInformation referenzierten Konzepte (yes --> 'Ja', no --> 'Nein', NASK --> 'Nicht erhoben' und ASKU --> 'Nicht ermittelbar') zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist nicht verpflichtend.

```xml
<item>
  <extension url="http://hl7.org/fhir/StructureDefinition/questionnaire-itemControl">
    <valueCodeableConcept>
      <coding>
        <system value="http://hl7.org/fhir/questionnaire-item-control" />
        <code value="drop-down" />
        <display value="Drop down" />
      </coding>
    </valueCodeableConcept>
  </extension>
  <linkId value="outbreak" />
  <text value="Kann der gemeldete Fall einem Ausbruch zugeordnet werden?" />
  <type value="choice" />
  <required value="true" />
  <repeats value="false" />
  <answerValueSet value="https://demis.rki.de/fhir/ValueSet/yesOrNoOrNoInformation" />
  <initial>
    <valueCoding>
      <system value="http://terminology.hl7.org/CodeSystem/v3-NullFlavor" />
      <code value="NASK" />
      <display value="not asked" />
    </valueCoding>
  </initial>
  ...
</item>  
```

#### "outbreakNote" (Fallbezogene Zusatzinformationen zum Ausbruch)

Über dieses Item können ergänzende Informationen zum Ausbruch in Form eines Freitextes gemacht werden. Die Angaben können gemacht werden, sofern "outbreak" mit "Ja" beantwortet wurde. Die Angaben sind nicht verpflichtend.

```xml
<item>
  <extension url="http://hl7.org/fhir/StructureDefinition/questionnaire-itemControl">
    <valueCodeableConcept>
      <coding>
        <system value="http://hl7.org/fhir/questionnaire-item-control" />
        <code value="drop-down" />
        <display value="Drop down" />
      </coding>
    </valueCodeableConcept>
  </extension>
  <linkId value="outbreakNote" />
  <text value="Fallbezogene Zusatzinformationen zum Ausbruch" />
  <type value="text" />
  <enableWhen>
    <question value="outbreak" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
  <required value="false" />
  <repeats value="false" />
</item>
```

#### "outbreakNotificationId (Notification-Id der zugehörigen Ausbruchsmeldung)

Über diese Item kann der DEMIS-Identifier zum Ausbruch angegeben werden. Die Angaben können gemacht werden, sofern "outbreak" mit "Ja" beantwortet wurde. Die Angaben sind nicht verpflichtend.

```xml
<item>
  <linkId value="outbreakNotificationId" />
  <text value="Notification-Id der zugehörigen Ausbruchsmeldung" />
  <type value="reference" />
  <enableWhen>
    <question value="outbreak" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
  <required value="false" />
  <repeats value="false" />
</item>
```

**Ressource**
{{xml:diseasequestionsmbvd}}
