# Challenge Queries Solutions

# Query : List of all books ("Book" type);

```
db.publis.find({type: "Book"})
```

# Query : List of publications since 2011 ;

db.publis.find({year: {$gte: 2011}})

# Query : List of books since 2014 ;

db.publis.find({type: "Book", year: {$gte: 2014}})

# Query : List of publications by author "Toru Ishida;

db.publis.find({authors: "Toru Ishida"})
db.publis.find({authors: {$all: ["Toru Ishida"]}})


# Query : List of all publishers ("publisher" type), distinct ;

db.publis.distinct("publisher")

# Query : List of all distinct authors ;

db.publis.distinct("authors")

# Query : Sort publications of "Toru Ishida" by book title and by start page, and Project the result on the title of the publication, and the pages;

db.publis.find({authors: "Toru Ishida"}, {title: 1, pages: 1}).sort({title: 1, "pages.start": 1})

# Query : Count the number of his publications;

db.publis.find({authors: "Toru Ishida"}).count()

# Query : Count the number of publications since 2011 and by type;

db.publis.distinct( "type" ).forEach((doc) => {var count = db.publis.find({type:doc,year :{$gte: 2011}}).count() ; print(doc + ":" + count)})
db.publis.aggregate([{$match: {year: {$gte: 2011}}},{$group: {_id: "$type", count: { $count : {}}}}])

# Query : Count the number of publications by author and sort the result in ascending order;

db.publis.distinct( "authors" ).reverse().forEach((doc) => {var count = db.publis.find({authors:doc}).count(); print(doc + ":" + count)})
