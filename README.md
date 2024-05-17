# APIS docker-compose

This repo is meant to allow for an easy and fast setup of a local [APIS](https://acdh-oeaw.github.io/apis-core-rdf/) instance.
It contains submodules to the [apis-base-container](https://github.com/acdh-oeaw/apis-base-container), which provides the basic docker container to run an APIS instance and to the [OEBL APIS instance](https://github.com/acdh-oeaw/apis-instance-oebl-pnp) which serves as a APIS ontology. Please note that the latter can be exchanged for any other APIS instance such as [Frischmuth](https://github.com/acdh-oeaw/apis-instance-frischmuth), [SiCProd](https://github.com/acdh-oeaw/apis-instance-sicprod) or [Tibschol](https://github.com/acdh-oeaw/apis-instance-tibschol).

Additionally it contains a `docker-compose` file that adds a local PostgreSQL DB and wires everything together, an `apis_local_settings.py` file to override any settings for local development and an `apis_settings.env` file to set environment variables for the APIS container.


# Startup and Configuration

To start a local APIS installation you need to have docker and docker-compose (or drop-in replacements such as podman) installed. The repo uses symlinks to put all the files in the correct location. If you are on a Windows machine, please make sure that symlinks work properly on your system or copy the files over.

To start simply type `docker-compose up` in your shell. If you changed any files that get copied in the docker container don't forget to restart with `docker-compose --build`.

# Usage

As soon as the system has started you should be able to access it under http://0.0.0.0:5000/
By default APIS instances are password protected. The docker-compose variant creates a user called `admin` with the password `admin`. These values can be changed in the `apis_settings.env` file.

If you are using the OEBL ontology (default) you need to create relation properties by hand. To do so:

- head to http://0.0.0.0:5000/admin/ the admin page allows you to create users, entities etc. 
- but we are mainly interested in the [properties](http://0.0.0.0:5000/admin/apis_relations/property/)
- [add a property](http://0.0.0.0:5000/admin/apis_relations/property/add/). To do so you need to define the set of Subjects (subj) that the property allows and the set of Objects (obj) it allows for. Given that a relation in APIS is directed you need to define a `Name forward` (shown if you look at the relation from Subject position) and a `Name reverse` (shown if you look at the relation from Object position). Please note that you can select several Subjects and several Objects.
- Before you leave the Admin interface, head to [Profession Categories](http://0.0.0.0:5000/admin/apis_ontology/professioncategory/) and create at least one profession category. In the OEBL ontology every Person needs to have one Profession Category and thus you cant create a Person without such an entry.
- Now you can create a [test person](http://0.0.0.0:5000/apis/apis_ontology.person/create). Add some values in the fields, select the Profession Category you just created and click `submit`.
- This brings you to the edit form of the Person you just created. You can further edit the metadata on the left and/or relate it to other entities on the right (if you dont have any other entity types in the right panel, head back to admin and add a property with Person selected in Subj and eg Place selected in Obj).
- For relating a Person to eg a Place you dont need to manually create the Place entry anymore but can search in the entities autocomplete and/or copy a reference URI from [GeoNames](https://www.geonames.org/), [Wikidata](https://www.wikidata.org/wiki/Wikidata:Main_Page) or [GND](https://lobid.org/gnd) in the autocomplete field. Eg https://sws.geonames.org/2764359/about.rdf (Steyr).


If you encounter problems, or have any questions please consider our [documentation](https://acdh-oeaw.github.io/apis-core-rdf/) or ask a question in the [GitHub Question Board](https://github.com/acdh-oeaw/apis-core-rdf/discussions).

