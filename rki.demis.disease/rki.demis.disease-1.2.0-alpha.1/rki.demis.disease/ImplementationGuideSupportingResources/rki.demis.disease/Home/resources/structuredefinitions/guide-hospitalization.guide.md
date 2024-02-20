# {{page-title}}
[https://demis.rki.de/fhir/StructureDefinition/Hospitalization](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/hospitalization)

Insbesondere vor dem Hintergrund der Ermittlung der Hospitalisierungsinzidenz ist das im Folgenden diskutierte Profil von besonderer Bedeutung. Mit seiner Hilfe können detaillierte Angaben zum Hospitalisierungsstatus einer betroffen Person gemacht werden.
Das [Hospitalization](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/hospitalization)-Profil definiert seine Einschränkungen und Erweiterungen direkt auf der HL7 [Encounter](https://www.hl7.org/fhir/encounter.html)-Definition.

{{render:image-profilehierarchyhospitalization}}

Ressourcen, die dieses Profil umsetzen, können die folgenden logischen Informationen als Bestandteil der Meldung transportieren:

{{render:image-logicalcontenthospitalization}}

Die Profilierung nimmt eine Reihe von Einschränkungen und Festlegungen vor, die primär die Verwendung bestimmter Elemente (z.B. type) unterbindet bzw. anderer Elemente (z.B. serviceType, period) fordert. Neben diesen Einschränkungen werden zusätzlich zwei Erweiterungen vorgenommen:

- **Region der Hospitalisierung** - zu nutzen, sofern kein konkretes Krankenhaus über serviceProvider referenziert werden kann
- **Hinweise/Notizen zur Hospitalisierung** - zu nutzen, sofern die meldepflichtige Person dem Gesundheitsamt gegenüber wichtige Hinweise zur Hospitalisierung übermitteln möchte.

Mit Blick auf das oben abgebildete logische Informationsmodell ergibt sich eine weitere Besonderheit: Ein Aufenthalt auf einer Intensivstation wird nicht anders dargestellt als der Aufenthalt auf einer "normalen" Station. Eine Unterscheidung findet ausschließlich über den angegebenen serviceType statt.

**Profilansicht**
{{tree:https://demis.rki.de/fhir/StructureDefinition/Hospitalization, hybrid}}

**Beispiel**

{{render:guides/ImplementierungsleitfadenfrDEMISArztmeldung/docs/example-encounter-hospitalization-02.doc.md}}
{{xml:ExampleResources/example-encounter-hospitalization-02.xml}}

{{xml:ExampleResources/example-encounter-hospitalization-01.xml}}

{{xml:ExampleResources/example-encounter-hospitalization-00.xml}}