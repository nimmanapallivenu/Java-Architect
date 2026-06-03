# 1   4 real life examples of SAR technique   Java Success.com

## Table of Contents

The SAR (Situation- Action- Result) technique is not only useful for writing great resumes , but also very useful for job interviews to tackle open questions
like – T ell me a time where you identified & fixed an issue? What are your recent achievements that you are most proud of? What do you look for when you
review others’ code? Why do you like software engineering ? and the list goes on.
Firstly , you state the situation that existed in your workplace, then you describe what you did about it and finally you describe the beneficial outcome.
Example 1: Tuning performance
Situation: Performance problem where the application server had to be restarted every second day .
Action:
– Used JMeter to simulate the load conditions.
– Identified the cause of the problem to be leaking database connections due to not properly closing the connections under an exceptional scenario.
– Used the profiling tool “ VisualVM ” & identified a memory leak where long living objects were holding on to short lived objects. jvisualvm to detect memory
leak.
– Fixed the database connection leaks by closing the connections in a finally block.
– Fixed the code to release memory consumed by the short lived objects.
– Tuned the JVM & GC settings. 15 key considerations to write low latency applications in Java
– Load and endurance tested the fixed code with the load testing tool JMeter to confirm that the issue has been fixed.
Result: The application became a true mission critical 24×7 type with a much improved performance.
Example 2: Code quality
Situation: Java code that is hard to maintain and reuse. Changes to one module may break another module.
Action:
– Wrote unit tests with proper Mockito mock objects for the existing un-maintainable code.
– Introduced SonarQube to ascertain code coverage & code quality metrics. Fixed the blocker , critical, and major severity items.

– Re-factored the code with OO concepts and design patterns in a test driven manner to improve maintainability .
– Lar ge procedural style if/else statements were replaced with objects adhering to the Open-Closed design principle .
– Code duplication was eliminated with the help of SonarQube tool & refactoring.
– Reran the unit tests to ensure that the functionality is not broken due to refactoring.
Result: The application became much easier to maintain, extend, and reuse. The test coverage was increased from 27% to 76%.
Ensuring code quality in Java applications
Example 3: Quick wins
Situation: The financial service websites are ranked by an independent body , and this particular company’ s website that I was the technical lead for was ranked
23rd out of 31 possible companies that took part.
Action:
– I took the initiative with the collaboration of the business and technical leaders to launch a “ QuickW ins” project to improve the overall ranking of the
website.
– An independent user experience consultant was hired to analyse and produce a report with 18 most important things that can potentially improve the user
friendliness, look and feel, and ease of use of the overall website.
– Out of those 18 recommendations, 4 of them needed major design and development changes, and did not stack up well in the cost-benefit analysis.
– The remaining 14 recommendations were implemented within 3 months.
– The implementation was fast-tracked by adopting some of the agile development practices like iterative development, daily stand-up meetings, test driven
development, CI/CD pipelines and regular catch-ups with the business.
Result: This initiative was a major success and the website ranking was improved from being 23rd to 7th. The management was very impressed, and the
contributions were well noticed and rewarded. That was also one of my longest and rewarding contracts.
Example 4: Concurrency Management
Situation: The production ready application consumed very less CPU and response times were very poor due to heavy I/O operations like database read/write
operations.

Action:
– Monitored the CPU usage with V isual VM tool.
– Got a series of thread dumps , say 7 to 10 at a particular interval, say 5 to 8 seconds and analysed those thread dumps by importing the thread dumps into
“Samurai”, which is a visual tool.
– Paid attention to the blocked threads in red. Alternatively , VisualVM is handy for debugging deadlocks & analyzing thread dumps .
– Fixed the concurrency issue by reducing the synchronization granularity in the code.
– The of fending SQL statement was identified with a SQL query planner and tuned.
Result: The response times were halved and the average CPU usage increased from 45% to 98%.
Examples 5 to 8
on design, security , technical capabilities & process improvements: 5 – 8 real life examples of SAR technique for Java developers to standout in job interviews .
Key Take Away
Sell your strengths and well-rounded abilities to add value to the business. Don’ t lie by memorising these SAR examples as you will be grilled with further
technical questions. This site has examples & tutorials to get hands-on experience with some of these tools. There are Y ouTube videos and other articles freely
available on the internet if you know what keywords to look for . I have covered myriad of Q&As with some key tutorials to quickly get up to speed with the 16+
technical key areas.
The key traits are: 1) passion 2) strong technical skills, 3) necessary soft skills, and 4) right attitude .
So, don’ t get annoyed when you are technically grilled or challenged. If you don’ t know or in the wrong accept it. Show your enthusiasm to learn. Employers
are looking for well r ounded pr ofessionals , and not just techies.



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
