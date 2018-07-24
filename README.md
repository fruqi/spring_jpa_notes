# Spring with JPA & Hibernate

## Intro
	> JPA - Java Persistence API
	> JPA started as Hibernate and then extracted a standard interface
	> ORM & POJO based
	> Annotation & XML configuration based
	> JPA is business focus

## Architecture
	> Presentation (Views)
	> Business Logic (Service)
	> Repository (DAO, database access)

	# Controller
		> annotated with @Controller
		> used for routing (our traffic cop)
		> no business logic
		> works with service and repositry (can work directly with repository if there is no business logic required)

	# Service
		> annotated with @Service
		> describe the verb (action) of a system
		> business logic is here
		> ensure business object is in valid state
		> where transaction often begins
		> might have the same method names with repository (but different focus)

	# Repository
		> annotated with @Repository	
		> where we interact with db (through JPA, jdbc, etc.)
		> one-to-one mapping with an Object
		> one-to-one mapping with a db table

## JPA Configuration
	> JPA requires us to use a file named persistence.xml
	> If we are using JPA without Spring, we need to define the settings in `persistence.xml`
		> such as: datasource, caching, allowed operation, etc.
	> When we use Spring, we still NEED persistence.xml but an empty one
		> it is located at 'src/main/resources/META-INF/persistence.xml'
	> Anything under src/main/resources will copied automatically to war file by Maven
	> JPA has nothing to do with DispatcherServlet, hence in the web.xml:
		1. Specify context-param attribute with the following value
			<context-param>
				<param-name>contextConfigLocation</param-name>
				<param-value>classpath*:/jpaContext.xml</param-value>
			</context-param>

		2. Add listener tag; Spring will pickup all the <listener> tags in the web.xml and perform necessary stuffs.
		   In this case we need to add the following:
		   	<listener>
		   		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
		   	</listener>

		   What this listener does is to tell Spring to look for contextConfigLocation where the jpaContext (in our case) lives

	> Entity Manager Factory is used to start up the JPA and Hibernate inside of our application
		> By using the class named LocalContainerEntityManagerFactoryBean (located in spring-orm.jar)
		> LocalContainerEntityManagerFactoryBean 
			> references our persistence unit
			> injects Datasource if one isn't defined in the persistence unit (peristence.xml)
			> defines what Vendor (provider) we are using (such as: Oracle, MySQL, Postgresql, etc.)
			> jpaPropertyMap
				> hibernate.dialect is vendor that we are going to use
				> hibernate.format_sql - it will format the sql when dumped to log
				> hibernate.hbm2ddl.auto possible values:
					> create - create the tables if we haven't set
					> create-drop - create the db when application start and drop when the application stop
					> update - update any fields if something is already change
					> validate - compare and verify the entities (objects) and db structure 
					> none - nothing is going to be done


	> JPA & Hibernate has no notion of transaction (no create, start, or rollback transaction)
		> JpaTransactionManager comes to the rescue
			> Takes entityManagerFactory as reference
			> Annotation driven
			> coming from spring-tx.jar

## JPA Intro
	> JPA is an interface to an ORM
	> ORM is an implementation of JPA
		> ORM (JPA provider/vendor) examples: Hibernate, DataNucleus, eclipseLink, OpenJPA, ObjectDB, Versant
	> JPA is just a specification
	> JPA is NOT SQL
		> use JPQL instead
	> Heavy use of POJO

