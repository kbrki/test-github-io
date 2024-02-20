# {{page-title}}
[https://demis.rki.de/fhir/StructureDefinition/ImmunizationInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/immunizationinformation)  

Das abstrakte **[ImmunizationInformation](https://simplifier.net/demisarztmeldung/~resources?canonical=https://demis.rki.de/fhir/structuredefinition/immunizationinformation)**-Profil profiliert direkt die HL7 Definition der [Immunization](https://www.hl7.org/fhir/immunization.html)-Ressource. Das Profil nimmt dabei eine Reihe von Festlegungen vor, die in allen von ihm abgeleiteten Kindprofilen zum Tragen kommen. Die Festlegungen sind so zugeschnitten, dass es sehr leicht möglich ist, die folgenden logischen Angaben zu einer Impfung kompakt in die entsprechenden Ressourcen einzubetten:

{{render:image-logicalcontentimmunizationinformation}}

Von besonderer Bedeutung bei der Darstellung der Impfinformationen ist die Kodierung des verabreichten Impfstoffes. Für alle Kindprofile von ImmunizationInformation kann die entsprechende Information entweder mithilfe von Konzepten aus SNOMED-CT oder PZN oder einem von zwei Null-Flavors ("nicht erhoben" / "nicht ermittelbar") dargestellt werden. Einzige Ausnahme von dieser Regel bildet ImmunizationInformationCVDD: Hier kann zusätzlich ein EMA-Code zur Darstellung des verabreichten Impfstoffes herangezogen werden. In den einzelnen Kindprofilen wird für die SNOMED-Slice ein zusätzliches Binding definiert, welches die Auswahlmöglichkeit der Kodierung weiter einschränkt.

Das ImmunizationInformation-Profil bildet die Grundlage für sämtliche meldetatbestandsspezifischen ImmunizationInformation-Profile. Derzeit sind mehrere Kindprofile definiert. Mit der Unterstützung weiterer Meldetatbestände gemäß §6 Absatz 1 IfSG werden weitere Kindprofile hinzukommen.

{{render:image-profilehierarchyimmunizationInformationxyzd}}

**Profilansicht**
 {{tree:https://demis.rki.de/fhir/StructureDefinition/ImmunizationInformation, hybrid}}
