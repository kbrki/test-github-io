# {{page-title}}

## Grundlagen
### Begriffsbestimmung
DEMIS unterscheidet aus konzeptioneller Sicht zwischen **Meldevorgängen**, **Meldungen** und **Fällen**. Diese Unterscheidung ist insbesondere mit Blick auf das Meldungslifecyclemanagement und die in diesem Zusammenhang genutzten Identifier von besonderem Interesse.

-----

#### **Fall**
Ein Fall ist in Bezug auf die epidemiologische Überwachung von Infektionskrankheiten die Grundlage der Arbeitsorganisation in den Gesundheitsämtern. Ein Fall repräsentiert dabei primär eine bestimmte Erkrankung oder ein Meldetatbestand, der im Zusammenhang mit einer Erkrankung steht (z.B. Tollwutexpositionsverdacht) in Bezug auf eine betroffene Person. Fälle werden in Auszügen nicht-namentlich an die zuständigen Landesstellen und von dort ans RKI übermittelt. Ob ein Fall übermittlungspflichtig ist, entscheiden die vom RKI herausgegebenen [Falldefinitionen](https://www.rki.de/DE/Content/Infekt/IfSG/Falldefinition/falldefinition_node.html). In diesem Zusammenhang ist jedoch zu berücksichtigen, dass diese Falldefinitionen NICHT die Kriterien für die Meldung an das Gesundheitsamt festlegen. Sie richten sich deshalb explizit NICHT an klinisch oder labordiagnostisch tätige Ärztinnen und Ärzte.

Die Quelle, der zu einem Fall erfassten Angaben, bilden - neben eigenen Ermittlungen des jeweiligen Gesundheitsamtes - zu einem bedeutenden Teil die an das Gesundheitsamt gesendeten Meldungen und jeweils zugehörigen Ergänzungen/Korrekturen. Dabei ist zu berücksichtigen, dass häufig auch mehrere Meldungen (von Arzt und Labor, Arzt und Gemeinschaftseinrichtung, Arzt und Tierarzt etc.) den benötigten Input für einen einzelnen Fall liefern können. Diese Meldungen können sich u.U. auch direkt aufeinander beziehen.

Auf Grundlage der aus den Meldungen übernommenen und ggf. selbst ermittelten Informationen kann die Software im Gesundheitsamt die Übermittlungspflichtigkeit eines Falles ermitteln.

---

#### **Meldevorgang**
Ein Meldevorgang (MV) ist die Nachricht eines Melders (z.B. eines Labors oder Krankenhauses) an die DEMIS Infrastruktur. Er kann als "Umschlag" um eine konkrete Version einer Meldung (Initialmeldung, Ergänzung/Korrektur) interpretiert werden. Unterschiedliche Meldevorgänge können sich somit grundsätzlich auf die gleiche logische Meldung beziehen. Dies ist beispielsweise dann der Fall, wenn Inhalte einer Meldung (M) in einem Meldevorgang MV1 durch deren Sender über weitere Meldevorgänge (MV2, MV3, ... ) ergänzt oder korrigiert werden. Meldevorgänge werden über DEMIS an die jeweils zuständigen Gesundheitsämter vermittelt und in den dortigen Fachverfahren verarbeitet. Im Ergebnis wird der Meldevorgang bzw. die darüber kommunizierte Meldung einem neuen oder ggf. auch einem bestehenden Fall zugeordnet.

Meldevorgänge werden innerhalb des DEMIS-Informationsmodells als profilierte FHIR-Bundle-Ressourcen abgebildet und sind mit einem eindeutigen Identifier, der *Meldevorgangs-Id (NotificationBundleId)*, versehen, welcher durch das DEMIS Backend gesetzt bzw. überschrieben wird. Jeder Meldevorgang hat somit einen individuellen Identifier, der ihn eindeutig identifiziert und als Ende-zu-Ende-Referenz zwischen Sender und Empfänger genutzt werden kann. Da die *Meldevorgangs-Id (NotificationBundleId)* durch das DEMIS-Backend gesetzt wird, erhält der Melder sie als Bestandteil der Antwort vom DEMIS-Backend (Melde-Response) strukturiert und als Bestandteil der PDF-Quittung zurück geliefert.

**Repräsentation der Meldevorgangs-Id**

```xml
<Bundle xmlns="http://hl7.org/fhir">
   ...
   <identifier>
      <system value="https://demis.rki.de/fhir/NamingSystem/NotificationBundleId"/>
      <value value="f6f4061a-1bdd-31c0-8d81-09b39f581270"/>
   </identifier>
   <type value="document"/>
  ...
</Bundle>
```
**Wichtig:** Da der Meldevorgangs-Id (NotificationBundleId) eine überaus wichtige Rolle im System zufällt und ihre Eindeutigkeit zwingend gewährleistet sein muss, wird der durch das meldende System gesetzte Identifier durch das DEMIS-Backend überschrieben! Der neu gesetzte Identifier wird als Bestandteil der Response-Nachricht dem sendenden System bekannt gegeben. Obwohl der Identifier durch das DEMIS-Backend gesetzt wird, muss das sendende System dennoch einen entsprechenden Wert generieren und übermitteln. Die Ursache hierfür liegt in den Basisanforderungen des zugrundeliegenden FHIR-Standards begründet, der für FHIR-Dokumente das Setzen eines entsprechenden Identifiers verpflichtend vorsieht.

**Bildungsvorschrift für die Meldevorgangs-Id**
* Als system MUSS "https://demis.rki.de/fhir/NamingSystem/NotificationBundleId" verwendet werden.
* Als value MUSS für jede Nachricht (Meldevorgang) eine neue Random-UUID (v4) gemäß RFC4122 gebildet werden.

---

#### **Meldung**
Eine Meldung (M) enthält meldepflichtige Informationen zu einem konkreten Fall (z.B. COVID-19 für Person ABC durch Arzt XYZ). Meldungen werden grundsätzlich eingebettet in Meldevorgängen (s.o.) transportiert. Eine logisch zusammengehörige Meldung (z.B. Initialmeldung + Ergänzungsmeldung, Initialmeldung + Korrekturmeldung) kann sich dabei über mehrere Meldevorgänge verteilen: Aus fachlicher Sicht wäre dann die Ergänzungsmeldung sozusagen eine neue Version der Initialmeldung.

Meldungen werden innerhalb des DEMIS-Informationsmodells als profilierte FHIR-Composition-Ressourcen abgebildet. Meldungen können eindeutig über die sogenannte *Meldungs-Id (NotificationId)* identifiziert werden, welche durch den Melder nach einem definierten Schema (s.u.) generiert werden muss. Ergänzungsmeldungen, Korrekturmeldungen usw. müssen jeweils die *Meldungs-Id (NotificationId)* der Initialmeldung tragen, um eine Zusammenführung der Inhalte in den verarbeitenden Fachverfahren im Gesundheitsamt zu ermöglichen. Dabei muss jedoch berücksichtigt werden, dass Meldungen verschiedener Melder (Arzt, Tierarzt oder Leiter von Einrichtungen der pathologisch-anatomischen Diagnostik) niemals die gleiche *Meldungs-Id (NotificationId)* tragen dürfen, auch wenn sie sich auf den gleichen Fall beziehen. Ähnlich wie die Meldevorgangs-Id wird auch die *Meldungs-Id* als Bestandteil der PDF-Quittung an den Melder zurück geliefert. 

**Repräsentation der Meldungs-Id**

```xml
<Composition xmlns="http://hl7.org/fhir">
  ...
  <identifier>
    <system value="https://demis.rki.de/fhir/NamingSystem/NotificationId"/>
    <value value="c13cd356-f147-5901-859d-31e6b2772465"/>
  </identifier>
  ...
</Composition>
```

**Hinweis:** Sämtliche durch einen Arzt gesendeten Ergänzungs- und/oder Korrekturmeldungen zu einem Fall tragen grundsätzlich dieselbe Meldungs-Id (NotifciationId) wie die Initialmeldung. Sie wird in allen Fällen über das Element Composition.identifier dargestellt. Gesonderte Regelungen gelten für den Fall, dass ein weiterer Melder (z.B. Tierarzt oder Leiter von Einrichtungen der pathologisch-anatomischen Diagnostik) sich auf die Meldung des jeweiligen erstmeldenden Arztes beziehen und diese ggf. ergänzen möchte (vgl. "Verweis auf Meldungen anderer Melder").

**Bildungsvorschrift für die Meldungs-Id**

* Als system MUSS "https://demis.rki.de/fhir/NamingSystem/NotificationId" verwendet werden.
* Der value MUSS eine durch das System des Melders generierte UUID sein, die gemäß einer der beiden im Folgenden beschriebenen Varianten gebildet wird:

  * **Variante 1 - Bildung einer Random-UUID (v4) gemäß RFC4122**<br>
Sendende Systeme KÖNNEN für die Darstellung der Meldungs-Id (NotificationId) für jeden Fall/Auftrag Random-UUIDs (v4) gemäß RFC4122 in eigener Verantwortung bilden. Um sicherzustellen, dass Ergänzungs- und Korrekturmeldungen zum entsprechenden Fall/Auftrag dieselbe Meldungs-Id (NotificationId) tragen, MÜSSEN die entsprechende Systeme intern sicherstellen, dass die generierte Id dauerhaft mit dem Fall/Auftrag assoziiert bleibt. Dies macht u.U. komplexere Anpassungen an den jeweiligen Systemen bzw. deren Datenmodell erforderlich.

  * **Variante 2 - Bildung einer name-based UUID (v5) gemäß RFC4122 (SHA1-basiert)**<br>
Sendende Systeme KÖNNEN für die Darstellung der Meldungs-Id (NotificationId) für jeden Fall/Auftrag name-based UUIDs (v5) gemäß RFC4122 (SHA1-basiert) in eigener Verantwortung bilden. In diesem Zusammenhang MUSS durch die Implementierung sichergestellt werden, dass die in die UUID-Generierung einfließenden Parameter so gewählt werden, dass sowohl für unterschiedliche Fälle als auch für unterschiedliche Melder niemals dieselben UUIDs generiert werden. Folgendes Verfahren KANN in diesem Zusammenhang zum Einsatz kommen:

    * Für jedes meldende System MUSS einmalig eine individuelle Namespace UUID als Random-UUID (v4) gemäß RFC4122 gebildet werden (z.B. "db5da554-9bb0-4393-9ee3-4866cad38c1e" → Bitte diese Namespace UUID NICHT nutzen. Sie ist lediglich ein Beispiel). Die einmalig generierte Namespace UUID fließt fortan dauerhaft in die Generierung der name-based UUID (v5) gemäß RFC4122 (SHA1-basiert) ein und stellt damit sicher, dass andere meldende Systeme keine identischen UUIDs bilden werden.

    * Für jedes meldende System MÜSSEN neben der Namespace UUID weitere Eingabeparameter definiert werden, die sicherstellen, dass die generierte UUID in Bezug auf den Fall DAUERHAFT eindeutig bleibt. Dies kann beispielsweise eine sinnvolle Konkatenierung der DEMIS Participant ID, der Jahreszahl und einer melderspezifischen Fallnummer sein (z.B. "12346_2022-007023"). Bei der Auswahl der jeweiligen Parameter sind insbesondere Situationen geschickt zu adressieren, in denen sich verwendete Parameter wiederholen könnten (z.B. Nummer des Abrechnungsfalls).

    * Bildung der name-based UUIDs (v5) gemäß RFC4122 (SHA1-basiert) unter Nutzung der Namespace UUID und der übrigen Eingabeparameter:
UUIDv5.nameUUIDFromNsAndString("db5da554-9bb0-4393-9ee3-4866cad38c1e", "12346_2022-007023") → c5a2325b-b296-522f-a410-918cb900fae3

  * Die zweite Variante hat den Vorteil, dass das meldende System die generierte UUID nicht dauerhaft mit dem Fall assoziieren muss, da aus den bereits verwalteten Angaben zu einem Fall dieselbe Meldungs-Id jederzeit neu berechnet werden kann.

**Wichtig:** Die Gewährleistung der Eindeutigkeit der *Meldungs-Id (NotificationId)* für einen konkreten Fall ist ESSENTIELL wichtig für die empfangenden Gesundheitsämter, da insbesondere über diesen Mechanismus eine (teil)automatisierte Zusammenführung von Initial- und Folgemeldungen leicht ermöglicht werden kann. Werden dieselben Meldungs-Ids (NotificationIds) jedoch für unterschiedliche Fälle verwendet, kann es u.U. zu größeren Verwerfungen in den Fachverfahren der Gesundheitsämtern kommen, da potentiell nicht zusammengehörige Meldungen einander zugeordnet werden. Bitte halten Sie sich daher zwingend an die formulierten Anforderungen!

**Verweis auf Meldungen anderer Melder**

Es existieren u.U. Szenarien, in denen verschiedene Melder (z.B. Arzt und Leiter von Einrichtungen der pathologisch-anatomischen Diagnostik), Meldungen an DEMIS versenden, die sich für alle beteiligten Akteure erkennbar auf den selben Fall beziehen. Um eine automatisierte Zuordenbarkeit dieser Meldungen bei den empfangenden Gesundheitsämtern sicherstellen zu können, ist es möglich, innerhalb einer Meldung M2 auf eine Meldung M1 eines anderen Melders zu verweisen. Verweise auf Meldungen anderer Melder, die sich auf den selben Fall beziehen MÜSSEN dabei im Composition.relatesTo Element der jeweiligen Ressource entsprechend des im Folgenden dargestellten Schemas eingebettet werden. Der verwendete Identifier ist dabei die Meldungs-Id (NotificationId) der Meldung auf welche verwiesen wird.

```xml
<Composition xmlns="http://hl7.org/fhir">
  ...
  <relatesTo>
    <code value="appends"/>
    <targetReference>
      <type value="Composition"/>
      <identifier>
        <system value="https://demis.rki.de/fhir/NamingSystem/NotificationId"/>
        <value value="c13cd356-f147-5901-859d-31e6b2772465"/>
      </identifier>
    </targetReference>
  </relatesTo>
  ...
 </Composition>
```
Voraussetzung für die Einbettung des entsprechenden Verweises in die Meldung ist, dass z.B. das beauftragte Labor nach der Probenuntersuchung dem einsendenden Arzt die entsprechende Meldungs-Id aus der DEMIS-Labormeldung im Rahmen der Befundbereitstellung übermittelt hat. Wie und auf welchem Weg dies erfolgt, liegt außerhalb der Regelungshoheit von DEMIS. Denkbar ist hier beispielsweise die Übermittlung der Meldungs-Id vom Labor zum Einsender als Bestandteil des Laborbefunds. 

**Wichtig:** Unabhängig von der Verwendung des Composition.relatesTo Elements MUSS immer eine eigene Meldungs-Id zum jeweiligen Fall erzeugt und in Composition.identifier eingebettet werden.

Die in diesem Abschnitt getroffenen Aussagen beziehen sich ausschließlich auf die Referenzierung von Meldungen ANDERER Melder. Eine Nutzung von Composition.relatesTo ist für eigene Ergänzungsmeldungen NICHT zielführend, da hier bereits die Zusammenführung der Inhalte über Composition.identifier (s.o.) erfolgt.

---

### Relevante Ressourcen und deren Inhalte
Im Zusammenhang mit der Abbildung eines geeigneten Lifecyclemanagements für Erkrankungsmeldung ist es erforderlich, die innerhalb der definierten FHIR-Profile zu diesem Zweck genutzten Status-Elemente genauer zu charakterisieren. Die folgende Abbildung kennzeichnet am Beispiel der COVID-19-Meldung, die für das szenarioübergreifende Lifecyclemanagement relevanten Profile:

{{render:image-lifecycle-resources}}

---
#### NotificationDisease und abgeleitete Kind-Profile (NotificationDiseaseXYZD)
Folgende Inhalte der NotificationDisease bzw. deren abgeleitete Kind-Profile (NotificationDiseaseXYZD) besitzen einen engen Bezug zum Meldungslifecyclemanagement:

* **Composition.identifier** - Wie eingangs beschrieben, wird über dieses Feld die durch den Melder generierte *Meldungs-Id* abgebildet und somit ein logischer Bezugspunkt geschaffen, dem innerhalb des Lebenszyklusmanagements eine wesentliche Rolle zufällt. Über alle Meldevorgänge (Initialmeldung, Folgemeldungen) eines Melders hinweg, die sich auf einen konkreten Fall beziehen, muss die Meldungs-Id somit identisch bleiben.

* **Composition.status** - Das "status"-Feld der genutzten meldetatsbestandsspezifischen Composition-Ressource wird ebenfalls zur Abbildung ausgewählter Lebenszyklusaspekte genutzt:

  * Um die Anzahl der abzubildenden Szenarien nicht unnötig zu erhöhen, gehen wir davon aus, dass es normalerweise genau eine Meldung geben wird, wenn die Erkrankung oder der Tod in Bezug auf eine meldepflichtige Krankheit gemeldet wird. Deshalb wird in diesen Fällen für diese Meldung (Initialmeldung) der Status von NotificationDiseaseXYZD grundsätzlich auf **"final"** gesetzt. Mit dem Status "preliminary" wird in diesen Szenarien NICHT gearbeitet. Dies gilt explizit auch dann, wenn der Melder zum Zeitpunkt der Meldung bereits weiß, dass unter Umständen auch weitere Informationen in Folgemeldungen abgesetzt werden könnten. 

  * Anders sieht es bei der Meldung des Verdachts einer Erkrankung in Bezug auf eine meldepflichtige Krankheit aus: Hier gehen wir davon aus, dass es mindestens zwei Meldungen geben muss. Die erste Meldung (Initialmeldung) hat dabei grundsätzlich den Status **"preliminary"**. Folgemeldungen, die den Verdacht bestätigen oder verwerfen weisen dann den Status "final" aus. Solange der Verdacht einer Erkrankung weder bestätigt noch verworfen werden konnte, bleibt der Status "preliminary" bestehen. Dies gilt insbesondere dann, wenn die Meldung des Verdachts in Bezug auf ihre Inhalte ergänzt oder korrigiert wird.

  * Der Status **"amended"** wird grundsätzlich nur für Folgemeldungen genutzt, deren jeweilige Vorgängermeldungen den Status **"final"** ausgewiesen haben.  

  * Der Status **"entered-in-error"** DARF NICHT verwendet werden. 

---

#### Disease und abgeleitete Kind-Profile (DiseaseXYZD)
Folgende Inhalte der Disease bzw. deren Kind-Profile (DiseaseXYZD) besitzen einen engen Bezug zum Meldungslifecyclemanagement:

* **Condition.clinicalStatus** - Das Feld "clinicalStatus" - wird im Kontext der DEMIS-Erkrankungsmeldung genutzt, um eine Bewertung in Bezug auf das Vorliegen eines bestimmten meldepflichtigen Tatbestandes vorzunehmen. Der jeweilige Tatbestand wird dabei über "Condition.code" zum Ausdruck gebracht.

  * Der Status **"active"** wird gesetzt, sofern die meldende Person davon ausgeht, dass der in den rechtlichen Grundlagen definierte Meldetatbestand erfüllt ist. Dies wird somit in sämtlichen Initialmeldungen der Fall sein. 

  * **"inactive"** wird nur dann verwendet, wenn eine vorangegangene Meldung irrtümlich abgesetzt wurde und der gemeldete Tatbestand nicht länger erfüllt ist. Der entsprechende Status kann somit grundsätzlich nur in Folgemeldungen auftreten.

  * In der Hierarchie unterhalb von "activ" und "inactive" angesiedelte Konzepte ("recurrence | relapse" bzw. "remission | resolved") DÜRFEN NICHT genutzt werden.

* **Condition.verificationStatus** - Über das "verificationStatus" Feld wird abgebildet, ob der meldepflichtige Tatbestand die bestätigte Erkrankung ist (**"confirmed"**) oder der meldepflichtige Verdacht auf die Erkrankung (**"unconfirmed"**). Damit ist dieses Feld für Meldekategorien, bei denen lediglich Erkrankungen und Tod, nicht aber der Verdacht auf die Erkrankung meldepflichtig sind, immer als **"confirmed"** zu kennzeichnen. Darüber hinaus kann der Status **"refuted"** genutzt werden, um kenntlich zu machen, dass ein gemeldeter Verdacht widerlegt wurde. Wurde ein Meldetatbestand irrtümlich gemeldet kann dies in Folgemeldungen über **"entered-in-error"** kenntlich gemacht werden.

* **Condition.note** - Über das "note" Feld ist es für die meldende Person bei Bedarf möglich, textuelle Zusatzinformationen, die den Lebenszyklus betreffen, an das Gesundheitsamt zu senden. Dies empfiehlt sich insbesondere dann, wenn vorangegangene Meldungen fehlerhaft waren oder irrtümlich abgesetzt wurden.

---

#### DiseaseInformationCommon, DiseaseInformation und abgeleitete Kind-Profile (DiseaseInformationXYZD)
Folgende Inhalte der jeweiligen QuestionnaireResponses besitzen einen engen Bezug zum Meldungslifecyclemanagement: 

* **QuestionnaireResponse.status** - Über das "status" Feld wird eine Aussage in Bezug auf den Fertigstellungsgrad des Fragebogens getroffen. Im Zusammenhang mit der DEMIS-Erkrankungsmeldung darf der Wert nur die Ausprägungen **"completed"** und **"amended"** annehmen.

  * Ein ausgefüllter Fragebogen hat immer den Status **"completed"**, wenn er keine Änderungen zu Vorgängermeldungen enthält. Ausgefüllte Fragebögen in Initialmeldungen haben somit grundsätzlich den Status **"completed"**. 

  * Ein ausgefüllter Fragebogen ist im Umkehrschluss immer dann als **"amended"** zu kennzeichnen, wenn sich Inhalte im Vergleich zur Initialmeldung und etwaigen Folgemeldungen verändert haben. Als Veränderungen gelten auch Anpassungen an Ressourcen, auf welche aus dem ausgefüllten Fragebogen heraus verwiesen wird (Hospitalization etc.).

  * Die Ausprägungen "in-progress", "entered-in-error" und "stopped" DÜRFEN NICHT verwendet werden.

**Hinweis:** In Folgemeldungen (Korrektur- oder Ergänzungsmeldungen) können Änderungen den allgemeinen Fragebogen betreffen oder den Meldekategorie-spezifischen Fragebogen. Entsprechend wird der QuestionnaireResponse.status entweder im DiseaseInformationCommon oder im DiseaseInforamtionXYZD von "completed" auf "amended" gesetzt.

---

#### Zusammenspiel der verschiedenen Statusangaben
Im Zusammenspiel der verschiedenen Angaben kann das die jeweiligen Initial- und Folgemeldungen empfangende Gesundheitsamt einen Rückschluss auf den jeweils aktuellen Zustand der vorliegenden Meldung ziehen. Entsprechende Informationen können dann automatisiert oder ggf. auch teilautomatisiert in die Falldokumentation übernommen werden. Im folgenden Abschnitt wird das konkrete Zusammenspiel der einzelnen Statusinformationen anhand konkreter Szenarien weiter vertieft. 

---

## Szenarien

### Überblick
Das folgende Zustandsdiagramm stellt ausgewählte Aspekte des Lifecyclemanagements der DEMIS-Erkrankungsmeldung im Überblick dar. Der üblichen UML-Notation folgend, definieren die an den Pfeilen in eckigen Klammern stehenden Beschreibungen die Auslöser (Guards) für einen Zustandsübergang. Die dahinter stehende Beschreibung definiert den Effekt, der beim Zustandsübergang auftritt. In unserem Fall ist der Effekt immer das Absetzen einer DEMIS-Meldung, die bestimmte Anforderungen an Inhalt und Struktur erfüllt.

{{render:image-lifecycle-scenarios}}

* **S** - Szenario
* **IM** - Initialmeldung
* **FM** - Folgemeldung
* **V** - Verdacht
* **E** - Erkrankung
* **T** - Tod
* **I** - Irrtum
* **V2E** - Verdacht "to" Irrtum ...

---

Die folgende Tabelle gibt einen groben Überblick über die sich aus dem Zustandsdiagramm ableitenden Szenarien (Effekten), in deren Zusammenhang eine Meldung (Initial- oder Folgemeldung) an DEMIS erfolgen muss. Für jedes Szenario wird dabei angegeben welche Ausprägungen die jeweiligen Statusangaben in den involvierten Ressourcen annehmen müssen. In den nachfolgenden Abschnitten werden die Einzelszenarien dann noch einmal detaillierter beschrieben. Eine entsprechende Zuordnung zwischen Abbildung, Tabelle und Erläuterung kann anhand der Szenario-Id vorgenommen werden.

|Szenario-Id|Beschreibung&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;|NotificationDiseaseXYZD<br>Composition|DiseaseXYZD<br>Condition|DiseaseInformationCommon<br>QuestionnaireResponse|DiseaseInformationXYZD<br>QuestionnaireResponse|
|-
|**S_IM_V**|**Initialmeldung** - Informationen zum Vorliegen des Verdachts einer Erkrankung in Bezug auf eine der meldepflichtigen Krankheiten|status=preliminary|clinicalStatus=active<br>verificationStatus=unconfirmed|status=completed|status=completed|
|**S_FM_V2V**|**Folgemeldung** - Ergänzen/Korrigieren von Inhalten mit Bezug zum gemeldeten Verdacht|status=preliminary|	clinicalStatus=active<br>verificationStatus=unconfirmed|status=completed OR amended|status=completed OR amended|
|**S_FM_V2I**|**Folgemeldung** - Information an das Gesundheitsamt, dass sich der bereits gemeldete Verdacht NICHT bestätigt hat|status=final|clinicalStatus=inactive<br>verificationStatus=refuted<br>note=...<br><br>Condition.note soll befüllt werden, um ggf. weitere Hintergrundinformationen zum Irrtum bereitzustellen|status=completed OR amended|status=completed OR amended|
|**S_FM_V2E**|**Folgemeldung** - Information an das Gesundheitsamt, dass sich der bereits gemeldete Verdacht bestätigt hat|status=final|clinicalStatus=active<br>verificationStatus=confirmed|status=completed OR amended|status=completed OR amended|
|**S_FM_V2T**|**Folgemeldung** - Information an das Gesundheitsamt, dass sich der gemeldete Verdacht bestätigt hat und die betroffene Person in Bezug auf die gemeldete Krankheit verstorben ist|status=final|clinicalStatus=active<br>verificationStatus=confirmed|status=amended<br><br>DiseaseInformationCommon muss Angaben zum Tod beinhalten|status=completed OR amended|
|**S_IM_E**|**Initialmeldung** - Information zum Vorliegen einer Erkrankung in Bezug auf eine der meldepflichtigen Krankheiten|status=final|clinicalStatus=active<br>verificationStatus=confirmed|status=completed|status=completed|
|**S_FM_E2E**|**Folgemeldung** - Ergänzen/Korrigieren von Inhalten mit Bezug zur gemeldeten Erkrankung|status=amended|clinicalStatus=active<br>verificationStatus=confirmed|status=completed OR amended|status=completed OR amended|
|**S_FM_E2I**|**Folgemeldung** - Information an das Gesundheitsamt, dass das Vorliegen eine Erkrankung irrtümlich gemeldet wurde|status=amended|clinicalStatus=inactive<br>verificationStatus=entered-in-error<br>note=...<br><br>Condition.note soll befüllt werden, um ggf. weitere Hintergrundinformationen zum Irrtum bereitzustellen|status=completed OR amended|status=completed OR amended|
|**S_FM_E2T**|**Folgemeldung** - Information an das Gesundheitsamt, dass die betroffene Person in Bezug auf die zuvor gemeldete Erkrankung verstorben ist|status=amended|clinicalStatus=active<br>verificationStatus=confirmed|status=amended<br><br>DiseaseInformationCommon muss Angaben zum Tod beinhalten|status=completed OR amended|
|**S_IM_T**|**Initialmeldung** - Information zum Vorliegen eines Todesfalls in Bezug auf eine der meldepflichtigen Krankheiten|status=final|clinicalStatus=active<br>verificationStatus=confirmed|status=completed<br><br>DiseaseInformationCommon muss Angaben zum Tod beinhalten|status=completed|
|**S_FM_T2T**|**Folgemeldung** - Ergänzen/Korrigieren von Inhalten mit Bezug zum gemeldeten Todesfall|status=amended|clinicalStatus=active<br>verificationStatus=confirmed|status=completed | amended<br><br>DiseaseInformationCommon muss Angaben zum Tod beinhalten|status=completed OR amended|
|**S_FM_T2V**|**Folgemeldung** - Information an das Gesundheitsamt, dass der Tod irrtümlich gemeldet wurde und weiterhin ausschließlich ein Verdacht besteht|status=amended|clinicalStatus=active<br>verificationStatus=unconfirmed<br>note=...<br><br>Condition.note soll befüllt werden, um ggf. weitere Hintergrundinformationen zum Irrtum bereitzustellen|status=amended<br><br>Angaben zum Tod müssen in DiseaseInformationCommon korrigiert worden sein|status=completed OR amended|
|**S_FM_T2E**|**Folgemeldung** - Information an das Gesundheitsamt, dass der Tod irrtümlich gemeldet wurde und weiterhin ausschließlich eine Erkrankung vorliegt|status=amended|clinicalStatus=active<br>verificationStatus=confirmed<br>note=...<br><br>Condition.note soll befüllt werden, um ggf. weitere Hintergrundinformationen zum Irrtum bereitzustellen|status=amended<br><br>Angaben zum Tod müssen in DiseaseInformationCommon korrigiert worden sein|status=completed OR amended|
|**S_FM_T2I_1**|**Folgemeldung** - Information an das Gesundheitsamt, dass die betroffene Person zwar verstorben ist, ein Meldetatbestand aber nicht erfüllt ist|status=amended|clinicalStatus=inactive<br>verificationStatus=entered-in-error<br>note=...<br><br>Condition.note MUSS befüllt werden, um ggf. weitere Hintergrundinformationen zum Irrtum bereitzustellen|status=completed OR amended|status=completed OR amended|
|**S_FM_T2I_2**|**Folgemeldung** - Information an das Gesundheitsamt, dass eine Person irrtümlich als verstorben in Bezug auf eine meldepflichtige Krankheit gemeldet wurde, die letztlich nicht vorliegt|status=amended|clinicalStatus=inactive<br>verificationStatus=entered-in-error<br>note=...<br><br>Condition.note MUSS befüllt werden, um ggf. weitere Hintergrundinformationen zum Irrtum bereitzustellen|status=amended<br><br>Angaben zum Tod müssen in DiseaseInformationCommon korrigiert worden sein|status=completed OR amended|

---

### S_IM_V - Initialmeldung - Informationen zum Vorliegen des Verdachts einer Erkrankung in Bezug auf eine der meldepflichtigen Krankheiten

Sobald eine meldepflichtige Person den Verdacht hat, dass eine Erkrankung in Bezug auf eine der meldepflichtigen Krankheiten vorliegt, muss sie diesen Verdacht entsprechend über DEMIS an das zuständige Gesundheitsamt melden. Da es sich um ihre erste Meldung zu diesem Fall handelt, sprechen wir in diesem Zusammenhang von einer Initialmeldung. Diese Meldung muss gemäß §9 Absatz 3 IfSG unverzüglich erfolgen und dem zuständigen Gesundheitsamt spätestens 24 Stunden, nachdem der Meldende Kenntnis erlangt hat, vorliegen.

Innerhalb dieser Meldungen nehmen die einzelnen Statusinformationen in den FHIR-Ressourcen die folgenden Werte an:

* **NotificationDiseaseXYZD**
  * _Composition.status_ = **preliminary**<br>Da lediglich ein Verdacht gemeldet wird, ist offensichtlich, dass noch mit mindestens einer Folgemeldung zu diesem Fall zu rechnen ist. Über diese Folgemeldung würde der entsprechende Verdacht dann bestätigt oder entkräftet werden. Entsprechend wird die Meldung (Composition) als "vorläufig" ("preliminary") gekennzeichnet.

* **DiseaseXYZD**
  * _Condition.clinicalStatus_ = **active**<br>Aus Sicht der meldepflichtigen Person liegt ein entsprechend in Condition.code referenzierter Meldetatbestand vor. Entsprechend wird clinicalStatus auf "active" gesetzt

  * _Condition.verificationStatus_ = **unconfirmed**<br>Da lediglich ein Verdacht gemeldet wird, ist der wird der Status der Verifikation des Meldetatbestands mit "unbestätigt" ("unconfirmed") angegeben.

* **DiseaseInformationCommon**
  * _QuestionnaireResponse.status_ = **completed**<br>Da unausgefüllte Fragebögen vom System abgelehnt werden, müssen zu den relevanten Inhalten zwingend Angaben gemacht werden. Selbst wenn in den entsprechenden Angaben von NullFlavors Gebrauch gemacht wird, gilt die QuestionnaireResponse als ausgefüllt und kann somit den Status "completed" tragen.

* **DiseaseInformationXYZD**
  * _QuestionnaireResponse.status_ = **completed**<br>Da unausgefüllte Fragebögen vom System abgelehnt werden, müssen zu den relevanten Inhalten zwingend Angaben gemacht werden. Selbst wenn in den entsprechenden Angaben von NullFlavors Gebrauch gemacht wird, gilt die QuestionnaireResponse als ausgefüllt und kann somit den Status "completed" tragen. 

---

### S_FM_V2V - Folgemeldung - Ergänzen/Korrigieren von Inhalten mit Bezug zum gemeldeten Verdacht
Sobald der meldepflichtigen Person zum gemeldeten Verdacht zusätzliche Informationen bekannt werden oder bereits gemeldete Informationen fehlerhaft waren, muss entsprechend §9 Absatz 3 IfSG unverzüglich eine Folgemeldung (Nachmeldung/Korrektur) über DEMIS an das zuständige Gesundheitsamt gesendet werden. In diesem speziellen Szenario wird davon ausgegangen, dass trotz der neuen/geänderten Informationen der Verdacht weiterhin besteht. 

Innerhalb dieser Meldungen nehmen die einzelnen Statusinformationen in den FHIR-Ressourcen die folgenden Werte an:

* **NotificationDiseaseXYZD**
  * _Composition.status_ = **preliminary**<br>Da lediglich Informationen bezogen auf einen bestehenden Verdacht ergänzt und/oder korrigiert werden, ist offensichtlich, dass noch mit mindestens einer weiteren Folgemeldung zu diesem Fall zu rechnen ist. Über diese Folgemeldung würde der entsprechende Verdacht dann bestätigt oder entkräftet werden. Entsprechend wird die Meldung (Composition) weiterhin als "vorläufig" ("preliminary") gekennzeichnet.

* **DiseaseXYZD**
  * _Condition.clinicalStatus_ = **active**<br>Aus Sicht der meldepflichtigen Person liegt weiterhin ein entsprechend in Condition.code referenzierter Meldetatbestand vor. Entsprechend wird clinicalStatus auch weiterhin auf "active" gesetzt

  * _Condition.verificationStatus_ = **unconfirmed**<br>Da auch weiterhin lediglich ein Verdacht besteht, wird der Status der Verifikation des Meldetatbestands ebenfalls weiterhin mit "unbestätigt" ("unconfirmed") angegeben.

* **DiseaseInformationCommon**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

* **DiseaseInformationXYZD**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten.

---

### S_FM_V2I - Folgemeldung - Information an das Gesundheitsamt, dass sich der bereits gemeldete Verdacht NICHT bestätigt hat
Sobald der meldepflichtigen Person zum gemeldeten Verdacht zusätzliche Informationen bekannt werden oder bereits gemeldete Informationen fehlerhaft waren, muss entsprechend §9 Absatz 3 IfSG unverzüglich eine Folgemeldung (Nachmeldung/Korrektur) über DEMIS an das zuständige Gesundheitsamt gesendet werden. In diesem speziellen Szenario wird davon ausgegangen, dass sich der bereits gemeldete Verdacht NICHT bestätigt hat und dies entsprechend mitgeteilt werden soll.

Innerhalb dieser Meldungen nehmen die einzelnen Statusinformationen in den FHIR-Ressourcen die folgenden Werte an:

* **NotificationDiseaseXYZD**
  * _Composition.status_ = **final**<br>Da sich der Verdacht nicht bestätigt hat, können wir nun davon ausgehen, dass wir eine letzte Meldung zum entsprechenden Fall senden werden. Da der Status in sämtlichen Vorgängermeldungen auf "preliminary" gesetzt war, können wir ihn nun auf "final" ändern.

* **DiseaseXYZD**
  * _Condition.clinicalStatus_ = **inactive**<br>Aus Sicht der meldepflichtigen Person wird der in Condition.code referenzierter Meldetatbestand nun nicht mehr erfüllt. Entsprechend wird clinicalStatus auf "inactive" gesetzt.

  * _Condition.verificationStatus_ = **refuted**<br>Der Verdacht hat sich nicht erhärtet/wurde widerlegt. Entsprechend wird der Status auf "refuted" gesetzt. 

  * _Condition.note_ = **[weiterführende Informationen]**<br>Sofern es aus Sicht der meldepflichtigen Person sinnvoll erscheint, dem Gesundheitsamt weiterführende Informationen zum entsprechenden Umstand mitzugeben, kann dies über dieses Feld erfolgen. Vorstellbar wäre in diesem konkreten Szenario beispielsweise folgende Information: "Nach einem negativen Laborbefund hat sich der geäußerte Verdacht nicht erhärtet..." 

* **DiseaseInformationCommon**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

* **DiseaseInformationXYZD**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten.  

---

### S_FM_V2E - Folgemeldung - Information an das Gesundheitsamt, dass sich der bereits gemeldete Verdacht bestätigt hat
Sobald der meldepflichtigen Person zum gemeldeten Verdacht zusätzliche Informationen bekannt werden oder bereits gemeldete Informationen fehlerhaft waren, muss entsprechend §9 Absatz 3 IfSG unverzüglich eine Folgemeldung (Nachmeldung/Korrektur) über DEMIS an das zuständige Gesundheitsamt gesendet werden. In diesem speziellen Szenario wird davon ausgegangen, dass sich der bereits gemeldete Verdacht bestätigt hat und dies entsprechend mitgeteilt werden soll. 

Innerhalb dieser Meldungen nehmen die einzelnen Statusinformationen in den FHIR-Ressourcen die folgenden Werte an:

* **NotificationDiseaseXYZD**
  * _Composition.status_ = **final**<br>Da sich der Verdacht bestätigt hat, können wir der Einfachheit halber nun davon ausgehen, dass wir eine letzte Meldung zum entsprechenden Fall senden werden. Da der Status in sämtlichen Vorgängermeldungen auf "preliminary" gesetzt war, können wir ihn nun auf "final" ändern.

* **DiseaseXYZD**
  * _Condition.clinicalStatus_ = **active**<br>Aus Sicht der meldepflichtigen Person liegt weiterhin ein entsprechend in Condition.code referenzierter Meldetatbestand vor. Entsprechend wird clinicalStatus auch weiterhin auf "active" gesetzt

  * _Condition.verificationStatus_ = **confirmed**<br>Der Verdacht hat sich in diesem Szenario bestätigt. Entsprechend ändern wir den verificationStatus nun auf "confirmed". 

* **DiseaseInformationCommon**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

* **DiseaseInformationXYZD**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

---

### S_FM_V2T - Folgemeldung - Information an das Gesundheitsamt, dass sich der gemeldete Verdacht bestätigt hat und die betroffene Person in Bezug auf die gemeldete Krankheit verstorben ist
Sobald der meldepflichtigen Person zum gemeldeten Verdacht zusätzliche Informationen bekannt werden oder bereits gemeldete Informationen fehlerhaft waren, muss entsprechend §9 Absatz 3 IfSG unverzüglich eine Folgemeldung (Nachmeldung/Korrektur) über DEMIS an das zuständige Gesundheitsamt gesendet werden. In diesem speziellen Szenario wird davon ausgegangen, dass die betroffene Person mittlerweile in Bezug auf die als Verdacht gemeldete Krankheit verstorben ist.

Innerhalb dieser Meldungen nehmen die einzelnen Statusinformationen in den FHIR-Ressourcen die folgenden Werte an:

* **NotificationDiseaseXYZD**
  * _Composition.status_ = **final**<br>Da sich der Verdacht bestätigt hat und der Patient in Bezug auf die Krankheit verstorben ist, können wir der Einfachheit halber nun davon ausgehen, dass wir eine letzte Meldung zum entsprechenden Fall senden werden. Da der Status in sämtlichen Vorgängermeldungen auf "preliminary" gesetzt war, können wir ihn nun auf "final" ändern.

* **DiseaseXYZD**
  * _Condition.clinicalStatus_ = **active**<br>Aus Sicht der meldepflichtigen Person liegt weiterhin ein entsprechend in Condition.code referenzierter Meldetatbestand vor. Entsprechend wird clinicalStatus auch weiterhin auf "active" gesetzt

  * _Condition.verificationStatus_ = **confirmed**<br>Der Verdacht hat sich in diesem Szenario bestätigt. Entsprechend ändern wir den verificationStatus nun auf "confirmed". 

* **DiseaseInformationCommon**
  * _QuestionnaireResponse.status_ = **amended**<br>Da die betroffene Person in diesem Szenario verstorben ist, müssen sich zwingend Angaben im entsprechenden Fragebogen zum Tod ändern. Der status ist somit auf "amended" zu setzen. 

* **DiseaseInformationXYZD**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten.

---

### S_IM_E - Initialmeldung - Information zum Vorliegen einer Erkrankung in Bezug auf eine der meldepflichtigen Krankheiten
Sobald eine Erkrankung in Bezug auf eine der meldepflichtigen Krankheiten festgestellt wird, muss diese durch die meldepflichtige Person über DEMIS an das zuständige Gesundheitsamt gemeldet werden. Da es sich um ihre erste Meldung zu diesem Fall handelt, sprechen wir in diesem Zusammenhang von einer Initialmeldung. Diese Meldung muss gemäß §9 Absatz 3 IfSG unverzüglich erfolgen und dem zuständigen Gesundheitsamt spätestens 24 Stunden, nachdem der Meldende Kenntnis erlangt hat, vorliegen.

Innerhalb dieser Meldungen nehmen die einzelnen Statusinformationen in den FHIR-Ressourcen die folgenden Werte an:

* **NotificationDiseaseXYZD**
  * _Composition.status_ = **final**<br>Wir treffen an dieser Stelle die vereinfachende Annahme, dass der meldepflichtigen Person bereits zum Zeitpunkt der Initialmeldung sämtliche benötigten Informationen vorliegen und die Meldung somit als "final" betrachtet werden kann. Sollten weitere Details zum jeweiligen Fall bekannt werden, können und müssen diese dem zuständigen Gesundheitsamt dennoch in Folgemeldungen bekannt gemacht werden (vgl. die Szenarien S_FM_E2E, S_FM_E2I und S_FM_E2T).

* **DiseaseXYZD**
  * _Condition.clinicalStatus_ = **active**<br>Aus Sicht der meldepflichtigen Person liegt ein entsprechend in Condition.code referenzierter Meldetatbestand vor. Entsprechend wird clinicalStatus auf "active" gesetzt

  * _Condition.verificationStatus_ = **confirmed**<br>Da nicht nur ein Verdacht sondern das Vorliegen einer Erkrankung gemeldet wird, wird der Status der Verifikation des Meldetatbestands mit "bestätigt" ("confirmed") angegeben.

* **DiseaseInformationCommon**
  * _QuestionnaireResponse.status_ = **completed**<br>Da unausgefüllte Fragebögen vom System abgelehnt werden, müssen zu den relevanten Inhalten zwingend Angaben gemacht werden. Selbst wenn in den entsprechenden Angaben von NullFlavors Gebrauch gemacht wird, gilt die QuestionnaireResponse als ausgefüllt und kann somit den Status "completed" tragen. 

* **DiseaseInformationXYZD**
  * _QuestionnaireResponse.status_ = **completed**<br>Da unausgefüllte Fragebögen vom System abgelehnt werden, müssen zu den relevanten Inhalten zwingend Angaben gemacht werden. Selbst wenn in den entsprechenden Angaben von NullFlavors Gebrauch gemacht wird, gilt die QuestionnaireResponse als ausgefüllt und kann somit den Status "completed" tragen. 

---

### S_FM_E2E - Folgemeldung - Ergänzen/Korrigieren von Inhalten mit Bezug zur gemeldeten Erkrankung
Sobald der meldepflichtigen Person zur gemeldeten Erkrankung zusätzliche Informationen bekannt werden oder bereits gemeldete Informationen fehlerhaft waren, muss entsprechend §9 Absatz 3 IfSG unverzüglich eine Folgemeldung (Nachmeldung/Korrektur) über DEMIS an das zuständige Gesundheitsamt gesendet werden. In diesem speziellen Szenario wird davon ausgegangen, dass sich lediglich kleinere Anpassungen am Meldungsinhalt ergeben. 

Innerhalb dieser Meldungen nehmen die einzelnen Statusinformationen in den FHIR-Ressourcen die folgenden Werte an:

* **NotificationDiseaseXYZD**
  * _Composition.status_ = **amended**<br>Da die zuvor versandte Meldung den Status "final" im entsprechenden Feld auswies, muss nun darauf hingewiesen werden, dass sich Inhalte geändert haben. Das Feld ist entsprechend auf "amended" zu setzen.

* **DiseaseXYZD
  * _Condition.clinicalStatus_ = **active**<br>Aus Sicht der meldepflichtigen Person liegt weiterhin ein entsprechend in Condition.code referenzierter Meldetatbestand vor. Entsprechend wird clinicalStatus auch weiterhin auf "active" gesetzt.

  * _Condition.verificationStatus_ = **confirmed**<br>Auch am Status der Verifikation des Meldetatbestands ändert sich nichts. Er wird weiterhin mit "bestätigt" ("confirmed") angegeben.

* **DiseaseInformationCommon**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

* **DiseaseInformationXYZD**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

---

### S_FM_E2I - Folgemeldung - Information an das Gesundheitsamt, dass das Vorliegen eine Erkrankung irrtümlich gemeldet wurde
Sobald der meldepflichtigen Person zur gemeldeten Erkrankung zusätzliche Informationen bekannt werden oder bereits gemeldete Informationen fehlerhaft waren, muss entsprechend §9 Absatz 3 IfSG unverzüglich eine Folgemeldung (Nachmeldung/Korrektur) über DEMIS an das zuständige Gesundheitsamt gesendet werden. In diesem speziellen Szenario wird davon ausgegangen, dass die Erkrankung einer Person irrtümlich gemeldet wurde und nun das Gesundheitsamt über diesen Sachverhalt informiert werden soll.

Innerhalb dieser Meldungen nehmen die einzelnen Statusinformationen in den FHIR-Ressourcen die folgenden Werte an:

* **NotificationDiseaseXYZD**
  * _Composition.status_ = **amended**<br>Da die zuvor versandte Meldung den Status "final" im entsprechenden Feld auswies, muss nun darauf hingewiesen werden, dass sich Inhalte geändert haben. Das Feld ist entsprechend auf "amended" zu setzen.

* **DiseaseXYZD**
  * _Condition.clinicalStatus_ = **inactive**<br>Aus Sicht der meldepflichtigen Person wird der in Condition.code referenzierter Meldetatbestand nun nicht mehr erfüllt. Entsprechend wird clinicalStatus auf "inactive" gesetzt.

  * _Condition.verificationStatus_ = **entered-in-error**<br>Es lag keine entsprechend meldepflichtige Erkrankung vor. Der Verifikationsstatus wird daher auf "entered-in-error" geändert. 

  * _Condition.note_ = **[weiterführende Informationen]**<br>Sofern es aus Sicht der meldepflichtigen Person sinnvoll erscheint, dem Gesundheitsamt weiterführende Informationen zum entsprechenden Umstand mitzugeben, kann dies über dieses Feld erfolgen. Vorstellbar wäre in diesem konkreten Szenario beispielsweise folgende Information: "Durch eine falsche Kodierung im System kam es irrtümlich zu einer Meldung. Ein entsprechender Meldetatbestand ist aktuell nicht mehr gegeben." 

* **DiseaseInformationCommon**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

* **DiseaseInformationXYZD**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

---

### S_FM_E2T - Folgemeldung - Information an das Gesundheitsamt, dass die betroffene Person in Bezug auf die zuvor gemeldete Erkrankung verstorben ist
Sobald der meldepflichtigen Person zur gemeldeten Erkrankung zusätzliche Informationen bekannt werden oder bereits gemeldete Informationen fehlerhaft waren, muss entsprechend §9 Absatz 3 IfSG unverzüglich eine Folgemeldung (Nachmeldung/Korrektur) über DEMIS an das zuständige Gesundheitsamt gesendet werden. In diesem speziellen Szenario wird davon ausgegangen, dass die betroffene Person mittlerweile in Bezug auf die gemeldete Krankheit verstorben ist. 

Innerhalb dieser Meldungen nehmen die einzelnen Statusinformationen in den FHIR-Ressourcen die folgenden Werte an:

* **NotificationDiseaseXYZD**
  * _Composition.status_ = **amended**<br>Da die zuvor versandte Meldung den Status "final" im entsprechenden Feld auswies, muss nun darauf hingewiesen werden, dass sich Inhalte geändert haben. Das Feld ist entsprechend auf "amended" zu setzen.

* **DiseaseXYZD**
  * _Condition.clinicalStatus_ = **active**<br>Aus Sicht der meldepflichtigen Person liegt weiterhin ein entsprechend in Condition.code referenzierter Meldetatbestand vor. Entsprechend wird clinicalStatus auch weiterhin auf "active" gesetzt

  * _Condition.verificationStatus_ = **confirmed**<br>Auch am Status der Verifikation des Meldetatbestands ändert sich nichts. Er wird weiterhin mit "bestätigt" ("confirmed") angegeben. 

* **DiseaseInformationCommon**
  * _QuestionnaireResponse.status_ = **amended**<br>Da die betroffene Person in diesem Szenario verstorben ist, müssen sich zwingend Angaben im entsprechenden Fragebogen zum Tod ändern. Der status ist somit auf "amended" zu setzen. 

* **DiseaseInformationXYZD**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

---

### S_IM_T - Initialmeldung - Information zum Vorliegen eines Todesfalls in Bezug auf eine der meldepflichtigen Krankheiten
Sobald der Tod in Bezug auf eine der meldepflichtigen Krankheiten festgestellt wird, muss dieser durch die meldepflichtige Person über DEMIS an das zuständige Gesundheitsamt gemeldet werden. Da es sich um ihre erste Meldung zu diesem Fall handelt, sprechen wir in diesem Zusammenhang von einer Initialmeldung. Diese Meldung muss gemäß §9 Absatz 3 IfSG unverzüglich erfolgen und dem zuständigen Gesundheitsamt spätestens 24 Stunden, nachdem der Meldende Kenntnis erlangt hat, vorliegen.

Innerhalb dieser Meldungen nehmen die einzelnen Statusinformationen in den FHIR-Ressourcen die folgenden Werte an:

* **NotificationDiseaseXYZD**
  * _Composition.status_ = **final**<br>Wir treffen an dieser Stelle die vereinfachende Annahme, dass der meldepflichtigen Person bereits zum Zeitpunkt der Initialmeldung sämtliche benötigten Informationen vorliegen und die Meldung somit als "final" betrachtet werden kann. Sollten weitere Details zum jeweiligen Fall bekannt werden, können und müssen diese dem zuständigen Gesundheitsamt dennoch in Folgemeldungen bekannt gemacht werden (vgl. andere Szenarien).

* **DiseaseXYZD**
  * _Condition.clinicalStatus_ = **active**<br>Aus Sicht der meldepflichtigen Person liegt ein entsprechend in Condition.code referenzierter Meldetatbestand vor. Entsprechend wird clinicalStatus auf "active" gesetzt

  * _Condition.verificationStatus_ = **confirmed**<br>Da nicht nur ein Verdacht sondern der Tod in Bezug auf die Krankheit gemeldet wird, wird der Status der Verifikation des Meldetatbestands mit "bestätigt" ("confirmed") angegeben.

* **DiseaseInformationCommon
  * _QuestionnaireResponse.status_ = **completed**<br>Da unausgefüllte Fragebögen vom System abgelehnt werden, müssen zu den relevanten Inhalten zwingend Angaben gemacht werden. Selbst wenn in den entsprechenden Angaben von NullFlavors Gebrauch gemacht wird, gilt die QuestionnaireResponse als ausgefüllt und kann somit den Status "completed" tragen. Im ausgefüllten Fragebogen müssen zwingend die Angabe zum Tod der betroffenen Person hinterlegt werden.

* **DiseaseInformationXYZD**
  * _QuestionnaireResponse.status_ = **completed**<br>Da unausgefüllte Fragebögen vom System abgelehnt werden, müssen zu den relevanten Inhalten zwingend Angaben gemacht werden. Selbst wenn in den entsprechenden Angaben von NullFlavors Gebrauch gemacht wird, gilt die QuestionnaireResponse als ausgefüllt und kann somit den Status "completed" tragen. 

---

### S_FM_T2T - Folgemeldung - Ergänzen/Korrigieren von Inhalten mit Bezug zum gemeldeten Todesfall
Sobald der meldepflichtigen Person zum gemeldeten Todesfall zusätzliche Informationen bekannt werden oder bereits gemeldete Informationen fehlerhaft waren, muss entsprechend §9 Absatz 3 IfSG unverzüglich eine Folgemeldung (Nachmeldung/Korrektur) über DEMIS an das zuständige Gesundheitsamt gesendet werden. In diesem speziellen Szenario wird davon ausgegangen, dass sich lediglich kleinere Anpassungen am Meldungsinhalt ergeben. 

Innerhalb dieser Meldungen nehmen die einzelnen Statusinformationen in den FHIR-Ressourcen die folgenden Werte an:

* **NotificationDiseaseXYZD**
  * _Composition.status_ = **amended**<br>Da die zuvor versandte Meldung den Status "final" im entsprechenden Feld auswies, muss nun darauf hingewiesen werden, dass sich Inhalte geändert haben. Das Feld ist entsprechend auf "amended" zu setzen.

* **DiseaseXYZD**
  * _Condition.clinicalStatus_ = **active**<br>Aus Sicht der meldepflichtigen Person liegt weiterhin ein entsprechend in Condition.code referenzierter Meldetatbestand vor. Entsprechend wird clinicalStatus auch weiterhin auf "active" gesetzt.

  * _Condition.verificationStatus_ = **confirmed**<br>Auch am Status der Verifikation des Meldetatbestands ändert sich nichts. Er wird weiterhin mit "bestätigt" ("confirmed") angegeben.

* **DiseaseInformationCommon**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

* **DiseaseInformationXYZD**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

---

### S_FM_T2V - Folgemeldung - Information an das Gesundheitsamt, dass der Tod irrtümlich gemeldet wurde und weiterhin ausschließlich ein Verdacht besteht
Sobald der meldepflichtigen Person zum gemeldeten Todesfall zusätzliche Informationen bekannt werden oder bereits gemeldete Informationen fehlerhaft waren, muss entsprechend §9 Absatz 3 IfSG unverzüglich eine Folgemeldung (Nachmeldung/Korrektur) über DEMIS an das zuständige Gesundheitsamt gesendet werden. In diesem speziellen Szenario wird davon ausgegangen, dass der Tod der betroffenen Person (z.B. aufgrund einer Verwechslung) irrtümlich als verstorben gemeldet wurde und doch lediglich nur ein Verdachtsfall vorliegt. 

Innerhalb dieser Meldungen nehmen die einzelnen Statusinformationen in den FHIR-Ressourcen die folgenden Werte an:

* **NotificationDiseaseXYZD**
  * _Composition.status_ = **amended**<br>Da die zuvor versandte Meldung den Status "final" im entsprechenden Feld auswies, muss nun darauf hingewiesen werden, dass sich Inhalte geändert haben. Das Feld ist entsprechend auf "amended" zu setzen.

* **DiseaseXYZD**
  * _Condition.clinicalStatus_ = **active**<br>Aus Sicht der meldepflichtigen Person liegt weiterhin ein entsprechend in Condition.code referenzierter Meldetatbestand vor. Entsprechend wird clinicalStatus auch weiterhin auf "active" gesetzt.

  * _Condition.verificationStatus_ = **unconfirmed**<br>In diesem speziellen Szenario liegt lediglich ein Verdacht vor. Der Status ist entsprechend auf "unbestätigt" ("unconfirmed") zu setzen.

  * _Condition.note_ = **[weiterführende Informationen]**<br> In diesem speziellen Szenario ist es sicher sinnvoll, dass die meldepflichtige Person dem Gesundheitsamt weiterführende Informationen zum entsprechenden Irrtum mitteilt. Dies kann über Freitext innerhalb dieses Feldes erfolgen. Vorstellbar wäre in diesem konkreten Szenario beispielsweise folgende Information: "Durch eine Verwechselung haben wir fälschlicherweise einen Todesfall im Zusammenhang mit COVID-19 gemeldet. Die entsprechende Person ist NICHT verstorben. Ein entsprechender Verdacht liegt jedoch weiterhin vor." 

* **DiseaseInformationCommon**
  * _QuestionnaireResponse.status_ = **amended**<br>Die Falschangaben zum Tod der betroffenen Person müssen zwingend innerhalb des ausgefüllten Fragebogens korrigiert werden. Entsprechend ist der status mit "amended" anzugeben. 

* **DiseaseInformationXYZD**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

---

### S_FM_T2E - Folgemeldung - Information an das Gesundheitsamt, dass der Tod irrtümlich gemeldet wurde und weiterhin ausschließlich eine Erkrankung vorliegt
Sobald der meldepflichtigen Person zum gemeldeten Todesfall zusätzliche Informationen bekannt werden oder bereits gemeldete Informationen fehlerhaft waren, muss entsprechend §9 Absatz 3 IfSG unverzüglich eine Folgemeldung (Nachmeldung/Korrektur) über DEMIS an das zuständige Gesundheitsamt gesendet werden. In diesem speziellen Szenario wird davon ausgegangen, dass der Tod der betroffenen Person (z.B. aufgrund einer Verwechslung) irrtümlich als verstorben gemeldet wurde und doch lediglich nur ein Erkrankung vorliegt. 

Innerhalb dieser Meldungen nehmen die einzelnen Statusinformationen in den FHIR-Ressourcen die folgenden Werte an:

* **NotificationDiseaseXYZD**
  * _Composition.status_ = **amended**<br>Da die zuvor versandte Meldung den Status "final" im entsprechenden Feld auswies, muss nun darauf hingewiesen werden, dass sich Inhalte geändert haben. Das Feld ist entsprechend auf "amended" zu setzen.

* **DiseaseXYZD**
  * _Condition.clinicalStatus_ = **active**<br>Aus Sicht der meldepflichtigen Person liegt weiterhin ein entsprechend in Condition.code referenzierter Meldetatbestand vor. Entsprechend wird clinicalStatus auch weiterhin auf "active" gesetzt.

  * _Condition.verificationStatus_ = **confirmed**<br>In diesem speziellen Szenario liegt eine bestätigte Erkrankung vor. Der Status ist entsprechend auf "bestätigt" ("confirmed") zu setzen.

  * _Condition.note_ = **[weiterführende Informationen]**<br>In diesem speziellen Szenario ist es sicher sinnvoll, dass die meldepflichtige Person dem Gesundheitsamt weiterführende Informationen zum entsprechenden Irrtum mitteilt. Dies kann über Freitext innerhalb dieses Feldes erfolgen. Vorstellbar wäre in diesem konkreten Szenario beispielsweise folgende Information: "Durch eine Verwechselung haben wir fälschlicherweise einen Todesfall im Zusammenhang mit COVID-19 gemeldet. Die entsprechende Person ist NICHT verstorben. Eine entsprechende Erkrankung liegt jedoch weiterhin vor." 

* **DiseaseInformationCommon**
  * _QuestionnaireResponse.status_ = **amended**<br>Die Falschangaben zum Tod der betroffenen Person müssen zwingend innerhalb des ausgefüllten Fragebogens korrigiert werden. Entsprechend ist der status mit "amended" anzugeben. 

* **DiseaseInformationXYZD**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

---

### S_FM_T2I_1 - Folgemeldung - Information an das Gesundheitsamt, dass die betroffene Person zwar verstorben ist, ein Meldetatbestand aber nicht erfüllt ist
Sobald der meldepflichtigen Person zum gemeldeten Todesfall zusätzliche Informationen bekannt werden oder bereits gemeldete Informationen fehlerhaft waren, muss entsprechend §9 Absatz 3 IfSG unverzüglich eine Folgemeldung (Nachmeldung/Korrektur) über DEMIS an das zuständige Gesundheitsamt gesendet werden. In diesem speziellen Szenario wird davon ausgegangen, dass die betroffene Person zwar verstorben ist, ein Meldetatbestand aber nicht erfüllt ist.

Innerhalb dieser Meldungen nehmen die einzelnen Statusinformationen in den FHIR-Ressourcen die folgenden Werte an:

* **NotificationDiseaseXYZD**
  * _Composition.status_ = **amended**<br>Da die zuvor versandte Meldung den Status "final" im entsprechenden Feld auswies, muss nun darauf hingewiesen werden, dass sich Inhalte geändert haben. Das Feld ist entsprechend auf "amended" zu setzen.

* **DiseaseXYZD**
  * _Condition.clinicalStatus_ = **inactive**<br>Aus Sicht der meldepflichtigen Person liegt ein entsprechend in Condition.code referenzierter Meldetatbestand nun nicht mehr vor. Entsprechend wird clinicalStatus auf "inactive" geändert.

  * _Condition.verificationStatus_ = **entered-in-error**<br>In diesem speziellen Szenario ist der Meldetatbestand - anders als zuvor gemeldet - nicht erfüllt. Der Status ist entsprechend auf "entered-in-error" zu setzen.

  * _Condition.note_ = **[weiterführende Informationen]**<br>In diesem speziellen Szenario ist es sicher sinnvoll, dass die meldepflichtige Person dem Gesundheitsamt weiterführende Informationen zum entsprechenden Irrtum mitteilt. Dies kann über Freitext innerhalb dieses Feldes erfolgen. Vorstellbar wäre in diesem konkreten Szenario beispielsweise folgende Information: "Labordiagnostische Untersuchungen haben gezeigt, dass der Tod der gemeldeten Person nicht im Zusammenhang mit der gemeldeten Erkrankung stand. Entsprechende Untersuchungsergebnisse waren negativ." 

* **DiseaseInformationCommon**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

* **DiseaseInformationXYZD**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

---

### S_FM_T2I_2 - Folgemeldung - Information an das Gesundheitsamt, dass eine Person irrtümlich als verstorben in Bezug auf eine meldepflichtige Krankheit gemeldet wurde, die letztlich nicht vorliegt
Sobald der meldepflichtigen Person zum gemeldeten Todesfall zusätzliche Informationen bekannt werden oder bereits gemeldete Informationen fehlerhaft waren, muss entsprechend §9 Absatz 3 IfSG unverzüglich eine Folgemeldung (Nachmeldung/Korrektur) über DEMIS an das zuständige Gesundheitsamt gesendet werden. In diesem speziellen Szenario wird davon ausgegangen, dass die Person keine meldepflichtige Erkrankung hat und irrtümlich als an einer meldepflichtigen Erkrankung verstorben gemeldet wurde.

Innerhalb dieser Meldungen nehmen die einzelnen Statusinformationen in den FHIR-Ressourcen die folgenden Werte an:

* **NotificationDiseaseXYZD**
  * _Composition.status_ = **amended**<br>Da die zuvor versandte Meldung den Status "final" im entsprechenden Feld auswies, muss nun darauf hingewiesen werden, dass sich Inhalte geändert haben. Das Feld ist entsprechend auf "amended" zu setzen.

* **DiseaseXYZD**
  * _Condition.clinicalStatus_ = **inactive**<br>Aus Sicht der meldepflichtigen Person liegt ein entsprechend in Condition.code referenzierter Meldetatbestand nun nicht mehr vor. Entsprechend wird clinicalStatus auf "inactive" geändert.

  * _Condition.verificationStatus_ = **entered-in-error**<br>In diesem speziellen Szenario liegt keine bestätigte Erkrankung vor. Der Status ist entsprechend auf "entered-in-error" zu setzen.

  * _Condition.note_ = **[weiterführende Informationen]**<br>In diesem speziellen Szenario ist es sicher sinnvoll, dass die meldepflichtige Person dem Gesundheitsamt weiterführende Informationen zum entsprechenden Irrtum mitteilt. Dies kann über Freitext innerhalb dieses Feldes erfolgen. Vorstellbar wäre in diesem konkreten Szenario beispielsweise folgende Information: "Durch eine Verwechselung haben wir fälschlicherweise einen Todesfall im Zusammenhang mit COVID-19 gemeldet. Die entsprechende Person ist NICHT verstorben. Eine entsprechende Erkrankung liegt ebenfalls nicht vor."

* **DiseaseInformationCommon**
  * _QuestionnaireResponse.status_ = **amended**<br>Die Falschangaben zum Tod der betroffenen Person müssen zwingend innerhalb des ausgefüllten Fragebogens korrigiert werden. Entsprechend ist der status mit "amended" anzugeben. 

* **DiseaseInformationXYZD**
  * _QuestionnaireResponse.status_ = **completed | amended**<br>Sofern sich Angaben innerhalb des Fragebogens (sowie in von diesem referenzierten Ressourcen) verändert haben oder ergänzt wurden, ist der status mit "amended" anzugeben. Wurden keine Veränderungen im Vergleich zur initialen oder einer der Folgeversion vorgenommen, bleibt der Status "completed" erhalten. 

