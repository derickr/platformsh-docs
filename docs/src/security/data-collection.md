---
title: Data collection
description: |
  As part of our normal business operations, we collect various types of data.
---

{{< description >}}

The following list describes the categories of data we collect in GDPR terms:

* Article 4 "personal data": Our accounts system contains some "personal data" (name, address, phone, etc.) to allow us to bill your account appropriately.  This information can be verified, changed, and deleted by [logging into your account](https://accounts.platform.sh/).
* Article 9 "special categories": We do not collect nor store any "special categories of personal data" (race, religion, sexual orientation, etc.) or any other attributes that are irrelevant to our business.
* Article 30 "records of processing activities": The only "records" we collect are IP addresses and Log files.

## Application logs

Application logs are those generated by the host application or application server (such as PHP-FPM). These logs are immutable to Customers to prevent tampering.They are also secured behind key-based SSH so that only the Customer and our relevant teams have access.

## System logs

Platform.sh records routine system logs.  We do not access Customer-specific system logs or the customer environment unless one of the following situations applies: 
1. we are requested to do so by the Customer,
2. to fix a problem or outage,
3. to prevent an outage, or
4. to comply with a legal obligation.

## Access logs

There are two main types of access logs: Web and SSH.

### Web access logs

Application access logs are immutable to Customers to prevent tampering. These logs are secured behind key-based SSH so that only the Customer and our relevant teams have access.

### SSH access logs

SSH access logs are securely stored in our infrastructure and are not accessible to Customers.  These logs can be accessed by Platform.sh support personnel as part of an audit, if requested.

Access by Customers and Platform.sh support personnel to customer environments is logged.  However, to protect Customer privacy, we only log the connection itself, not what was done during the session.

## Vendor data sharing

We have identified and mapped all data we collect and share with vendors. We know what categories of data are collected and to which geographical destinations such data is transferred. All of our vendors have been assessed for security and GDPR compliance.  We have the relevant Standard Contractual Clauses (SCCs) and Data Processing Agreements (DPAs) in place where applicable.
