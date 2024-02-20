# {{page-title}}
[https://demis.rki.de/fhir/StructureDefinition/DiseaseDEND](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseasedend)

Das **[DiseaseDEND]((https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseasedend))**-Profil ist die Denguefieber-spezifische Ausgestaltung des [Disease](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/disease)-Profils. Dieses Profil kann und muss instanziiert werden, wenn eine Denguefieber Meldung zusammengestellt wird.

Wesentliche Unterschiede zum [Disease](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/disease)-Profil ergeben sich in Bezug auf die Festlegung eines konkreten kodierten Wertes, der den Meldetatbestand abschließend bestimmt sowie das Binding eines meldetatbestandsspezifischen ValueSets (hier: [https://demis.rki.de/fhir/ValueSet/evidenceDEND](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/valueset/evidencedend)), über welches Symptome/Manifestationen passend zugeordnet werden können.

**Profilansicht**
{{tree:https://demis.rki.de/fhir/StructureDefinition/DiseaseDEND, hybrid}}

**Beispiel**
{{xml:ExampleResources/example-condition-diseasedend-01.xml}}
