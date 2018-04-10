# Identifier Interoperability

This document offers ways for platforms that are part of the NIH Data Commons
Pilot to demonstrate Key Capability 2 (KC2), which coordinates the findability of
data across Commons Platforms. This document was prepared for Team Calcium 
in order to coordinate the interoperability of data access across its members.
Examples and links from existing platforms will be added as they become
available.

If you know of useful Identifier Schemes or Services please make a Pull Request!

1. [Data Findability](#findability)
2. [Concepts](#concepts)
3. [Use Cases](#usecases)
4. [Core Metadata](#coremetadata)
5. [Identifier Schemes](#schemes)
6. [Identifier Services](#services)
7. [Case Studies](#casestudies)

## Data Findability <a name="findability"></a>

We don't know all the things we would like to identify ahead of time, so
the hope is that we can support the identifier schemes that folks have, when they
have them. Identifier schemes are often tied to projects, and in the case of 
[TCGA barcodes](https://wiki.nci.nih.gov/display/TCGA/TCGA+barcode) even contain
some metadata.

This document is meant to provide strategies for Data Platforms, which have 
internal data management needs, to integrate with Identifier Services, 
Resolvers, and Prefix Services. Though Data Platforms may offer these 
services internally, examples and use cases include interoperation with some
popular and common services and identifier schemes.

Data that is within a Data Platform will usually guarantee uniqueness 
within that platform. However, the same identifier may represent different 
data in another platform. Globally Unique Identifiers (GUIDs) provide a 
way to address data across various platforms.

This document's Use Cases are structured in such a way to protect Data 
Platforms from changes that would alter their internal functionality: 
Data Platforms should use the identifier scheme that best suits their 
existing use case. 

Satisfying Key Capability 2 (GUIDs) minimally requires Data Platforms 
to maintain aliases to GUIDs when they are available to satisfy the needs 
of findability or reproducibility. This requires, at least, a way to 
modify metadata to include newly "minted" GUIDs and later find that data
using the new identifier.

## Concepts <a name="introduction"></a>

### Data 

For the purposes of this document it is important to separate the concepts of
data from metadata. Data are the first order items one would like to share, 
for example, a VCF might be data, while the file checksum would be metadata.

### Metadata

Metadata describe data and are usually string keys paired with string, numeric, 
array, or object-like values. Metadata should be representable in JSON schemas.

### JSON

JavaScript Object Notation is a scheme for transmitting data between web 
services. Both metadata and interface methods for services in this document
communicate using JSON.

### Data Object

A file, resource, or API that has been uniquely identified for a given 
service, and which provides a minimum of fields from the Data Object 
Service schema.

### Data Provider

Data providers coordinate their platforms using internal tools, which they 
have they often have the ability to rapidly iterate on. Data providers may 
provision data using a variety of storage and metadata indexing services.

### Identifier Service

Registries allow identifiers for data to be managed and shared separately
from the data. They require core metadata in order to register an item.

### Prefix Service

In order to provide stable identifier namespaces, prefix services like 
[identifers.org](https://identifiers.org) allow one to redirect prefixes, 
like `dos`, to stable URLs. They only store metadata about the service.

### Identifier Resolver Service

Using a given identifier, these services allow clients to find the proper 
service to resolve more metadata about the Data Object.

### Identifier Scheme

The template that is used to issue new identifiers for a given service, 
for example, UUID has the format `4be0071d-b36e-4414-a7ee-7b879f60be7a`, 
whereas, another service may iterate numerically from 0.

## Use Cases <a name="introduction"></a>

### 1 Providing a GUID for a Data Object via client

A Data Provider offers some data that can be uniquely identified using an
internal identifer scheme. An authorized client accesses this metadata, 
and makes a local copy. The client then modifies the local metadata format 
to accord to an Identifier Service's schema. The client then requests 
a "newly minted" identifier for the Data Object from the Identifier Service.
The Identifier Service responds with the GUID and the client makes 
and authorized request to modify the metadata to include the GUID.

### 2 Providing a GUID for a Data Object automatically

By automating the interaction with Identifier Services, Data Providers 
can automatically make their data resolvable using GUIDs (like DOI, 
ark, etc.). To satisfy this use case, when new data to be given a GUID
are indexed by a data provider, they should request a new identifier
and update the metadata similar to A.

### 3 Using a client to find data using a GUID

A client with a GUID should be able to make a request to a Data Provider 
for data that matches that GUID. If the metadata for the item includes 
a GUID, the metadata for that item will be returned, which includes 
details necessary to access or download the Data Object.

### 4 Resolving Data Object Identifiers across platforms

A client with a Data Object Identifier should be able find the Data 
for that identifier without requesting from each of the Commons 
Platforms. Instead of making the request against each platform, 
they make their request to an identifier resolver, which will either
return the proper metadata from the Data Provider, or redirect 
the client to it.

### 5 Resolving Data Object Identifiers across platforms using a Prefix Service

Using an identifier and a prefix, a client should be able to request 
more metadata for a given Data Object. The client first makes a request 
against a prefix service with the proper prefix and identifier, the 
request is then redirected to an Identifier Resolver Service, which 
either redirects or returns metadata necessary to access the Data 
Object.


## Core Metadata Requirements <a name="introduction"></a>

The minimal metadata required to register for a GUID depends on the 
underlying service and use case. However, to improve interoperability, 
these metadata should be describable using [JSON schemas](http://json-schema.org/).

For the purposes of registering a Data Object, which makes accessible 
some data via URL, it is expected that a URL at minimum is provided. A 
list of typed checksums should be provided when available to 
verify downloads. For more information see [Data Object Service Schemas](https://github.com/ga4gh/data-object-service-schemas).

Identifier services will require more or less information depending 
on the use case covered. For example, issuing a DOI for a paper would require 
a list of authors.

## Identifier Schemes <a name="schemes"></a>

Data Platforms are NOT required to use a normalized identifier scheme.
A number of identifier schemes exist that can be used to ensure 
uniqueness across services.

* [minid](http://eid.difi.no/en/minid) - Minimum viable identifier.
  * [White paper](http://bd2k.ini.usc.edu/assets/all-hands-meeting/minid_v0.1_Nov_2015.pdf)
* [Archival Resource Key (ark)](https://ipfs.io/ipfs/QmXoypizjW3WknFiJnKLwHCnL72vedxjQkDDP1mXWo6uco/wiki/Archival_Resource_Key.html) - Persistent URL based identifiers.
* [Universally Unique Identifier (UUID)](https://en.wikipedia.org/wiki/UUID) - 128-bit number to uniquely address data.
* [ORCID](https://orcid.org/) - Link researchers and research.

## Identifier and Prefix Services <a name="services"></a>

Public services for registering identifiers exist. They differ in 
their required metadata, provided services, and necessary metadata.
These services should be used with the appropriate above services
to register a GUID as necessary.

* [Name to Things (n2t.net)](http://n2t.net/) - Register URLs to resolve identifiers.
* [identifiers.org](http://identifiers.org/) - Register prefixes, interoperable with n2t.
* [DataCite.org](https://www.datacite.org/) - Register DOIs.

## Case Studies

### HCA Data Storage System Case Study <a name="casestudy"></a>

To provide practical instruction into how GUID resolution can 
work in the in the Data Commons we offer this brief case study 
of interoperating with the Human Cell Atlas Data Storage System, 
which replicates data across cloud stores.

TODO

