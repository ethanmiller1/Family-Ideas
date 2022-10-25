* Earn trust: Tell me about a time when you uncovered a significant problem in your team. How did you **communicate it to your manager**? [[Software Stories#Logging PCI information (CC data)|PCI Data]]
* Earn trust: Give me an example of a tough or critical piece of **feedback** you received. [[Software Stories#Mentorship|Mentorship]]
* Deliver results: Tell me about a time when you delivered an important project under a **tight deadline**. [[Software Stories#System Log Drain|System Log Drain eCaas to Logstash]]
* Deliver results: Tell me about a time when you had significant, **unanticipated obstacles** to overcome in achieving a key goal. [[Software Stories#Memory watcher|Memory Watcher]]
* Have backbone; disagree and commit: Describe a time when **you felt strongly** about something on a project, but **the team decided to go in a different direction**.  [[Software Stories#Performance Testing|Performance Testing]] | [[Software Stories#Cucumber|Cucumber]]
* Ownership: Tell me about a time when you took on something significant **outside your area of responsibility**. [[Software Stories#JsonSubTypes|JsonSubTypes]]
* Learn and be curious: What was the coolest thing you learned on your own that has helped you better perform your job?
* Insist on the highest standards:  Tell me about a time when you have been **unsatisfied with the status quo**. What did you do to change it?

[16 Leadership Principles](https://www.aboutamazon.com/about-us/leadership-principles)

## Customer Obsession
```
Leaders start with the customer and work backwards. They work vigorously to earn and keep customer trust. Although leaders pay attention to competitors, they obsess over customers.
```

Everything we do ultimately comes back to the goal of creating the best costumer experience we can. And on the back end of things, that's expressed in two way: (1) Making our system as **fast** as possible, and (2) making our systems as **reliable** as possible. And so when we're designing the service flow of how to fulfill a particular product, maybe they want to 10 prepaid bags, we want to look at, "How can I minimize the network waiting time?" Maybe that's finding ways to call our travel reservation system (Sabre Web Service) fewer times, or if an upstream service makes a call to get information about a reservation, can we cache that response somewhere so the downstream services don't can to repeat that call. We're always trying to optimize our SLOs so that we can provide the best possible customer experience in our portion of the product.

* Caching reservation response, forceRefreshIdicator
* Meticulous testing, full scenario coverage as opposed to code coverage
* Reducing network waiting time with fewer calls to reservation system.
* Rolling update deployment strategy
* Canary Deployments
* Verbose logs
* Robust monitoring and alerting

---
What we do is collect SLIs through application surveys and form objectives based on customer feedback. The great thing about our product owners is they are open to having conversations about what we think customers will want, and then we confirm that with data given to us by various surveys.

For example we have a product called **Extended Hold** where a customer isn't sure they want to buy the ticket, so they pay $50 to reserve it for 3 days or 7 days. We have a system that notifies the customer 1 day before the expiration. That application belonged to my team but was going to be given to another team, and the product owner decided he didn't want to spend the effort doing that. So we opened up a conversation with him to talk about why this is important for the customer, and he changed his mind. Mark Blessing was his name.

* Emails.
* Feedback at airports.
* Fighting with product owners.

## Ownership
```
Leaders are owners. They think long term and don’t sacrifice long-term value for short-term results. They act on behalf of the entire company, beyond just their own team. They never say “that’s not my job".
```

This is something dear to my heart, both with writing the code, and thinking through what are the best design patterns to ensure its maintainability as well as taking ownership of the product when channels contact us with issues in testing. When we're on call I see a lot of, "Of that's AOL's problem, can you look into that?" And we had a big shift in emphasizing, "We're all one product." If I get a call, and I see, 'ROL001' error code, and that's not me, I'm still capable of investigating the logs, looking at what went wrong, and reporting on it.

I think about some of the **microservices I've written as my babies**. They keep getting better and cleaner, whether I've learned about a new annotation, design pattern, or a **log interceptor**, or building stubs out to avoid unnecessary dependencies,

## Invent and Simplify
```
Leaders expect and require innovation and invention from their teams and always find ways to simplify. They are externally aware, look for new ideas from everywhere, and are not limited by “not invented here". As we do new things, we accept that we may be misunderstood for long periods of time.
```

-   Cucumber reflection.
-   Terminal API Request Faceted Builder.
-   Log Interceptor.

## Are right, A Lot
```
Leaders are right a lot. They have strong judgment and good instincts. They seek diverse perspectives and work to disconfirm their beliefs.
```


Rancher migration with Uday.
Config Maps with Bilal.

## Learn and Be Curious
```
Leaders are never done learning and always seek to improve themselves. They are curious about new possibilities and act to explore them.
```

Every Friday we have a learning journey, and that time is pointed specifically at, "What can I learn to improve the way we do things on the team?" So maybe you learn about Comparators, or design patterns, or Sabre Web Services, or Priority Queues, or Docker, or Kubernetes. I have asked 22 questions on Stack Overflow, and in some of those questions I've uncovered bugs in a framework. I worked with one of the developers of Quarkus about an issue with their Spring-Data-Rest library.

## Hire and Develop the Best
```
Leaders raise the performance bar with every hire and promotion. They recognize exceptional talent, and willingly move them throughout the organization. Leaders develop leaders and take seriously their role in coaching others. We work on behalf of our people to invent mechanisms for development like Career Choice.
```

I interviewed several candidates to work on our team, and one of the biggest things I look for is communication, because if a developer can communicate, than he can learn very quickly. In one year one of the guys I hired fresh out of college was an intermediate developer.

## Insist on the Highest Standards
```
Leaders have relentlessly high standards - many people may think these standards are unreasonably high. Leaders are continually raising the bar and driving their teams to deliver high quality products, services and processes. Leaders ensure that defects do not get sent down the line and that problems are fixed so they stay fixed.
```

This is something that we revisit a lot, and that is testing and code review. We also keep eyes on the glass after every production deployment, because no amount of CI/CD wil replace that.

## Think Big
```
Thinking small is a self-fulfilling prophecy. Leaders create and communicate a bold direction that inspires results. They think differently and look around corners for ways to serve customers.
```

I think the best developers have the big picture in mind. It's not just a User Story, it's something that we can trace back to a feature that improves the user experience.

## Bias for Action
```
Speed matters in business. Many decisions and actions are reversible and do not need extensive study. We value calculated risk taking.
```

At my first consulting company they had me take Gallup's Strength Finders, and my number two strength was "Activator."

#### Activator
##### SHARED THEME DESCRIPTION
People who are especially talented in the Activator theme can make things happen by **turning thoughts into action**. They are often impatient.

#### YOUR PERSONALIZED STRENGTHS INSIGHTS

What makes you stand out?

By nature, you excel at launching new projects, assignments, jobs, and other activities. Many times, **you are the person who motivates people to begin tasks**. You prefer difficult-to-reach goals. You prefer to take charge. **You forge ahead rather than wait for someone in authority to give you permission to start**. Instinctively, you probably stimulate individuals to be more enthusiastic about what they are doing. How? You repeatedly remind them that you truly value their contributions, talents, experience, and expertise. It’s very likely that **you compel individuals or groups to quickly move into action by using official-sounding words**. Your extensive vocabulary also contains hard-to-understand terminology that sets you apart. Thus, you speak with an air of authority that you often fail to hear. So, when you display impatience and say, “Let’s get started right now!” many **people refrain from questioning your decision**. Driven by your talents, **you have a gift for raising a person’s self-esteem**. You notice something special he or she said. Then you formally or informally **acknowledge the individual’s most interesting ideas**. Because of your strengths, **you regularly energize people with your ideas** about what can be changed or done better. You frequently describe how individuals or groups can benefit from your suggestions. Your optimistic approach is apt to inspire people to design improvement plans. You probably rally individuals to support and execute those plans.

## Frugality
```
Accomplish more with less. Constraints breed resourcefulness, self-sufficiency and invention. There are no extra points for growing headcount, budget size or fixed expense.
```

* DevTest into ReadyAPI.
* ReadyAPI into Cucumber.
* Least number of Sabre calls.
* Retry Mechanisms.

## Earn Trust
```
Leaders listen attentively, speak candidly, and treat others respectfully. They are vocally self-critical, even when doing so is awkward or embarrassing. Leaders do not believe their or their team’s body odor smells of perfume. They benchmark themselves and their teams against the best.
```

This is probably where I excel the most, which is why our architect wanted to build a rapport with me because he noticed everybody comes to me for help, or just to chat, and every time we're in the office my desk is crowded with people. Which believe me is a blessing and a curse, but mostly a blessing.

Franklin Covey wrote a book called the Speed of Trust that my first company made us read, where the author outlines 13 behaviors of trust.

1.  Talk Straight    
2.  Demonstrate Respect    
3.  Create Transparency    
4.  Right Wrongs    
5.  Show Loyalty    
6.  Deliver Results    
7.  Get Better    
8.  Confront Reality    
9.  Clarify Expectations    
10.  Practice Accountability    
11.  Listen First    
12.  Keep Commitments    
13.  Extend Trust

And to this day I am very happy that my career started out with these principles, because they have played a huge role in my technical success, but also in my success with people, which are not separate domains. Some of the best technical learnings I have had were from my relationship with very smart people who likes having me around because I learned early on from some very amazing people how to foster a culture of trust.

For example there's a guy named Rubens, and he talks a lot, kind of slowly, but he's really smart, and I say they were not treating him very well, so I cut our lead off at one point and said, "To Rubens point," and filled in the blanks, and ever since then Rubens always liked having me around, and I learned a ton from him.

I was working on a cloud migration from epaas to Rancher, and our DevOps guy left, and he gave me the functional account and the API keys for all of our connection services, container registries, that we use to deploy apps, and I became a very important player just because I was friends with a guy who knew a lot. He told me he trusted me with them because of how much effort and diligence I put into the work we did together.

## Dive Deep
```
Leaders operate at all levels, stay connected to the details, audit frequently, and are skeptical when metrics and anecdote differ. No task is beneath them.
```

This goes along with being curious.

## Have Backbone; Disagree and Commit
```
Leaders are obligated to respectfully challenge decisions when they disagree, even when doing so is uncomfortable or exhausting. Leaders have conviction and are tenacious. They do not compromise for the sake of social cohesion. Once a decision is determined, they commit wholly.
```

* Faceted Builder Pattern.
* GitOps Approach.
* Prometheus.
* Logging using FluentD rather than MiNiFi.

## Deliver Results
```
Leaders focus on the key inputs for their business and deliver them with the right quality and in a timely fashion. Despite setbacks, they rise to the occasion and never settle.
```

  
Really everything we do comes down to what we deliver. And that's why it's important to really clarify the expectations of what we are building, because we want to make sure that we give accurate estimations, and that we give what is expected.

## Strive to be Earth's Best Employer
```
Leaders work every day to create a safer, more productive, higher performing, more diverse, and more just work environment. They lead with empathy, have fun at work, and make it easy for others to have fun. Leaders ask themselves: Are my fellow employees growing? Are they empowered? Are they ready for what's next? Leaders have a vision for and commitment to their employees' personal success, whether that be at Amazon or elsewhere.
```

Every team I've been on is the best team that I've been on. It's so easy to create synergy by first creating energy. Humor goes a long way in teams. My manager was perplexed, he said, "You guys are the most efficient team in T&R, and every standup you guys spend time talking about your personal lives." I told him, "We spend more waking hours with our co-workers than with our families. We will be a lot more productive when we spend time getting to know each other, and when we like each other. We work so well as a team."

## Success and Scale Bring Broad Responsibility
```
We started in a garage, but we're not there anymore. We are big, we impact the world, and we are far from perfect. We must be humble and thoughtful about even the secondary effects of our actions. Our local communities, planet, and future generations need us to be better every day. We must begin each day with a determination to make better, do better, and be better for our customers, our employees, our partners, and the world at large. And we must end every day knowing we can do even more tomorrow. Leaders create more than they consume and always leave things better than how they found them.
```

Amazon is huge. And we don't just deliver packages. We deliver technical solutions. I do believe that it is truly good for the world.

* Tripple Bottom line.
* Ikegi.