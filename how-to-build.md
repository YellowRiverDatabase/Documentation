# Database Build Instructions

The relational database can be rebuilt from the .csv files available on the GitHub repository. This may be desirable to create complex and custom queries, to integrate the data with other data sources, or to perform different geospatial operations through QGIS, Esri products, Python, or other analysis environments. For routine placename queries, the accompanying web atlas provides an adequate interface to support interactive exploration of metadata, name-based search, event-year filtering, and other simple visualizations. 

One of the most popular options for creating a relational database with geospatial data is PostgreSQL with the PostGIS extension, which allows for native geospatial object support and spatial reasoning within the database itself. This is an ideal choice when storing geospatial data for use with multiple programs such as QGIS, ArcMap, or Python applications. SQLite is another option; there are no native geospatial operations on the data, but the format is an efficient means of serving or storing the data when space is an issue and geospatial reasoning and data typing can be performed by other software solutions. 

After installing the database software of your choice, the next step is to download the geodata repository located at [https://github.com/YellowRiverDatabase/geodata](https://github.com/YellowRiverDatabase/geodata). The files in the repository are for reconstructing the database, and are as follows:

places.csv: This file holds the unique ID as well as a Chinese name and an English transliteration.

* place\_id: The unique ID of the place  
* tr\_title: Chinese characters of a representative name for the place  
* ch\_pinyin: English transliteration of the name

places\_to\_attestation.csv: This file associates the places with attestations from the original body of work for the yrdb

* ​​ptatt\_id: The unique ID of the entry  
* place\_id:  place\_id from the places table  
* attestation\_id: old ID information

places\_to\_establishment.csv: This table holds the establishment data for places (if known)

* plest\_id: The unique ID of the entry  
* place\_id:  place\_id from the places table  
* Est\_year: Year the place was established

places\_to\_types.csv: This table links a place to any number of different place types

* ptty\_id: The unique ID of the entry  
* place\_id: place\_id from the places table  
* ft\_id: yft\_id from the feature\_types table

event\_types.csv: A mini-ontology of the different event types and classes in the *Yellow River* analysis.

* event\_type\_id: a unique ID for the event type  
* zh\_cn\_category: Chinese characters of which category the event belongs to  
* zh\_ch\_title: Chinese character title of the event type  
* en\_category: English description of the category  
* en\_title: English transliteration of the title  
* en\_type: English translation of the event type  
* description: more information about the particular event type

events\_to\_places.csv: This table connects individual events to any number of places by the event\_id and the place\_id.

* evtp\_id: The unique ID of the entry  
* place\_id: place\_id from the places table 	  
* attestation: This is an id field that allows the database to cross-reference with the legacy tables that it was built from	  
* event\_id: Event ID from the events table.


sources\_to\_events.csv: This table connects individual sources to their events 

* src\_to\_evt\_id: The unique ID of the entry  
* event\_id: The ID from the events table  
* source\_id: The ID from the sources table 

events\_to\_types.csv: this table matches each event with one or more types

* evetotyp\_id: The unique ID of the entry  
* event\_id: Event ID from the events table.  
* event\_type\_id: ID from the event\_types table

events.csv: This table is an event gazetteer that contains basic information for each event

* event\_id: The unique ID for the event  
* ch\_date: date of the event in Chinese characters  
* en\_date\_start: At least the starting year of the event, if known  
* en\_date\_end : At least the ending year of the event, if known  
* description: An optional English description of the event  
* notes: An optional field for further necessary information


event\_categories.csv: This table contains the categories and descriptions for events

* evc\_id: The unique ID of the entry  
* zh\_cn\_category: The category in Chinese characters  
* en\_category: The category in English

feature\_types.csv: This table delineates the different types of features in the database

* yft\_id: The unique ID of the entry  
* fct\_id: The ID from the feature\_class table  
* ch\_title: Unique Chinese title of the feature type  
* en\_title: English translation of the title      
* ref\_num : Reference for feature types

feature\_class.csv: This table differentiates different feature classes

* fct\_id: The unique ID of the entry  
* feature\_class: Description of the feature class

locations.csv: This table provides information on the geospatial locations of different places. There may be more than one location assigned to each place.

* location\_id: The unique id of the location within the database  
* place\_id: place\_id from the places table 	  
* attestation: This is an id field that allows the database to cross-reference with the legacy tables that it was built from	  
* lat: numeric latitude value  
* lon: numeric longitude value

names.csv: This lists various names for each place; each place may have more than one name

* name\_id: The unique id of the location within the database      
* name: The actual name      
* ch\_pinyin: a transliteration of the name into English, if necessary      
* place\_id: place\_id from the yrdb\_places table 	  
* attestation: This is an id field that allows the database to cross-reference with the legacy tables that it was built from	

sources.csv: This table has information on the sources used for the project

* source\_id: The unique ID of the entry  
* source: The source of the information      
* page: Page in the source      
* chinese\_date: The date in chinese characters of the source      
* western\_date: The date of the source in western format  
* old\_placename\_chinese: Placename association (historical)       
* modern\_placename\_chinese: Placename association (modern)  
* event\_type\_chinese: Event type according to the source (superseeded by the database)  
* event\_name: Event name from the source (superseeded by the database)       
* event\_description: Event description from the source (superseeded by the database)      
* primary\_source\_1: Source text      
* primary\_source\_2: Source text  
* notes: Any additional notes    

The table structure for the finished database respecting primary (PK) and foreign (FK) keys are:  
---

**places**

| Column | Type | Key |
| ----- | ----- | ----- |
| place\_id | varchar | PK |
| tr\_title | varchar |  |
| ch\_pinyin | varchar |  |

---

**feature\_class**

| Column | Type | Key |
| ----- | ----- | ----- |
| fct\_id | varchar | PK |
| feature\_class | varchar |  |

---

**feature\_types**

| Column | Type | Key |
| ----- | ----- | ----- |
| yft\_id | varchar | PK |
| fct\_id | varchar | FK → feature\_class.fct\_id |
| ch\_title | varchar |  |
| en\_title | varchar |  |
| ref\_num | int |  |

---

**locations**

| Column | Type | Key |
| ----- | ----- | ----- |
| location\_id | varchar | PK |
| place\_id | varchar | FK → places.place\_id |
| attestation | varchar |  |
| lat | decimal |  |
| lon | decimal |  |

---

names

| Column | Type | Key |
| ----- | ----- | ----- |
| name\_id | varchar | PK |
| name | varchar |  |
| ch\_pinyin | varchar |  |
| place\_id | varchar | FK → places.place\_id |
| attestation | varchar |  |

---

**event\_categories**

| Column | Type | Key |
| ----- | ----- | ----- |
| evc\_id | varchar | PK |
| zh\_cn\_category | varchar |  |
| en\_category | varchar |  |

---

**event\_types**

| Column | Type | Key |
| ----- | ----- | ----- |
| event\_type\_id | varchar | PK |
| zh\_ch\_title | varchar |  |
| en\_title | varchar |  |
| en\_type | varchar |  |
| evc\_id | varchar | FK → event\_categories.evc\_id |
| description | text |  |

---

**events**

| Column | Type | Key |
| ----- | ----- | ----- |
| event\_id | varchar | PK |
| ch\_date | varchar |  |
| western\_date | varchar |  |
| description | text |  |
| notes | text |  |

---

**events\_to\_places**

| Column | Type | Key |
| ----- | ----- | ----- |
| evtp\_id | varchar | PK |
|  |  |  |
| place\_id | varchar | FK → places.place\_id |
| attestation | varchar |  |
| event | varchar | FK → events.event\_id |

---

**sources**

| Column | Type | Key |
| ----- | ----- | ----- |
| source\_id | int | PK |
| source | varchar |  |
| page | int |  |
| chinese\_date | varchar |  |
| western\_date | varchar |  |
| old\_placename\_chinese | varchar |  |
| modern\_placename\_chinese | varchar |  |
| event\_type\_chinese | varchar |  |
| event\_name | varchar |  |
| event\_description | varchar |  |
| primary\_source\_1 | varchar |  |
| primary\_source\_2 | varchar |  |
| notes | varchar |  |

---

**sources\_to\_events**

| Column | Type | Key |
| ----- | ----- | ----- |
| src\_to\_evt\_id | varchar | PK |
| event\_id | varchar | FK → events.event\_id |
| source\_id | int | FK → sources.source\_id |

---

**events\_to\_types**

| Column | Type | Key |
| ----- | ----- | ----- |
| evetotyp\_id | varchar | PK |
| event\_id | varchar | FK → events.event\_id |
| event\_type\_id | varchar | FK → event\_types.event\_type\_id |

---

**places\_to\_types**

| Column | Type | Key |
| ----- | ----- | ----- |
| ptty\_id | varchar | PK |
| place\_id | varchar | FK → places.place\_id |
| ft\_id | varchar | FK → feature\_types.yft\_id |

---

**places\_to\_attestation**

| Column | Type | Key |
| ----- | ----- | ----- |
| ptatt\_id | varchar | PK |
| place\_id | varchar | FK → places.place\_id |
| attestation\_id | int |  |

---

**places\_to\_establishment**

| Column | Type | Key |
| ----- | ----- | ----- |
| plest\_id | varchar | PK |
| place\_id | varchar | FK → places.place\_id |
| est\_year | int |  |

![][image1]

To build the database in an ER model, you should build the base tables of information, then link them together with foreign keys. You could also use these tables as the basis for a graph database.

The recommended order for building the database is:

1. Import places.csv  
2. Import events.csv  
3. Import sources.csv  
4. Import event\_categories.csv  
5. Import event\_types.csv and link to event\_categories  
6. Import events\_to\_types.csv and link to events and event\_types  
7. Import events\_to\_places.csv and link to events and places  
8. Import sources\_to\_events.csv and link to events and sources  
9. Import feature\_class.csv  
10. Import feature\_types.csv and link to feature\_class  
11. Import locations.csv and link to places table  
12. Import places\_to\_establishment.csv and link to places table  
13. Import places\_to\_types.csv and link to places and feature\_types  
14. Import names.csv and link to places table  
15. Import places\_to\_attestation.csv and link to places
