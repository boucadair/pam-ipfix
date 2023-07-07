---
coding: utf-8

title: Export of Flow Precision Availability Metrics Using IPFIX
abbrev: pam-ipfix
docname: draft-clemm-opsawg-ippm-pam-ipfix-latest
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
  RFC7011:
  RFC7012:
  I-D.ietf-ippm-pam:

--- abstract

This document defines a set of IP Flow Information eXport (IPFIX) Information Elements that allow to export precision availability data associated with IP flows, specifically flows that are associated with stringent service level objectives such as latency or latency variation (aka jitter).  

--- middle

# Introduction

IP Flow Information eXport (IPFIX) {{RFC7011}} is a protocol that is widely deployed in operators networks to collect records containing a wide array of statistics about IP flows.  The records are used for a wide array of purposes, including network security (e.g. detection of denial-of-service attacks), accounting (e.g. identifying "top talkers"), monitoring and service assurance (e.g. analyzing records for anomalies), and network planning (e.g. maintaining traffic matrices and detecting usage trends).  Accordingly, at the core of IPFIX is the definition of a set of basic data items that can be maintained by a network device in a flow cache and exported as part of a flow record.  These data items are commonly referred to as Information Elements (IEs) {{RFC7012}}. 

Increasingly, to be provided with mere connectivity is no longer sufficient for many networking applications.  There is a growing demand for high-precision services that underly stringent service level objectives (SLOs), such as a given latency that must be met.  When a guaranteed property of a service is not met, this will in many cases be considered as equivalent to the service not being available.  This is particularly the case in which the application using the service does not degrade gracefully with deteriorating service levels (examples would be video or voice), but in which violation of an SLO will cause the application to abruptly cease to function (examples include industrial control and Control-as-a-Service applications or telehaptics). 

IPFIX IEs currently focus largely on statistics such as traffic volume, packet lengths, header fields, or route properties.  However, there is currently a lack of IEs that indicate a flow's "quality".  Specifically, IPFIX does not support IEs that indicate compliance of a flow with an SLO.  This specification aims to close that gap.  It defines a set of IEs that are based on Precision Availability Metrics that have been recently subjected to standardization {{I-D.ietf-ippm-pam}}.  This allows those metrics to be exported as part of flow records using IPFIX.  


# Terminology

This document uses IPFIX-specific terminology as defined in Section 2 of {{RFC7011}}.  In addition, this document uses terminology associated with Precision Availability Metrics, as defined in Section 2 of {{I-D.ietf-ippm-pam}}.  For the reader's convenience, some of the acronyms that are defined in those documents are listed below.   

IE
: Information Element

IPFIX
: IP Flow Information eXport

PAM
: Precision Availability Metric

SLO
: Service Level Objective

VI
: Violated Interval

VFI
: Violation-Free Interval

# Precision Availability Information Elements 

The following section defines a set of Information Elements to allow precision availability data to be exported as part of flow records.  At the core of precision availability metrics is the notion of an "interval", i.e. an observation interval (a small unit of time) for which the presence or absence of violations is noted.  What constitutes a violation or not depends on the definition of the service - i.e. the length of the interval (for example a millisecond) and the service level objective (for example, a not-to-exceed latency treshold or packet interarrival delay treshold).  

Accordingly, IEs are grouped into two categories.  The first category contains IEs that reflect precision availability metrics per {{I-D.ietf-ippm-pam}}.  The second category contains IEs that are used to define context that is necessary to interpret IEs in the first category correctly, such as the SLO that underlies the definition of precision availability for that particular flow.  This context can be thought of as a manifest for that flow record.  

## IEs based on Precision Availability Metrics

### Violated Intervals Count

Name: viCount

ElementID: TBD

Description: Contains a count of intervals over the duration of the flow during which the service was not available with the required precision, i.e. a count of intervals for which an SLO violation was observed.  

### Violation-Free Intervals Count

Name: vfiCount

ElementID: TBD

Description: Contains a count of intervals over the duration of the flow during which the required precision was available, i.e. during which the flow was in compliance with its SLO.  In practical terms, the vfiCount corresponds to the number of intervals over the duration of the flow minus the viCount.  

TBD: Assess size of this parameter (for the case of long flow durations with short interval durations).   

### Violated Packet Count

Name: vpCount

ElementID: TBD

Description: Contains a count of packets for which packet-level violations of an SLO were observed for the flow.  

### Severely Violated Intervals Count

Name: sviCount

ElementID: TBD

Description: Contains a count of intervals over the duration of a flow during which a particularly severe violation was observed.  

### Severely Violated Packet Count

Name: svpCount

ElementID: TBD

Description: Countains a count of packets for which particularly severe packet-level violations of an SLO were observed for the flow.  

### Mean Time between VIs

Name: mtbv

ElementID: TBD

Description: Contains the Mean Time Between Violated Intervals over the duration of the flow.  The mean time is indicated by the number of intervals, so really corresponds to Mean Number of Intervals between violated intervals.  In case of an sviCount of 0, the mtbv is 0.  In case of an sviCount of 0, the mtbv corresponds to vfiCount DIV 2.   

### Mean Number of Packets between VIs

Name: mpbv

ElementID: TBD

Description: Contains the Mean Number of Packets between packet-level violations over the duration of the flow.  In case of a vpCount of 0, mpbv does not apply (to be indicated by a special value for further study).  

## IEs representing SLO Manifest Information

The following IEs provide context regarding what "violations" and "severe violations" mean for the particular flow.  For the time being, IEs for the interval length and for a reference to an SLO are being defined.  Whether SLOs themselves are to be encoded - including the service level parameter subjected to the SLO (e.g. latency, packet delay variation), the objective itself (upper not-to-exceed threshold or lower threshold and threshold value) is for further study.  Likewise, IEs to represent manifest information regarding severity semantics (for severe violations) are for further study.  

### Precision Availability Interval Length

Name: pail 

ElementID: TBD

Description: Indicates the duration of an availability interval. (for further study)

### SLO Identifier

Name: sloId

ElementID: TBD

Description: A reference to an SLO defining the semantics of what is considered precision availability for the flow.  (for further study)

## Precision Availability Metrics not considered here

{{I-D.ietf-ippm-pam}} lists a number of additional metrics for which no corresponding IEs are defined, for reasons as stated below.  

- Time since the last violated interval.  This is a metric that is of interest while a flow is in progress, but arguably not applicable for export in a flow record once the flow has concluded.  

- Number of packets since the last violated packet.  By the same token, this is a metric that is of interest while a flow is in progress, not for export in a flow record once the flow has concluded.  

- Time since the last severely violated interval.  Analogous reason as for "time since the last violated interval". 

- Number of packets since the last severely violated packet. Analogous reason as for "number of packets since the last violated interval". 

- Mean time between SVIs. For further study.   

- Mean packets between SVIs.  For further study.  

- Violated Interval Ratio.  This can be easily computed by the processor of the record and does not warrant a separate IE.  

- Severely Violated Interval Ratio.  This can be easily computed by the processor of the record and does not warrant a separate IE.  


# Security Considerations

TBD

# IANA Considerations

This document requests IANA to add the following new IEs to the IANA registry entitled "IP Flow Information Export (IPFIX) Entities": 

TBD

--- back
