# {{page-title}}
[https://demis.rki.de/fhir/StructureDefinition/DiseaseHFAD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseasehfad)

Das **[DiseaseHFAD]((https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/diseasehfad))**-Profil ist die Hämorrhagisches Fieber-spezifische Ausgestaltung des [Disease](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/disease)-Profils. Dieses Profil kann und muss instanziiert werden, wenn eine Hämorrhagisches Fieber Meldung zusammengestellt wird.

Wesentliche Unterschiede zum [Disease](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/disease)-Profil ergeben sich in Bezug auf die Festlegung eines konkreten kodierten Wertes, der den Meldetatbestand abschließend bestimmt sowie das Binding eines meldetatbestandsspezifischen ValueSets (hier: [https://demis.rki.de/fhir/ValueSet/evidenceHFAD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/valueset/evidencehfad)), über welches Symptome/Manifestationen passend zugeordnet werden können.

**Profilansicht**
{{tree:https://demis.rki.de/fhir/StructureDefinition/DiseaseHFAD, hybrid}}

**Beispiel** - Erstmeldung
{{xml:ExampleResources/example-condition-diseasehfad-01a.xml}}

**Beispiel** - Ergänzungsmeldung
{{xml:ExampleResources/example-condition-diseasehfad-01b.xml}}
