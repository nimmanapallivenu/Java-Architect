# SDLC for Java Developers

> **Module**: SDLC  
> **Topic**: SDLC for Java Developers

---

## 📋 Table of Contents



- [Q1: What are the SDLC activities you perform as a Java developer?](#q1)
- [Q2: What tools have you used to assist with the SDLC?](#q2)
- [Q3: What are the dif ferent types of group sharing you have been in a software devel](#q3)
- [Q4: What are the dif ferent types of testing performed in a software development pro](#q4)
- [Q5: What are functional requirements? What are Non-funcrional requirements?](#q5)

---

## Q1: What are the SDLC activities you perform as a Java developer?

**Answer:**

Don’ t get overwhelmed by this activities list. This proves why employers favor experience to just academic qualifications alone. It also emphasizes the fact
why good technical skills must be complimented with good soft skills and right attitudes get things done as a software developer .
Pre-development activities
Review business requirements document (aka BRD) along with other functional and non functional specification documents prepared by the business
analysts.
Review and understand the baseline architecture created by the architect(s).
Create technical specification documents and get them reviewed and signed of f by the relevant stake holders like architect(s), team leads, testers, business
analysts, etc. Use tools like Confluence or W iki for the documentation. Use appropriate diagrams in your documentation like
Physical and logical conceptual diagrams to give a big picture.
Entity Relationship Diagrams (ERD) if you have new database tables.
Relevant class, sequence, state, and component diagrams.
Choose the 
programming languages to develop in (e.g. Java, JavaScript, CSS, HTML, Groovy, etc)
frameworks (e.g. Spring, Hibernate, angularjs as MVC framework, JUnit for unit testing, Drools as business rules engine, etc)
tools (Eclipse IDE, Maven for dependency management, build and deploy, Subversion for code repository, Jenkins for continuos integration,
SonarQube for code quality, DBArtisan/SQLDeveloper/DBV isualizer for database management, BeyondCompare to compare files,
Putty/Cygwin/MSYS/MobaXterm/W inSCP as Unix ssh emulators, etc )
integration servers (web Methods, Oracle Service Bus, W ebsphere MQ, Apache Camel, Mule, SAP, Pega BPM, etc) & database servers (e.g.
Oracle, Sybase, etc) to be accessed.
Perform impact analysis from the requirements where required.
Perform capacity planning from the non functional requirements to see if the new applications could be deployed to existing infrastructure or new
infrastructure needs to be commissioned.
Listing project risks, and probability of happening as less likely, likely, and most likely, and ranking the impact of a risk as low, medium, and high.
Listing design alternatives, pros & cons of each alternative, and describing reasons for selecting a particular approach.
Decide if new applications need to be built or existing applications can be enhanced.
If new application, create a new maven project in the subversion trunk. If enhancing an existing application, create a new branch for this project in the
code repository like subversion or GitHub.
Set up the continuous integration server to get into the rhythm of build, deploy, and test, SonarCube server for code quality, Maven repository and Nexus
for jar dependency, software version control, and release management.
Follow up on infrastructur e requir ements like requiring new hosts, more hard disk space, load balancers, single-sign-on security requirements, domain
names, system monitoring via nagios or tivoli, data archival and retention, auditing via triggers and logging via splunk, etc.
Raise access requests to all relevant environments, servers, and systems.

During development activities
List all components that need to be newly developed or enhanced in the detailed technical specification document. For example, web components, web
services, batch jobs and scripts for ETL (Extract T ransform Load), integration with SOA, BPM, or MOM, etc,
In an agile project, you develop in iterations, and get into the rhythm of build, test (i.e. write unit and functional tests), release, and deploy. 
Provide environment and test support to the testers by fixing any environmental issues and coding bugs. T ools like JIRA, HP Quality Control, can be used
for issue tracking, control, and management.
Regularly rebase your subversion branch with trunk to synchronize with changes from other projects to the shared code base.
Continuously update technical specification document with relevant details as you build and deploy in iterations.
Perform peer code reviews via tools like Crucible.
Continuously refactor and improve code quality as a result of reviewing the Sonar code metrics, peer code reviews via crucible, and self code reviews.
Use code coverage tools to ascertain unit test coverage and improve on the code coverage percentage.
Continuously follow up that infrastructure requests have been met.
Perform system outage testing (e.g. to ensure load balancer is performing as expected, proper timeouts are configured, and service retry mechanisms are
in place), penetration testing to fix security vulnerabilities, performance testing to ensure that SLAs specified in the non functional specs are met, and
cross browser compatibility testing.
Write or enhance scripts to ensure proper data archival, auditing, and monitoring is in place.
Perform Maven release process with proper artifact version numbers as a readiness to be released. 
Promote your applications through dif ferent environments like test, integration test, and stage.
Post development activities
Wrap up the tasks that you started previously during development like documentations, infrastructure requests, non-functional testing like penetration
testing, outage testing, cross browser compatibility, and performance, etc. 
Write support hand over documents and user manuals.
Get final sign-of fs and approvals from the relevant stake holders so that the applications can be promoted to production.
Write implementation plans and other production ready tasks like lining up resources, sending out communications, etc.
Perform dif fs of your production deployment ready artifacts like war/ear files, windows zip files, RPMs (Red hat Package Manager), etc against artifacts
currently in production with tools like beyond compare to ensure that the differences are only changes that you added. This is required for enhancements
of existing artifacts.
Post pr oduction r elease activities
Merge your code changes in the branch back to trunk.
Send communications out to relevant cross functional teams.
Tidy up any other loose ends in terms of documentation, code or environment clean up tasks, etc.
Conduct project r etrospective sessions to see what went well, what did not go well, and how things can be improved next time.
Move on to the next project.

So, you are not only being a developer, but a facilitator and a change agent. It is a team ef fort. There will be cross functional teams with varying technical
strengths. So, you will be performing some tasks yourself, and taking on the role as a facilitator for the other tasks where you need to chase people to get things
done. No wonder why you need to have good non-technical skills to get things done as a Software developer. Now I understand why SDLC interview questions
are very common. Brush up on agile interview questions and answers under “SDLC”. Every or ganization uses some form of agile techniques.

---

## Q2: What tools have you used to assist with the SDLC?

**Answer:**

#1. Confluence or wiki for documentation, JIRA for defect, change, & task management, Maven & ANT for builds, Jenkins or Bamboo for continuous
integration and automated builds, Sonar for code quality and test coverage and Crucible for code reviews.
#2. Eclipse or NetBeans IDE for development. Putty as ssh client, DBArtisan, SQL developer, Squirr el, etc for running SQL queries, Altova XML Spy for
working with XML documents, Subversion as code repository, Cygwin or MobaXterm as your windows ssh client, and W inSCP to transfer files.
#3. Visio and Gliffydraw for conceptual diagrams, UML diagrams, and ER diagrams.
#4. SoapUI is a popular functional testing tool for various protocols such as SOAP, REST, and HTTP. The Firefox “poster” plug-in is handy for posting
messages via Restful services.
#5. Administration and deployment tools in a Message-Oriented Middleware (aka MOM) like HermesJMS for Java to help you interact with messaging
providers like web Methods, making it simpler to publish, edit, browse, search, and delete messages.
#6. Wireshark is handy for monitoring network traf fic. It is quite often handy to snif f what is being sent between a client and a server for debugging purpose.
#7. FileZilla is an ef ficient FTP and SFTP client.
#8. BeyondCompar e makes comparing ASCII and binary files and folders a breeze.
#9. FireFox plug-ins like FireBug, YSlow, LinkChecker, Tamper data, etc to analyze web pages and tamper HTTP data. Fiddler 2 is an HTTP(S) debugging
proxy server to capture and fiddle with the HTTP(S) traf fic as it is being sent. By default, traf fic from Microsoft’ s WinINET HTTP(S) stack is automatically
directed through Fiddler 2 at runtime, but any browser or application can be configured to route traf fic through Fiddler .
#10. Google Skipfish is an application security tool from Google, and it is really easy to run your website through a fairly comprehensive set of security
penetration tests.
#11. RegexBuddy and regexpal.com to test and debug regular expressions.
#12. Automated regression testing tools like BadBoy, Selenium, and OpenST A to record/playback tests without learning a test scripting language. The
OpenST A can perform scripted HTTP and HTTPS for heavy load tests with performance measurements. BadBoy can convert captured scripts to load testing
tools like JMeter. The Karma test runner, which used to be called T estacular from Google is a popular and light weight web testing tool.

#13. JMeter to write performance testing scripts.
Good developers are lazy, and have a good grasp of “which tool to use when” to get the job done more productively .

---

## Q3: What are the dif ferent types of group sharing you have been in a software development project?

**Answer:**

#1. Project kick of f meeting.
#2. Daily stand ups to provide regular updates on what you worked on yesterday, what you will be working on today, and list of blockers.
#3. Requirements gathering, business requirements review, design review, test plan review, implementation plan review, code review, etc.
#4. Project hand-over meetings. Hand over to the support staf f.
#5. Project retrospective sessions. What worked well? what didn’ t work well? How to improve next time?, etc.

---

## Q4: What are the dif ferent types of testing performed in a software development project?

**Answer:**

#1. Unit testing where the developers test their code by writing unit tests. Code coverage tools are used to determine the extend to which the code is covered.
#2. System integration tests are generally conducted by the developers to test integration of their code with external CMS (Content Management) Systems like
Vignette, CRM (i.e Customer Relationship Management) systems like Siebel or Salesforce, and any other third party system. During development, the external
systems can be mocked, and during system integration testing, both happy path and unhappy paths like service failures, service timeouts, sticky versus non-
sticky sessions, and service retry need to be tested.
#3. Cr oss br owser compatibility testing. The modern applications are very rich in client experience and use lots of client side scripting like JavaScript. So, it is
imperative to perform browser compatibility testing to ensure that the code works with major browsers like internet explorer, Firefox, Google chrome, and
safari. There are tools like BrowserShots, IE T ester (for various IE versions), Adobe Browser Lab, etc.
#4. Performance T esting – uses automated tools that are designed to test and tweak system performance. For example, use JMeter to write performance testing
scripts.
#4. Load T esting – helps determine how well the product handles heavy demand for system resources. For example, how well a trading system handles panic
sells during a financial meltdown.
#5. Security T esting – to guard against accidental miss-use, hackers, or known computer malware attack. For example, using a tool like Skipfish from Google.
#6. Functional T esting

— Alpha T esting – is conducted after the majority of the software functionality is complete but before end-users are going to be involved.
— Beta T esting – is conducted after project code is complete.
— Acceptance T esting – is carried out by the testers, business analysts, and users to ensure that the system meets the functional and non functional
requirements. The software is packaged and released with release candidate version numbers like RC1, RC2, etc to the user acceptance testing environment.
#7. Regr ession T esting – is conducted to check if bug fixes have been implemented successfully. Also checks for the presence of new bugs or flaws that could
have been created from correcting the original errors and ensures no baseline functionality has been lost.
There are other testings like smoke testing or sanity checks to quickly test of a system is stable and functional. Another testing term used is the PVT (Post
Verification T esting) to see if the system functions as expected.

---

## Q5: What are functional requirements? What are Non-funcrional requirements?

**Answer:**

Functional r equir ements are captured in a document with use cases or via story boards which contain what a certain system has to do to achieve a number
of user objectives.This task is carried out during the priliminary stage of SDLC. For example, allow investors to place buy and sell trades. Prior to placing
trades, the user inputs need to be validated, etc. Use case diagrams are used to capture the user experience.
Non-functional r equir ements addresses aspects that a software will never function properly without them. Response times, security, reliability, accuracy, high
availability, and cross browser compatibility are examples of non functional requirements. The Non functional requirements decide how a software will be
percieved by its users. Users wouldn’ t be happy using a system that is not highly available and riddled with security holes.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03