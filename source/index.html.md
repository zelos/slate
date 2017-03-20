---
title: ETL Docs | Zelos

language_tabs:
  - shell
  - ruby
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the Kittn API! You can use our API to access Kittn API endpoints, which can get information on various cats, kittens, and breeds in our database.

We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/tripit/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

#Setup
Python 3.5
Mongodb

# Naming Conventions
## Each Scrapy item is one of 2 kinds...
   1) An entity (6 total)
   2) A relationship between two entities (14 total)

## Entities
 * labeled as: `ent_<entity_type>`
 * Each entity item is 1 of 6 kinds
 * Each entity item has:
   * Two id fields:
     * `<entity_type>_eid` = an extracted ID corresponding to the website
     * `<entity_type>_uid`  = a UUID for DB loading 
   * unique information regarding that entity

Level | Full Name | Abbreviation
----- | --------- | ------------
1.0 | Performance | `pfrm`
2.0 | Individual | `indv`
3.0 | Section | `sctn` 
4.0 | Meet | `meet` 
5.0 | Venue | `venu` 
6.0 | Afilliation |  `affn`  


## Relationships

* labeled as:  `<entity_type>_<entity_type>` (lower level)=>(upper level) 
* Each relationship item has:
  * Four id fields ( one for each entity in relationship )
  * information regarding that unique relationship

# Items
## 1.0 - ent_pfrm_Item
### Entity to model an official performance result 

```json
{
    "_id" : ObjectId("58cef6fdd2b38b293a13eda5"),
    "unit" : "s",
    "pfrm_uid" : LUUID("bb40a701-74e7-4ec0-af72-8fad3bba0dc7"),
    "datetime" : "2016-03-10T00:00:00",
    "pfrm_eid" : "pfrm_R66566271",
    "mark" : "11.72",
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:13.949Z")
    }
}
```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
1.0.0 |  pfrm_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
1.0.1 |  pfrm_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
1.0.2 |  datetime | ||||
1.0.3 |  mark | "Measured time, height, or distance for a performance"|Performance in section result | dec | "10,2"|yes
1.0.4 |  unit | Unit of measurement for performance | Performance in section result | string | 1 | yes
1.0.5 |  attempts | Data structure for field-event attempts |Performance in section result if available | dict | |no
1.0.6 |  reaction | Reaction time for sprint events | Performance in section result if available | dec | "3,2"|no
1.0.7 |  detail_dq | Reason for dq | Performance in section result if available | ||no
1.0.8 |  detail_thoumark | thousands mark | Performance in section result if available | string | 20 | no
||||||



##&nbsp;&nbsp;&nbsp;1.1 - pfrm_indv_Item
### Relationship tying a performance to an individual ||

```json
{
    "_id" : ObjectId("58cef6fed2b38b293a13eda9"),
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:14.033Z")
    },
    "pfrm_uid" : LUUID("bb40a701-74e7-4ec0-af72-8fad3bba0dc7"),
    "indv_eid" : "indv_A5538145",
    "pfrm_eid" : "pfrm_R66566271",
    "indv_uid" : LUUID("b7c63d26-18df-4874-a8f4-d44f4f578943")
}
```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
1.1.0 |  pfrm_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
1.1.1 |  pfrm_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
1.1.2 |  age | Documented age of athlete at time of performance | Performance in section result if available. |string*||no
1.1.3 |  indv_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
1.1.4 |  indv_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
||||||

##&nbsp;&nbsp;&nbsp;1.2 - pfrm_sctn_Item
### Relationship tying a performance to a section 

```json
{
    "_id" : ObjectId("58cef6fed2b38b293a13edad"),
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:14.111Z")
    },
    "place" : "1",
    "pfrm_uid" : LUUID("bb40a701-74e7-4ec0-af72-8fad3bba0dc7"),
    "pfrm_eid" : "pfrm_R66566271",
    "sctn_eid" : "sctn_meet_270976_1_1_F",
    "sctn_uid" : LUUID("02fa1056-6994-45a5-a189-93e60ffb5ac6")
}
```
Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
1.2.0 |  pfrm_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
1.2.1 |  pfrm_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
1.2.2 |  place | The ranking within a section | Performance in section result | int | |yes
1.2.3 |  qualification | "If the performance qualified for a higher round. (Not-qualified, Automatic, time-based)"|"Performance in section result if available. If athlete competed in next round in same event, and meet"|int | |no
1.2.4 |  sctn_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
1.2.5 |  sctn_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
||||||

##&nbsp;&nbsp;&nbsp;1.3 - pfrm_meet_Item
### Relationship tying a performance to a meet 

```json
{
    "_id" : ObjectId("58cef6fed2b38b293a13edb1"),
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:14.183Z")
    },
    "pfrm_uid" : LUUID("bb40a701-74e7-4ec0-af72-8fad3bba0dc7"),
    "pfrm_eid" : "pfrm_R66566271",
    "meet_eid" : "meet_270976",
    "meet_uid" : LUUID("8cf0289f-6f48-4d4d-a505-64d0d965b781")
}

```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
1.3.0 |  pfrm_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
1.3.1 |  pfrm_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
1.3.2 |  bib_no | Athlete or team bib number for meet |Performance in section result if available.|string | |no
1.3.3 |  meet_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
1.3.4 |  meet_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
||||||

##&nbsp;&nbsp;&nbsp;1.4 - pfrm_venu_Item
### Relationship tying a performance to a venue 

```json
{
    "_id" : ObjectId("58cef6fed2b38b293a13edb5"),
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:14.253Z")
    },
    "pfrm_uid" : LUUID("bb40a701-74e7-4ec0-af72-8fad3bba0dc7"),
    "venu_uid" : LUUID("87843d3f-c204-42a4-9635-30e0366bc897"),
    "venu_eid" : "venu_7c0b88f9-5629-4f5a-a8e2-831f87e86ba2",
    "pfrm_eid" : "pfrm_R66566271"
}

```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
1.4.0 |  pfrm_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
1.4.1 |  pfrm_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
1.4.2 |  venu_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
1.4.3 |  venu_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
||||||

##&nbsp;&nbsp;&nbsp;1.5 - pfrm_affn_Item
### 

```json
{
    "_id" : ObjectId("58cef6fed2b38b293a13edb9"),
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:14.330Z")
    },
    "pfrm_uid" : LUUID("bb40a701-74e7-4ec0-af72-8fad3bba0dc7"),
    "affn_uid" : LUUID("7fa78674-51d3-4ac7-b351-76e3377b4c59"),
    "academic_class" : "12",
    "affn_eid" : "affn_T1582",
    "pfrm_eid" : "pfrm_R66566271"
}
```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
1.5.0 |  pfrm_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
1.5.1 |  pfrm_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
1.5.2 |  academic_class | Grade of athlete that performed |Performance in section result if available.|string | |no
1.5.3 |  affn_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
1.5.4 |  affn_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
||||||

## 2.0 - ent_indv_Item
### Relationship tying a performance to an Affiliation

```json
{
    "_id" : ObjectId("58cef6fdd2b38b293a13eda0"),
    "indv_eid" : "indv_A5538145",
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:13.761Z")
    },
    "gender" : "M",
    "indv_uid" : LUUID("b7c63d26-18df-4874-a8f4-d44f4f578943"),
    "lName" : "Guerrero",
    "fName" : "Niko"
}
``` 

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
2.0.0 |  indv_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
2.0.1 |  indv_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
2.0.2 |  fName | First Name | Performance in section result | string | |yes
2.0.3 |  lName | Last Name |Performance in section result | string | |yes
2.0.4 |  gender | Sex of person | "Section title in meet results.  NLP from articles, profiles"|int | |no
2.0.5 |  DOB | Date object for person birth |" NLP from articles, profiles"|datetime | |no
2.0.6 |  DOD | Date object for person death | " NLP from articles, profiles "|datetime | |no
2.0.7 |  hometown | Array object for hometowns and regions | " NLP from articles, profiles "|array | |no
2.0.8 |  biography | Text object from webscraping | " NLP from articles, profiles "|text | |no
2.0.9 |  accolades | Dict of top performances and awards | " NLP from articles, profiles "|dictionary | |no
2.0.10 |  pfrm_arr | ||||
||||||

##&nbsp;&nbsp;&nbsp;2.1 - indv_sctn_Item
### Relationship tying a Person to a Section prior to the start of section

```json
{
    "_id" : ObjectId("58cef6fed2b38b293a13edbd"),
    "indv_eid" : "indv_A5538145",
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:14.400Z")
    },
    "indv_uid" : LUUID("b7c63d26-18df-4874-a8f4-d44f4f578943"),
    "sctn_eid" : "sctn_meet_270976_1_1_F",
    "sctn_uid" : LUUID("02fa1056-6994-45a5-a189-93e60ffb5ac6")
}
```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
2.1.0 |  indv_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
2.1.1 |  indv_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
2.1.2 |  seed_mark | Mark athlete was entered as |Heat sheets if available | int | |no
2.1.3 |  sctn_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
2.1.4 |  sctn_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
||||||

##&nbsp;&nbsp;&nbsp;2.2 - indv_meet_Item
### Relationship tying an individual athlete  to a meet 

```json
{
    "_id" : ObjectId("58cef6fed2b38b293a13edc1"),
    "indv_eid" : "indv_A5538145",
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:14.486Z")
    },
    "meet_eid" : "meet_270976",
    "meet_uid" : LUUID("8cf0289f-6f48-4d4d-a505-64d0d965b781"),
    "indv_uid" : LUUID("b7c63d26-18df-4874-a8f4-d44f4f578943")
}
```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
2.2.0 |  indv_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
2.2.1 |  indv_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
2.2.2 |  timestamp | Unofficial timestamp at time of import | "python date-util, or other means"|datetime | |yes
2.2.3 |  meet_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
2.2.4 |  meet_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
||||||

##&nbsp;&nbsp;&nbsp;2.3 - indv_affn_Item
### A binding or official relationship between individual and affiliation 

```json
{
    "_id" : ObjectId("58cef6fed2b38b293a13edc5"),
    "affn_uid" : LUUID("7fa78674-51d3-4ac7-b351-76e3377b4c59"),
    "title" : "Athlete",
    "affn_eid" : "affn_T1582",
    "indv_eid" : "indv_A5538145",
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:14.552Z")
    },
    "indv_uid" : LUUID("b7c63d26-18df-4874-a8f4-d44f4f578943")
}
```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
2.3.0 |  indv_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
2.3.1 |  indv_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
2.3.2 |  start_date | "Start date of agreement, or press release"|"Date of first performance. NLP from articles, press releases, profiles"|datetime | |yes
2.3.3 |  end_date | "Start date of agreement, or press release"|"NLP from articles, press releases, profiles"|datetime | |no
2.3.4 |  title | Unique and Official title for role | "NLP from articles, press releases, profiles"|string | |
2.3.5 |  commission_type | Category of role |Categorization after parsing contract title | int | |yes
2.3.6 |  enrollment_type | Categories describing type selection procedures prior to engagement  |"Public records, NLP from articles, press releases, profiles"|int | |no
2.3.7 |  compensation | Dictionary object of cash flows at a given time |"Public records, NLP from articles, press releases, profiles"|int | |
2.3.8 |  source_entity_id | ID of information source for a given property in the contract |"Affiliation ID if available, Autoincrement if not"|string | |
2.3.9 |  source_id | URL of source in DB |URL from most recent http request success |string | |
2.3.10 |  affn_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
2.3.11 |  affn_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
||||||

## 3.0 - ent_sctn_Item
### "Entity to model a race, round, or heat in meet

```json
{
    "_id" : ObjectId("58cef6fdd2b38b293a13ed92"),
    "event" : "1",
    "is_Relay" : false,
    "order_in_round" : "1",
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:13.544Z")
    },
    "sctn_eid" : "sctn_meet_270976_1_1_F",
    "qual_round" : "F",
    "gender" : "M",
    "sctn_uid" : LUUID("02fa1056-6994-45a5-a189-93e60ffb5ac6"),
    "title" : "100 Meters"
}
```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
3.0.1 |  sctn_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
3.0.2 |  sctn_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
3.0.3 |  event | Event Type |Section title of in meet result | int | |yes
3.0.4 |  qual_round | "Round of Competition, Prelims, Semis, Final  "|Section details in meet result if available | int | |yes
3.0.5 |  order_in_round | Heat/ Flight number in a round | Section details in meet result if available | int | |yes
3.0.6 |  advancement | "dict object specifying auto qualifiers, time qualifiers and size of next round"|Section details in meet result if available | dict | |no
3.0.7 |  wind | Wind reading for sprint and horizontal jumps |Performance in section result or  section details in meet results if available |int | |no
3.0.8 |  conditions | dict object with basic weather at date and time of section or meet |Given the datetime of meet and the geolocation |int | |yes
3.0.9 |  timestamp | Time of section completion |Section timestamp of meet result if available if live results. Meet timestamp if available | datetime | |yes
3.0.10 |  title | Full text of the Section | Performance in section result if available | string | |yes
3.0.11 |  gender | Gender of event | Performance in section result if available | int | |no
3.0.12 |  is_Relay | If the event is a relay | Performance |int | |yes
3.0.13 |  pfrm_arr | ||||
||||||

##&nbsp;&nbsp;&nbsp;3.1 - sctn_meet_Item
### Relationship tying a section to a meet

```json
{
    "_id" : ObjectId("58cef6fdd2b38b293a13ed96"),
    "param_implement" : null,
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:13.613Z")
    },
    "meet_eid" : "meet_270976",
    "meet_uid" : LUUID("8cf0289f-6f48-4d4d-a505-64d0d965b781"),
    "sctn_eid" : "sctn_meet_270976_1_1_F",
    "sctn_uid" : LUUID("02fa1056-6994-45a5-a189-93e60ffb5ac6"),
    "param_meet" : [ 
        "Varsity"
    ]
}
```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
3.1.0 |  sctn_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
3.1.1 |  sctn_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
3.1.2 |  start_datetime | Scheduled time for event |Meet schedule if available | datetime | |no
3.1.3 |  event_num | Event ID for an event assigned by the meet |Section in meet results or meet schedule if available | int | |no
3.1.4 |  param_age | Grouping of competitors based on age | Section in meet results or meet schedule if available | string | |no
3.1.5 |  param_level | Grouping of competitors based mark or skill | Section in meet results or meet schedule if available | string | |no
3.1.6 |  param_implement | Specified parameters on implements height or weight |Section in meet results or meet schedule if available | dict | |no
3.1.7 |  param_affiliation | "Parameters for participants unique to affiliation size, or governing body "|Section in meet results or meet schedule if available | dict | |no
3.1.8 |  param_meet | Grouping of competitors unique to and specified by meet | Section in meet results or meet schedule if available | string | |no
3.1.9 |  meet_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
3.1.10 |  meet_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
||||||

##&nbsp;&nbsp;&nbsp;3.2 - sctn_affn_Item
### Relationship tying a section result to a publishing affiliation

```json
{
    "_id" : ObjectId("58cef6fdd2b38b293a13ed9b"),
    "affn_uid" : LUUID("bdc2bc27-7a4f-4cab-b72e-e012abab205a"),
    "sctn_uid" : LUUID("02fa1056-6994-45a5-a189-93e60ffb5ac6"),
    "affn_eid" : "Affn_Anet",
    "file_url" : "aNet-Meet_meet_270976.html",
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:13.686Z")
    },
    "sctn_eid" : "sctn_meet_270976_1_1_F",
    "web_url" : "<200 http://www.athletic.net/TrackAndField/MeetResults.aspx?Meet=270976>"
}

``` 

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
3.2.0 |  sctn_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
3.2.1 |  sctn_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
3.2.2 |  timestamp | Timestamp at time of publish | python-dateutil or HTTP header Last-Modified | datetime | |yes
3.2.3 |  file_url | Location in document database |On file save | string | |yes
3.2.4 |  web_url | Location initially scraped from |URL from most recent http request success |string | |yes
3.2.5 |  affn_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
3.2.6 |  affn_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
||||||

## 4.0 - ent_meet_Item
### Enitity to model track meet 

```json
{
    "_id" : ObjectId("58cef6fcd2b38b293a13ed7c"),
    "start_date" : "2016-04-02T00:00:00",
    "end_date" : "2016-04-02T00:00:00",
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:12.609Z")
    },
    "meet_eid" : "meet_254369",
    "name" : "AKFCA Big C Relays ",
    "meet_uid" : LUUID("38c98898-551f-44e0-bc55-ff3bf1285c20")
}
```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
4.0.0 |  meet_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
4.0.1 |  meet_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
4.0.2 |  name | Name/ title of meet | Meet result title | string | |yes
4.0.3 |  start_date | Start date of meet | Meet result details | datetime | |yes
4.0.4 |  end_date | End date of meet | Meet result details | datetime | |yes
4.0.5 |  pfrm_arr | ||||
||||||

##&nbsp;&nbsp;&nbsp;4.1 - meet_venu_Item
### Relationship tying a meet to a venue 

```json
{
    "_id" : ObjectId("58cef6fcd2b38b293a13ed7e"),
    "venu_uid" : LUUID("2114ccc5-a345-4435-afbf-4ceb5256cd28"),
    "start_date" : "2016-04-02T00:00:00",
    "venu_eid" : "venu_abf68984-19ee-445c-bb79-604944cf1069",
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:12.671Z")
    },
    "meet_eid" : "meet_254369",
    "meet_uid" : LUUID("38c98898-551f-44e0-bc55-ff3bf1285c20"),
    "end_date" : "2016-04-02T00:00:00"
}
```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
4.1.0 |  meet_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
4.1.1 |  meet_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
4.1.2 |  start_date | Start date of meet holding venue | Meet result details | datetime | |yes
4.1.3 |  end_date | End date of meet holding venue | Meet result details | datetime | |yes
4.1.4 |  venu_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
4.1.5 |  venu_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
||||||

##&nbsp;&nbsp;&nbsp;4.2 - meet_affn_Item
### Relationship tying an competing affiliation to a meet at earliest date 

```json
{
    "_id" : ObjectId("58cef6fdd2b38b293a13ed89"),
    "affn_uid" : LUUID("0d3ab659-4343-4d20-b1da-15721c3f312d"),
    "affn_eid" : "affn_T1544",
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:13.448Z")
    },
    "meet_eid" : "meet_270976",
    "meet_uid" : LUUID("8cf0289f-6f48-4d4d-a505-64d0d965b781")
}

```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
4.2.0 |  meet_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
4.2.1 |  meet_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
4.2.2 |  timestamp | Time of published entry prior to section start |"Heat sheets, accepted entries list, meet results"|datetime | |yes
4.2.3 |  affn_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
4.2.4 |  affn_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
||||||

## 5.0 - ent_venu_Item
### Entity to model stadium and location of competiton 

```json
{
    "_id" : ObjectId("58cef6fdd2b38b293a13ed81"),
    "venu_uid" : LUUID("2114ccc5-a345-4435-afbf-4ceb5256cd28"),
    "formatted_address" : "6501 Changepoint Dr, Anchorage, AK 99518, USA",
    "elevation" : 32.7129096984863,
    "gapi_place_id" : "ChIJT7Mgi4mXyFYReTcd_tTTlt4",
    "address" : ", Anchorage, AK ",
    "venu_eid" : "venu_abf68984-19ee-445c-bb79-604944cf1069",
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:13.114Z")
    },
    "name" : "The Dome",
    "geolocation" : [ 
        61.1619297, 
        -149.9022397
    ],
    "gapi_types" : [ 
        "establishment", 
        "point_of_interest", 
        "stadium"
    ]
}
```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
5.0.0 |  venu_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
5.0.1 |  venu_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
5.0.2 |  name | "Name of Stadium, or affiliation "|"Schema.org, Venue details in meet results"|string | |yes
5.0.3 |  address | Address of venue | "Schema.org, Venue details in meet results"|string | |no
5.0.4 |  formatted_address | Full address of venue | google maps api address | string | |no
5.0.5 |  geolocation | Latitude and longitude of venue |google maps api address | list | |no
5.0.6 |  elevation | Altitude of venue location | google maps api and geolocation |int | |no
5.0.7 |  gapi_types | Google API | google maps api | list | |no
5.0.8 |  gapi_place_id | UID for Google API lookup | google maps api | string | |no
5.0.9 |  is_banked | Whether track is banked | Performance details in ranking list if matched | int | |no
5.0.10 |  is_oversize | Whether track is oversized | Performance details in ranking list if matched | int | |no
5.0.11 |  pfrm_arr | ||||
||||||

##&nbsp;&nbsp;&nbsp;5.1 - venu_affn_Item
### Relationship tying venue to affiliation

```json
```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
5.1.0 |  venu_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
5.1.1 |  venu_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
5.1.2 |  start_date | "Date of purchase, or establishment "|"Public Records, profiles"|datetime | |no
5.1.3 |  end_date | "Date of transfer, or closing "|"Public Records, profiles"|datetime | |no
5.1.4 |  affn_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
5.1.5 |  affn_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
||||||

## 6.0 - ent_affn_Item
### Entity to model any organization with ties to another entity in regards to track and field

```json
{
    "_id" : ObjectId("58cef6fed2b38b293a13edb1"),
    "scrapy-mongodb" : {
        "ts" : ISODate("2017-03-19T21:24:14.183Z")
    },
    "pfrm_uid" : LUUID("bb40a701-74e7-4ec0-af72-8fad3bba0dc7"),
    "pfrm_eid" : "pfrm_R66566271",
    "meet_eid" : "meet_270976",
    "meet_uid" : LUUID("8cf0289f-6f48-4d4d-a505-64d0d965b781")
}
```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
6.0.0 |  affn_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
6.0.1 |  affn_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
6.0.2 |  name | "Name of Stadium, or affiliation "|"Schema.org, Venue details in meet results"|string | |yes
6.0.3 |  address | Address of venue | "Schema.org, Venue details in meet results"|string | |no
6.0.4 |  formatted_address | Full address of venue | Google Api | string | |no
6.0.5 |  geolocation | Full address of venue | Google Api | ||
6.0.6 |  elevation | Latitude and longitude of venue |From address | list | |no
6.0.7 |  gapi_types | Altitude of venue location | from geolocation |int | |no
6.0.8 |  gapi_place_id | ||||
6.0.9 |  establishment_date | ||||
6.0.10 |  pfrm_arr | ||||
||||||

##*&nbsp;&nbsp;&nbsp;6.1 - affn_affn_Item
### Relationship modeling heirarchies of authority within 

```json
```

Level |  Properties |   Description |  Taken From |  data type |  size |  required?
----- |  ---------- |   ----------- |  ---------- |  --------- |  ---- |  ---------
6.1.0 |  affn_uid | PK for indexing | given a uuid during transform UUID.uuid4()|string | |yes
6.1.1 |  affn_eid | PK for indexing | ID in URL or HTML if available.  Autoincremented if not available.|string | 8 | yes
6.1.2 |  start_date | Earliest record of the governing relationship | "Public records, participation in  conference meets"|datetime | |yes
6.1.3 |  end_date | Date of relationship termination | "Public records, participation in  different conference/ league  meets"|datetime | |yes
6.1.4 |  violations | "Diction object with Violation id, violation date, violation clause, and sentence"|"Public records, articles, website, profiles"|dict | |no
6.1.5 |  exec_id | List of IDs defining chain of higher order governing bodies |"Public records, website, profiles"|string | |yes
6.1.6 |  gov_affn_eid | ||||

# Todos
* Assign UUID as `ent_uid` to each `ent_eid` field for every item
* Fix MongoDBPipeline to post entities
* Separate ISO Date fields into an array (minute, hour, day, month, year )
* Append dates to 
* ( For MongoDBPipeline ) For every relationship, append a list of lower level uids in array
* Create a separate file for xpath items (nessesary ??)


## Get All Kittens

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
include_cats | false | If set to true, the result will also include cats.
available | true | If set to false, the result will include kittens that have already been adopted.

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
api.kittens.get(2)
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the kitten to retrieve

