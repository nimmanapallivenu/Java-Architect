# Agile Methodology

> **Module**: SDLC  
> **Topic**: Agile Methodology

---

## 📋 Table of Contents



- [Q1: What are the typical roles and responsibilities of an agile core team?](#q1)
- [Q2: What do you understand by the terms epic and user story?](#q2)
- [Q3: What do you understand by the term “Condition Of Satisfaction (aka COS)”?](#q3)
- [Q4: How do you estimate development and testing ef fort of a user story?](#q4)
- [Q5: What are some of the key features and objectives of the daily stand-ups?](#q5)
- [Q6: What are some of the key features of agile development?](#q6)
- [Q7: How do you know that you are using agile development?](#q7)
- [Q8: What is a task borad?](#q8)
- [Q9: What do you understand by the agile term timeboxed?](#q9)
- [Q10: What are CRC cards?](#q10)
- [Q11: What do you understand by the term “collective ownership”?](#q11)
- [Q12: What do you understand by the term Behavior Driven Development (BDD)?](#q12)
- [Q13: What do you understand by the terms user stories, story mapping and story splitt](#q13)
- [Q14: How is an agile project monitored for progress?](#q14)
- [Q15: What are dif ferent roles in an agile project?](#q15)
- [Q16: What are the key benefits of agile mehodology?](#q16)
- [Q17: It looks too good, now what are the drawbacks of agile?](#q17)
- [Q18: What are the agile resource Management principles?](#q18)

---

## Q1: What are the typical roles and responsibilities of an agile core team?

**Answer:**

The Product Owner represents the stakeholders and is the voice of the customer. He or she is accountable for ensuring that the team delivers value to the
business. The Product owner writes typically the user stories, prioritizes them, and adds them to the product backlog.
The Development T eam is responsible for delivering potentially shippable product in increments at the end of each Sprint. A Development T eam is made up of
3–9 people with cross-functional skills who do the actual work (analyze, design, develop, test, technical communication, document, etc.). The development team
consisting of developers, testers, operations staf f, etc define the definition of “Done”, which includes non-functional requirements like security, performance,
cross browser compatibility testing, etc. Generally, you will have the doers and the helpers in the development team. The Development T eam in a Scrum is self-
organizing.
Scrum Master is a facilitator, and is accountable for removing impediments to the ability of the team to deliver the sprint goal/deliverables. The Scrum Master
is not the team leader, but acts as a buf fer between the team and any distracting influences. The Scrum Master ensures that the Scrum process is used as
intended. The Scrum Master is the enforcer of rules. A product manager cannot be the scrum master .

---

## Q2: What do you understand by the terms epic and user story?

**Answer:**

An agile Epic is a group of related user stories. For example, an online shopping cart app will have epics like user administration, product management,
and shopping experience. An epic will have a number of broken down user stories. For example, the “user administration” epic can be broken down into user
stories like “add, modify, and delete user”, “manage user passwords”, “generate raw data downloads”, “generate aesthetically pleasing reports”, etc.

Agile theme, epic, story and task
— A theme is an objective that may span projects and products. Themes can be broken down into sub-themes, which are more likely to be product-specific. At
its most granular form, a theme can be an epic.

An epic is a group of related user stories. An epic needs to be broken down into a user story before it is sized using fibonacci number (eXtra Smal [XS=1 pt]
Small [S = 2 pt], Medium [M = 3], large (L = 5), Xtralarge (XL = 8), XtraXtralarge [XXL = 13pts], XtraXtraXtralarge [XXXL = 21 pts] ) and introduced
into a sprint. The sizing is similar to the T -shirt sizes. These points are also known as the velocity points to monitor progress.
— A user story is an Independent, Negotiable, V aluable, Estimatable, Small, T estable requirement, which is abbreviated to “INVEST”. Even though stories are
independent, they have no direct dependencies with other requirements and user stories may be combined into epics when represented on a product road map.
User stories need to be defined prior to sprint planning in ABC (Accelerated Business Case) sessions where the development teams, QA (i.e. testing) teams and
product managers can use them to discuss, size and prioritise at sprint-level. User stories will often be broken down into tasks during the sprint planning Process
unless the stories are small enough to be consumed on their own. T ools like “MindMap” is used to capture the stories in the form of COS (i.e. Condition Of
Staisfaction).
— The sized cards are prioritized interms of its value add to achieve the MVP (Minimum V iable Product) defined by the product manager and the business
owners.
The user stories are in the format of
“As Who I want What so that Why ”
Here is an example.
“As an Investment Manager I want to be able to view client account balances, so that I can make informed investment decisions. ”
“As a support staf f I want the system to allow me run multiple searches at the same time, so that I can do my job faster. ”

---

## Q3: What do you understand by the term “Condition Of Satisfaction (aka COS)”?

**Answer:**

For each story, the business user, analyst, owner, or stake holder will define the “ condition of satisfaction (COS) ” criteria to satisfy that particular story .
You can also think of it as an “acceptance criteria”. A COS questions the User Story, and encourages conversation between the Product Owner and the team. A
good way of gathering COS is asking questions such as:
— ‘What if … ?’,
— ‘Where …?’,
— ‘When …?’,
— ‘How …?’.
The test cases will be written by the testers and development tasks will be carried out by the developers to satisfy the COS. T o be accepted, the development
task should also satisfy other non functional tasks like writing unit tests, continuous integration and build, security, performance, data archival, etc. The story
will also have to be fully tested. For example, COS will look like
– A search functionality to be able to search clients by client code.
– Display the client list sorted in alphabetical order .
– Ability to click on a particular client to display his or her account balances.

– The account balances need to be sorted by account type.
….and so on including non functional requirements as well.

---

## Q4: How do you estimate development and testing ef fort of a user story?

**Answer:**

Agile projects normally have a number of sprints to finish the user stories. Each sprint will be of 2-3 weeks. One or more user stories are allocated to each
sprint. The developers and testers will estimate on it to have story developed and fully tested to satisfy the COS. If a story is big enough so that cannot be
completed within a sprint (i.e. in 2 weeks), that particular sub story can be split further into 2 or more stories.
agile story with points allocated
The stories are estimated by allocating points. It is known as the “velocity points”. The velocity points are allocated to each story. Stories are tagged like T -shirt
sizes — Small, Medium, large, and Extra large, etc. Each of these sizes are allocated velocity points using the Fibonacci series values. For example, 3, 5, 8 and
13. Where Small is given 3 points, Medium is given 5 points, large is given 8 points, and extra large is given 13 points. A the end of each sprint, the velocity
points for all the user stories are added and reported to the management in a “showcase” to monitor progress. The points play(i.e. the user stories that are not
completed within this sprint) that are added to the next sprint.

agile velocity diagram
In the diagram above, you can see “commitment” and the “completed”.

---

## Q5: What are some of the key features and objectives of the daily stand-ups?

**Answer:**

Daily Stand-ups are named literally after the way the meeting is conduced — i.e. while standing up. This method helps ensure the meetings remain time-
boxed to 15 minutes and not dragged on.
The general featur es ar e:
– It is a ritual that happens at the same time, in the same location, every day .
– The meeting is led by the Scrum Master, who keeps all attendees focused on answering the following key – questions to meet the project objectives.
1) What have I achieved since the last Stand-up?
2) What do I intend to do before the next Stand-up?
3) What are my impediments to making progress?

Here is the picture of user stories stuck to a board, and this is where the daily “stand ups” or “scrum sessions” take place.
agile stand up board

---

## Q6: What are some of the key features of agile development?

**Answer:**

– Collective code ownership and freedom to change with proper unit tests, integration tests, and continuous build & integration.
– Incremental approach (e.g. user stories are incrementally implemented)
– Automation (e.g. TDD — T est Driven Development and continuous build & integration).
– Customer focused (for e.g. internal and external users and business analysts are your immediate customers)
– Design must be simple. Designing is an ongoing activity with constant re-factoring to achieve the rules of code simplicity like no duplication, verified by
automated tests, separation of responsibilities, and minimum number of classes, methods, and lines.

---

## Q7: How do you know that you are using agile development?

**Answer:**

You are using an agile practice when
– You have daily stand-up meetings.
– You use CRC (Class Responsibilities and Collaborators) cards.
– You use timeboxed task boards.

– You use TDD (T est Driven Development) or BDD (Behavior Driven Development), Continuous Integration, regular code reviews, pair programming,
automated builds, continuous deployment and delivery, etc.
– You have iteration planning meetings and carry out iterative development.

---

## Q8: What is a task borad?

**Answer:**

It is generally a white board divided into 3 sections — T o Do, In Progress, and Done. Each task is written on a sticky note, and moved from one section to
another to reflect the current status of the tasks. The task board is frequently updated, especially during the daily stand up meetings. Dif ferent layouts can be
used.
Each task allocated to each team member is timeboxed. Y ou can have variation to the layout as shown below and each sticky not can have points that add up
towards the velocity points (calculated by adding up the estimates of the features, user stories, requirements or backlog items that are successfully delivered in
an iteration. ).
Agile task board
The task board is also known as the kanban board. Kanban is a Japanese word meaning card or sign. Each card or sign is equated with a user story. Whenever a
particular user story is blocked for whatever reason, then the priority is to clear current work-in-process with the help of other team members to help those
working on the activity that’ s blocking the flow .

---

## Q9: What do you understand by the agile term timeboxed?

**Answer:**

A timebox is a previously agreed period for a particular task to be completed by an individual or team. The key aspect of the timebox approach is that

stopping of work when the time limit is reached and evaluating what was accomplished instead of allowing the work to continue until the goal is reached, and
evaluating the time taken.

---

## Q10: What are CRC cards?

**Answer:**

CRC stands for Class, Responsibilities, and Collaborators. It is used for rapidly sketching an Object Oriented design and playing out the roles and
responsibilities to validate the design. The role play dialog will be something like
Hello, I am a trader and responsible for placing and cancelling buy and sell orders on behalf of my customers. Before placing a trade, I must know my trader
details like number, name, address. I need to collaborate with order to fill in the relevant order details.

Agile – CRC

---

## Q11: What do you understand by the term “collective ownership”?

**Answer:**

Collective ownership, as the name suggests, every team member is not only allowed to change other team member ’s code, but in fact has a responsibility
to make changes to any code artifact as necessary. This means every developer will review code written by others when integrating others’ changes from the

code repository into their code to familiarize themselves and to identify any potential issues and mistakes. Every developer will be motivated to check in the
code progressively and incrementally with proper automated unit and integration test cases as part of the continuous code integration.

---

## Q12: What do you understand by the term Behavior Driven Development (BDD)?

**Answer:**

Behaviour -Driven Development (BDD) is an evolution in the thinking behind T est Driven Development (TDD — W riting tests before writing code) and
Acceptance T est Driven Development (A TDD — write acceptnce tests, and for many agile teams, acceptance tests are the main form of functional specification
and the formal expression of the business requirements). The BDD basically combines TDD and Domain Driven Design. It aims to provide common vocabulary
that can be used between business and technology .
The acceptance tests are generally written using the “Given-When-Then” approach. For a given story/context, when some action is carried out, then a set of
observable consequences should be obtained. For example, Given that you have enough available cash, when you place a trade within your available cash, then
placing of your trades should succeed without any errors.

---

## Q13: What do you understand by the terms user stories, story mapping and story splitting?

**Answer:**

User stories: Dividing up of the customer ’s or product owner ’s requirements into “functional increments” so that it can be worked on via the task board.
This is done in consultation with the customers, product owners, or business analysts.
Story mapping: When you have a backlog full of user stories, you can select a few of them to work on during the next iteration. This step involves ordering of
the user stories. The “map” arranges user tasks along the horizontal axis in rough order of priority and the vertical axis addresses the implementation details. It
can be done either on a white board with sticky notes or using tools like Silver Stories.
Story splitting: Before a story is ready to be scheduled for implementation, it needs to be small enough to pass the usual rule of thumb that “a story should be
completed within the iteration”. So, “story splitting” consists of breaking up one user story into smaller ones, while preserving the property that each user story
separately has measurable business value.

---

## Q14: How is an agile project monitored for progress?

**Answer:**

Any one who has worked on an agile project would have come across thes key terms:
Colocation
Frequent delivery
Daily standups
Timeboxed tasks
The best evidence that a software project is on track is working software, preferably deployed to production.
Secondly, a burn down chart is used to present the progress to the management. Each sprint is basically 2 weeks and this is plotted on the X axis. The Y axis will
have the velocity points. In each Sprint depending on the team size, a number of sized up cards are picked and the individual points are added up. For example
say, 40 points. This chart is about how quickly you burn through the stories. These graphs can be plotted manually or via tools like Excel spreadsheet or JIRA.

agile velocity graph or burn down chart
Velocity is the key to agile project management.

---

## Q15: What are dif ferent roles in an agile project?

**Answer:**

Mandatory r oles:
Team lead: This role, called “Scrum Master” in Scrum or team coach or project lead in other methods
Team member: This role, sometimes referred to as developer or programmer, is responsible for the creation and delivery of a system. This includes modeling,
programming, testing, and release activities, as well as others.
Product owner: The product owner is responsible for the prioritized work item list
Stakeholder: is anyone who is a direct user, indirect user, manager of users, senior manager, and operations staf f member .
Optional r oles that ar e typically adopted only on very complex pr ojects
Technical experts: Sometimes the team needs the help of technical experts, such as build masters to set up their build scripts or an agile DBA to help design
and test their database.
Domain experts: Sometimes the product owner will sometimes bring in domain experts to work with the team
Independent tester: to validate functional and non-functional testing. For example, security testing, performance testing, cross browser compatibility testing,
etc.

---

## Q16: What are the key benefits of agile mehodology?

**Answer:**

– Decreased time to market as agile process is based on the philosophy of early and regular releases. The iterative nature of agile development means features
are delivered incrementally, enabling some benefits to be realised early as the product continues to develop.
– Better quality as testing is integrated throughout the sprints, enabling regular inspection of the working product as it develops. This allows the product owner
to make adjustments if necessary and gives the product team early sight of any quality issues.
– Improved communications due to active involvement and colocated multi-disciplinary teams. This provides excellent visibility for key stakeholders, both of
the project’ s progress and of the product itself, which in turn helps to ensure that expectations are ef fectively managed.
– Lower costs because unlike in traditional development projects, where you write a big spec up-front and then tell business owners how expensive it is to
change anything, particularly as the project goes on. In fear of scope creep and a never -ending project, we resist changes and put people through a change
control committee to keep them to the bare minimum. Agile development principles are dif ferent. In agile development, change is accepted. Instead of rejecting
changes, the timescale is fixed and requirements progressively emer ge and evolve as the product is developed.

---

## Q17: It looks too good, now what are the drawbacks of agile?

**Answer:**

– Agile is a very sophisticated process and requires full management commitment and requires experienced and dedicated teams to achieve things within the
allocated sprints. Otherwise, everything will blow up to become a backlogs.
– Time boxing can encour gage agile members to cut corners in terms of unit test coverage, documentation, code quality, design, failing to test negative
scenarios, etc. It can also lead to over worked staf f who lose motivation.
– It assumes that all developers are created equal. Doing a particular task might be a 5 point task for developer A, whereas for developer B, it takes 20 points.
For example, this can happen if developer B is a back-end developer and not experienced with GUI development.
– Not all projects nor all phases are good candidate for agile. Y ou should use agile if you can deliver tangible releases that can be tested by the testers within the
sprint. For example, a GUI screen. Some projects may spend months developing back-end architecture without any tangible deliveries. W ebsite development is
a good candidate for agile development. It is also not suited where dif ferent teams use dif ferent methodologies. What happens when a development and QA
team adheres to the principles of Agile, but the platform and production support do not?
So, there are pros and cons and some or ganizations make use of the hybrid approach to get the best of both worlds. Agile is commonly believed to be a set a
practices, processes and tools, when in fact, Agile is really more of a mind-set and culture.

---

## Q18: What are the agile resource Management principles?

**Answer:**

The resourcing principles can be abbreviated to F ASTEST as shown below .

Agile resourcing principles
Agile projects require require experienced developers and testers to get things done in allocated sprints. More and more or ganizations are getting into agile
methodology, and it is really worth learning and experiencing it. I was fortunate enough to work on a number of such projects. These projects do need full

commitment and support of the senior management.
Have you completed this unit? Then mark this unit as completed.
 Mark as Completed
« Previous Unit Next Unit »

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03