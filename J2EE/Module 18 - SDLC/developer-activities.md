# Developer Activities in SDLC

> **Module**: SDLC  
> **Topic**: Developer Activities in SDLC

---

## 📋 Table of Contents



Don’ t get overwhelmed by this activities list. This proves why employers favor experience to just academic qualifications alone. It also emphasizes the fact why
good technical skills must be complimented with good soft skills and right attitudes get things done as a software developer .
I also vividly remember my first job interview for a Java developer role where I was asked if I understand SDLC. My answer was a big “No”, and then followed
up with a question to the interviewer — out of curiosity, what is SDLC? The interviewer response was — “it is complex, and cannot be explained in a few
sentences”.
Why is building a softwar e is mor e complex than building a bridge? 
Building a bridge is a concrete concept, where you can touch, walk on and feel the bridge. Bridge building has a well defined set of rules and processes to
follow .
Whereas developing a software is an abstract concept, where abstract requirements from the users, stake holders, and business analysts need to be converted to
rules and processes to accomplish the final product that meets both functional and non functional requirements. Some of these rules can have many
permutations and combinations, and makes it impossible to cater for all possible scenarios by the developers and testers. Software development environment is
very dynamic with multiple streams of projects working on the same code base by creating so called branches. Environments can easily become unstable if
proper control and processes are not in place.
Let’s see the SDLC activities from a Java developer perspective.
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
Are you overwhelmed by these tasks? Don’ t be afraid. It is a team ef fort. There will be cross functional teams with varying technical strengths. So, you
will be performing some tasks yourself, and taking on the role as a facilitator for the other tasks where you need to chase people to get things done. No wonder
why you need to have good non-technical skills to get things done as a Java developer .
Can you list any other developer activities not covered here?
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03