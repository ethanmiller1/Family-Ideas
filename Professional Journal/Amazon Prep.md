# Ethan's #1 Tip: Take control

Conduct the interview as if you were the interviewer. I've conducted interviews before and I know that most interviewers aren't giving this interview as much thought as the interviewee. They can be nervous too. So break the ice, take control, and say something, "Hey Hamid, thanks for joining." Calm, cool, and high energy. You are totally relaxed. This is how I do my interviews and the technique always give me an edge over other candidates that sound nervous and awkward, or too enthusiastic and unnatural.

[Polya’s Problem Solving Techniques](https://math.berkeley.edu/~gmelvin/polya.pdf)

# Hamid

## Email

[Interview prep for Software Development Engineer (SDE)](https://www.amazon.jobs/en/software-development-interview-prep?cmpid=ECOTOT700005B#/)

1. **Interview journey.** In this section, you'll learn what the interview process looks like from start to finish and who you’ll meet along the way. Estimated time: 10 minutes.
    
2. **Technical interview competencies.** This is divided into **system design** and **coding competencies**. These sections will dive deeper into the competencies, including examples of questions asked and how to best showcase your skills. Estimated time: 65 minutes for course content, _Additional enhanced content_ (podcast and video): 40 minutes play-time
    
3. **Leadership Principles.** In this section, you'll review Amazon's Leadership Principles and discover how to best answer questions based on the STAR method. Estimated time: 35 minutes.


## **Interview competencies**

### Technical Competencies

- [ ] **System design competency**
    
You should have a working knowledge of a few common and useful **design patterns**, along with how to write software in an object-oriented way. You likely won’t be asked to describe the details of how specific design patterns work, but expect to apply concepts to the particular problem and defend your design choices.
    
1. software components
2. distributed systems
3. system trade-offs

Be prepared to discuss:
- [ ] deployment
- [ ] scaling
- [ ] failures
- [ ] availability
- [ ] performance
- [ ] latency and concurrency

#### Clarifying questions:
-   Who are you designing the system for and why?
-   What expectations do they have in terms of functionality?
-   What things would a customer just assume will be in the system but they may not think about in the forefront of their minds? (e.g. it'll be fast and secure)
-   What happens if we become hyper-popular with customers? What does 2x growth look like? Or 10x? And how would that influence the design?
-   Things that influence the design typically include: non-functional requirements (amount of load, load distribution, security), ways of interacting with the system (user access, scheduled processes, synchronous/asynchronous communication), and data flow.
-   Explicitly mention all the assumptions that are being made.
-   Spend majority of time on the Critical Requirements identified and on the core functionality.
-   Justify the design choices that are made.
-   Design the solution for scale, i.e. would more transactions seamlessly work.
-   Research and read through best practices in the tech stack or infrastructure you would like to use. It helps to know how code and hardware can be maintained, scaled, and be made available.
-    What are the key business and technical metrics for the system? How will someone use that to identify problems?
-   What are the potential bottlenecks or pain points?
-   What failure points exist?
-   What redundancy can we build in to reduce single points of failure?
-   How does someone get logs and debug the system?
-   If there are multiple ways to design a system that satisfy the requirements, describe the trade-offs of both approaches, choose one and explain why. Having a conversation with the interviewer and explaining your thought process, might help you to make a choice.
-   We are looking for a high level understanding of how to design a system and deep knowledge into at least one area. If the interviewer asks you to dive deep into something you are unfamiliar with, tell them and suggest some other area you are familiar with and dive into that.
-    Domain Driven Design
-    On system design, we are talking more about different components and how those interact between each other on a high level on a product that we are building

- Nth Tier Architecture (Front-end, Middle Tier, Back-end)
- Microservice Oriented Architecture (Payment Service, Fulfilment Service, etc)
- Data Design (User table, Order Table, Product Table, ORM like Hybernate)
- Centralized Logging
- API (Integration point for Frontend/Backend)
- Domain Modeling

[Amazon System Design Preparation (SIP)](https://www.youtube.com/watch?v=gNQ9-kgyHfo)
[System Design Video](https://duaa2xs5z3ldn.cloudfront.net/assets/ae9Gg7INmujM-3uC_transcoded-Kn4JaKQ61tn_-btu-REACTION%20VIDEO%20FINAL.mp4?v=1)


**Topics to review:**

- Often times, software systems need software components, something to store data, something to make decisions (such as business logic) and APIs or processes.
    
- Knowledge of distributed systems, **SOA**, and **n-tiered software architecture** is very important in answering systems design questions. If you don’t work with these concepts regularly, be sure to review them.
    
- Also, be sure to practice drawing your system design by hand and be prepared to whiteboard.

- [ ] **Three coding competencies**
    
This is your chance to show off your knowledge and understanding of computer science fundamentals (e.g., hash maps, arrays, binary trees, algorithms). Your evaluation will include three areas of coding:
    
1. data structures and algorithms
2. logical and maintainable
3. problem-solving. 

## Logical and Maintainable

Lay out your code with functions, classes, to break things up into logical components. And that's much more important, I think, for logical and maintainable than trying to find the most optimal or best solution.

Most of the candidates, they struggle to take the interviewer as your colleague and explain their ideas.


"Okay I'm trying to think about what you said, I'm not arriving at a lightbulb moment. Can you phrase that hint in a different way?"

Logical and maintainable is really probably, maybe even the most important of the three coding competencies.

## Data structures and algorithms

Some of the most widely used techniques and methods which can be used in order to customize and improve the design of a system are:
1. redundancy
2. partitioning
3. caching
4. indexing
5. load balancing
6. queues

Ask the interviewer if they agree with them as you go. 

**Here are a few items about what to expect from your day:**

  
·      **Your technical/functional experience** within the job function. Attached is prep doc for **Software Development Engineer** at Amazon.  
·      **Your leadership potential:** We evaluate each of our candidates against a set of Amazon Leadership Principles so we’ll be asking you lot of “tell me about a time when” time questions.  We want to make sure that you can be successful at Amazon. Please visit this link to review our [Leadership Principles](https://www.amazon.jobs/principles) and check out this [video](https://youtu.be/CpcxVE5JIX4) on our LPs, too!    
  
**FOCUS on these Leadership Principles for your interview**: **1) Insist on the Highest Standards 2) Ownership 3) Earn Trust 4) Have Backbone; Disagree and Commit 5) Customer Obsession 6) Bias for Action 7) Are Right, A Lot 8) Deliver Results 9) Think Big 10) Invent and Simplify**   
      ·      **Hint**: I highly recommend printing out our Leadership Principles, and jotting down a few examples of stories you can speak to that relate to each! This will greatly help you in the "Tell me about a time when..." questions you'll get onsite.   
      ·      **Hint**: We ask a lot of follow-up questions! Don't be afraid to reflect and think about 'Looking back', 'What would you do differently?', 'Knowing what you know now...'         
·      **Whiteboarding:** During your onsite you will be asked to discuss how you would approach a problem and you will need to write code (no pseudo code). You can use AWSboard at [https://awwapp.com/#](https://awwapp.com/)).The link to the whiteboard app will be included in your confirmation email.   
  
**STAR**  
·      You will meet with several interviewers (One on One for 1 hour). Each interviewer will cover a specific topic with you – remember to employ the **STAR** Methodology (**S**ituation, **T**ask, **A**ction, **R**esult) when responding to your interview questions!   
      ·      **Hint**: We are a metrics-driven company - the more data-driven impacts/**results** you have - the better! (See attached STAR Method & Amazon’s Leadership Principles pdf doc).

**TIPS**

-   **Ask Clarifying Questions** - During your interview, remember to: ask clarifying questions, process out loud, and be vocally self-critical (Amazon takes risks, fails fast, and iterates quickly – we genuinely want to hear about situations where you’ve taken ownership and the lessons you’ve learned from “failures”)
-   **Prepare Questions for your Interviewers -** Come with a few questions of your own! We’re curious to hear what excites you about AWS
-   **“I” versus “We”** - We obviously want you to give credit where credit is due when explaining previous team efforts you’ve lead/been a part of, but please remember this is YOUR time to speak about your individual contributions and successes. Be aware of how you’re describing examples and lean toward using examples where you use “I” more than “we”. This will ensure you’re giving your main personal examples and the best data points for the team to draw from.
-   **Leadership Principles -** You’ll want to look at the leadership principles and think of specific examples to talk about through the conversation. Our [Amazon Leadership Principles](https://l.shatrk.com/sh-trk-112p68p/r/e/8VxJKHAoW13heK3Z?r=https://www.amazon.jobs/principles) are the fabric that make up Amazon’s culture.  They are also heavily relied upon in the interview process, so definitely take a moment to think of a couple anecdotes from your career for each one!  The more specificity you can give in your answers, around how and why you did what you did will serve you well. They want to see that you had a full view of the work you were doing.

**Coding**

You want to make sure your code is logical, without too many dependencies, and can be easily worked on by someone coming in behind you. The core fundamentals around algorithms and data structures still matter at this level. Be sure to think about edge cases, “Big O” Notation and worst-case scenarios, as all things at AWS will eventually scale to a large degree.  

-   **Clarify:** There will be live coding, so be prepared for that. You can ask clarifying questions, and even find out if the interviewer wants an optimal solution or just something that works.  Because time is limited, they may just want you to brute force something if you have to.
-   Code or script in the language you are most comfortable using. 
-   I’ve attached a document for coding and design tips to be aware of and to help you prep.

**Technical Direction and Ownership**

-   Be sure to call out the things that you have OWNED and where you set the technical direction for things like: the technology used on the team, the system design and architecture, how the team would be structured, etc. 
-   They will want to see this ownership as a constant theme in your experience. Highlight areas where you may have disagreed with ideas of others, but were able to find common ground and a path forward. 
-   Talk about your scope of influence, both inside and outside your team, as they will want to see how you influence decisions that affect not only your group, but others as well.
-   Details on the service/platform/teams you’ve led (questions around availability, complexity of service, interesting problems they dealt with, monitoring the service, etc.) **Highlight scope and scale of the work and systems you’ve had the chance to work in.**

**Mentoring and Developing Others**

-   Whether through code reviews or other mechanisms, talk about how you have been able to shape the growth and development of others on your teams.
-   Highlight specific examples as much as you can, where you have had a direct influence on the development of others.

**CODING (cont’d)**

Coding questions outside of system design will focus on **problem solving**, **logical and maintainable code**, and **data structures and algorithms.**

-   **Problem Solving:** We look at your ability to take a high level coding/technical question/problems, thinking through problems, identify alternatives, and weigh different options; you should be able to see potential roadblocks and challenges with your solutions.
    -   Consider these questions: 
        -   _How do you engage and approach a vague problem?_ 
        -   _What do you do when you’re solving a problem and get stuck?_ 
        -   _Run through your initial thoughts, gather requirements, determine and voice the most important piece of the problem, translate it to clean written code, find edge cases, etc_.
-   **Data Structures & Algorithms:** Focusing on your general CS fundamentals and ability to work effectively through data structures and algorithms. Sometimes starting with intentionally vague scenarios designed to observe your ability to work through a problem.
    -   Consider these questions: 
        -   _How well do you understand and apply data structures?_ 
        -   _Consider runtimes for common operations and understand how they use memory._ 
        -   _Understand data structures you would encounter in core libraries ie Trees, Hash Maps, Lists, Arrays, Queues, etc._
        -   _Understand common algorithms (i.e. traversal first, recursion)_
        -   _Discuss runtimes, theoretical limitations and algorithms_ 
-   **Logical & Maintainable:** While assessment of technical skills will happen throughout the entire interview process, this part is focused on making sure we receive a large enough chunk of code to assess that you can do the following: write code this is syntactically correct and does what it is intended to do; build maintainable and readable code; write idiomatic code
    -   Consider these questions: 
        -   _The code you write during your interview should be scalable, robust and well-tested_
        -   _We expect you to produce enough code for us to evaluate how your solution addresses the problem_
        -   _While your coding ask yourself, “If someone looked at this code in the future, would they be able to understand it without explanation, comments or documentation?”_

**Write _Production Quality_ Code!**

-   Write code as it would sit inside production.
-   Write REAL code, NOT pseudo-code. Write code that would be able to be compiled.
-   Write elegant code, NOT brute force code. (Is it simple? Is it effective? Is it efficient?)
-   Use clean coding syntax (Is it readable? Is it understandable?)
-   Use quality naming conventions (Does it make sense? Is it easy and obvious for someone to follow along or is it difficult to interpret?

**Coding/Algorithms/Data Structures – Practice Resources**

I wanted to include a few of these, as they tend to help with sharpening skills as they relate to coding practice, and algorithms & data structures. If it’s been a while, this would be a good set of resources to explore and practice with. 

**LeetCode** - [https://leetcode.com/](https://leetcode.com/)

**HackerRank** - [https://www.hackerrank.com/](https://www.hackerrank.com/)

**AlgoExpert** - [https://www.algoexpert.io/product](https://www.algoexpert.io/product)

   
**Additional Resources:**

Virtual Interviewing at Amazon: [https://www.amazon.jobs/en/landing_pages/virtual-interview-prep](https://www.amazon.jobs/en/landing_pages/virtual-interview-prep)

SDE Interviewing at Amazon: [https://www.amazon.jobs/en/landing_pages/software-development-topics](https://www.amazon.jobs/en/landing_pages/software-development-topics)

-   Video Amazon Leadership Principles: [https://youtu.be/CpcxVE5JIX4](https://youtu.be/CpcxVE5JIX4)
-   VIDEO Amazon Coding Sample: [https://youtu.be/mjZpZ_wcYFg](https://youtu.be/mjZpZ_wcYFg)
-   VIDEO Amazon System Design: [https://www.youtube.com/watch?v=gNQ9-kgyHfo](https://www.youtube.com/watch?v=gNQ9-kgyHfo)
-   VIDEO Onsite Interview Prep: [https://youtu.be/Vh20A2TMVKE](https://youtu.be/Vh20A2TMVKE)
-   VIDEO Leadership Principles: [https://youtu.be/CpcxVE5JIX4](https://youtu.be/CpcxVE5JIX4)

Here's a link to our [Benefits](https://www.amazon.jobs/en/landing_pages/benefitsoverview-us-copay)  
      ·      [Here](https://www.amazon.jobs/en/landing_pages/benefitsoverview-us) are benefits for states excluding CT/NC/UT and WI)  
      ·      Also, a review of our [Vacation/PTO](https://www.amazon.jobs/en/landing_pages/pto-overview-us) policies 

 I’ve had a number of candidates coming through that have found this to be useful. This is a former AWS Recruiter who runs a YouTube channel to help prepare people for their virtual onsite interview with AWS.

[https://www.youtube.com/c/AmazonInterviewWhizz/videos](https://www.youtube.com/c/AmazonInterviewWhizz/videos)

 **Amazon Articles (Optional Reading):**

-   [Amazon Earnings Numbers Show AWS Success](http://t.sidekickopen01.com/e1t/c/5/f18dQhb0S7lC8dDMPbW2n0x6l2B9nMJW7t5XZs3LPXXdW5vfR-61pNx_CW2B89Qs56dPDKf7V5Qh202?t=http%3A%2F%2Fsearchaws.techtarget.com%2Fopinion%2FAmazon-earnings-numbers-show-AWS-success-IaaS-growth&si=5624094474633216&pi=e1e14e21-f6ee-4084-9945-85d8ae08a882)
-   [Jeff Bezos’ Top Ten Leadership Lessons](http://t.sidekickopen01.com/e1t/c/5/f18dQhb0S7lC8dDMPbW2n0x6l2B9nMJW7t5XZs3LPXXdW5vfR-61pNx_CW2B89Qs56dPDKf7V5Qh202?t=http%3A%2F%2Fwww.forbes.com%2Fsites%2Fgeorgeanders%2F2012%2F04%2F04%2Fbezos-tips%2F&si=5624094474633216&pi=e1e14e21-f6ee-4084-9945-85d8ae08a882) 
-   [#11 on Forbes “Most Innovative Global Companies”](http://www.forbes.com/innovative-companies/list/#tab:rank)
-   [World's Most Admired Companies](http://t.sidekickopen01.com/e1t/c/5/f18dQhb0S7lC8dDMPbW2n0x6l2B9nMJW7t5XZs3LPXXdW5vfR-61pNx_CW2B89Qs56dPDKf7V5Qh202?t=http%3A%2F%2Fmoney.cnn.com%2Fmagazines%2Ffortune%2Fmost-admired%2F&si=5624094474633216&pi=e1e14e21-f6ee-4084-9945-85d8ae08a882)
-   [http://aws.amazon.com/blogs/aws/](http://t.sidekickopen01.com/e1t/c/5/f18dQhb0S7lC8dDMPbW2n0x6l2B9nMJW7t5XZs3LPXXdW5vfR-61pNx_CW2B89Qs56dPDKf7V5Qh202?t=http%3A%2F%2Faws.amazon.com%2Fblogs%2Faws%2F&si=5624094474633216&pi=e1e14e21-f6ee-4084-9945-85d8ae08a882)
-   [This chart shows why Amazon's cloud will be hard for Microsoft, Google, and everyone else to beat](http://www.businessinsider.com/aws-cloud-innovation-in-one-chart-2015-7)
-   [Amazon Web Services CEO Andy Jassy ‘optimistic’ AWS will become a $100B business, but no spinoff plans](http://www.geekwire.com/2016/amazon-web-service-ceo-andy-jassy-optimistic-aws-will-become-100b-business-no-spinoff-plans/)

   
**As a reminder:**  the attire here at Amazon is very casual. I recommend to not wear a suit. Feel free to dress in business casual attire if you desire.   
   
When you have a moment, please complete the candidate survey that will be sent to you following the onsite interview if you have time (Recruiter How’s My Driving).  Hiring is one of the most important things we do at Amazon to grow and scale our business and your feedback is valuable as we strive to improve the experience candidates have during the interview process.   
  
After your interview the team will huddle to discuss how you did during the process. I’ll be in touch within 5 business days either by phone or email to let you know the next step, which is typically a hire or no hire decision.  Good luck!

Thanks,

**Hamid Bangura**

## Call
* Moved here from Chicago 4 years ago.
* Lives in McKinney.
* Had unlimited PTO, employees abused it, he only got 2 weeks.

Keep 4 to 6 good stories that have depth. Go for depth over quantity.

STAR storytelling technique:
1.  **Situation**: An event, project, or challenge faced.
2.  **Task**: Your responsibilities and assignments for the situation.
3.  **Action**: Steps or procedure taken to relieve or rectify situation.
4.  **Result**: Results of actions taken.

3 types of [compensation](https://www.levels.fyi/companies/amazon/salaries/software-engineer/levels/sde-ii?limit=50):
* Base Salary
* Stock Grant (/yr)
* Signing Bonus

Stock compensation:
-   End of year one – 5% of the initial grant ($30,000)
-   End of year two – 15% ($90,000)
-   Six months later – 20% ($120,00)
-   End of year three 20% ($120,000) for a total of $240,000 in year 3
-   Six months later – 20% ($120,000)
-   End of year four – 20% ($120,000) for a total of $240,000 in year 4
-   Your total stock vests = $600,000

25 days vacation, an extra 5 after 6 months.

# Final Interview Tips from [Saji](https://www.youtube.com/watch?v=rAAmnKCU_hk&feature=youtu.be)

* Laptop, camera, and headsets are recommended.
* 4 rounds of interview (3 coding, 1 system design)

### Data structures

* Generic computer science fundamentals. Basic things.
* Are you able to work through data structures and algorithms?
* How well do you understand and apply data structures?
* Are you considering runtimes for common operations and understand how they use memory?

## Algorithms

* Divide and conquer
* When to use BFS vs. DFS.
* Are **you** discussing runtimes, theoretical limitations, basic implementations, strategies for different classes of algorithms.
* Before coding ask questions.
* Voice assumptions.
* Ask for examples.
* Ask them to validate their ideas.
* While coding explain decision making.
* Describe your code.
* Write syntactically correct code.
* Test the code or explain it.

### Problem Solving
### Logical
### Maintainable
### System Design

* Draw it out.
* Tradeoffs.
* Talk about advantages and disadvantages of your design.
* Operational performance.

#### What is the expectation for an L5?

* L5 will be owning software features within the team.
* Codes independently.
* Receives guidance on software design.
* Solving difficult software problems.

Most common leadership questionsg:
1.  Customer Obsession
2.  Ownership
3.  Invent and Simplify
4.  Are Right, A Lot
5.  Learn and Be Curious
6.  Insist on the Highest Standards
7.  Think Big
8.  Bias for Action
9.  Earn Trust
10.  Dive Deep
11.  Have Backbone; Disagree and Commit
12.  Deliver Results

Each round has 1 Technical Aspect and 2 Leadership Principle questions (7 minutes each).

Live code link and whiteboard link will be sent 48 hours prior to the interview. **Download everything ahead of time**.

* Keep a copy of your **resume** handy.
    
* Keep a **pen and paper** handy in case the whiteboard tool doesn't work.
    
* Keep a fully charged cell **phone** nearby.
    
* Keep a spare **headset** in case battery dies.
    
* Keep your **favorite drink** because you will be talking a lot.
    
* Stay in a **quiet place** where you won't be disturbed.
    
* Wear a **button down shirt**.
    
* Let them know if you need a bathroom break so you can be comfortable.

Within 5 business days of the interview, the interview team and the recruitment team has a conversation what was there experience with you on the interview date, and that is called debrief, and that's where they come to a result. The client lead will call to inform you of the results. He will not give feedback.

## Rule # 1: Ask clarifying questions.

This is the most important rule Saji emphasized, that she repeated over and over again.

Avoid awkward pauses and try to think out loud if possible. If you're not getting to an answer in 2 or 3 minutes, ask for help or a hint.

Mix success stories with failure stories. A lesson changes a failure into a success.

### You are the star

Say I, not we. It's awkward to talk about ourselves, but that is the expectation.

* Were you the key driver?
* What was your biggest contribution?
* What obstacles did you face?
* How did you overcome them?
* How did you influence the outcome?

### Keep eye contact during the introduction

### Speak clearly

I know you are good, so we don't have to worry about that. [17:53](https://youtu.be/rAAmnKCU_hk?t=1073)

The interviewers are listening, taking notes, and asking the follow-up questions all at the same time. They might miss certain things, so make sure things are done in a way that they don't miss anything.

### Data is crucial

Amazon is data-driven, so focus on quantitative or qualitative data in your stories.

* Numbers
* Money
* Process improved
* Time saved
* Tradeoffs
* What were the lessons?
* What would you do differently next time around?

## Problem Solving

* Ability to make a high level coding technical questions.
* How are you thinking through this?
* Identifying alternatives.
* Weighing different options.
* See potential roadblocks and challenges with your solutions.
* Run your initial thoughts.
* Gather requirements.
* Determine what is the most important piece of the problem.
* Are you able to translate them into clean code?
* Find edge cases.

They may give you a vague problem and see how you're approaching it. 

# First Interview with [Poojah](https://www.youtube.com/watch?v=Tu5o8My1ahY)

# Email Prep

Come prepared with questions that you would like to explore in more detail (e.g. initiatives/projects, team culture, scope of role).

[Amazon Chime Guide](https://www.amazon.jobs/en/landing_pages/amazon-chime-instructions)


-   Practice makes perfect! Interview prep for our technical roles is found [here](https://www.amazon.jobs/in-software-development-topics).
    
-   Learn more about our commitment to [Diversity and Inclusion at Amazon](https://www.amazon.jobs/en/landing_pages/diversity-and-inclusion) 
    
-   Read about one employee’s journey to [getting hired at AWS](https://www.linkedin.com/pulse/how-do-you-get-hired-aws-frank-boosman/).
    

  

The [Coding Assessment Demo](https://9n3bwgl2.r.us-west-2.awstrack.me/L0/https:%2F%2Fwww.hackerrank.com%2Ftest%2F63ek10mhil5%2F60d306ab105867d80fca544041154273/1/0201000047hr4dsd-nne7msoq-ve7m-qlv8-jt0k-05v9uip2ch80-000000/zqRdjQtZxAr8xHuPDQ7Lg74hd-A=287) is optional to help you get familiar with the platform before starting the actual assessment. You can use it as part of your preparation.

  

Here are some helpful links you can use to prepare:

-   CS Dojo YouTube channel is helpful for brushing up on Maps, Trees, Tables, and Algorithms. - [https://www.youtube.com/channel/UCxX9wt5FWQUAAz4UrysqK9A](https://www.youtube.com/channel/UCxX9wt5FWQUAAz4UrysqK9A)
    
-   Algorithms and Data Structures for interview preparation - 
    
    [https://www.youtube.com/watch?v=BchPukWb0CU](https://www.youtube.com/watch?v=BchPukWb0CU)
    
-   [https://www.hackerrank.com/challenges/tree-height-of-a-binary-tree](https://urldefense.proofpoint.com/v2/url?u=https-3A__www.hackerrank.com_challenges_tree-2Dheight-2Dof-2Da-2Dbinary-2Dtree&d=DQMGaQ&c=clK7kQUTWtAVEOVIgvi0NU5BOUHhpN0H8p7CSfnc_gI&r=cAeCaj04PjHfbmPIEFWCfA&m=ezaKG8zDhAvLG-Ia2cGHPBX-jbJBMtQAUJ2bjtbTn7E&s=B5QB5zK6KKHUS1br_lP3fiVk8cChsg8x8vCoyhVKsmM&e=)
    
-   [https://www.hackerrank.com/challenges/tree-level-order-traversal](https://urldefense.proofpoint.com/v2/url?u=https-3A__www.hackerrank.com_challenges_tree-2Dlevel-2Dorder-2Dtraversal&d=DQMGaQ&c=clK7kQUTWtAVEOVIgvi0NU5BOUHhpN0H8p7CSfnc_gI&r=cAeCaj04PjHfbmPIEFWCfA&m=ezaKG8zDhAvLG-Ia2cGHPBX-jbJBMtQAUJ2bjtbTn7E&s=9VQn-kHZ9K5iF9I9mfJhGEX-aBZszOwhxruK8fyfz_s&e=)
    
-   [https://www.hackerrank.com/challenges/balanced-brackets](https://urldefense.proofpoint.com/v2/url?u=https-3A__www.hackerrank.com_challenges_balanced-2Dbrackets&d=DQMGaQ&c=clK7kQUTWtAVEOVIgvi0NU5BOUHhpN0H8p7CSfnc_gI&r=cAeCaj04PjHfbmPIEFWCfA&m=ezaKG8zDhAvLG-Ia2cGHPBX-jbJBMtQAUJ2bjtbTn7E&s=di_uXWsZirwuEx3v_aM0RBNNhe1CGIWpmfTurqGc0Wg&e=)
    
-   [https://www.hackerrank.com/challenges/contacts](https://www.hackerrank.com/challenges/contacts)
    
-   [https://www.hackerrank.com/challenges/find-the-running-median](https://www.hackerrank.com/challenges/find-the-running-median)
    
-   [https://www.hackerrank.com/challenges/swap-nodes-algo](https://www.hackerrank.com/challenges/swap-nodes-algo)
    

  

The next step will be a Technical Phone Screen. Please find some information below to help you prepare for your Technical Phone Interview. Remember, this will be 1 hour in length (half technical, half behavioral and likely a live coding exercise). Please feel free to reach out to me at any time should you have any questions. I’m here to help!


**1. Technical/coding and design questions:**These questions will test your ability to solve a technical problem. If you are rusty, you should study up on:

- [ ]   Trees (binary/red black trees/N-ary trees, tree symmetric (inverse image), tree traversal, sorting trees)
    
	- [ ] Iterative Depth First Search
    
    - [ ] Recursive Depth First Search
    
    - [ ] Breadth First Search
    
- [ ]   hash maps, stacks/queues/heap/priority que
    
- [ ]   link lists/reverse link lists (space vs time)
    
- [ ]   recursion/recursive strings
     
	 - [ ] Base case
	 
- [ ]   binary search
    
- [ ]   Arrays - 2d/3d
    
- [ ]   graphs/graph traversal
    
- [ ]   Big O notation


**Some tools you might find useful to study up for the coding portion include:**

-   [https://leetcode.com/](https://leetcode.com/)
    
-   [https://www.hackerrank.com/](https://www.hackerrank.com/)
    
-   [https://www.amazon.com/Elements-Programming-Interviews-Insiders-Guide/dp/1479274836](https://www.amazon.com/Elements-Programming-Interviews-Insiders-Guide/dp/1479274836)
    
-   [https://www.amazon.com/Cracking-Coding-Interview-Programming-Questions/dp/098478280X](https://www.amazon.com/Cracking-Coding-Interview-Programming-Questions/dp/098478280X)
    
-   [https://aws.amazon.com/architecture/](https://aws.amazon.com/architecture/) – these are the tools that we use to develop amazon services/products.
    

**2. Behavioral questions using our [https://www.amazon.jobs/principles](https://www.amazon.jobs/principles)**

For the behavioral questions, you’ll want to read through the Leadership Principles to become familiar with them (you don’t need to memorize them). Have a few specific examples prepared that you can potentially use to address the questions. It’s good to think of examples regarding projects you’ve done at work, teams you’ve worked with, and products you’ve shipped/delivered (don’t share anything that is considered confidential).

  

**Useful links:**

**[Amazon’s Leadership Principles](https://www.amazon.jobs/en/principles)***Highly Recommend at this stage**

**[Interviewing at Amazon](https://www.amazon.jobs/en/landing_pages/interviewing-at-amazon)***Highly Recommend at this stage**

[Prepping for your Online Assessment](https://www.amazon.jobs/en/landing_pages/assessments)

**[Prepping for your Phone Interview](https://www.amazon.jobs/en/landing_pages/phone-interview)***Highly Recommend at this stage**

[Prepping for your On-Site Interview](https://www.amazon.jobs/en/landing_pages/in-person-interview)

[SDE Interviewing at Amazon](https://www.amazon.jobs/en/landing_pages/software-development-topics)*****Highly Recommend at this stage**

[https://youtu.be/CpcxVE5JIX4](https://youtu.be/CpcxVE5JIX4)*****Highly Recommend at this stage**

[https://youtu.be/mjZpZ_wcYFg](https://youtu.be/mjZpZ_wcYFg)*****Highly Recommend at this stage**

[https://youtu.be/gNQ9-kgyHfo](https://youtu.be/gNQ9-kgyHfo)*****Highly Recommend at this stage**

[https://www.youtube.com/watch?v=Q2YyN9qbpD8](https://www.youtube.com/watch?v=Q2YyN9qbpD8)

[https://www.youtube.com/watch?v=DERzYnthq1s&list=WL&index=26](https://www.youtube.com/watch?v=DERzYnthq1s&list=WL&index=26)

[AWS’ Homepage](https://aws.amazon.com/)

[What is Cloud Computing?](http://aws.amazon.com/what-is-cloud-computing/?nc1=h_l2_cc)

[https://www.youtube.com/watch?v=GC3bWcFFZTo&feature=youtu.be](https://www.youtube.com/watch?v=GC3bWcFFZTo&feature=youtu.be)

[AWS' YouTube Page](https://www.youtube.com/user/AmazonWebServices)

[Amazon Employee Benefits](https://www.amazon.jobs/en/landing_pages/benefitsoverview-us)

[About Amazon](https://www.amazon.jobs/en/landing_pages/about-amazon)

[Set up your free AWS account here](https://portal.aws.amazon.com/billing/signup?nc2=h_ct&src=header_signup&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start)

[https://www.youtube.com/watch?v=RY9ERVyNsyI](https://www.youtube.com/watch?v=RY9ERVyNsyI)

**Amazon Articles (Optional Reading):**

[What Is Cloud Computing?](https://aws.amazon.com/what-is-cloud-computing/)

[Analyst Reports](http://t.sidekickopen01.com/e1t/c/5/f18dQhb0S7lC8dDMPbW2n0x6l2B9nMJW7t5XZs3LPXXdW5vfR-61pNx_CW2B89Qs56dPDKf7V5Qh202?t=https%3a%2f%2faws.amazon.com%2fresources%2fanalyst-reports%2f%3fsc_ichannel%3dHA%26sc_ipage%3dhomepage%26sc_iplace%3deditorial_r1_left_banner%26sc_icampaigntype%3dgeneral_info%26sc_icampaign%3dha_gartner_iaas_mq_2013%26sc_icountry%3dUS&si=5624094474633216&pi=1566caac-5b22-4f0f-a938-39ea0cb78bd5)

[AWS Revenue Is Bigger Than Its Four Closest Competitors Combined](http://t.sidekickopen01.com/e1t/c/5/f18dQhb0S7lC8dDMPbW2n0x6l2B9nMJW7t5XZs3LPXXdW5vfR-61pNx_CW2B89Qs56dPDKf7V5Qh202?t=http%3A%2F%2Fwww.businessinsider.com%2Faws-revenue-is-bigger-than-its-four-closest-competitors-combined-2015-4&si=5624094474633216&pi=e1e14e21-f6ee-4084-9945-85d8ae08a882)

[Amazon Earnings Numbers Show AWS Success](http://t.sidekickopen01.com/e1t/c/5/f18dQhb0S7lC8dDMPbW2n0x6l2B9nMJW7t5XZs3LPXXdW5vfR-61pNx_CW2B89Qs56dPDKf7V5Qh202?t=http%3A%2F%2Fsearchaws.techtarget.com%2Fopinion%2FAmazon-earnings-numbers-show-AWS-success-IaaS-growth&si=5624094474633216&pi=e1e14e21-f6ee-4084-9945-85d8ae08a882)

[America's Best CEO's](http://t.sidekickopen01.com/e1t/c/5/f18dQhb0S7lC8dDMPbW2n0x6l2B9nMJW7t5XZs3LPXXdW5vfR-61pNx_CW2B89Qs56dPDKf7V5Qh202?t=http%3A%2F%2Fwww.forbes.com%2Fsites%2Fscottdecarlo%2F2012%2F04%2F04%2Famericas-best-ceos%2F&si=5624094474633216&pi=e1e14e21-f6ee-4084-9945-85d8ae08a882)

[Jeff Bezos’ Top Ten Leadership Lessons](http://t.sidekickopen01.com/e1t/c/5/f18dQhb0S7lC8dDMPbW2n0x6l2B9nMJW7t5XZs3LPXXdW5vfR-61pNx_CW2B89Qs56dPDKf7V5Qh202?t=http%3A%2F%2Fwww.forbes.com%2Fsites%2Fgeorgeanders%2F2012%2F04%2F04%2Fbezos-tips%2F&si=5624094474633216&pi=e1e14e21-f6ee-4084-9945-85d8ae08a882)

[#11 on Forbes “Most Innovative Global Companies”](http://www.forbes.com/innovative-companies/list/#tab:rank)

[World's Most Admired Companies](http://t.sidekickopen01.com/e1t/c/5/f18dQhb0S7lC8dDMPbW2n0x6l2B9nMJW7t5XZs3LPXXdW5vfR-61pNx_CW2B89Qs56dPDKf7V5Qh202?t=http%3A%2F%2Fmoney.cnn.com%2Fmagazines%2Ffortune%2Fmost-admired%2F&si=5624094474633216&pi=e1e14e21-f6ee-4084-9945-85d8ae08a882)

[http://aws.amazon.com/blogs/aws/](http://aws.amazon.com/blogs/aws/)

[This chart shows why Amazon's cloud will be hard for Microsoft, Google, and everyone else to beat](http://www.businessinsider.com/aws-cloud-innovation-in-one-chart-2015-7)

[Amazon Web Services CEO Andy Jassy ‘optimistic’ AWS will become a $100B business, but no spinoff plans](http://www.geekwire.com/2016/amazon-web-service-ceo-andy-jassy-optimistic-aws-will-become-100b-business-no-spinoff-plans/)

  

**Here a few Overall Tips to be best prepared for the phone interview:**

-   Remember to ask clarifying questions—treat the interviewer as you would a customer. Do you have all the information needed to go ahead and solve the posed questions? Amazonians are customer obsessed so always work backwards from what the customer needs are.
    
-   Is there any assumptions that need to be confirmed? If so confirm them before moving forward, but also act when you have enough info.
    
-   They would love you to dive as deep as you can go. Remember to answer the question, and think out loud. In some cases you may have a live link to sketch out your answer but generally its by phone. Make sure you give the interviewer your full attention—don’t have interruptions or don’t type or search the internet for answers, be open and honest about what you know or don’t know and do your best. We want someone that can interface with customers honestly so it is an important part of the phone interview to listen and try to answer.
    
-   The screener will try and get a sense of your ability and scale of experience. This phone screen determines the level of your experience either an Ops Engineer II or Ops Engineer III, or could recommend another role.
    
-   Both roles are critical, so don’t worry, but I’ve put some of the key parts of the roles below so you can think about solving problems in the area of OS from Front end through to back, problem solving and including storage, network bottlenecks, scale, capacity management, compatibility, future loads….In other words Best Practice.
    
-   Anything on your resume is FAIR GAME for an interview. Please make sure you can speak to your experience on your resume.
