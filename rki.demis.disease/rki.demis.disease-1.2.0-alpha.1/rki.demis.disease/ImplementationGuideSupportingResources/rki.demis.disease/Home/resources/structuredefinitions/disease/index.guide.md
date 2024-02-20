# {{page-title}}
[https://demis.rki.de/fhir/StructureDefinition/Disease](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/disease)

Das abstrakte **[Disease](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/disease)**-Profil ist eine Spezialisierung der FHIR [Condition](https://www.hl7.org/fhir/condition.html).

In diesem Profil werden allgemeine Festlegungen (Kardinalität etc.) bzgl. der für alle Meldetatbestände gemäß §6 Absatz 1 IfSG relevanten Inhalte getroffen. Aus logischer Sicht sind dies die Folgenden:

{{render:image-logicalcontentdisease}}

Eine weitere Verfeinerung dieses Zuschnitts (z.B. das normative Binding von krankheitsspezifischen Symptomen/Manifestationen an ausgewählte ValueSets) findet in krankheitsspezifischen Unterprofilen (z.B. [DiseaseCVDD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseasecvdd)) statt. Derzeit sind 13 Kindprofile definiert. Mit der Unterstützung weiterer Meldetatbestände gemäß §6 Absatz 1 IfSG werden zusätzliche Kindprofile hinzukommen.

{{render:image-profilehierarchydiseasexyzd}}

**Profilansicht**
{{tree:https://demis.rki.de/fhir/StructureDefinition/Disease, hybrid}}
