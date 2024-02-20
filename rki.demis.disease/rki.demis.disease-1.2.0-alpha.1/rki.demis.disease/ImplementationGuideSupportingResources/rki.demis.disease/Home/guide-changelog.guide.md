# {{page-title}}
Auf der folgenden Seite wird die Änderungshistorie seit der initialen Veröffentlichung der Profile zur Arztmeldung dargestellt. Insbesondere während der Einführungsphase sollte regelmäßig geprüft werden, ob Änderungen an den Profile oder mit diesen assoziierten Ressorcen erfolgt sind, die ggf. Auswirkungen auf die Implementierung in den Systemen der Sender und Empfänger haben.


23.12.2022 (rki.demis.disease-1.2.0-alpha.1)
- feat: Ressourcen für die Meldetatbestände Milzbrand, Keuchusten, Botulismus, Diphtherie, Denguefieber, Ebolafieber, Gelbfieber, Hämorrhagisches Fieber, Lassafieber, Marburgfieber und Poliomyelitis hinzugefügt
- feat: Slicing in ImmunizationInformation.vaccineCode hinzugefügt: die entsprechende Information kann entweder mithilfe von Konzepten aus SNOMED-CT und PZN oder einem von zwei Null-Flavors ("nicht erhoben" / "nicht ermittelbar") dargestellt werden; bei CVDD können weiterhin die Konzepte aus EMA genutzt werden

13.12.2022 (rki.demis.disease-1.1.0)
- feat: Übernahme als produktiv genutzte Version

11.11.2022 (rki.demis.disease-1.1.0-rc.3)
- doc: Hinzufügen eines Hinweises bzgl. der (noch in Diskussion befindlichen) Umsetzung des Hospitalisierungsgrundes innerhalb des COVID-19 spezifischen Fragebogens.

03.11.2022 (rki.demis.disease-1.1.0-rc.2)
- feat: Hinzufügen eines weiteren Impstoffes (EU/1/21/1624 - Valneva) ins ValueSet https://demis.rki.de/fhir/ValueSet/vaccineCVDD
- fix: Korrektur des Codes für Vaxservaria von "EU/1/20/1529" auf "EU/1/21/1529" im ValueSet https://demis.rki.de/fhir/ValueSet/vaccineCVDD

26.08.2022 (rki.demis.disease-1.1.0-rc.1)
- feat: Änderung an NotificationDisease: identifier ist verpflichtend unter Nutzung des NamingSystems https://demis.rki.de/fhir/NamingSystem/NotificationId anzugeben
- feat: Setzen der Immunization Reference auf "false" in DiseaseQuestionsCVDD
- feat: Hinzufügen der Frage nach dem Hospitalisierungsgrund in DiseaseQuestionsCVDD
- feat: Erweitern der Beispiele um Angabe des Hospitalisierungsgrundes
- fix: Setzen des usage-status von CVDD auf "active" in CodeSystem NotificationDiseaseCategory
- doc: Hinzufügen szenariobezogener Beschreibung des Lifecyclemanagement
- doc: Hinzufügen einer Unterseite im textuellen Leitfaden für 'Known Issues'

10.06.2022 (rki.demis.disease-1.0.1)
- fix: Korrektur des Anzeigetexts für 'immunization' in DiseaseQuestionsCVDD (alt: 'Vorhandensein von Impfinformationen' --> neu: 'Wurde die betroffene Person jemals in Bezug auf die Krankheit geimpft?')
- doc: Ergänzen von Beschreibungen zum meldetatbestandsspezifisch (COVID-19) genutzten Fragebogen (DiseaseQuestionsCVDD)

25.03.2022 (rki.demis.disease-1.0.0 + rki.demis.disease-1.0.0-alpha.3)
- fix: Korrektur an bestehenden Beispielen zur inhaltlich korrekten Abbildung des derzeitigen Aufenthaltsortes der betroffenen Person (zusätzliches address-Element mit AddressUse-Extension ('current') und verschobener Referenz auf die zugehörige NotifiedPersonFacility
- doc: Ergänzen detailierter Beschreibungen zum meldetatbestandsübergreifend genutzten Fragebogen (DiseaseQuestionsCommon)

14.02.2022 (rki.demis.disease-1.0.0-alpha.2)
- fix: Korrektur des referenzierten Profils in der extension zur linkid 'immunization' in 'DiseaseQuestionsCVDD' ('ImmunizationInformation' --> 'ImmunizationInformationCVDD')
- fix: Korrekturen an bestehenden Beispielen zur Sicherstellung der Validierbarkeit durch das Backend
- feat: Hinzufügen von extensions in DiseaseQuestionsCommon für die items 'labSpecimenLab' und 'infectProtectFacilityOrganization' zur Typisierung der 'reference'
- feat: Hinzufügen weiterer Meldungsbeispiele inkl. eines Beispiels zur Abbildung eines intensivmedizinisch betreuten Patienten
- feat: Anpassen der Displaywerte im 'vaccineCVDD' ValueSet zum Sicherstellen der leichteren Unterscheidbarkeit der Impfstoffe (PEI-Naming)
- doc: Überarbeiten der Aussagen zur Vererbungshierachie von 'DiseaseInformationCommon' und 'DiseaseInformationCVDD'
- doc: Hinzufügen von Dokumentationsartefakten für alle StructureDefinitions und Definitional Artifacts, welche im Simplifier-Projekt gerendert werden

18.01.2022 (rki.demis.disease-1.0.0-alpha.1)
- Anpassung auf Grundlage interner QS und externen Feedbacks
- Aktualisieren von immunizationCVDD (Hinzufügen eines neu zugelassenen Impfstoffes - EU/1/21/1618 (Nuvaxovid), Hinzufügen von relevanten NullFlavors)
- Aktualisieren von evidenceCVDD (Zuschnitt auf Anforderungen des Meldeportals - Verschieben ähnlicher Konzepte)
- Metadatenaktualisierung (Setzen der Versionsnummern aller Metadata Resources auf 1.0.0, Setzen des status auf final, Setzen des Veröffentlichungsdatums) für alle Metadata Resources
- Hinzufügen eines CodeSystem Supplements für die übergangsweise Übersetzungen von SNOMED-CT Konzepten für Symptome/Manifestatione
- Hinzufügen eines CodeSystem Supplements für die Übersetzungen von HL7-Null-Flavors
- Ändern der binding "strength" von "required" auf "extensible" für mehrere Inhalte in verschiedenen StructureDefinitions
- Ergänzen/Aktualisieren ausgewählter Metadaten (description, date) für die verwendeten Questionnaire Ressourcen
- Aktualisieren ausgewählte textueller Beschreibungen im Implementierungsleitfaden

06.12.2021 (rki.demis.disease-0.1.0-alpha.1)
- Erstveröffentlichung der Draft-Version
