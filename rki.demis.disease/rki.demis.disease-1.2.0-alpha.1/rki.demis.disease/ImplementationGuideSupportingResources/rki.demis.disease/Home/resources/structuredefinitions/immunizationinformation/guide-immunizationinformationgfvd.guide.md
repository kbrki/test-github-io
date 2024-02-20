# {{page-title}}
[https://demis.rki.de/fhir/StructureDefinition/ImmunizationInformationGFVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/immunizationinformationgfvd)

Das **[ImmunizationInformationGFVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/immunizationinformationgfvd)**-Profil ist eine weitergehende Spezialisierung des [ImmunizationInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/immunizationinformation)-Profils.

Das Profil nimmt zusätzliche Gelbfieber-spezifische Einschränkungen an der zugrundeliegenden Immunization-Ressource vor. Dazu gehört insbesondere die normative Bindung des SNOMED-Slice in vaccineCode an ein Gelbfieber-spezifisches ValueSet für entsprechende Impfstoffe.

Die kodierte Angabe eines Wertes für vaccineCode (Impfstoff) ist entsprechend der Vorgaben der StructureDefinition verpflichtend. Dies wurde bei der Ausgestaltung der referenzierten ValueSets berücksichtigt, so dass auch Fälle abbildbar sind, in denen der konkret verabreichte Impfstoff nicht in der Vorselektion des RKI enthalten ist (-> Other (qualifier value)), nicht erhoben wurde, welcher Impfstoff zum Einsatz kam (-> NASK Null-Flavor) oder aber der verwendete Impfstoff nicht ermittelbar ist (-> ASKU Null-Flavor).

**Hinweis:** Eine **[ImmunizationInformationGFVD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/immunizationinformation)**-Instanz kann grundsätzlich nur für die Darstellung einer erhaltenen/durchgeführten Impfung genutzt werden. Hat die betroffene Person mehrere Impfungen erhalten, müssen auch mehrere Ressourcen dieses Typs angelegt und über den ausgefüllten krankheitsspezifischen Fragebogen referenziert werden.

**Profilansicht**
{{tree:https://demis.rki.de/fhir/StructureDefinition/ImmunizationInformationGFVD, hybrid}}

**Beispiel**
{{xml:ExampleResources/example-immunization-immunizationinformationgfvd-01.xml}}
