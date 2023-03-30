#### Da Vinci Burden Reduction
This implementation guide is part of a set of interrelated implementation guides that are focused on reducing clinician and payer burden.  The Da Vinci 'Burden Reduction' implementation guides are:

1. [Coverage Requirements Discovery (CRD)](http://hl7.org/fhir/us/davinci-crd) which provides decision support to providers at the time they're ordering diagnostics, specifying treatments, making referrals, scheduling appointments, etc.
2. [Documentation Templates and Rules (DTR)](http://hl7.org/fhir/us/davinci-dtr) which allows providers to download 'smart' questionnaires, rules (e.g. CQL), and provides a [SMART on FHIR](http://www.hl7.org/fhir/smart-app-launch/) app or EHR app that executes them to gather information relevant to a performed or planned service.  Execution of the questionnaires and rules may also be performed by an application that is part of the provider's EHR.
3. [Prior Authorization Support (PAS)](http://hl7.org/fhir/us/davinci-pas) allows provider systems to send (and payer systems to receive) prior authorization requests using FHIR, while still meeting regulatory mandates to have X12 278 used, where required, to transport the prior authorization, potentially simplifying processing for either or both exchange partners.

The general flow of activity across all three IGs can be seen in the following diagram:

{% capture image %}
{% include burdenReduction.svg %}
{% endcapture %}
{{ image | remove_first: '<?xml version="1.0" encoding="UTF-8" standalone="no"?>'}}

The guides overlap in the following ways:

* CRD can indicate whether prior authorization is or is not required and whether there are or are not 'special documentation requirements' related to the planned service.  The CDS Hook cards returned by CRD can include a link to a DTR SMART application that will then guide the clinician in capturing the relevant information
* DTR can be triggered by a CRD hook.  It allows the capture of information needed to support prior authorization requests and that can be included as part of the request
* PAS can be used to submit a prior authorization based on a requirement identified by CRD and using information gathered by DTR.  If the Prior Authorization request includes the DTR response, it will include a reference to the DTR bundle.  The PA submission will include the DTR bundle.  The X12 278 request will not point to the DTR bundle.

All three implementation guides should be used together to perform business functions related to prior authorization.  However, the first two IGs also offer functionality that's
unrelated to prior authorization.  The guides can function independently in several ways:

* CRD can provide information unrelated to prior authorization and 'special documentation'.  For example, providing an estimate of patient cost, suggesting appropriate use criteria, identifying duplicate therapies, etc.
* DTR might be invoked directly by a clinician to validate documentation regarding an item or service meets a responsible payer’s requirements.
* Information gathered by DTR will normally be used for submission via PAS to support a prior authorization request.  However, the Questionnaire Response and its associated references may be exchange, using other methods,  with a preforming provider, payer or other entity to supply medical necessity documentation.
* PAS can be used for prior authorization submissions even if the requirement is not identified by CRD and the supporting documentation is exchange via anther method.

The greatest benefit to clinical workflow and reduction of manual intervention is achieved by implementing all three IGs at the same time.  However, implementers can choose to roll out these three implementation guides in whatever order or combination best meets their business objectives.
