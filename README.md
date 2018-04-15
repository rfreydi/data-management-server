# data-management-server
The aim of this repo is to implement a full server-side environment that manage entities with a potentially vast number 
of properties available for each. To get a good subject I'll try to create an application that manage lifecycle in 
production factory, we will need to manage history, export, flexible entities and also real-time system, administration 
of the entities through an interface and more... The front part will be split into another repository, so we will
implement a REST API to be able to switch from multiple front-end (Angular, React, VueJS).

## Code Style
The code style of this project is based on the StandardJS code, but with some changes :

- Semi-columns are mandatory
- No space before functions declaration parentheses
- No trailing comma

## Technology used

Here is the current choice for the technology used. If you want to know why these choice as been made, look at the R&D 
history.

- NodeJS
- MySQL

*NB: For NodeJS, it's the default choice since I love JavaScript and don't really like PHP syntax. The second reason is 
it need less skill to build backend and frontend if they're based on the same language. So even if I'm more comfortable 
with PHP 7 because I never used NodeJS before, I'll stick on JavaScript and acquire skill in NodeJS.*

## R&D

Here will be listed all sources that as been seem/read to make technical decision and what as been chosen/extracted from
sources.

### Subject to define later : Data Access Layer choice

- http://jayurbain.com/msoe/se380/slides/swarch-dao-patterns.pdf
    - [details on these patterns](https://softwareengineering.stackexchange.com/questions/205462/fowlers-data-access-layer-patterns)

### Next subject: Interface Layer choice

### Next subject: Message-based Interface Layer choice (RESTful API Framework)

### Next subject: Business Layer choice

### 2018-04-15: Layers Architecture

My current knowledge on database access is through an Access Layer like DAO which I already used in a project and ORM
which I didn't use it yet. I searched for the best way to access the database, globally and for the JavaScript/NodeJS. I
found that it's better to build the Business Layer first and then exploit it to see if it's reliable before starting to
create the database itself. Beside the choice of the Data Access Layer, it showed that we need to create an abstraction
level between Business Layer and Data Access Layer to reduce dependencies. We will implement the architecture as follow
using a top-down interaction approach :

- Message-based Interface Layer (RESTful API)
- Business Layer
- Interface Layer (Abstract interface, Common design type, Dependency inversion)
- Data Access Layer (ORM, DAO, ...)
- Database (MySQL)

Sources:

- http://blog.gauffin.org/2013/01/data-layer-the-right-way/
- https://msdn.microsoft.com/en-us/library/ee658116.aspx

### 2018-04-01: Data Model choice

I searched for an easy way to manage flexible properties and come across the Entity-Attribute-Value model (also named as
vertical database model or open schema). After reading some article, I found one who spoke about the implementation of 
JSON Data Type to PostgreSQL and that it's perform better than EAV model. Perfect, it confirm my intention to use JSON
Data Type for flexible properties. Beside this article, I also read one who showed the downside of using an EAV model

Sources:

- https://en.wikipedia.org/wiki/Entity%E2%80%93attribute%E2%80%93value_model
- https://coussej.github.io/2016/01/14/Replacing-EAV-with-JSONB-in-PostgreSQL/
- https://www.ncbi.nlm.nih.gov/pmc/articles/PMC2215074/

### 2018-04-01: Database choice

My first though was to go for a NoSQL database, since we need a flexible way to add properties (so our code is generic 
as much as possible). But since we need to follow th ACID principles, NoSQL database seem to not be a good choice.
Beside this specification MySQL released JSON Data Type in the v5.7.8, and will implement roles management in the v8, which 
seem to fit what I want to achieve: managing entities with flexible properties (and fixed properties) with access 
restriction on them. For the choice between MySQL or other RDBMS like PostgreSQL, it just a preference since I have a
lot of knowledge on MySQL, I'll stick on it.

Sources:

- https://youtu.be/Jt_w2swkXAk
- https://youtu.be/cW6MRLCS1jI
- https://dzone.com/storage/assets/7307583-dzone-2017guidetodatabases.pdf
- https://dev.mysql.com/doc/refman/5.7/en/json.html