# {{page-title}}
[https://demis.rki.de/fhir/Questionnaire/DiseaseQuestionsCVDD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/Questionnaire/DiseaseQuestionsCVDD)

Für COVID-19 gibt es eine Reihe meldetatbestandsspezifischer Angaben, die über den vorliegenden Questionnaire bzw. die zugehörige QuestionnaireResponse erfasst werden müssen. Dazu gehören aus logischer Sicht die folgenden Inhalte:

{{render:image-diseasequestionscvdd}}

## Inhaltliche Erläuterung

Die folgenden Abschnitte beschreiben die innerhalb des COVID-19-spezifischen Fragebogens abgebildeten logischen Informationsbedarfe im Detail und geben Hinweise wie diese entsprechend über einen ausgefüllten Fragebogen (QuestionnaireResponse) anforderungsgerecht zu befriedigen sind. In diesem Zusammenhang sei noch einmal explizit auf die offiziellen HL7 FHIR Spezifikationen ([Questionnaire](https://www.hl7.org/fhir/questionnaire.html) + [QuestionnaireResponse](https://www.hl7.org/fhir/questionnaireresponse.html)) verwiesen. Diese vermitteln zwingend erforderliches Hintergrundwissen zu diesem Themenbereich.

Der hier diskutierte Fragebogen unterteilt sich in verschiedene "Items" und Unter-"Items", die verschiedene Fragen und Fragenbereiche repräsentieren. Diese Items/Unter-Items weisen dabei einen sehr engen Bezug zur weiter oben dargestellten Grafik auf und werden im Folgenden als Orientierungshilfe dienen. Die Referenzierung einzelnen Items erfolgt dabei grundsätzlich über sogenannte "linkIds". Diese "linkIds" finden sich entsprechend auch in der zugehörigen QuestionnaireResponse wieder.

### Kontakt zu bestätigtem Fall

#### **infectionSource** (Kontakt zu bestätigtem Fall)
Über dieses Item wird erfragt, OB die betroffene Person in jüngster Vergangenheit Kontakt zu einem bestätigten Fall (nachweislich positiv getestete Person) hatte. Als Antwortmöglichkeiten sind die in [https://demis.rki.de/fhir/ValueSet/yesOrNoOrNoInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/valueset/yesornoornoinformation) referenzierten Konzepte (yes --> 'Ja', no --> 'Nein', NASK --> 'Nicht erhoben' und ASKU --> 'Nicht ermittelbar') zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  ...
  <linkId value="infectionSource" />
  <text value="Kontakt zu bestätigtem Fall" />
  <type value="choice" />
  <required value="true" />
  <answerValueSet value="https://demis.rki.de/fhir/ValueSet/yesOrNoOrNoInformation" />
  <initial>
     <valueCoding>
        <system value="http://terminology.hl7.org/CodeSystem/v3-NullFlavor" />
        <code value="NASK" />
        <display value="not asked" />
     </valueCoding>
  </initial>
</item>
```

### Angaben zum Infektionsumfeld
Als Bestandteil der Meldung werden Angaben zum Infektionsumfeld erfasst. Die Rechtsgrundlage für die Erhebung bildet §9 Absatz 1 Satz 1 Nummer 1 Buchstabe k IfSG. Die Abbildung innerhalb des Fragebogens erfolgt über mehrere ineinander verschachtelte Items.


#### **infectionEnvironmentSetting** (Infektionsumfeld vorhanden)
Über dieses Item wird erfragt, OB Informationen zum Infektionsumfeld vorliegen. Als Antwortmöglichkeiten sind auch in diesem Item die in [https://demis.rki.de/fhir/ValueSet/yesOrNoOrNoInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/valueset/yesornoornoinformation) referenzierten Konzepte (yes --> 'Ja', no --> 'Nein', NASK --> 'Nicht erhoben' und ASKU --> 'Nicht ermittelbar') zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss abermals 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  ...
  <linkId value="infectionEnvironmentSetting" />
  <text value="Infektionsumfeld vorhanden" />
  <type value="choice" />
  <required value="true" />
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

#### **infectionEnvironmentSettingGroup**
Mithilfe des infectionEnvironmentSettingGroup-Items können Mehrfachangaben zum Infektionsumfeld innerhalb des Fragebogens erfasst werden. Das Item stellt somit nur ein Hilfskonstrukt beim Aufbau des Questionnaires bzw. der zugehörigen QuestionnaireResponse dar. Die Verwendung ist verpflichtend, sofern 'infectionEnvironmentSetting' mit 'Ja' beantwortet wurde.

```xml      
<item>
  ...
  <linkId value="infectionEnvironmentSettingGroup" />
  <type value="group" />
  <enableWhen>
    <question value="infectionEnvironmentSetting" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
  <required value="true" />
  <repeats value="true" />
  ...
</item>
```

#### **infectionEnvironmentSettingKind** (Wahrscheinliches Infektionsumfeld)
Über dieses Item wird das wahrscheinlichen Infektionsumfeldes konkretisiert. Die Frage wird nur gestellt/angezeigt, sofern 'infectionEnvironmentSetting' mit 'Ja' beantwortet wurde. Als Antwortmöglichkeiten sind die in https://demis.rki.de/fhir/ValueSet/infectionEnvironmentSettingCVDD referenzierten Konzepte (Kita, Gesundheitseinrichtung etc.) zulässig. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  <linkId value="infectionEnvironmentSettingKind" />
  <text value="Wahrscheinliches Infektionsumfeld" />
  <type value="choice" />
  <enableWhen>
    <question value="infectionEnvironmentSetting" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
  <answerValueSet value="https://demis.rki.de/fhir/ValueSet/infectionEnvironmentSettingCVDD" />
</item>
```

#### **infectionEnvironmentSettingBegin** (Beginn Infektionsumfeld)
Über dieses Item wird erfragt, WANN die Exposition im Infektionsumfeld begonnen hat. Die Frage wird nur gestellt/angezeigt, sofern 'infectionEnvironmentSetting' mit 'Ja' beantwortet wurde. Die Antwort muss vom Typ 'Datum' sein. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist optional.

```xml
<item>
  <linkId value="infectionEnvironmentSettingBegin" />
  <text value="Beginn Infektionsumfeld" />
  <type value="date" />
  <enableWhen>
    <question value="infectionEnvironmentSetting" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
</item>
```

#### **infectionEnvironmentSettingEnd** (Ende Infektionsumfeld)
Über dieses Item wird erfragt, WANN die Exposition im Infektionsumfeld geendet hat. Die Frage wird nur gestellt/angezeigt, sofern 'infectionEnvironmentSetting' mit 'Ja' beantwortet wurde. Die Antwort muss vom Typ 'Datum' sein. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist optional.

```xml
<item>
  <linkId value="infectionEnvironmentSettingEnd" />
  <text value="Ende Infektionsumfeld" />
  <type value="date" />
  <enableWhen>
    <question value="infectionEnvironmentSetting" />
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

### Impfangaben
Als Bestandteil der Meldung werden Angaben zum Impfstatus der betroffenen Person erfasst. Die Rechtsgrundlage für die Erhebung bildet §9 Absatz 1 Satz 1 Nummer 1 Buchstabe q IfSG. Die Abbildung innerhalb des Fragebogens erfolgt über mehrere ineinander verschachtelte Items.


#### **immunization** (Wurde die betroffene Person jemals in Bezug auf die Krankheit geimpft?)
Über dieses Item wird erfragt, OB die die betroffene Person jemals in Bezug auf die gemeldete Krankheit geimpft wurde. Als Antwortmöglichkeiten sind auch in diesem Item die in [https://demis.rki.de/fhir/ValueSet/yesOrNoOrNoInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/valueset/yesornoornoinformation) referenzierten Konzepte (yes --> 'Ja', no --> 'Nein', NASK --> 'Nicht erhoben' und ASKU --> 'Nicht ermittelbar') zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss abermals 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  ...
  <linkId value="immunization" />
  <text value="Wurde die betroffene Person jemals in Bezug auf die Krankheit geimpft?" />
  <type value="choice" />
  <required value="true" />
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

#### **immunizationRef** (Impfinformationen)

Über dieses Item wird auf eine Immunization-Ressource mit einer Profilierung gemäß https://demis.rki.de/fhir/StructureDefinition/ImmunizationInformationCVDD verwiesen. Für jede bekannte Impfung gegen COVID-19 soll in diesem Zusammenhang eine eigene Immunization-Ressource angelegt und als Bestandteil der Meldung transportiert werden. Entsprechend kann 'immunizationRef' innerhalb einer QuestionnaireResponse mehrfach vorkommen. Die Angabe ist jedoch nicht verpflichtend, auch wenn 'immunization' mit 'Ja' beantwortet wurde.

```xml
<item>
  <extension url="http://hl7.org/fhir/StructureDefinition/questionnaire-referenceProfile">
    <valueCanonical value="https://demis.rki.de/fhir/StructureDefinition/ImmunizationInformationCVDD" />
  </extension>
  <linkId value="immunizationRef" />
  <text value="Impfinformationen" />
  <type value="reference" />
  <enableWhen>
    <question value="immunization" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
  <required value="false" />
  <repeats value="true" />
</item>
```

### Angabe des Hospitalisierungsgrundes (IN DISKUSSION)

**ACHTUNG:** Derzeit wird zwischen den involvierten Stakeholdern noch immer intensiv diskutiert, ob und wie der Hospitalisierungsgrund in die Meldung aufgenommen werden soll. Wir möchten die implementierenden Hersteller daher an dieser Stelle bitten, dieses optionale Item bis auf Weiteres nicht innerhalb ihrer Software umzusetzen. Wir können momentan leider nicht ausschließen, dass in Folgeversion dieses Paketes eine Streichung oder Neupositionierung der entsprechenden Information vorgenommen werden muss.


Als Bestandteil der Meldung werden Angaben zum Grund der Hospitalization der betroffenen Person erfasst. Die Rechtsgrundlage für die Erhebung bildet die Verordnung über die Erweiterung der Meldepflicht nach § 6 Absatz 1 Satz 1 Nummer 1 des Infektionsschutzgesetzes auf Hospitalisierungen in Bezug auf die Coronavirus-Krankheit-2019. Die Angabe ist von großer Relevanz für die Gesundheitsämter.
Als Antwortmöglichkeiten sind die in [https://demis.rki.de/fhir/ValueSet/hospitalizationReason](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/valueset/hospitalizationReason) referenzierten Konzepte (becauseOfDisease --> 'Hospitalisiert aufgrund der gemeldeten Krankheit' , becauseOfOtherReason --> 'Hospitalisiert aufgrund einer anderen Ursache als der gemeldeten Krankheit', forIsolation --> 'Hospitalisiert zur Isolierung', NASK --> 'Nicht erhoben' und ASKU --> 'Nicht ermittelbar') zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden.


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
 <linkId value="reason" />
 <text value="Grund der Hospitalisierung" />
 <type value="choice" />
 <required value="false" />
 <answerValueSet value="https://demis.rki.de/fhir/ValueSet/answerSetHospitalizationReason" />
 <initial>
    <valueCoding>
       <system value="http://terminology.hl7.org/CodeSystem/v3-NullFlavor"></system>
       <code value="NASK"></code>
       <display value="not asked"></display>
    </valueCoding>
 </initial>
</item>
```

**Hinweis:** Für dieses item wird "required=false" definiert. Aus technischer Sicht müsste somit nicht zwingend eine Antwort auf diese Frage in die zugehörige QuestionnaireResponse aufgenommen werden. Diese Festlegung wurde ausschließlich aus Gründen der Rückwärtskompatibilität der Profile getroffen. Alle Hersteller, die die Implementierung dieses Fragebogens bisher noch nicht abgeschlossen haben, möchten wir an dieser Stelle daher explizit darum bitten, das item trotzdem als verpflichtend zu betrachten und in ihren Systemen entsprechend umzusetzen. In einem zukünftigen Folgerelease wird für dieses item "required=true" definiert.


**Ressource**
{{xml:diseasequestionscvdd}}
