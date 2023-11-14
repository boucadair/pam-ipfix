---
title: Export of Flow Precision Availability Metrics Using IPFIX
abbrev: pam-ipfix
docname: draft-clemm-ippm-pam-ipfix-latest
wg: OPSAWG
category: std
ipr: trust200902
stream: IETF

stand_alone: yes
pi:
   toc: yes
   tocdepth: 5
   sortrefs: yes
   symrefs: yes
   comments: yes

author:
  -
       name: Alexander Clemm
       org: Futurewei
       street: 2220 Central Expressway
       city: Santa Clara
       code: CA 95050
       country: USA
       email: ludwig@clemm.org
       role: editor
  -
       name: Mohamed Boucadair
       org: Orange
       city: Rennes
       code: 35000
       country: France
       email: mohamed.boucadair@orange.com
  -
       name: Greg Mirsky
       org: Ericsson
       country: USA
       email: gregimirsky@gmail.com

normative:

informative:
     IANA-IPFIX:
        title: IP Flow Information Export (IPFIX) Entities
        author:
        -
          organization: "IANA"
        target: https://www.iana.org/assignments/ipfix/ipfix.xhtml
        date: false

--- abstract

This document defines a set of IP Flow Information Export (IPFIX) Information Elements to export precision availability data associated with Flows, specifically Flows that are associated with stringent Service Level Objectives (SLOs) such as latency or packet delay variation.

--- middle

# Introduction

IP Flow Information Export (IPFIX) {{!RFC7011}} is a protocol that is widely deployed in operators networks to collect Records containing a wide array of statistics about Flows. The Records are used for many purposes, including network security (e.g., detection of denial-of-service attacks), accounting (e.g., identifying "top talkers"), monitoring and service assurance (e.g., detection of anomalies and abnormal behaviors), and network planning (e.g., maintaining traffic matrices and detecting usage trends). To that aim, IPFIX relies upon a set of basic data items that can be maintained by network devices and exported as part of a Flow Record.  These data items are commonly referred to as Information Elements (IEs) {{!RFC7012}}.

Increasingly, to be provided with mere connectivity is no longer sufficient for many networking applications.  There is a growing demand for high-precision services that underly stringent Service Level Objectives (SLOs), such as a given latency that must be met by the (connectivity) service. When a guaranteed property of a service (typically, traffic performance metrics) is not met, this is considered in many cases as equivalent to the service not being available. This is particularly the case in which an application relying upon the service does not degrade gracefully with deteriorating service levels (e.g., video or voice), but in which violation of an SLO will cause the application to abruptly cease to function (e.g., industrial control and Control-as-a-Service applications or telehaptics).

Existing IPFIX IEs largely focus on statistics such as traffic volume, packet lengths, header fields, or route properties. However, there is a lack of IEs that indicate a Flow's "quality". Specifically, IPFIX does not support IEs that indicate compliance of a Flow with an SLO. This specification fills that void by defining a set of IEs that are based upon Precision Availability Metrics (PAM) {{!I-D.ietf-ippm-pam}}. PAMs can thus be exported as part of Flow Records using IPFIX.

# Terminology

{::boilerplate bcp14-tagged}

This document uses the IPFIX-specific terminology (Information Element, Template, Collector, Data Record, Flow Record, Exporting Process, Collecting Process, etc.) defined in {{Section 2 of !RFC7011}}. As in {{!RFC7011}}, these IPFIX-specific terms have the first letter of a word capitalized.

Also, this document uses terminology associated with Precision Availability Metrics (PAM), as defined in {{Section 2 of !I-D.ietf-ippm-pam}}. For the reader's convenience, some of the acronyms that are used in the document are provided below:

IE:
: Information Element

IPFIX:
: IP Flow Information Export

PAM:
: Precision Availability Metric

SLO:
: Service Level Objective

VI:
: Violated Interval

VFI:
: Violation-Free Interval

# Precision Availability Information Elements

The following subsections define a set of IEs to export precision availability data as part of Flow Records. At the core of PAMs is the notion of an "interval", i.e. an observation interval (a small unit of time) for which the presence or absence of violations is noted.  What constitutes a violation or not depends on the definition of the service, i.e., the length of the interval (e.g., a millisecond) and the SLO (e.g., a not-to-exceed latency threshold or packet inter-arrival delay threshold).

Accordingly, IEs are grouped into two categories.  The first category contains IEs that reflect PAMs per {{!I-D.ietf-ippm-pam}}. The second category contains IEs that are used to define the context that is necessary to adequately interpret the IEs in the first category, such as the SLO that underlies the definition of precision availability for that particular Flow. This context can be thought of as a manifest for that Flow Record.

## IEs Based on Precision Availability Metrics

### Violated Intervals Count {#sec-vic}

Name:
: violatedIntervalsCount

ElementID:
: TBD1

Description:
:  Contains a count of intervals over the duration of the Flow during which the service was not available with the required precision. That is, a count of intervals for which an SLO violation was observed for the Flow.

Abstract Data Type:
: unsigned

Data Type Semantics:
: quantity

Additional Information:
: See {{!I-D.ietf-ippm-pam}} for the general definition of PAM.

Reference:
: This-Document

### Violation-Free Intervals Count  {#sec-vfic}

Name:
: violationFreeIntervalsCount

ElementID:
: TBD2

Description:
:  Contains a count of intervals over the duration of the Flow during which the required precision was available, i.e., the period during which the Flow was in compliance with its SLO. In practical terms, the violationFreeIntervalsCount corresponds to the number of intervals over the duration of the Flow minus the violatedIntervalsCount.

Abstract Data Type:
: unsigned

Data Type Semantics:
: quantity

Additional Information:
: See {{!I-D.ietf-ippm-pam}} for the general definition of PAM.

Reference:
: This-Document



> TBD: Assess size of this parameter (for the case of long Flow durations with short interval durations).

### Violated Packet Count  {#sec-vpc}

Name:
: violatedPacketCount

ElementID:
: TBD3

Description:
:  Contains a count of packets for which packet-level violations of an SLO were observed for the Flow.

Abstract Data Type:
: unsigned

Data Type Semantics:
: quantity

Additional Information:
: See {{!I-D.ietf-ippm-pam}} for the general definition of PAM.

Reference:
: This-Document


### Severely Violated Intervals Count  {#sec-svic}

Name:
: severelyViolatedIntervalsCount

ElementID:
: TBD4

Description:
:  Contains a count of intervals over the duration of a Flow during which a particularly severe violation was observed.

Abstract Data Type:
: unsigned

Data Type Semantics:
: quantity

Additional Information:
: See {{!I-D.ietf-ippm-pam}} for the general definition of PAM.

Reference:
: This-Document

### Severely Violated Packet Count  {#sec-svpc}

Name:
: severelyViolatedPacketCount

ElementID:
: TBD5

Description:
:  Contains a count of packets for which particularly severe packet-level violations of an SLO were observed for the Flow.

Abstract Data Type:
: unsigned

Data Type Semantics:
: quantity

Additional Information:
: See {{!I-D.ietf-ippm-pam}} for the general definition of PAM.

Reference:
: This-Document

### Mean Time Between VIs  {#sec-mtbv}

Name:
: meanTimeBetweenViolatedIntervals

ElementID:
: TBD6

Description:
: Contains the Mean Time Between Violated Intervals over the duration of the Flow.
: The mean time is indicated by the number of intervals and thus corresponds to mean number of intervals between violated intervals.
: If severelyViolatedIntervalsCount is equal to 0, then the meanTimeBetweenViolatedIntervals must be 0.
: If severelyViolatedIntervalsCount is equal to 0, then the meanTimeBetweenViolatedIntervals must be violationFreeIntervalsCount DIV 2.

Abstract Data Type:
: unsigned

Data Type Semantics:
: quantity

Additional Information:
: See {{!I-D.ietf-ippm-pam}} for the general definition of PAM.

Reference:
: This-Document

### Mean Number of Packets Between VIs  {#sec-mpbv}

Name:
: meanNumberPacketsBetweenViolatedIntervals

ElementID:
: TBD7

Description:
: Contains the mean number of packets between packet-level violations over the duration of the Flow.
: if violatedPacketCount is equal to 0, then the meanNumberPacketsBetweenViolatedIntervals does not apply.

Abstract Data Type:
: unsigned

Data Type Semantics:
: quantity

Additional Information:
: See {{!I-D.ietf-ippm-pam}} for the general definition of PAM.

Reference:
: This-Document

>  TBD: Which special value to use to indicate that the meanNumberPacketsBetweenViolatedIntervals does not apply.

## IEs Representing SLO Manifest Information

The following IEs provide context regarding what "violations" and "severe violations" mean for a particular Flow.

In this version, IEs for the interval length and for a reference to an SLO are defined. Whether SLOs themselves are to be encoded, including the service level parameter subjected to the SLO (e.g., latency or packet delay variation), the objective itself (upper not-to-exceed threshold or lower threshold and threshold value) is for further study. Likewise, IEs to represent manifest information regarding severity semantics (for severe violations) are for further study.

### Precision Availability Interval Length  {#sec-pail}

Name:
: precisionAvailabilityIntervalLength

ElementID:
: TBD8

Description:
: Indicates the duration of an availability interval.

Abstract Data Type:
: unsigned

Data Type Semantics:
: identifier

Additional Information:
: See {{!I-D.ietf-ippm-pam}} for the general definition of PAM.

Reference:
: This-Document

### SLO Identifier  {#sec-sloid}

Name:
: sloId

ElementID:
: TBD9

Description:
: A reference to an SLO defining the semantics of what is considered precision availability for the Flow.

Abstract Data Type:
: unsigned

Data Type Semantics:
: identifier

Additional Information:
: See {{!I-D.ietf-ippm-pam}} for the general definition of PAM.

Reference:
: This-Document

## Precision Availability Metrics Not Considered

{{!I-D.ietf-ippm-pam}} lists a number of additional metrics for which no corresponding IEs are defined for the following reasons:

Time since the last violated interval:
: This is a metric that is of interest while a Flow is in progress, but arguably not applicable for export in a Flow Record once the Flow has concluded.

Number of packets since the last violated packet:
:  By the same token, this is a metric that is of interest while a Flow is in progress, not for export in a Flow Record once the Flow has concluded.

Time since the last severely violated interval:
:  Analogous reason as for "time since the last violated interval".

Number of packets since the last severely violated packet:
:Analogous reason as for "number of packets since the last violated interval".

Mean time between SVIs:
: For further study.

Mean packets between SVIs:
:  For further study.

Violated Interval Ratio:
:  This can be easily computed by the processor of the Record and does not warrant a separate IE.

Severely Violated Interval Ratio:
:  This can be easily computed by the processor of the Record and does not warrant a separate IE.


# Security Considerations

IPFIX security considerations are discussed in {{Section 8 of !RFC7012}}.

# IANA Considerations

This document requests IANA to add the following new IPFIX IEs to the IANA IPFIX registry {{IANA-IPFIX}}:

|Value|	Name|	Reference|
|TBD1| violatedIntervalsCount|{{sec-vic}} of This-Document|
|TBD2| violationFreeIntervalsCount|{{sec-vfic}} of This-Document|
|TBD3| violatedPacketCount|{{sec-vpc}} of This-Document|
|TBD4|severelyViolatedIntervalsCount|{{sec-svic}} of This-Document|
|TBD5|severelyViolatedPacketCount|{{sec-svpc}} of This-Document|
|TBD6| meanTimeBetweenViolatedIntervals|{{sec-mtbv}} of This-Document|
|TBD7| meanNumberPacketsBetweenViolatedIntervals|{{sec-mpbv}} of This-Document|
|TBD8| precisionAvailabilityIntervalLength|{{sec-pail}} of This-Document|
|TBD9| sloId|{{sec-sloid}} of This-Document|
{: title="New IPFIX Information Elements"}

--- back
