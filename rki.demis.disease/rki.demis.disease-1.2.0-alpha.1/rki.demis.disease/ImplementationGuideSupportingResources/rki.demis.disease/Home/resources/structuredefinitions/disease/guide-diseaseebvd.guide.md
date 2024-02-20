# {{page-title}}
[https://demis.rki.de/fhir/StructureDefinition/DiseaseEBVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseebvd)

Das **[DiseaseEBVD]((https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseebvd))**-Profil ist die Ebolafieber-spezifische Ausgestaltung des [Disease](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/disease)-Profils. Dieses Profil kann und muss instanziiert werden, wenn eine Ebolafieber Meldung zusammengestellt wird.

Wesentliche Unterschiede zum [Disease](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/disease)-Profil ergeben sich in Bezug auf die Festlegung eines konkreten kodierten Wertes, der den Meldetatbestand abschließend bestimmt sowie das Binding eines meldetatbestandsspezifischen ValueSets (hier: [https://demis.rki.de/fhir/ValueSet/evidenceEBVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/valueset/evidenceebvd)), über welches Symptome/Manifestationen passend zugeordnet werden können.

**Profilansicht**
{{tree:https://demis.rki.de/fhir/StructureDefinition/DiseaseEBVD, hybrid}}

**Beispiel**
{{xml:ExampleResources/example-condition-diseaseebvd-01.xml}}
