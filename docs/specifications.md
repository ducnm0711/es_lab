[[_TOC_]]

# Functional and technical specification of ElasticSearch Cluster

## Questions

- Which email domains are used ?
  List to be completed:
    - mra.mu
    - ntla.mu
    - example.com 
- Which unique identifier is used for user authentication ?
  Several possibilities:
    - email
    - left part of email
    - employeeNumber
    ...
- Is there moderation for sending to large email list ?  
  yes
- Should we provide replicas to relieve the master directories?
  no, 2 nodes configured in multi-master replication mode
- What is a functional mailbox ?*
  A functional mailbox is a group with an email address and whose type is 'mailbox'

## Introduction

The LDAP directory is a technical groupware directory. 

It is used for managment and definition of: 
  - individual user mailbox 
  - groups: 
    - functional mailbox 
    - mailing list 
  - organizational unit (hierarchical organization chart of government) 
  - password policies (one default password policies and others)
  - application services account  

## Data

###  Data DIT structure

DIT struture is:

```
c=mu
└── o=gov
    ├── ou=users
    ├── ou=groups
    │    └── ou=lists
    ├── ou=infra
    │   ├── ou=accounts
    │   │   ├── ou=admin
    │   │   ├── ou=read
    │   │   └── ou=write
    │   └── ou=groups
    ├── ou=nomenclature
    │       └── ou=domains
    ├── ou=organization
    └── ou=ppolicies

```

Branches description:
- `users`: user mailboxes (flat list)
- `groups`: groups, mailing lists, functional mailboxes (flat list)
- `groups/lists`: mailing lists (flat list)
- `infra/accounts`: accounts used by applications, sub-ou are used for LDAP ACL and performance. 
  - `read`: accounts with read access except userPassword attribute. 
  - `write`: accounts with read/write access except userPassword attribute. 
  - `admin`: accounts with full read/write access. 
- `nomenclature`: nomenclature
- `nomenclature/domains`: mauritius government mail domains (hierarchical structuration)
- `organization`: mauritius government chart (hierarchical structuration)
- `ppolicies`: default and other password policies

###  Data Model

#### Schemas  

Standard used schemas are:
- core : core openlda schema
- cosine : cosine and X509 schema
- nis: network information Service schema
- inetorgperson : schema for individual person 
- ppolicy : password policy schema
- dyngroup : schema for dynamic groups defined by LDAP URI 

Specific used schema is:
- qmail : qmail **partial** schema for mail attributes
- twake: specific attributes and objects for twake groupware platform 

#### Twake schema

##### OID

Proposal for identification of Twake specific schemas:

| OID                          | Description                                                                           |
| ---------------------------- | ------------------------------------------------------------------------------------- |
| 1.3.6.1.4.1                  | IANA-assigned company OIDs, used for private MIBs                                     |
| 1.3.6.1.4.1.4203             | OpenLDAP                                                                              |
| 1.3.6.1.4.1.10943            | Linagora (Cf [IANA](https://www.iana.org/assignments/enterprise-numbers/?q=linagora)) |
| 1.3.6.1.4.1.10943.1          | Linagora LDAP OIDs                                                                    |
| 1.3.6.1.4.1.10943.2          | Linagora SNMP OIDs                                                                    |
| 1.3.6.1.4.1.10943.3          | Linagora PKI OIDs                                                                     |
| 1.3.6.1.4.1.10943.1.50       | Linagora Twake Platform LDAP OIDs                                                     |
| 1.3.6.1.4.1.10943.1.50.1     | Linagora Twake Platform LDAP attribute types OIDs                                     |
| 1.3.6.1.4.1.10943.1.50.2     | Linagora Twake Platform LDAP object classes OIDs                                      |
| 1.3.6.1.4.1.10943.1.50.3     | Linagora Twake Platform LDAP attribute syntaxes OIDs   , functional mailboxes                                |
| 1.3.6.1.4.1.10943.1.50.3     | Linagora Twake Platform LDAP matching rules OIDs                                      |
| 1.3.6.1.4.1.10943.1.50.5     | Linagora Twake Platform LDAP controls OIDs                                            |
| 1.3.6.1.4.1.10943.1.50.6     | Linagora Twake Platform LDAP extended operations OIDs                                 |
| 1.3.6.1.4.1.10943.1.50.7     | Linagora Twake Platform LDAP features OIDs                                            |
| 1.3.6.1.4.1.10943.1.50.nnn   | Linagora Twake Platform LDAP nnn OIDs                                                 |
| 1.3.6.1.4.1.10943.1.50.1.0   | First Linagora Twake Platform LDAP attribute type OID                                 |
| 1.3.6.1.4.1.10943.1.50.1.1   | Second Linagora Twake Platform LDAP attribute type OID                                |
| 1.3.6.1.4.1.10943.1.50.1.nnn | nnn Linagora Twake Platform LDAP attribute type OID                                   |


##### Objects and attributes

**User**

Example: 
```
```



| Attribut          | Type     | Mandatory | Value example   |   Description        |
| ----------------- | -------- | --------- | --------------- | -------------------- |
| 



deliveryMode: 
- normal: Maildir/box delivery only if no forwards or programs are executed
- nombox: ignore all maildir/mbox deliveries
- forward: forward messages
- localdelivery: always deliver locally. In conjunction with forwarding this means to deliver a local copy.
- reply: send also an auto-reply-mail with text from mailReplyText

It is possible to set more than one value, be carefull.


**Group**




##  Configuration 

###  Configuration DIT structure 

TBD





## Technical specification

### Hardware 

2 load-blancers ha-proxy (2 vCPU, 2 Go)
2 master directories in multi-master replication mode (2 vCPU, 2 Go)
2 slave directories  (read only for applications) ?

### Software 

Système: debian12
Annuaire: openldap LTB 2.6


# LDAP Directory

## DIT 

### DATA 

Base DN : ```c=gov,c=mu```

### Configuration

Base DN : ```cn=config```

### Monitoring 

Base DN : ```cn=monitoring```

## LDAP Accounts

- RootDN Administation DIT: cn=admin,o=gov,c=mu 

- RootDN Administration configuration LDAP : cn=config

- RootDN Monitoring LDAP: cn=monitor

- ldapsync service account dn: uid=ldapsync,ou=read,ou=accounts,ou=infra,o=gov,c=mu

- LinID DM service account dn: uid=linniddm,ou=admin,ou=accounts,ou=infra,o=gov,c=mu

- tmail service account dn: uid=tmail,ou=read,ou=accounts,ou=infra,o=gov,c=mu

- lemonldap service account dn: uid=lemonldap,ou=admin,ou=accounts,ou=infra,o=gov,c=mu

**NB**: 
  - default password is MAUSecret2008! (***Password must be changed***).

## LDAP user Accounts

**NB**: 
  - default password is Secret2008# (***Password must be changed***).




===========================================

CR


mailForwardingAddress 


