# Java industry knowledge & self assesment questions & answers

## Table of Contents

- [Q1: Why are you leaving your current job? What do you like and dislike about your cu](#q1)
- [Q2: What was the biggest accomplishment and failure in your current or past jobs?](#q2)
- [Q3: How do you keep your knowledge current?](#q3)
- [Q4: How do you know what technologies, frameworks or tools are in demand?](#q4)
- [Q5: How would you go about learning a new piece of technology , framework, or a tool](#q5)
- [Q6: Why do you think good programmers are often lazy?](#q6)
- [Q7: What qualities do you look for in an ef fective programmer? How do you recognize](#q7)
- [Q8: What do you understand by the term “hidden experience”?](#q8)
- [Q9: When you are are coding, what documentation do you have handy?](#q9)
- [Q10: Do you periodically review your strengths and weaknesses? if yes, what are your ](#q10)

---

## Q1: Why are you leaving your current job? What do you like and dislike about your current job?

**Answer:**

Firstly , never be negative about your current or previous jobs. Also, don’ t bring salary into the discussion.
Likes:
The team of 6 with varying strengths complimented each other to get the job done, and learned a lot from each other .
Enjoyed solving business problems and learning new languages (e.g. Groovy & Scala), paradigm (e.g. Functional Programming), technologies (e.g.
Apache Spark & Hadoop), and tools (e.g. Cloudera Manager Server and AgileCraft to manage the Scrum process)
Took pride in identifying gaps and getting them rectified in a collaborative manner .
Pro-actively identified and fixed performance issues using JMeter , VisualVM, thread dumps, and GC logs.
Increasing the unit tests coverage to 85% with Mockito for mocking, SonarQube for coverage, JBehave for BDD, JMeter for load testing, and “Metrics
Core” for capturing through-put and latencies.
Dislikes:
Reasons for leaving:

Not challenged enough at my current job. The company of fered a great opportunity as someone with 2-3 year experience, and appreciate the skills I
acquired there, but now with 5 year hands-on experience, and having completed 6 projects through the full SDLC, I am ready to take on more challenges.
Looking forward to expanding my domain knowledge in finance, especially in “high frequency trading”.
Looking for an opportunity to get more involvements in mentoring & applying my leadership skills.

---

## Q2: What was the biggest accomplishment and failure in your current or past jobs?

**Answer:**

Reflect back on your career achievements and provide quantified answers.
Achievements:
Successfully completed a full stack online trading system written in Java 8, Spring, and AngularJS, which handles 400+ concurrent users and integrates
with 4 other back-end systems .
Designed and developed a Java based non-blocking server that communicates with 4500+ EFTPOS outlets with a latency of 20ms
Spearheaded a “Quick-W in” program that improved the site ranking from 24th to 9th within 5 months in terms of user experience, responsiveness, and
reliability .
Failur es:
We all learn more from our mistakes or failures than from our successes or achievements. When you mention your mistakes or failures, make it a point to
mention what you had learned from those mistakes.
The team was using a cut down database in the development environment and the technical solution I provided worked well for the low volume of data,
but when was moved to production like data, it caused some performance issues. Learned a valuable lesson of validating the solution with more
production like data set early on in the SDLC. I quickly pr ovided the r evised & tuned solution within a week and got the big thumbs up fr om the
testers .
Wrote some JUnit based integration tests that were bit fragile due to data fluctuations. The failing tests were causing the builds to fail. T ook the initiative
to fix this problem by performing the integration tests via more stable data sets that were populated via DBUnit during the set up phase and the data were
removed during the tear down phase. Also, introduced other strategies for integration tests by using in memory databases like HSQL DB as opposed to the
actual database.

---

## Q3: How do you keep your knowledge current?

**Answer:**

Via online articles, blogs, and books.
http://www .infoq.com
http://www .theserverside.com
YouTube videos by V enkat Subramaniam, Martin Fowler , etc and following the channels like “Java Brains”, “Cave Of Programming”, etc.
Following T witter handles like “@BrianGoetz”, “@joshbloch”, “@AdamBien”, etc.

---

## Q4: How do you know what technologies, frameworks or tools are in demand?

**Answer:**

Some technologies/frameworks don’ t even make it to the mainstream. So, do your research before learning. 
1. Through online job portals like “Indeed.com”, “IT Jobs watch”, “Glassdoor .com”, http://www .google.com/trends, GIT commits, Stack overflow tags, etc to
understand what the prospective employers are looking for? what problems are they trying to solve? what are the modern trends? E.g. ETL vs EL T, MVC vs
MVW , BigData vs Data W arehouse, Actor based concurrency models? and so on.
2. Through personal interests, experience and networking with the fellow professionals via LinkedIn. For example, fascination for writing concurrent programs
due to the challenges the concurrency presents and tuning the design, code & the JVM to achieve better throughput & lower latency .

---

## Q5: How would you go about learning a new piece of technology , framework, or a tool?

**Answer:**

STEP 1: Get started with good online tutorials. Google for relevant tutorials.
STEP 2: Read online articles, blogs, and books about a particular framework, technology , or tool to understand its high level architecture, core concepts, best
practices, and potential pitfalls.
STEP 3: It is a best practice to always keep the Java API Docs handy for the framework or tool. Also, accessing the Git Hub link for the open-source code.
STEP 4: Watch a few Y ouTube videos on the topic to further improve the understanding.
STEP 5: If you still have any doubts to be clarified by searching on Google for StackOverflow answers. For example, search string like
Check with your mentors and networked fellow professionals via LinkedIn.com, quora.com and forums like JavaRanch.com.
STEP 6: Find a way to apply or use a particular framework, technology , or tool in a commercial, self-taught, or open-source project. Nothing beats getting
hands-on . All your previous ef forts will go in vain if you don’ t apply .Spring error site:stackoverflow .com
Spring error site:java-success .com

STEP 7: Add this particular framework, technology , or tool to your resume’ s or CV’ s skills and experience section. Also, blog about your experience as you not
only learn more by blogging as you have to research more, but also it will be very useful as your future reference material and for increasing your employ-
ability by showing of f your passion and the technical know how , only if done uniquely & properly .
You can explore one of the following blogging tools.
http://www .wordpress.or g
http://drupal.or g
http://www .joomla.or g
http://www .blogger .com

---

## Q6: Why do you think good programmers are often lazy?

**Answer:**

Good programmers
hate repetitive and monotonous tasks. They find the right tools and ways to automate these monotonous and repetitive tasks.
don’t reinvent the wheel. They will first look for the right tool, API, library , or framework to get the job done with minimal ef fort.
constantly learn and refactor the code with good code coverage. They start to dislike the code that they wrote an year ago.
ask the right questions. Is this code thread-safe? What if an exception is thrown here? Should this class be immutable? Is this class open for extension, but
closed for modification? W ould this logic require automated unit tests only , integration tests only , or both? Should this config be externalized? W ould this
be a good candidate to be written as a functional program?

---

## Q7: What qualities do you look for in an ef fective programmer? How do you recognize a good programmer?

**Answer:**

General T raits
Passion, continuous learning, taking pride in their achievements, ability to look at the big picture and pay attention to details, right attitude, and ability to
communicate their thoughts clearly . For example, talk to the business and the stake holders without any technical jar gon. Most importantly , getting things done .
Specific T raits
1) Ability to write quality code with best practices & good test coverage.
2) Ability to break a complex problem into manageable chunks with relevant conceptual design diagrams, UML diagrams, ER diagrams, algorithms, and
pseudo-code
3) Ability to think and ask the right questions in terms of the 16 key ar eas?

---

## Q8: What do you understand by the term “hidden experience”?

**Answer:**

Good programmers are mainly self-taught and they acquire lots of so called “hidden experience” through proactive and continuous learning and helping
others solve their problems. If you just rely only on your experience alone, it can take a lot longer to learn the “ key ar eas” and the “ basic concepts ” of
programming. It is imperative that you bring out your “ hidden experience ” in your CV and at job interviews.

---

## Q9: When you are are coding, what documentation do you have handy?

**Answer:**

1. The APIs for the r elevant technologies used . For example Java API, Enterprise Java API, Spring API, jQuery API, JavaScript API, etc. Thes APIs can also
be googled at will with the right search keys like “jQuery API”, “Java 6 API” , etc.
2. The r elevant r eference manual and home web sites bookmarked for the relevant technologies. The home sites can be Googled with right keywords like
“Spring framework home”, “jQuery home”, etc and the reference manual can be Googled with keywords like “Spring framework 2.0 reference”,etc.
3. Often not knowing the key terms for a particular piece of technology is a major challenge for the beginners of a particular technology . This is where the
“cheat sheets” come in very handy
4. Sites like Java Practices provide good working examples.

---

## Q10: Do you periodically review your strengths and weaknesses? if yes, what are your current weaknesses?

**Answer:**

When you mention a weaknesses, make it a point to clarify as to how you are going about fixing improving your weaknesses. Y ou can also flaunt your
industry knowledge.
Example:
Recently identified that I don’ t have much knowledge or experience on scripting languages like Perl or Python. Hence, I have recently started working on
some Python tutorials from site XYZ.
JavaScript based frameworks like Angular JS is gaining lots of adoption, so I have started learning JavaScript and Angular JS in my spare time.
Heard everyone talking about BigData, so downloaded the Cloudera express server on VMware, and wrote some basic Spark jobs to gain more insights
into Hadoop.
In some cases a weakness can be construed as a strength
Example:
When I am faced with a complex problem, I tend to keep at it for a long time until I reach a resolution. This gets me into looking more within the square.
Even though I know that taking a break from the problem or chatting to a colleague is more conducive to looking outside the square.

So, I am currently working on taking a break from a complex problem after trying for say 45 minutes or so. Often I get a solution whilst taking a brief
walk, commuting to work or explaining the problem to a colleague.
At times I tend to be less of a listener and more of a talker to add value or contribute. I strongly understand that a good communicator must be a great
listener , hence I have been consciously working on this, and recently noticed a significant improvement.

---



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
