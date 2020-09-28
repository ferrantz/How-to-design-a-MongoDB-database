# How-to-design-a-MongoDB-database
This is a way to design a mongodb database to work with tweets (in my code, 05/01 Covid-19 Tweets).
MongoDB is a document-oriented database that, like the majority of nosql
databases is schema-less, which means that documents structure isn’t supposed to be indispensably the same within the same group of data. MongoDB associates to every loaded document an identifier of the document that
is used to construct an index useful to access to data. Users can, however,
construct indexes that they can optimize particular queries (it can be determinant with very large datasets). MongoDB manages redundancy of data with a master-slave architecture in which slaves are called replica sets. In
particular, failure management is transparent to client, id est client doesn’t
see that a node is failed, and in case of failure of the primary node, its replicas
will elect automatically a new primary in a totally transparent way to client.
Considering CAP theorem, MongoDB ensures (Eventual) Consistency and
Partition Tolerance. Eventual Consistency means that after a period nodes,
master and slaves, will store the same information, while Partition Tolerance
means that database is resilient to nodes failure. Last, MongoDB makes extensive use of json representations both for APIs and their query languages,
and as regards the information handling part. The fact of having json allows
us to overcome ORM, i.e. Object Relational Mapping, that is the semantic
difference that exists in sql between a table and an object (for example Java
or Python). In MongoDB the concept of table existing in sql take the place
of concept of collection and change also the concept of row replaced with
concept of document. In particular, a collection contains a set of documents;
in contrast to the row of the table, documents can have missing or different
fields, precisely because the database is schema-less and storage is formatted
BSON. Another difference between sql and nosql databases is the possibility
to do joins. In relational databases the concept of join lies in having relations
among tables, in MongoDB these kind of relations are not modelled with a
link between tables, but with an Embedded Document, that means essentially that if there is a relation between two documents, one of these will
be included in the other. This phenomenon generates redundancy because
the same data is stored more time, and avoids document to do an integrity
control on data. In the case of the dataset that I will use, downstream
of hydration, the documental representation of tweets (that doesn’t require
any phase of modelling) provides: an ID that identifies the tweet, the time-stamp in which was been created, the full text that is the text of the tweet, a series of information
related to geolocation of tweet, the language, the number of tweet and the
user. There are three Embedded Documents: entities, retweet status and
user. Entities is a document that contains structured information related
to tweet (like hashtag and media), retweet status document contains information about original tweet if the native tweet is a retweet, and finally
document user contains information related to the user.
To do tweet processing, first thing we had to do was create a Twitter Developer profile (developer.twitter.com/en), fundamental operation to receive
APIs (through keys and tokens) and to get data. In particular, they gave
us tweet ids (and not original tweet for privacy reasons), for this reason
to get the data it was necessary proceed with the “Hydration”. Obtained
“hydrated” data I was finally been able to design our database MongoDB.
To work I used Jupyter Notebook and I worked first on data of 02/24 and
then on 05/01 ones. In this site I found datasets of hundreds of millions of
multilingual COVID-19 tweets with location information: https://crisisnlp.qcri.org/covid19.
From notebook, I preliminarily imported .tsv file of tweets id of 02/24 and
then created a .txt file containing only the column of tweet ids and not the
column with the users ids.
There are two ways to do the hydration: or using the open source soft-ware Hydrator or working from the notebook loading the library twarc, that
substantially is a Python command line tool. I used twarc because, even
in its greatest complexity, offers as much flexibility. At this stage, I hydrated the first 20000 tweets and appended them to an empty list of tweets.
I selected only the first 20000 tweets essentially for computational reasons:
in the first test of this work I tried to process on the totality of tweets of
the two days, struggling our machines to an excessive computational effort
and shrinking storage. After several attempts, I chose that a sample of
20000 tweets was representative of trend of the whole population and didn’t
expose our machines to an excessively high effort. To generate a file to be fed to our nosql database, I generated a json
file in which every row corresponds to a tweet represented in json (format
particularly adapted for document-based storage). json files can be used in
nosql databases. To use MongoDB, must be installed the db in local and can be accessed in two ways: or with the Python driver or with Compass, a graphic interface. Using Python driver (pymongo), I created a connection specifying
the mongodbclient with the URL in which database is located (in this case
in local) and port (mongodb standard port is 27017). After that, database
and collection can be accessed with their names and in addition data entry
can be done (in our case our json list) with a simple insert many. MongoDB implements a kind of lazy evaluation, i.e. database and its collections are created only when files are been loaded, thus we can see database
only after the insert many. Now I made some queries with driver
Python. We can have a confirm that there are 20000 tweets asking how
many tweet are there, and how many are italian. To be able to do natural language processing, we needed to create a .txt
file with all Italian tweets.

For any questions, feel free to contact me.
