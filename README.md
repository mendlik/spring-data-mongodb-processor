Annotation processor for models annotated with `@Document`
=========================================================

An annotation processor for [spring-data-mongodb](http://www.springsource.org/spring-data/mongodb) document classes. 

What it does?
-------------

- generates meta model for all classes annotated with `@Document`
- enables creation of stringless queries

Usage example
-------------

Using this annotation processor and [spring-data-mongodb](http://www.springsource.org/spring-data/mongodb) you can create stringless queries like:

		// Simple fields:		
		Criteria.where(User_.email).is(email);
		// Instead of: Criteria.where("user.email").is(email);

		// Nested documents:	
		Criteria.where(User_.details.address).is(address);
		// Instead of: Criteria.where("user.details.address").is(address);

		// Arrays:	
		Criteria.where(User_.settings.index(5));
		// Instead of: Criteria.where("user.settings.5");

Maven repository
----------------

In order to use this library add [repository](http://github.com/mendlik/mvn-repo) location in your `pom.xml`:

		<repositories>
		    <repository>
		        <id>mendlik-releases</id>
		        <url>https://github.com/mendlik/mvn-repo/raw/master/releases</url>
		    </repository>
		</repositories>

		<dependencies>
		    <dependency>
		        <groupId>org.springframework.data</groupId>
		        <artifactId>spring-data-mongodb-processor</artifactId>
		        <version>${spring-data-mongodb-processor.version}</version>
		    </dependency>
		</dependencies>

Projects
--------
This project is separated into projects:

		spring-data-mongodb-processor-parent
		   |- spring-data-mongodb-processor-example
		   |- spring-data-mongodb-processor

- `spring-data-mongodb-processor-parent` - Just groups subprojects.
- `spring-data-mongodb-processor-example` - Contains test cases and an usage example of spring-data-mongodb-processor.
- `spring-data-mongodb-processor` - Contains source of annotation processor.

Why QueryDSL for MongoDB is not sufficient? 
-------------------------------------------
- QueryDSL can be used only in queries. What about updating and deleting data?
- QueryDSL comes with a bunch of dependencies. Do we really need them? Our need is simple - just generate meta model in order to replace string queries.
- In application that uses spring-data-mongodb, QueryDSL is a kind of a second (duplicated) way of querying data. I know that QueryDSL predicates are quite handy but I would like all project queries to use the same mechanism. Moreover sytax used in pure spring-data-mongodb is much more similar to MongoDB shell syntax.
- How can I use QueryDSL mechanism with plain MonogoTemplate?
- QueryDSL have some minor problems with querying arrays of nested documents in QueryDSL. For example it's impossible to create such a query: `db.inventory.find( { 'memos.0.by': 'shipping' } )` (see MongoDB doc: [match-a-field-in-the-subdocument-using-the-array-index](http://docs.mongodb.org/manual/tutorial/query-documents/#match-a-field-in-the-subdocument-using-the-array-index))

Maybe some of these arguments come from my lack of knowledge. Maybe I didn't study the documentation as hard as I should. If I am wrong please correct me.

Spring feature
--------------
This annotation processor is proposed as feature on [Springsource Jira](https://jira.springsource.org/browse/DATAMONGO-744).
