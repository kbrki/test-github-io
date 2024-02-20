# {{page-title}}
[https://demis.rki.de/fhir/Questionnaire/DiseaseQuestionsCLOD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/Questionnaire/DiseaseQuestionsCLOD)

Für Botulismus gibt es eine Reihe meldetatbestandsspezifischer Angaben, die über den vorliegenden Questionnaire bzw. die zugehörige QuestionnaireResponse erfasst werden müssen. Dazu gehören aus logischer Sicht die folgenden Inhalte:

{{render:image-diseasequestionsclod}}

## Inhaltliche Erläuterung

Die folgenden Abschnitte beschreiben die innerhalb des Botulismus-spezifischen Fragebogens abgebildeten logischen Informationsbedarfe im Detail und geben Hinweise wie diese entsprechend über einen ausgefüllten Fragebogen (QuestionnaireResponse) anforderungsgerecht zu befriedigen sind. In diesem Zusammenhang sei noch einmal explizit auf die offiziellen HL7 FHIR Spezifikationen ([Questionnaire](https://www.hl7.org/fhir/questionnaire.html) + [QuestionnaireResponse](https://www.hl7.org/fhir/questionnaireresponse.html)) verwiesen. Diese vermitteln zwingend erforderliches Hintergrundwissen zu diesem Themenbereich.

Der hier diskutierte Fragebogen unterteilt sich in verschiedene "Items" und Unter-"Items", die verschiedene Fragen und Fragenbereiche repräsentieren. Diese Items/Unter-Items weisen dabei einen sehr engen Bezug zur weiter oben dargestellten Grafik auf und werden im Folgenden als Orientierungshilfe dienen. Die Referenzierung einzelnen Items erfolgt dabei grundsätzlich über sogenannte "linkIds". Diese "linkIds" finden sich entsprechend auch in der zugehörigen QuestionnaireResponse wieder.

### Krankheitsform

#### "form" (Welche Krankheitsform liegt vor?)

Über dieses Item wird gemäß § 9 Abs. 1 Nr. 1 Buchst. i IfSG erfragt, in welcher Form sich die Erkrankung manifestiert. Als Antwortmöglichkeiten sind die in https://demis.rki.de/fhir/ValueSet/answerSetFormCLOD referenzierten Konzepte (Lebensmittelbedingter Botulismus, Wundbotulismus, etc. sowie NASK→ "Nicht erhoben" und ASKU →"Nicht ermittelbar") zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss abermals 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

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
  <linkId value="form" />
  <text value="Welche Krankheitsform liegt vor?" />
  <type value="choice" />
  <required value="true" />
  <repeats value="false" />
  <answerValueSet value="https://demis.rki.de/fhir/ValueSet/answerSetFormCLOD" />
  <initial>
    <valueCoding>
      <system value="http://terminology.hl7.org/CodeSystem/v3-NullFlavor" />
      <code value="NASK" />
      <display value="not asked" />
    </valueCoding>
  </initial>
</item>
```

### Infektionsweg

#### infectionPathKindGroup (Auf welchem Weg hat sich die betroffene Person wahrscheinlich infiziert?)

```xml
<item>
  <linkId value="infectionPathKindGroup" />
  <text value="Auf welchem Weg hat sich die betroffene Person wahrscheinlich infiziert?" />
  <type value="group" />
  <required value="true" />
  <repeats value="false" />
  <enableWhen>
    <question value="form" />
    <operator value="=" />
    <answerCoding>
      <system value="http://snomed.info/sct" />
      <code value="414488002" />
    </answerCoding>
  </enableWhen>
  <enableWhen>
    <question value="form" />
    <operator value="=" />
    <answerCoding>
      <system value="http://snomed.info/sct"/>
      <code value="398530003"/>
    </answerCoding>
  </enableWhen>
  <enableWhen>
    <question value="form"/>
    <operator value="="/>
    <answerCoding>
      <system value="http://snomed.info/sct"/>
      <code value="398523009"/>
    </answerCoding>
  </enableWhen>
  <enableBehavior value="any" />
...
</item>  
```

#### honey (Hat der Säugling Honig verzehrt?)

Über dieses Item wird gemäß § 9 Abs. 1  Nr.1 Buschst. k IfSG gefragt, ob im Falle eines Säuglingsbotulismus die betroffene Person Honig verzehrt hat. Als Antwortmöglichkeiten sind die in https://demis.rki.de/fhir/ValueSet/yesOrNoOrNoInformation referenzierten Konzepte zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

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
  <linkId value="honey" />
  <text value="Hat der Säugling Honig verzehrt?" />
  <type value="choice" />
  <required value="true" />
  <repeats value="false" />
  <enableWhen>
    <question value="form"/>
    <operator value="="/>
    <answerCoding>
      <system value="http://snomed.info/sct"/>
      <code value="414488002"/>
    </answerCoding>
  </enableWhen>
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

#### drugs (Konsumiert die betroffene Person intravenöse Drogen?)

Über dieses Item wird gemäß § 9 Abs. 1  Nr.1 Buschst. k IfSG gefragt, ob im Falle eines Wundbotulismus die betroffene Person Hintravenöse Drogen konsumiert hat. Als Antwortmöglichkeiten sind die in https://demis.rki.de/fhir/ValueSet/yesOrNoOrNoInformation referenzierten Konzepte zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

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
  <linkId value="drugs" />
  <text value="Konsumiert die betroffene Person intravenöse Drogen?" />
  <type value="choice" />
  <required value="true" />
  <repeats value="false" />
  <enableWhen>
    <question value="form" />
    <operator value="=" />
    <answerCoding>
      <system value="http://snomed.info/sct" />
      <code value="398530003" />
    </answerCoding>
  </enableWhen>
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

#### food (Welches Lebensmittel hat wahrscheinlich die Infektion verursacht?)

Über dieses Item wird gemäß § 9 Abs. 1  Nr.1 Buschst. k IfSG gefragt, über welches Lebensmittel im Falle eines lebensmittelbedingten Botulismus die Infektion wahrscheinlich übertragen wurde. Als Antwortmöglichkeiten sind die in https://demis.rki.de/fhir/ValueSet/answerSetFoodCLOD referenzierten Konzepte ("Wurst", "Schinken", "Fisch", etc. sowie NASK --> 'Nicht erhoben' und ASKU --> 'Nicht ermittelbar') zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  <linkId value="food" />
  <text value="Welches Lebensmittel hat wahrscheinlich die Infektion verursacht?" />
  <type value="choice" />
  <required value="true" />
  <repeats value="false" />
  <answerValueSet value="https://demis.rki.de/fhir/ValueSet/answerSetFoodCLOD" />
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

#### otherfood (Welches andere Lebensmittel hat wahrscheinlich die Infektion verursacht?)

Über dieses Item können in Freitext-Form gemäß § 9 Abs. 1  Nr.1 Buschst. k IfSG weitere Angaben zu einem verzehrten Lebensmittel, das nicht in https://demis.rki.de/fhir/ValueSet/answerSetInfectionPathCLOD referenziert wurde, gemacht werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist nicht verpflichtend.

```xml
<item>
  <linkId value="otherfood" />
  <text value="Welches andere Lebensmittel hat wahrscheinlich die Infektion verursacht?" />
  <type value="text" />
  <enableWhen>
    <question value="food" />
    <operator value="=" />
    <answerCoding>
      <system value="http://snomed.info/sct" />
      <code value="346602001" />
    </answerCoding>
  </enableWhen>
  <required value="false" />
  <repeats value="false" />
</item>
```

#### consumptionWhen (Verzehr am?)

Über dieses Item wird gemäß § 9 Abs. 1  Nr.1 Buschst. j IfSG gefragt, wann das Lebensmittel, das im Falle eines lebensmittelbedingten Botulismus die Infektion wahrscheinlich übertragen hat, verzehrt wurde. Als Antwortmöglichkeit ist eine Datumsangabe möglich.  Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist nicht verpflichtend.

```xml
<item>
  <linkId value="consumptionWhen" />
  <text value="Verzehr am?" />
  <type value="date" />
  <required value="false" />
  <repeats value="false" />
</item>
```

#### consumptionWhere (Verzehr wo?)

Über dieses Item wird gemäß § 9 Abs. 1 Nr.1 Buschst. k IfSG gefragt, wo das Lebensmittel, das im Falle eines lebensmittelbedingten Botulismus die Infektion wahrscheinlich übertragen hat, verzehrt wurde. Als Antwortmöglichkeiten sind die in https://demis.rki.de/fhir/ValueSet/answerSetConsumptionCLOD referenzierten Konzepte ("Haushalt", "Restaurant", etc. sowie NASK --> 'Nicht erhoben' und ASKU --> 'Nicht ermittelbar') zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  <linkId value="consumptionWhere" />
  <text value="Verzehr wo?" />
  <type value="choice" />
  <required value="true" />
  <repeats value="false" />
  <answerValueSet value="https://demis.rki.de/fhir/ValueSet/answerSetConsumptionCLOD" />
  <initial>
    <valueCoding>
      <system value="http://terminology.hl7.org/CodeSystem/v3-NullFlavor" />
      <code value="NASK" />
      <display value="not asked" />
    </valueCoding>
  </initial>
</item>
```

#### conservation (Auf welche Art wurde das Lebensmittel konserviert?)

Über dieses Item wird gemäß §9 Abs.1 S.1 Nr.1 lit.k IfSG gefragt, wie das Lebensmittel, das im Falle eines lebensmittelbedingten Botulismus die Infektion wahrscheinlich übertragen hat, konserviert wurde. Als Antwortmöglichkeiten sind die in https://demis.rki.de/fhir/ValueSet/answerSetConservationCLOD referenzierten Konzepte ("Konnserve", "Vakuum-verpackt", etc. sowie NASK --> 'Nicht erhoben' und ASKU --> 'Nicht ermittelbar') zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  <linkId value="conservation" />
  <text value="Auf welche Art wurde das Lebensmittel konserviert?" />
  <type value="choice" />
  <required value="true" />
  <repeats value="false" />
  <answerValueSet value="https://demis.rki.de/fhir/ValueSet/answerSetConservationCLOD" />
  <initial>
    <valueCoding>
      <system value="http://terminology.hl7.org/CodeSystem/v3-NullFlavor" />
      <code value="NASK" />
      <display value="not asked" />
    </valueCoding>
  </initial>
</item>
```

#### manufacturing (Aus welcher Herstellung stammt das Lebensmittel?)

Über dieses Item wird gemäß § 9 Abs. 1  Nr.1 Buschst. k IfSG IfSG erfragt, wie das Lebensmittel, das im Falle eines lebensmittelbedingten Botulismus die Infektion wahrscheinlich übertragen hat, hergestellt wurde. Als Antwortmöglichkeiten sind die in https://demis.rki.de/fhir/ValueSet/answerSetManufacturingCLOD referenzierten Konzepte ("eigene (private) ", "industrielle", etc. sowie NASK --> 'Nicht erhoben' und ASKU --> 'Nicht ermittelbar') zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  <linkId value="manufacturing" />
  <text value="Aus welcher Herstellung stammt das Lebensmittel?" />
  <type value="choice" />
  <required value="true" />
  <repeats value="false" />
  <answerValueSet value="https://demis.rki.de/fhir/ValueSet/answerSetManufacturingCLOD" />
  <initial>
    <valueCoding>
      <system value="http://terminology.hl7.org/CodeSystem/v3-NullFlavor" />
      <code value="NASK" />
      <display value="not asked" />
    </valueCoding>
  </initial>
</item>
```

#### toxinInFood (Wurde das Toxin im Lebensmittel nachgewiesen?)

Über dieses Item wird gemäß §9 Abs.1 S.1 Nr.1 lit.k IfSG gefragt, ob Botulismustoxin im Lebensmittel nachgewiesen wurde. Als Antwortmöglichkeiten sind die in https://demis.rki.de/fhir/ValueSet/yesOrNoOrNoInformation referenzierten Konzepte zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  <linkId value="toxinInFood" />
  <text value="Wurde das Toxin im Lebensmittel nachgewiesen?" />
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
</item>
```

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
{{xml:diseasequestionsclod}}
