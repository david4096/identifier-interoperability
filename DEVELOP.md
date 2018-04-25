# Development for the Identifier Interoperability Document

## minid development

As a demonstration of how to use minids, this repository has been given
a minid using the below method:

### Install dependencies

I started by [generating an ORCID](https://orcid.org/my-orcid) for myself, if you haven't already done
so. This helps with provenance for identifiers you ask to have created.

The minid client is available via pypi and is in the `requirements.txt`
of this repository.

```
virtualenv env
source env/bin/activate
pip install -r requirements.txt
```

### Create a minid account

Then following the directions from https://github.com/fair-research/minid

```
minid --register_user --email <email> --name "<name>" --orcid  0000-0001-6683-2270
```

### 
