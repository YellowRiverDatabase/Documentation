**The Tracks of Yu: Spatial Data for Yellow River History**  
**Data Release Notes**  
**Winter 2026**

[**Ryan Horne**](https://rmhorne.github.io/) **(ORCID: 0000-0002-5009-3116), [Nathan Michalewicz]() (ORCID: 0000-0002-0049-9206), [Ruth Mostern](https://www.history.pitt.edu/people/ruth-mostern) (ORCID: 0000-0001-8219-7174), Sharon Zhang** 

| About the Project |
| :---: |

**The Tracks of Yu: Spatial Data for Yellow River History** is a data publication hub that accompanies Ruth Mostern’s book [*The Yellow River: A Natural and Unnatural History*](https://yalebooks.yale.edu/book/9780300238334/the-yellow-river/) (Yale University Press, 2021), a three-thousand-year history of China’s Yellow River and the legacy of interactions between humans and the natural landscape. It is a lightly revised version of the data upon which the book was based.

*The Yellow River: A Natural and Unnatural History* demonstrates that from Neolithic times to the present day, the Yellow River and its watershed have both shaped and been shaped by human society. The data presented here is that which allowed Ruth Mostern, for that book to unravel the three millennium history of the human relationship with water and soil and the consequences, at times disastrous, of ecological transformations that resulted from human decisions. It demonstrates how governments consistently ignored the dynamic interrelationships of the river’s varied ecosystems—grasslands, riparian forests, wetlands, and deserts—and the ecological and cultural impacts of their policies. 

[The appendix to *The Yellow River*](https://github.com/YellowRiverDatabase/Documentation/blob/main/The_Yellow_River_A_Natural_and_Unnatural_History_----_\(Appendix_Tracking_Yu_Developing_a_Data_System_for_Yellow_River_History\).pdf) describes the sources that comprise this data collection and the process of database authorship. While we have made changes to the data structure and have made updates based on additional cleaning and accuracy checking, we have not completed new research since 2021 and we have not added new content.

The most substantial changes we have made since the publication of *The Yellow River* relate to the presentation of location information, which was embedded in the data and not disambiguated in the book as published. This has now been made explicit. This permits people to query by place and discover all of the events associated with each place.   
   
The database system as described in the appendix to *The Yellow River* worked as intended for the specific queries and functionality needed for the publication of the text, but since the focus of *The Yellow River* was on events rather than places, constructing a comprehensive and disambiguated gazetteer of locations was not necessary beyond ensuring that places were not duplicated in a specific event. Moreover, the query system was built within a Jupyter Notebook environment that greatly facilitated the work of the *Yellow River* team, but was also largely built around the specific research objectives, idiosyncratic interests, and working preferences of the team.

In order to make the data more accessible and compatible with other research projects and Linked Open Data systems, we have now reorganized the data according to a place-based perspective. Individual places impacted by river events now each have their own entry, and entries are cross-referenced with one other to create a single entry for each place. In addition, each place has its position within the administrative hierarchy defined for each time period. Creating the place name gazetteer involved a comprehensive multi-step process to clean, disambiguate, and enrich historical geographic data. The current version contains 4,225 core entities, each assigned a standardized “yrdb” ID, following the automated merging of exact text matches from a preliminary data dictionary of 6,731 entries. To address the limitations of automated matching, we conducted manual disambiguation using geographic coordinates and source validation to resolve entries with Unicode inconsistencies, formatting anomalies, or ambiguous references. The dataset was further enriched by classifying place names according to feature type, identified through frequency patterns in final characters. We produced a refined typology across administrative, settlement, natural, and water-related categories. To support broader accessibility and machine readability, we added Pinyin transliterations with careful adjustment for polyphonic characters and misclassifications. Supporting documentation tracked original forms and naming conflicts to maintain data transparency. Together, these processes produced a clean, disambiguated, and multilingual gazetteer available on our GitHub site, which supports both academic research and digital mapping applications.  
   
Anchored by a gazetteer of places, this database also offers tables that link places to events, events to event types, places to place types, places to geographic locations, events to sources, names to places, and geographic information including the changing courses of the Yellow River, geographic regions, and political / administrative regions. These are linked by disambiguated common identifiers, such as yrdb\_id being the unique ID for every place and event\_id as the unique ID for events.      

Data publication permits other projects and queries on related topics and allows anybody to explore details about specific places, eras, types of river events, or excerpts from primary sources that the book was not able to address. The data is freely available for download as well as online browsing through a simple online interactive map for exploratory inquiry.

In addition to the database, we have published a web atlas. The atlas is built as a [React](https://react.dev/) web application, using [MapBox](https://www.mapbox.com/) to render that map, and [Deck.gl](http://Deck.gl) to visualize the data. The React library allows for reactive interaction in modern web applications to update the current state of the site without re-rendering the entire html page. Since its introduction in 2013, React has transformed the way the web works through its reactive architecture that has been adopted across numerous JavaScript frameworks. For our purposes, React allows us to update the data available on the map without reloading the page. It and the DeckGL library are essential to render and update the page. DeckGL helps solve a structural problem in the world of data visualization, which is the need to visualize and explore large quantities of data without over summarizing it. Typical mapping software uses SVG technology to render nodes (data points) as standard HTML DOM (document-object-model, or the architecture of the browser user interface) elements over the top of raster tiles (pre-rendered images of the map). This model is CPU-centeric, limiting its ability to process large, complex datasets such as ours. Mapping libraries built on this architecture tend to slow as the number of features rendered grows closer to 1,000. DeckGL and Mapbox are built on vectorized tiles of the base maps (data representations of geographic features). Mapbox receives raw vector data of the map to render it dynamically, and it uses modern browser APIs (such as WebGL and WebGPU) to gain access to the computer’s Graphical Processing Units to render the map. When mixed with the DeckGL visualization library that similarly uses WebGL2 to accelerate graphics rendering for large quantities of data, we are able to render three-dimensional representations of data that would otherwise take too long to load through non-GPU-accelerated processes. 

| Building the Relational Database from Tables |
| :---: |

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

| About the Authors |
| :---: |

**Ryan Horne** is a Digital Research Specialist in the Office of Advanced Research Computing at the University of California, Los Angeles. A former software engineer, Ryan holds a Ph.D. in ancient history from UNC Chapel Hill. He is an expert in leveraging Linked Open Data for digital humanities research, with particular attention on geospatial and network analysis. In addition to working on dozens of digital projects, Ryan serves as a managing editor for the Pleiades project, was an awardee of a NEH/Mellon grant for digital publication, and has worked with the Digital Ethnic Futures Consortium, the Black Lunchtable oral history archive, the Ancient Itineraries Institute, Pelagios Network, and served as the director of the Ancient World Mapping Center at UNC.

**Nathan Michalewicz** is an Assistant Professor of History at Queens University of Charlotte. He received his Ph.D. at George Mason University, specializing in early modern Europe and digital history. He held a post doctoral position at the University of Pittsburgh, working on the World Historical Gazetteer and teaching courses on geo-spatial history. His research interests focus on Christian-Muslim interactions as well as the intersection of data analytics, information management, and history.

**Ruth Mostern** is Professor of History and Director of the Institute for Spatial History Innovation at the University of Pittsburgh, and President of the World History Association. She directed the Pitt World History Center from 2018 to 2025\. She is the author of two single-authored books: *Dividing the Realm in Order to Govern: The Spatial Organization of the Song State, 960-1276 CE* (Harvard Asia Center, 2011), and *The Yellow River: A Natural and Unnatural History* (Yale University Press, 2021), winner of the Joseph Levenson Prize from the Association for Asian Studies in 2022\.  She is also co-editor of *Placing Names: Enriching and Integrating Gazetteers* (Indiana University Press, 2016), and of a special issue of *Open Rivers Journal* (2017).  She is the author or co-author of over thirty articles published in books and peer reviewed journals.  Ruth is Principal Investigator and Project Director of the World Historical Gazetteer, a prize-winning digital infrastructure platform for integrating databases of historical place name information.  

**Sharon Zhang** is a Ph.D. candidate in History at the University of Pittsburgh. Her dissertation investigates material and cultural exchanges involved in gift-giving rituals between China and the wider world, particularly Europe, during the Mongol-Yuan era of the 14th-14th centuries. Her research examines how the circulation of tributary gifts facilitated the formation of transregional networks and influenced power dynamics in the global medieval world. Sharon’s broader academic interests extend to Sino-foreign interactions during China’s middle period (circa 800-1400), the experiences of medieval Eurasian travelers along both overland and maritime Silk Routes, and spatial and digital approaches for premodern history studies.   


[image1]: <data:image/png;base64,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>
