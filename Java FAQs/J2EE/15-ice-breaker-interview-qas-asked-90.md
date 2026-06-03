# 01  15 Ice breaker interview Q&As asked 90%   Java Success.com

## Table of Contents

Q1. T ell us about yourself?

— Acted as a change agent to enforce standards and improve processes. A change agent not only changes a system’ s or application’ s behavior but also
people’ s behavior , attitude, and culture towards writing quality software. People’ s behavior can be changed by enforcing good coding standards. Substandard
software development processes can also demotivate developers and adversely impact their behavior and attitude. The development processes need to be
continuously improved by adopting good tools and practices.
— Identified performance issues and bench marked them with JMeter to prevent the server from having to be restarted every second day . I have never
been in a project or or ganization that is yet to have any performance or scalability issues. It is a safe bet to talk up your achievements in fixing performance or
memory/resource leak issues in job interviews. Unlike web security testing, performance testing is very prevalent in many applications. Premature optimization
of your code is bad as it can compromise on good design or writing maintainable and testable code. But one needs to be aware of potential causes for
performance issues that can occur due to major bottlenecks in a handful of places or minor inef ficiencies in thousands of places (i.e. death by thousand cuts ).
— Championed continuous improvement programs. Or ganizations must develop a culture of continuous improvement in order to flourish. I am yet to work
for an or ganization that did not have any environmental or process related issues and challenges. Many of these tasks can be simplified , automated , and
integrated to avoid monotonous and error prone human intervention. So, experience with the agile practices, continuous integration and build, environmental
improvements, etc will be of great asset to any or ganization. A software must be designed for deployability . Deployability is a non-functional requirement that
addresses how reliably and easily a software can be deployed from development into the production environment.
— Assessed non-functional r equir ements . Many non-functional requirements like security , performance, transaction management, thread-safety , quality
of service, etc are often neglected or overlooked during application development. Inexperienced or rushed developers can create systems that work great in
development, but have many problems in the test or production environments due to sheer load, concurrent access, locale or timezone settings, portability issues
due to operating system specific file separator or new line character , hard coded values requiring externalization, etc. If you identify any gaps in these non-
functional requirements, take the initiative to close the gap as you would do with any functional requirements.
16 technical key areas Q&As with code & diagrams will help most of the drill down questions you can get.
Train for the following Q2 to Q15 via SAR technique .
Q2.T ell us about the most satisfying pr oject you worked on in the last year? What wer e your contributions?
[Hint : Rewriting the whole batch process framework that not only used to take 17 hours to run but also was very hard to maintain as changing one part of the
code can break another part of the application. The re-written batch framework with modular design, multi-threading, distributed caching, tuned SQL
statements, and revised database tables completes in under 3 hours. My contributions involved designing the solution and leading the development.]
Q3. What ar e the challenges you faced both technically and non-technically in your past assignments?
[Hint : Fixing performance, thread safety , transactional issues and security holes, spearheading process improvements, taking the initiatives to close the gaps in
business requirements, convincing the management with the open-source adoption, streamlining the release and deployment processes with CI/CD pipelines ,
and getting the business users to agree on alternative solutions.]

Q4. Why do you like softwar e development?
[Hint : Passion for building things, and satisfaction you get in seeing all the functional requirements and non-functional technical key areas like design, design
patterns, thread safety , performance, security , code quality , etc fit together like a puzzle . Solving problems relating to above mentioned key areas gives me a
sense of accomplishment. Like the fact that one needs to look at the big picture as well as pay attention to details. It is also quite interesting to look at things
from both business/end user perspective (e.g. user interface design, ease of use, how the business operates) and technical perspective. Derive pleasure in
working with people to get things done in a team environment by collaborating, mentoring, and facilitating to get things over the line.]
Q5. What do you do when the r equir ements ar e not clear?
[Hint : take initiatives to get it clarified and properly documented. Conduct workshops, meetings, and brain storming sessions to gather requirements. Clearly
document your assumptions, potential risks, impacts, and mitigation strategies.]
Q6. How would you pr oceed if you had been assigned a task that involves an unknown technology or business domain?
[Hint : Learn by doing a quick tutorial and grasping its fundamentals. Seek others’ help. T ake it as a challenge. For example, had to collaborate with the access
control team that looks after Siteminder to get SSO (Single-Sign-On) working for our application.]
Q7. In your experience, what ar e some of the harder issues to r esolve, and how would you go about r esolving them?
[Hint : Thread-safety and transactional issues. A better candidate will also touch on non-technical challenges like ef fectively managing changing requirements,
proactively gathering requirements, and taking the initiatives to closing the gaps in requirements and technical design. They may also touch on implementing
agile practices , recognizing the importance of collaborative ef fort, following up with the business for clarification or convincing the business with simpler
and more ef fective solutions to meet their requirements. For example, identified an intermittent database deadlock issue by emulating user
experience under load with JMeter , and fixed it by adding deadlock retry service interceptors to address intermittent database lockouts and service
outages.]
Q8. Describe the types of teams you have worked in, and tell me what worked well and what did not?
[Hint : Team with generalists and specialists who complement each other to achieve a common goal. T eam that engages relevant agile practices for better
communication and corporation. W ell balanced team with right technical skills and good soft skills. T eam that hires good calibre people via rigorous screening
process.
A team that blame each other , a team that does not communicate well with each other , a team that says “I did it” instead of “we did it”, etc ]
Q9. What was the hardest “sell” of an idea or method you have had to make to get it accepted?

[Hint: Adoption of open-source technologies and agile development practices. Quick win strategies to enhance user experience and website rankings.
Convinced the business and the management to rewrite an application as opposed to providing a band-aid solution.
Good programmers are open to new ideas and constructive criticisms. Most importantly communicate their ideas ef fectively in simple terms with examples,
mockups, demonstration, diagrams, and analogy]
Q10. Can you draw me a high level ar chitectur e of the system you wer e involved in your last or curr ent position?
[Hint : Good candidates are careful to explain things well in simple terms that even a non-technical person can understand. They will start by drawing a simple
box diagram of the architecture and elaborate it further based on their tar get audience – business goals achieved, design alternatives, design decisions, lessons
learned, protocols & data formats used, concepts & patterns applied, development methods used, choice of frameworks, etc. ] Java integration styles &
architecture Interview Q&As .
Q11. Explain how application you built added value to your business and what was the business pr oblem that it solved?
[Hint: Better customer experience by fully automating a number of manual and partially automated systems. Less error -proneness and much shorter lead times
leading to more loyal customers, and much happier support staf f. ]
Q12. T ell me about an instance wher e you had to solve a complex design issue and how did you go about solving it?
[My experience : A customer order with a complex life cycle of state changes like pending, authorized, completed, canceled, amended, partially filled, rejected,
purged, etc had to be managed properly by capturing the state changes. A state chart diagram was created to capture and clarify things. Broke the problem into
more manageable tasks. Did my own research to get a better understanding and others’ perspectives on the problem. Used process flow , sequence, and state
diagrams to get better clarity and presented it ef fectively to others to ensure that we are on the same page. Prepared a matrix highlighting all possible “ what if ”
scenarios. W rote down the possible solutions. Analyzed the pros and cons of each solution. Listed how my solution addressed each scenario. ]
Q13. Have you ever been assigned a task of optimizing an application, and how did you go about it?
[My experience : Complex online reports generation clogged up the application with high CPU usage. Converted the report generation process to be
asynchronous by submitting the report requests captured via online GUI to a queue, and having a separate process pickup from the queue to generate the report
asynchronously and make the report available either via emails or file downloads. The report generation failures are reported via emails.]
[Hint : Understand user behaviors and gather load requirements. Monitor the application and gather metrics. Identify and isolate any similar patterns or
behaviors. Simulate the load in a non-production environment using tools like JMeter to gather benchmarks. Profile the application under simulated load.
Identify the bottlenecks. Fix the bottlenecks through relevant design changes, configuration changes, and code changes. Run your simulated load tests again, and
compare the results against the benchmarks. Document the results for future tuning and reference.]
Q14. What ar e the critical factors you look for in evaluating the work of others?

[Hint : Ability to look at the bigger picture and drill down to details. Ability to get things done methodically by keeping things simple and asking the right
questions – what if an exception is thrown?, What if this method gets accessed by multiple-threads?, etc. Passion to write quality code by constantly improving
on the “first-cut” or draft code with proper test cases. Motivation to use code quality and unit test coverage tools like Sonar . ] Ensuring code quality in Java
Q&As .
Note : Remember the fact when you talk about others, the interviewers are thinking about your suitability .
Q15. Describe a situation in which you had to solve a pr oblem without having all the information you needed – what did you do and what happened?
[Hint : In lar ger or ganizations, knowledge and expertise are scattered. Collaborate within and outside your team to gather relevant information. Or ganize
meetings and send emails requesting for relevant documentation. Conduct work shops and brainstorming sessions to pick others’ brains. Come up with all
possible causes. A cause effect diagram can come in handy for more complex problems. Identify and isolate possible causes, and try to reproduce it by
emulating the real life scenarios with appropriate load.]
Q16. When you have competing tasks, how will you go about managing your time to get things done?
[Hint : Firstly , apply the 80/20 rule that suggests 20 percent of your activities will account for 80 percent of your results. This being the case, you should
change the way you set goals forever . Prioritise your tasks into 4 quadrants as in important/not ur gent, important/ur gent, not important/ur gent, not important/not
urgent.
You have to make the time for the important, but not ur gent tasks . Because if you don’ t important but not ur gent tasks have a way of becoming ur gent – and still
important. Make sure you keep up with your important and ur gent tasks, but most importantly , make time for the important, but not ur gent tasks .
For example, Applying the 16 technical key areas, etc are important, but not urgent tasks .
]
Q17. What is the key consideration when setting objectives to achieve both car eer & work goals?
[Hint : Your goals must follow the “ SMAR T” principle.
Objective: “I want to incr ease my earnings to 1 10K/year as a senior Java developer in 8 months .”
Specific : The objective is very specific to become a senior Java developer .
Measurable : 110K is measurable.

Attainable : Attainable as my current salary is 80k, and I am going to get a good handle on the 16+ technical key areas in 8 months to perform well in my job
interview .
Relevant : it is relevant to my overall career goal of becoming an architect in 2 years to earn 150K+.
Time-bound : In 8 months.
]

[Hint : Focus on your strengths and marry up your strengths with the job specification. For example,
— 5 year hands on experience in building web based applications to integrating disparate systems using web services, messaging, and XML/JSON based
data formats.
— Completed 3 pr ojects thr ough full SDLC pr ocess using sought-after technologies X, Y , and Z, and frameworks P and Q in 2 of those projects. One
of the projects was a mission critical transactional system that handles 300 concurr ent users
— Enhanced my skills in micr o services based ar chitectur e at XYZ Bank.
— Initiation of any “ QuickW ins” projects. The focus is to improve the overall ef fectiveness and usefulness of a system through small changes in a
collaborative ef fort with the business. For example, automating tedious manual tasks, improving website user experience/rankings, stabilising the
environment, etc.
— Fixed security holes. Security is of paramount importance to any application or website. Applications with security vulnerabilities can not only tarnish the
reputation of a company , but also can adversely impact the bottom-line of that or ganization.
— Isolated and r eproduced intermittent issues . Intermittent issues are hard to reproduce and debug. A novice developer will promptly mark those
defects in the bug tracking system as “cannot be reproduced” without having the skill to isolate, reproduce and then fix the issue. An experienced developer with
good grasp on the key areas will not only have will not only have the competency to isolate, reproduce, and fix these issues, but also will have the skill to
identify these potential issues during code review .



**Source**: Extracted from PDF
**Last Updated**: 2026-06-03
