# Hibernate cache interview questions and answers

## Table of Contents

Q. What is a first-level cache in Hibernate?
A. First-level cache always is associated with a “Session” object. Hibernate uses this cache by default. Y ou can’ t turn it of f. Hibernate caches the SQL
statements like insert, update, delete, etc in the first-level cache. By default, Hibernate will flush changes automatically for you
1) before some query executions.
2) when a transaction is committed.
When you explicitly call flush() you force hibernate to execute the SQL commands on Database. But do understand that changes are not “committed” yet. So,
after doing flush and before doing commit, if you access DB directly , you will not see the changes.
Q. When do you need to explicitly call flush?
A. One common case for explicitly flushing is when you create a new persistent entity and you want it to have an artificial primary key generated and assigned
to it, so that you can use it later on in the same transaction. In that case calling flush would result in your entity being given an id.
Another reason is when you do bulk batch inserts to conserve memory by regularly flushing the cache.

First & Second level cache in Hibernate
Q. What is a second-level cache in Hibernate?
A. Hibernate uses two dif ferent caches for objects: first-level cache and second-level cache. First-level cache is associated with the Session object, while
second-level cache is associated with the SessionFactory object. By default, Hibernate uses first-level cache on a per -transaction basis. Hibernate uses this cache
mainly to reduce the number of SQL queries it needs to generate within a given transaction. For example, if an object is modified several times within the same
transaction, Hibernate will generate only one SQL UPDA TE statement at the end of the transaction, containing all the modifications. The second-level cache
needs to be explicitly configured. Hibernate provides a flexible concept to exchange cache providers for the second-level cache. By default Ehcache is used as
caching provider . However more sophisticated caching implementation can be used like the distributed JBoss Cache or Oracle Coherence.
The Hibernate configuration looks like:
The ehcache.xml can be configured to cache objects of type com.myapp.Order as shown below<property name ="hibernate.cache.use_second_level_cache" >true</property >
<property name ="hibernate.cache.provider_class" >org.hibernate .cache .EhCacheProvider </property >
<cache name ="com.myapp.Order"
 maxElementsInMemory ="300"
 eternal ="true"
 overflowT oDisk ="false"
 timeT oIdleSeconds ="300"
 timeT oLiveSeconds ="300"
 diskPersistent ="false"
 diskExpiryThreadIntervalSeconds ="120"
 memoryStoreEvictionPolicy ="LRU" 
>

second-level cache reduces the database traf fic by caching loaded objects at the SessionFactory level between transactions. These objects are available to the
whole application, not just to the user running the query . The ‘second-level’ cache exists as long as the session factory is alive. The second-level cache holds on
to the ‘data’ for all properties and associations (and collections if requested) for individual entities that are marked to be cached. It is imperative to implement
proper cache expiring strategies as caches are never aware of changes made to the persistent store by another application. he following are the list of possible
cache strategies.
Read-only : This is useful for data that is read frequently , but never updated. This is the most simplest and best-performing cache strategy .
Read/write : Read/write caches may be appropriate if your data needs to be updated. This carry more overhead than read-only caches. In non-JT A
environments, each transaction should be completed when session.close() or session.disconnect() is called.
 
Nonstrict r ead/write : This is most appropriate for data that is read often but only occasionally modified.This strategy does not guarantee that two
transactions won’ t simultaneously modify the same data. 
 
Transactional : This is a fully transactional cache that may be used only in a JT A environment.
It can be enabled via the Hibernate mapping files as shown below:
Note: The usage options are: transactional|read-write | nonstrict-read-write|read-only . The cache can also be enabled at dif ferent granular level (e.g. parent,
children, etc). The active orders will be cached for 300 seconds.
Q. How does the hibernate second-level cache work?
A. Hibernate always tries to first retrieve objects from the session and if this fails it tries to retrieve them from the second-level cache. If this fails again, the
objects are directly loaded from the database. Hibernate’ s static initialize() method, which populates a proxy object, will attempt to hit the second-level cache
before going to the database. The Hibernate class provides static methods for manipulation of proxies.<class name ="com.myapp.Order" >
 <cache usage ="read-write" />
 ....
</class >

As a consequence of using the Hibernate second-level cache, you have to be aware of the fact that each call of a data access method can either result in a cache
hit or miss. So, configure your log4j.xml to log your hits and misses.
Alternatively , you can use Spring AOP to log the cache access on your DAO methods.
The second level cache is a powerful mechanism for improving performance and scalability of your database driven application. Read-only caches are easy to
handle, while read-write caches are more subtle in their behavior . Especially , the interaction with the Hibernate session can lead to unwanted behavior .
Q. What is a query cache in Hibernate?
A. The query cache is responsible for caching the results and to be more precise the keys of the objects returned by queries. Let us have a look how Hibernate
uses the query cache to retrieve objects. In order to make use of the query cache we have to modify the person loading example as follows.public final class Hibernate extends Object {
 ....
public static void initialize (Object proxy ) throws HibernateException
 ....
}
<logger name ="org.hibernate.cache" >
 <level value ="DEBUG" />
</logger >
Query query = session .createQuery ("from Order as o where o.status=?" );
query .setInt (0, "Active" );

You also have to change the hibernate configuration to enable the query cache. This is done by adding the following line to the Hibernate configuration.
Q. Where can you apply a 2nd level cache in Hibernate?
A. Use it when you read certain objects very often.
1) The 2nd level cache is key-value pair based, and works only if you get your entities by id . The 2nd level cache works on children objects only if
fetch=”select” is used. When we say fetch=”select”, then it will always fire separate queries to retrieve the association objects even if it is lazy =”false”. So, if
you do “from Employee where name = :name” then the 2nd level cache will not be hit.
2) The 2nd level cache is invalidated/updated on a per entity basis when an entity is updated/deleted via hibernate . If the entities are updated outside hibernate,
then they are not invalidated and you run the risk of working with stale entities. In this scenario, you need to perform an impact analysis of objects being stale,
and reviw your cache expiration strategy .
3) When using queries with join statements, then use the “ query cache “.
Q. What are the pitfalls of second level and query caches?
A. Memory is a finite resource, and over use or incorrect usage like caching the Order object and all its referenced objects can cause OutOfMemoryError . Here
are some tips to overcome the pitfalls relating to caching.
1. Set entity’ s keys as query parameters, rather than setting the entire entity object. Critreria representations should also use identifiers as parameters. W rite HQL
queries to use identifiers in any substitutable parameters such as WHERE clause, IN clause etc.
In the example below , the entire customer and everything he/she references would be held in cache until either the query cache exceeds its configured limits and
it is evicted, or the table is modified and the results become dirty .query .setCacheable (true); // the query is cacheable
List l = query .list();
<property name ="hibernate.cache.use_query_cache" >true</property >

Instead of setting the whole customer object as shown above, just set the id.
2. Hibernate’ s query cache implementation is pluggable by decorating Hibernate’ s query cache implementation. This involves overriding the put( ) method to
check if a canonical equivalent of a query results object already exist in the Object[][], and assign the same QueryKey if it exists.
3. If you are in a single JVM using in memory cache only , use hibernate.cache.use_structured_entries=false in your hibernate configuration.
Here are some general performance tips:
1. Session. load will always try to use the cache. Session. find does not use the cache for the primary object, but cause the cache to be populated. Session. iterate
always uses the cache for the primary object and any associated objects.
2. While developing, enable the show SQL and monitor the generated SQL.final Customer customer = ... ;
final String hql = "FROM Order as order WHERE order .custOrder = ?"
final Query q = session .createQuery (hql);
q.setParameter (0, customer );
q.setCacheable (true);
final Order customer = ... ;
final String hql = "from Order as order where order .cusomer .id = ?"
final Query q = session .createQuery (hql);
q.setParameter (0, customer .getId ());
q.setCacheable (true);

Also enable the “ org.hibernate.cache ” logger in your log4j.xml to monitor cache hits and misses.<property name ="show_sql" >true</property >



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
