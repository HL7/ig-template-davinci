#### Da Vinci Burden Reduction
This implementation guide is part of a set of interrelated implementation guides that are focused on reducing clinician and payer burden. The Da Vinci 'Burden Reduction' implementation guides are:

1. [Coverage Requirements Discovery (CRD)](http://hl7.org/fhir/us/davinci-crd) which provides decision support to providers at the time they're ordering diagnostics, specifying treatments, making referrals, scheduling appointments, etc.
2. [Documentation Templates and Rules (DTR)](http://hl7.org/fhir/us/davinci-dtr) which allows providers to download 'smart' questionnaires, rules (e.g. CQL), and provides a [SMART on FHIR](http://www.hl7.org/fhir/smart-app-launch/) app or EHR app that executes them to gather information relevant to a performed or planned service. Execution of the questionnaires and rules may also be performed by an application that is part of the provider's EHR.
3. [Prior Authorization Support (PAS)](http://hl7.org/fhir/us/davinci-pas) allows provider systems to send (and payer systems to receive) prior authorization requests using FHIR, while still meeting regulatory mandates to have X12 278 used, where required, to transport the prior authorization, potentially simplifying processing for both exchange partners.
4. [Clinical Data Exchange (CDex)](https://hl7.org/fhir/us/davinci-cdex/index.html) documents launching DTR to gather additional information ("attachments").  The portion of CDex that is relevant to Burden Reduction is the use of the PAS Task profile.  The PAS Task Profile uses the launch of DTR to communicate to the provider a URL of a data request FHIR Questionnaire. The `Task.code` indicates to the provider system to use DTR to complete the Questionnaire, and the URL of the Questionnaire "package" is communicated in `Task.input`.

The general flow of activity across all three IGs can be seen in the following diagram:

{% capture image %}
{% include burdenReduction.svg %}
{% endcapture %}
{{ image | remove_first: '<?xml version="1.0" encoding="us-ascii" standalone="no"?>'}}

The guides overlap in the following ways:

* CRD can indicate whether prior authorization is required and whether there are 'special documentation requirements' related to the planned service. The CDS Hook system actions can indicate that additional clinical and/or administrative information needs to be captured, allowing the EHR to prompt the appropriate user(s) to launch the DTR process to guide the user(s) in capturing the relevant information.
* The need for DTR is indicated in an extension created by a CRD system action. DTR allows the capture of information needed to support prior authorization requests and that can be included as part of such requests in PAS.
* PAS can be used to submit a prior authorization based on a requirement identified by CRD and include information requested by the payer in the form of a QuestionnaireResponse Bundle. The QuestionnaireResponse Bundle is included in the PAS request bundle and referenced by the PA profile on the claim as `Claim.supportingInformation`. The entire PAS bundle passed to the payer includes the QuestionnaireResponse Bundle.

All four implementation guides should be used together to perform business functions related to prior authorization.  [CMS Interoperability and Prior Authorization Final Rule CMS-0057-F](https://www.cms.gov/newsroom/fact-sheets/cms-interoperability-and-prior-authorization-final-rule-cms-0057-f) recommends (and CMS may in the future require) that payers implement the functionality defined in the CRD, DTR, and PAS guides within their APIs. However, all IGs other than PAS also offer functionality that is unrelated to prior authorization. The guides can function independently in several ways:

* CRD can provide information unrelated to prior authorization and 'special documentation'. For example, providing an estimate of patient cost, suggesting appropriate use criteria, identifying duplicate therapies, etc.
* DTR might be invoked directly by a clinician to validate documentation regarding an item or service that meets a responsible payer’s requirements.
* Information gathered by DTR will normally be used for submission via PAS to support a prior authorization request. However, the Questionnaire Response and its associated references may be exchanged using other methods with a performing provider, payer, or other entity to supply documentation of medical necessity.
* PAS can be used for prior authorization submissions even if the requirement is not identified by CRD and the supporting documentation is exchanged via another method.
* HRex can be used to solicit information unrelated to prior authorization.

The greatest benefit to clinical workflow and reduction of manual intervention is achieved by implementing all four IGs at the same time. However, implementers can choose to roll out these implementation guides in whatever order or combination best meets their business objectives.  (Note that regulations may set expectations for when certain implementation guides must be used.)
