# {{page-title}}
[https://demis.rki.de/fhir/StructureDefinition/ImmunizationInformationCVDD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/immunizationinformationcvdd)  

Das **[ImmunizationInformationCVDD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/immunizationinformationcvdd)**-Profil ist eine weitergehende Spezialisierung des [ImmunizationInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/immunizationinformation)-Profils.

Das Profil nimmt zusätzliche COVID-19-spezifische Einschränkungen an der zugrundeliegenden Immunization-Ressource vor. Dazu gehört insbesondere die normative Bindung des SNOMED-Slice in vaccineCode an ein COVID-19-spezifisches ValueSet für entsprechende Impfstoffe. Zusätzlich kann zur Darstellung des verabreichten Impfstoffes ein EMA-Code herangezogen werden.

Die kodierte Angabe eines Wertes für vaccineCode (Impfstoff) ist entsprechend der Vorgaben der StructureDefinition verpflichtend. Dies wurde bei der Ausgestaltung der referenzierten ValueSets berücksichtigt, so dass auch Fälle abbildbar sind, in denen der konkret verabreichte Impfstoff nicht in der Vorselektion des RKI enthalten ist (-> Other (qualifier value)), nicht erhoben wurde, welcher Impfstoff zum Einsatz kam (-> NASK Null-Flavor) oder aber der verwendete Impfstoff nicht ermittelbar ist (-> ASKU Null-Flavor).

**Hinweis:** Eine **[ImmunizationInformationCVDD](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/immunizationinformation)**-Instanz kann grundsätzlich nur für die Darstellung einer erhaltenen/durchgeführten Impfung genutzt werden. Hat die betroffene Person mehrere Impfungen erhalten, müssen auch mehrere Ressourcen dieses Typs angelegt und über den ausgefüllten krankheitsspezifischen Fragebogen referenziert werden.

**Profilansicht**
{{tree:https://demis.rki.de/fhir/StructureDefinition/ImmunizationInformationCVDD, hybrid}}

**Beispiel**
{{xml:ExampleResources/example-immunization-immunizationinformationcvdd-01.xml}}
