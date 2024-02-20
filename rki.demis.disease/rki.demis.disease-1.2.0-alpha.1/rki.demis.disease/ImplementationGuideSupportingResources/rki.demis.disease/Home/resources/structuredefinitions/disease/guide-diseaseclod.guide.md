# {{page-title}}
[https://demis.rki.de/fhir/StructureDefinition/DiseaseCLOD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseclod)

Das **[DiseaseCLOD]((https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseclod))**-Profil ist die Botulismus-spezifische Ausgestaltung des [Disease](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/disease)-Profils. Dieses Profil kann und muss instanziiert werden, wenn eine Botulismus Meldung zusammengestellt wird.

Wesentliche Unterschiede zum [Disease](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/disease)-Profil ergeben sich in Bezug auf die Festlegung eines konkreten kodierten Wertes, der den Meldetatbestand abschließend bestimmt sowie das Binding eines meldetatbestandsspezifischen ValueSets (hier: [https://demis.rki.de/fhir/ValueSet/evidenceCLOD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/valueset/evidenceclod)), über welches Symptome/Manifestationen passend zugeordnet werden können.

**Profilansicht**
{{tree:https://demis.rki.de/fhir/StructureDefinition/DiseaseCLOD, hybrid}}

**Beispiel**
{{xml:ExampleResources/example-condition-diseaseclod-01.xml}}
