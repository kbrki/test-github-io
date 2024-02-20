# {{page-title}}
[https://demis.rki.de/fhir/Questionnaire/DiseaseQuestionsCommon](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/Questionnaire/DiseaseQuestionsCommon) 

Im allgemeinen Fragebogen werden Informationsbedarfe gebündelt, die für alle definierten Meldetatbestände gemäß §6 Absatz 1 gleichermaßen existieren. Dazu gehören aus logischer Sicht die folgenden Inhalte:

{{render:image-diseasequestionscommon}}

## Inhaltliche Erläuterung

Die folgenden Abschnitte beschreiben die innerhalb des allgemeinen Fragebogens abgebildeten logischen Informationsbedarfe im Detail und geben Hinweise wie diese entsprechend über einen ausgefüllten Fragebogen (QuestionnaireResponse) anforderungsgerecht zu befriedigen sind. In diesem Zusammenhang sei noch einmal explizit auf die offiziellen HL7 FHIR Spezifikationen ([Questionnaire](https://www.hl7.org/fhir/questionnaire.html) + [QuestionnaireResponse](https://www.hl7.org/fhir/questionnaireresponse.html)) verwiesen. Diese vermitteln zwingend erforderliches Hintergrundwissen zu diesem Themenbereich.

Der hier diskutierte Fragebogen unterteilt sich in verschiedene "Items" und Unter-"Items", die verschiedene Fragen und Fragenbereiche repräsentieren. Diese Items/Unter-Items weisen dabei einen sehr engen Bezug zur weiter oben dargestellten Grafik auf und werden uns im Folgenden als Orientierungshilfe dienen. Die Referenzierung einzelnen Items erfolgt dabei grundsätzlich über sogenannte "linkIds". Diese "linkIds" finden sich entsprechend auch in der zugehörigen QuestionnaireResponse wieder.

### Angaben zum Tod
Innerhalb des allgemeinen Fragebogens werden Angaben zum Tod der betroffenen Person erhoben. Derzeit sind dies ausschließlich Informationen in Bezug auf das Vorliegen eines Todesfalls sowie das Todesdatum. Die entsprechenden Angaben werden dabei über zwei ineinander verschachtelte Items abgebildet. Die Rechtsgrundlage für die Erhebung entsprechender Angaben bildet §9 Absatz 1 Satz 1 Nummer 1 Buchstabe j IfSG.

#### **isDead** (Verstorben)
Über dieses Item wird erfragt, OB die betroffene Person verstorben ist. Als Antwortmöglichkeiten sind die in [https://demis.rki.de/fhir/ValueSet/yesOrNoOrNoInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/valueset/yesornoornoinformation)  referenzierten Konzepte (yes --> 'Ja', no --> 'Nein', NASK --> 'Nicht erhoben' und ASKU --> 'Nicht ermittelbar') zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  ...
  <linkId value="isDead" />
  <text value="Verstorben" />
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

#### **deathDate** (Datum des Todes)
Über dieses Item wird erfragt, WANN die betroffene Person verstorben ist. Die Frage wird nur gestellt/angezeigt, sofern 'isDead' mit 'Ja' beantwortet wurde. Die Antwort muss vom Typ 'date' sein. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist optional.

```xml
<item>
  <linkId value="deathDate" />
  <text value="Datum des Todes" />
  <type value="date" />
  <enableWhen>
    <question value="isDead" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
</item>
```


**Hinweis:** Im Rahmen der Profilerstellung wurde intensiv diskutiert, ob die Angaben zum Tod der betroffenen Person nicht besser an der NotifiedPerson (Patient-Ressource) selbst dokumentiert werden sollten, zumal dort bereits zum Zeitpunkt der Profilierung der Errergernachweismeldung entsprechende Angaben als 'mustSupport' gekennzeichnet wurden. Folgende Gründe haben im Endeffekt zur Präferierung der hier beschriebenen Abbildung geführt:

- Die von HL7 FHIR in der Patient-Ressource vorgesehene Darstellung erfolgt entweder über Patient.deceasedBoolean oder Patient.deceasedDateTime. In diesem Modell lassen sich die - bezogen auf ihre Semantik - doch sehr unterschiedlichen NullFlavors 'Nicht erhoben' und 'Nicht ermittelbar' nicht unterscheiden. In den Fachverfahren im Gesundheitsamt wird diese Unterscheidung jedoch vorgenommen und ist u.a. auch für Datenauswertungen von Interesse.
- Sollen zukünftig konkretisierende Angaben zum Tod der betroffenen Person erhoben werden, wird ein "Ort" benötigt, an dem diese abgebildet werden können. Konkrete Diskussionen wurden hier beispielsweise in Bezug auf folgende weiterführende Frage geführt: "Steht der Tod in einem direkten Zusammenhang mit der gemeldeten Erkrankung?" Eine Bündelung entsprechender Angaben an einer Stelle ist dann entsprechend zielführender.

Vor dem Hintergrund dieser Diskussion gilt somit die folgende zusätzliche Festlegung: Angaben zum Tod der betroffenen Person sollen ausschließlich über den hier dargestellten Fragebogen bzw. dessen ausgefülltes Pendant erfolgen. Erfolgt aus zwingenden technischen Gründen dennoch eine Darstellung an beiden Stellen, so dürfen sich die Angaben nicht widersprechen. Fachverfahren im Gesundheitsamt müssen die in der QuestionnaireResponse gemachten Angaben in die eigene Datenhaltung übernehmen.


### Bundeswehr
Das Infektionsschutzgesetz definiert in §9 Absatz 1 Satz 1 Nummer 1 Buchstabe r IfSG eine weitere Angabe, die Bestandteil der Meldung werden muss: die Zugehörigkeit zu den in § 54a Absatz 1 Nummer 1 bis 5 IfSG genannten Personengruppen. Diese Personengruppen umfassen: 
- Angehörige des Geschäftsbereiches des Bundesministeriums der Verteidigung während ihrer Dienstausübung,
- Soldaten außerhalb ihrer Dienstausübung,
- Personen, während sie sich in Liegenschaften der Bundeswehr oder in ortsfesten oder mobilen Einrichtungen aufhalten, die von der Bundeswehr oder im Auftrag der Bundeswehr betrieben werden,
- Angehörige dauerhaft in der Bundesrepublik Deutschland stationierter ausländischer Streitkräfte im Rahmen von Übungen und Ausbildungen, sofern diese ganz oder teilweise außerhalb der von ihnen genutzten Liegenschaften durchgeführt werden,
- Angehörige ausländischer Streitkräfte auf der Durchreise sowie im Rahmen von gemeinsam mit der Bundeswehr stattfindenden Übungen und Ausbildungen

Entsprechende Angaben werden innerhalb des Fragebogens über ein einzelnes Item abgefragt.

#### **militaryAffiliation** (Zugehörigkeit zur Bundeswehr)
Als Antwortmöglichkeiten sind die in [https://demis.rki.de/fhir/ValueSet/answerSetMilitaryAffiliation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/ValueSet/answerSetMilitaryAffiliation) referenzierten Konzepte zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  ...
  <linkId value="militaryAffiliation" />
  <text value="Zugeh&#246;rigkeit zur Bundeswehr" />
  <type value="choice" />
  <required value="true" />
  <answerValueSet value="https://demis.rki.de/fhir/ValueSet/answerSetMilitaryAffiliation" />
  <initial>
    <valueCoding>
      <system value="http://terminology.hl7.org/CodeSystem/v3-NullFlavor" />
      <code value="NASK" />
      <display value="not asked" />
    </valueCoding>
  </initial>
</item>
```

### Laborbeauftragung
§9 Absatz 1 Satz 1 Nummer 2 IfSG sieht die Erfassung von Name, Anschrift und weiteren Kontaktdaten der Untersuchungsstelle vor, die mit der Erregerdiagnostik beauftragt ist. Entsprechende Angaben werden über zwei ineinander verschachtelte Items abgebildet bzw. über diese referenziert.

#### **labSpecimenTaken** (Laborbeauftragung)
Über dieses Item wird erfragt, OB ein Labor mit der Erregerdiagnostik beauftragt wurde. Als Antwortmöglichkeiten sind die in [https://demis.rki.de/fhir/ValueSet/yesOrNoOrNoInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/valueset/yesornoornoinformation)  referenzierten Konzepte (yes --> 'Ja', no --> 'Nein', NASK --> 'Nicht erhoben' und ASKU --> 'Nicht ermittelbar') zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  ...
  <linkId value="labSpecimenTaken" />
  <text value="Laborbeauftragung" />
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

#### **labSpecimenLab** (Beauftragtes Labor)
Über dieses Item wird erfragt, WELCHES Labor mit der Erregerdiagnostik beauftragt wurde. Die Frage wird nur gestellt/angezeigt, sofern 'labSpecimenTaken' mit 'Ja' beantwortet wurde. In diesem Fall ist die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse verpflichtend. Im Ergebnis wird innerhalb der QuestionnaireResponse über 'labSpecimenLab' jedoch lediglich eine Referenz auf eine Organization-Ressource transportiert. Die referenzierte Organization-Ressorce mit den entsprechenden Angaben zum Labor (Name, Anschrift, Kontaktdaten) muss somit zusätzlich durch das System des Meldenden generiert und in die Meldung eingebettet werden.

```xml
<item>
  <extension url="http://hl7.org/fhir/StructureDefinition/questionnaire-referenceResource">
    <valueCode value="Organization" />
  </extension>
  <linkId value="labSpecimenLab" />
  <text value="Beauftragtes Labor" />
  <type value="reference" />
  <enableWhen>
    <question value="labSpecimenTaken" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
  <required value="true" />
</item>
``` 

### Hospitalisierung
Die Fragen/Angaben zur Hospitalisierung der betroffenen Person werden über drei ineinander verschachtelte Items abgebildet. Die Rechtsgrundlage für die Erhebung der entsprechenden Informationen bilden §9 Absatz 1 Satz 1 Nummer 1 Buchstabe o IfSG sowie §1 (2) Nr. 1 f RVO Hospitalisierung.

#### **hospitalized** (Aufnahme in ein Krankenhaus)
Über dieses Item wird erfragt, OB eine Hospitalisierung der betroffenen Person vorliegt. Als Antwortmöglichkeiten sind die in [https://demis.rki.de/fhir/ValueSet/yesOrNoOrNoInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/valueset/yesornoornoinformation) referenzierten Konzepte (yes --> 'Ja', no --> 'Nein', NASK --> 'Nicht erhoben' und ASKU --> 'Nicht ermittelbar') zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  ...
  <linkId value="hospitalized" />
  <text value="Aufnahme in ein Krankenhaus" />
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

#### **hospitalizedGroup**
Mithilfe des hospitalizedGroup-Items können Mehrfachangaben zur Hospitalisierung (Hospitalisierungs-'Episoden') innerhalb des Fragebogens erfasst werden. Das Item stellt somit nur ein Hilfskonstrukt beim Aufbau des Questionnaires bzw. der zugehörigen QuestionnaireResponse dar. Die Verwendung ist verpflichtend, sofern 'hospitalized' mit 'Ja' beantwortet wurde.

```xml
<item>
  ..
  <linkId value="hospitalizedGroup" />
  <type value="group" />
  <enableWhen>
    <question value="hospitalized" />
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

#### **hospitalizedEncounter**
Über dieses Item wird auf eine Encounter-Ressource mit einer Profilierung gemäß https://demis.rki.de/fhir/StructureDefinition/Hospitalization verwiesen. Für jede Hospitalisierungs-'Episode' (Aufenthalt auf Normalstation, Aufenthalt auf Intensivstation etc.) muss in diesem Zusammenhang eine eigene Encounter-Ressource angelegt und als Bestandteil der Meldung transportiert werden. Entsprechend kann 'hospitalizedEncounter' innerhalb einer QuestionnaireResponse mehrfach vorkommen. Die Angabe ist verpflichtend sofern 'hospitalized' mit 'Ja' beantwortet wurde.

```xml
<item>
  <extension url="http://hl7.org/fhir/StructureDefinition/questionnaire-referenceProfile">
    <valueCanonical value="https://demis.rki.de/fhir/StructureDefinition/Hospitalization" />
  </extension>
  <linkId value="hospitalizedEncounter" />
  <type value="reference" />
  <enableWhen>
    <question value="hospitalized" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
</item>
```

### Betreuung/Unterbringung/Tätigkeit in Gemeinschaftseinrichtung
Als Bestandteil der Meldung sind ebenfalls Informationen bezüglich der Betreuung, Unterbringung oder Tätigkeit der betroffenen Person in einer Gemeinschaftseinrichtung zu erfassen. Die Rechtsgrundlage für die Erhebung bildet §9 Absatz 1 Satz 1 Nummer 1 Buchstabe f und h IfSG. Die Abbildung innerhalb des Fragebogens erfolgt über mehrere ineinander verschachtelte Items.

#### **infectProtectFacility** (Tätigkeit, Betreuung oder Unterbringung in Einrichtungen mit Relevanz für den Infektionsschutz (siehe § 23 Abs. 3 IfSG oder § 36 Abs. 1 oder 2 IfSG))
Über dieses Item wird erfragt, OB überhaupt eine Tätigkeit, Betreuung oder Unterbringung entsprechend des gesetzlich definierten Rahmenwerks vorliegt. Als Antwortmöglichkeiten sind auch in diesem Item die in [https://demis.rki.de/fhir/ValueSet/yesOrNoOrNoInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/valueset/yesornoornoinformation) referenzierten Konzepte (yes --> 'Ja', no --> 'Nein', NASK --> 'Nicht erhoben' und ASKU --> 'Nicht ermittelbar') zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss abermals 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  ...
  <linkId value="infectProtectFacility" />
  <text value="T&#228;tigkeit, Betreuung oder Unterbringung in Einrichtungen mit Relevanz f&#252;r den Infektionsschutz (siehe &#167; 23 Abs. 3 IfSG oder &#167; 36 Abs. 1 oder 2 IfSG)" />
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

#### **infectProtectFacilityGroup**
Mithilfe des 'infectProtectFacilityGroup'-Items können Mehrfachangaben zur Betreuung/Unterbringung/Tätigkeit innerhalb des Fragebogens erfasst werden. Das Item stellt somit nur ein Hilfskonstrukt beim Aufbau des Questionnaires bzw. der zugehörigen QuestionnaireResponse dar. Die Verwendung ist verpflichtend, sofern 'infectProtectFacility' mit 'Ja' beantwortet wurde. Die im Folgenden aufgeführten Items sind Bestandteil der Gruppe und können somit im Zusammenspiel für jeweils vollständige Mehrfachangaben gebündelt werden.

```xml
<item>
  ...
  <linkId value="infectProtectFacilityGroup" />
  <type value="group" />
  <enableWhen>
    <question value="infectProtectFacility" />
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

#### **infectProtectFacilityBegin** (Beginn der Tätigkeit/Betreuung/Unterbringung)
Über dieses Item wird erfragt, WANN die Tätigkeit/Betreuung/Unterbringung in der Gemeinschaftseinrichtung begonnen hat. Die Frage wird nur gestellt/angezeigt, sofern 'infectProtectFacility' mit 'Ja' beantwortet wurde. Die Antwort muss vom Typ 'Datum' sein. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist optional.

```xml
<item>
  <linkId value="infectProtectFacilityBegin" />
  <text value="Beginn der T&#228;tigkeit/Betreuung/Unterbringung" />
  <type value="date" />
  <enableWhen>
    <question value="infectProtectFacility" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
</item>
```

#### **infectProtectFacilityEnd** (Ende der Tätigkeit/Betreuung/Unterbringung)
Über dieses Item wird erfragt, WANN die Tätigkeit/Betreuung/Unterbringung in der Gemeinschaftseinrichtung geendet hat. Die Frage wird nur gestellt/angezeigt, sofern 'infectProtectFacility' mit 'Ja' beantwortet wurde. Die Antwort muss vom Typ 'Datum' sein. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist optional.

```xml
<item>
  <linkId value="infectProtectFacilityEnd" />
  <text value="Ende der T&#228;tigkeit/Betreuung/Unterbringung" />
  <type value="date" />
  <enableWhen>
    <question value="infectProtectFacility" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
</item>
```

#### **infectProtectFacilityRole** (Rolle)
Über dieses Item wird der jeweilige Bezug zur Gemeinschaftseinrichtung konkretisiert. Die Frage wird nur gestellt/angezeigt, sofern 'infectProtectFacility' mit 'Ja' beantwortet wurde. Als Antwortmöglichkeiten sind die in https://demis.rki.de/fhir/ValueSet/organizationAssociation referenzierten Konzepte (Tätigkeit, Betreuung, Unterbringung) zulässig. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  <linkId value="infectProtectFacilityRole" />
  <text value="Rolle" />
  <type value="choice" />
  <enableWhen>
    <question value="infectProtectFacility" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
  <required value="true" />
  <answerValueSet value="https://demis.rki.de/fhir/ValueSet/organizationAssociation" />
</item>
```

#### **infectProtectFacilityOrganization** (Einrichtung)
Über dieses Item wird erfragt, in WELCHER Gemeinschaftseinrichtung die betroffene Person tätig oder untergebracht ist oder alternativ betreut wird. Die Frage wird nur gestellt/angezeigt, sofern 'infectProtectFacility' mit 'Ja' beantwortet wurde. In diesem Fall ist die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse jedoch verpflichtend. Im Ergebnis wird innerhalb der QuestionnaireResponse über 'infectProtectFacilityOrganization' jedoch  lediglich eine Referenz auf eine Organization-Ressource transportiert. Die referenzierte Organization-Ressorce mit den entsprechenden Angaben zur Gemeinschaftseinrichtung (Name, Anschrift, Kontaktdaten) muss somit zusätzlich durch das System des Meldenden generiert und in die Meldung eingebettet werden.

```xml
<item>
  <extension url="http://hl7.org/fhir/StructureDefinition/questionnaire-referenceResource">
    <valueCode value="Organization" />
  </extension>
  <linkId value="infectProtectFacilityOrganization" />
  <text value="Einrichtung" />
  <type value="reference" />
  <enableWhen>
    <question value="infectProtectFacility" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
  <required value="true" />
</item>
```

**Hinweis:** Aus inhaltlicher Sicht überschneiden sich die Angaben zur Hospitalisierung und zur Unterbringung in einer Gemeinschaftseinrichtung. Sofern entsprechende Angaben zur Unterbringung im Krankenhaus bereits im Zusammenhang mit der Hospitalisierung kommuniziert wurden, müssen sie in diesem Bereich nicht noch einmal erfasst werden.

### Exposition
Entsprechend §9 Absatz 1 Satz 1 Nummer 1 Buchstabe l IfSG müssen Meldende Angaben zum Ort machen, an dem die Infektion wahrscheinlich erworben (Expositionsort) wurde. In diesem Zusammenhang ist für Orte in Deutschland jeweils ein Landkreis oder eine kreisfreie Stadt anzugeben. In anderen Fällen ist die Angabe des Staates ausreichend. Zur Abbildung der entsprechenden Informationsbedarfe werden mehrere ineinander verschachtelte Items genutzt.

#### **placeExposure** (Wahrscheinlicher Expositionsort bekannt)
Über dieses Item wird erfragt, OB eine Information zum wahrscheinlichen Expositionsort vorliegt. Als Antwortmöglichkeiten sind die in [https://demis.rki.de/fhir/ValueSet/yesOrNoOrNoInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/valueset/yesornoornoinformation) referenzierten Konzepte (yes --> 'Ja', no --> 'Nein', NASK --> 'Nicht erhoben' und ASKU --> 'Nicht ermittelbar') zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  ...
  <linkId value="placeExposure" />
  <text value="Wahrscheinlicher Expositionsort bekannt" />
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

#### **placeExposureGroup**
Mithilfe des 'placeExposureGroup'-Items können Mehrfachangaben zu wahrscheinlichen Expositionsorten innerhalb des Fragebogens erfasst werden. Das Item stellt somit nur ein Hilfskonstrukt beim Aufbau des Questionnaires bzw. der zugehörigen QuestionnaireResponse dar. Die Verwendung ist verpflichtend, sofern 'placeExposure' mit 'Ja' beantwortet wurde. Die im Folgenden aufgeführten Items sind Bestandteil der Gruppe und können somit im Zusammenspiel für jeweils vollständige Mehrfachangaben gebündelt werden.

```xml
<item>
  ...
  <linkId value="placeExposureGroup" />
  <type value="group" />
  <enableWhen>
    <question value="placeExposure" />
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

#### **placeExposureBegin** (Beginn des Aufenthalts am wahrscheinlichen Expositionsort/Datum des Aufenthalts)
Über dieses Item wird erfragt, WANN der Aufenthalt am wahrscheinlichen Expositionsort begonnen hat. Die Frage wird nur gestellt/angezeigt, sofern 'placeExposure' mit 'Ja' beantwortet wurde. Die Antwort muss vom Typ 'date' sein. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist optional.

```xml
<item>
  <linkId value="placeExposureBegin" />
  <text value="Beginn des Aufenthalts am wahrscheinlichen Expositionsort/Datum des Aufenthalts" />
  <type value="date" />
  <enableWhen>
    <question value="placeExposure" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
</item>
```

#### **placeExposureEnd** (Ende des Aufenthalts am wahrscheinlichen Expositionsort)
Über dieses Item wird erfragt, WANN der Aufenthalt am wahrscheinlichen Expositionsort beendet wurde. Die Frage wird nur gestellt/angezeigt, sofern 'placeExposure' mit 'Ja' beantwortet wurde. Die Antwort muss vom Typ 'date' sein. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist optional.

```xml
<item>
  <linkId value="placeExposureEnd" />
  <text value="Ende des Aufenthalts am wahrscheinlichen Expositionsort" />
  <type value="date" />
  <enableWhen>
    <question value="placeExposure" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
</item>
```

#### **placeExposureRegion** (Wahrscheinlicher Expositionsort)
Über dieses Item wird erfragt, um welchen konkreten (geografischen) Ort es sich handelt. Entsprechend der gesetzlichen Vorgaben muss für Ort in Deutschland ein Landkreis oder eine kreisfreie Stadt ausgewählt werden. Für Orte außerhalb von Deutschland ist hingegen die Angabe des Staates ausreichend. Die Frage wird nur gestellt/angezeigt, sofern 'placeExposure' mit 'Ja' beantwortet wurde. Als Antwortmöglichkeiten sind die in [https://demis.rki.de/fhir/ValueSet/answerSetGeographicRegion](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/ValueSet/answerSetGeographicRegion) referenzierten Konzepte (Orte) zulässig. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  <linkId value="placeExposureRegion" />
  <text value="Wahrscheinlicher Expositionsort" />
  <type value="choice" />
  <enableWhen>
    <question value="placeExposure" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
  <required value="true" />
  <answerValueSet value="https://demis.rki.de/fhir/ValueSet/answerSetGeographicRegion" />
  <initial>
    <valueCoding>
      <system value="http://terminology.hl7.org/CodeSystem/v3-NullFlavor" />
      <code value="NASK" />
      <display value="not asked" />
    </valueCoding>
  </initial>
</item>
```

#### **placeExposureHint** (Anmerkungen zum Expositonsort)
Über dieses Item können textuelle Zusatzinformationen zum wahrscheinlichen Expositionsort kommuniziert werden. Die Frage wird nur gestellt/angezeigt, sofern 'placeExposure' mit 'Ja' beantwortet wurde. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist optional.

```xml
<item>
  <linkId value="placeExposureHint" />
  <text value="Anmerkungen zum Expositonsort" />
  <type value="text" />
  <enableWhen>
    <question value="placeExposure" />
    <operator value="=" />
    <answerCoding>
      <system value="https://demis.rki.de/fhir/CodeSystem/yesOrNoAnswer" />
      <code value="yes" />
    </answerCoding>
  </enableWhen>
</item>
```


### Blut-/Organ-/Gewebespende
Entsprechend §9 Absatz 1 Satz 1 Nummer 1 Buchstabe p IfSG muss der Meldende - soweit vorliegend - Angaben dazu machen, ob die betroffene Person in den zurückliegenden 6 Monaten ein Spender für eine Blut-, Organ-, Gewebe- oder Zellspende war. Der entsprechende Informationsbedarf wird im allgemeinen Fragebogen über ein einzelnes Item abgebildet.

#### **organDonation** (Spender für eine Blut-, Organ-, Gewebe- oder Zellspende in den letzten 6 Monaten)
Als Antwortmöglichkeiten für dieses Item sind die in [https://demis.rki.de/fhir/ValueSet/yesOrNoOrNoInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/valueset/yesornoornoinformation) referenzierten Konzepte (yes --> 'Ja', no --> 'Nein', NASK --> 'Nicht erhoben' und ASKU --> 'Nicht ermittelbar') zulässig. Als vorbelegter Wert in einem unausgefüllten Fragebogen muss 'NASK' verwendet werden. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist verpflichtend.

```xml
<item>
  ...
  <linkId value="organDonation" />
  <text value="Spender f&#252;r eine Blut-, Organ-, Gewebe- oder Zellspende in den letzten 6 Monaten" />
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

### Zusatzinformationen

#### **additionalInformation** (Wichtige Zusatzinformationen)
Über dieses Item werden potentiell wichtige Zusatzinformationen zum Meldetatbestand erfragt, die der Meldende dem Gesundheitsamt übermitteln möchte. Dabei kann es sich sowohl um Informationen handeln, die die weiter oben beschriebenen Angaben konkretisieren oder auch um neue für die Fallbearbeitung wichtige Sachverhalte. Die Angabe der entsprechenden Information in der zugehörigen QuestionnaireResponse ist optional und erfolgt als Freitext.

```xml
<item>
  <linkId value="additionalInformation" />
  <text value="Wichtige Zusatzinformationen" />
  <type value="text" />
</item>
```

**Ressource**
{{xml:diseasequestionscommon}}