# {{page-title}}
[https://demis.rki.de/fhir/StructureDefinition/DiseaseBAND](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseband)

Das **[DiseaseBAND]((https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseaseband))**-Profil ist die Milzbrand-spezifische Ausgestaltung des [Disease](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/disease)-Profils. Dieses Profil kann und muss instanziiert werden, wenn eine Milzbrand Meldung zusammengestellt wird.

Wesentliche Unterschiede zum [Disease](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/disease)-Profil ergeben sich in Bezug auf die Festlegung eines konkreten kodierten Wertes, der den Meldetatbestand abschließend bestimmt sowie das Binding eines meldetatbestandsspezifischen ValueSets (hier: [https://demis.rki.de/fhir/ValueSet/evidenceBAND](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/valueset/evidenceband)), über welches Symptome/Manifestationen passend zugeordnet werden können.

**Profilansicht**
{{tree:https://demis.rki.de/fhir/StructureDefinition/DiseaseBAND, hybrid}}

**Beispiel**
{{xml:ExampleResources/example-condition-diseaseband-01.xml}}
