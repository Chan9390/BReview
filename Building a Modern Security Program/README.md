## Building a Modern Security Program - Zane Lackey with Rebecca Huehls

The TOC:

**1. Shifting the Security Team to a DevOps Mindset**

- How DevOps and the Cloud Change the Challenges Security Teams Face
- The Problems with Waterfall
- Developing and Iterating on Production: A Perspective Shift
- Focusing on Mean Time to Reaction

**2. The Future of Development and Security**

- Tools for Continuous Development
  - Feature Flags
  - Ramp Ups
  - A/B Testing
- Instrumentation and Visibility to the Organization
  - Make Information Available for Everyone, Not Just the Security Team
  - Change Binary Thinking About Threats
- Access Control

**3. The Keys to an Effective Security Culture**

- Communicate with Empathy (aka Don’t Be a Jerk)
- Make Realistic Trade-offs
- Explain a Vulnerability’s Impact Without Jargon
- Reward Communication with the Security Team
- Take the False-Positive Hit Yourself
- Scale via Team Leads

**4. Building a New Feedback Loop by Starting a Bug-Bounty Program**

- The Concerns about Bug-Bounty Programs
- The Goals of a Bug-Bounty Program
- Launching a Bug-Bounty Program
  - Provide Specific Guidelines and Processes
  - Record Expectations and Goal-Based Metrics
  - Inform All Teams Before the Bug-Bounty Launch
  - Review Helper Systems for Scaling Problems
  - Attacks Will Begin Almost Immediately
- Communicating with Researchers

**5. Obtaining Better Feedback by Shifting from Penetration Testing to Attack Simulations**

- Attack Simulations versus Pen-Testing
- Laying the Groundwork for an Attack Simulation
  - Goals
  - Full Organization in Scope
- Conducting the Attack Simulation
  - Simulate Realistic Compromise Patterns
  - Simulate Varied Attack Profiles
  - Break the Simulation into Iterations
- Attack Simulation Output
- Conclusions

----

### 1. Shifting the Security Team to a DevOps Mindset

- most of the classic approaches to security simply weren’t going to survive in DevOps environment
- Remember,
  - Application and Infra changes now happen weekly, daily or even in the span of minutes
  - More into DevOps, more people having access to production
  - Risk shifted from infra level to app level
- Attack-Driven Defense model - defensive efforts align with how your attackers are likely to actually attack you.
- Each developer has become their own QA department, security department, and performance team.
- In a DevOps model, the techniques used include feature flags, ramp ups, and A/B testing to test the code and slowly release new features to users.

> No matter which development methodology you have, **vulnerabilities occur in all of them**. But, with its focus on allowing frequent deployments to production, only DevOps gives you the speed to react to those vulnerabilities faster than your attackers.

- a security team in a DevOps environment ***needs to focus on the ability to respond quickly***
- Releasing the patch is just another deploy
- **Lesson:** the goal of a modern security team adapts from being a gatekeeper to focusing on making the rest of the organization security self-sufficient.

### 2. The Future of Development and Security

- Three important things
  - the security team needs to understand the techniques developers use to make all of those code-deploys 
  - instead of the security team holding onto its specialized knowledge, the team needs to provide leadership that encourages folks throughout the organization to think about security day in and day out
  - security team needs an approach to access control that still enables everyone to do their job
- **Feature Flags** : instead of adding a feature after completely developing, you begin developing the feature *on production* which is like a IF condition : "If this is enabled, then execute this code". When the flag is off, the code isn’t actually running, and that’s how developers are able to safely commit large amounts of code to production.
- **Ramp Up** : The ramp up is when a small group of users start to see the feature. If any corner-case bugs, feature flag set to off and fix the problem.
- **A/B Testing** : With an A/B test, users see different versions of the site or service with the goal to determine which version of a feature does a better job of meeting some business goal.
- ongoing security threats - visible to the organization. With instrumentation, every employee can obtain insight into how the business is performing.
- Make Information Available for Everyone, Not Just the Security Team
- You can take away their access but dont take away their capabilities. Methodology:
  - Determine what capability is needed. This might be the developers’ ability to view logs or deploy code to production.
  - Build an alternate way to perform the needed function in a safe way.
  - Transition the organization over to the safe way.
  - Create an alert when someone uses the old unsafe way or shut off access to the unsafe way altogether.
- Example: Creating central logging stops the developers from SSHing to the production servers to look at the logs

### 3. The Keys to an Effective Security Culture

- incentivize other teams to reach out to the security team
- Communicate with Empathy to the developers.
- Make Realistic Trade-offs. Not every vulnerability is critical. Try to prioritize the vulnerabilities and patch them immediately / when possible depending on the severity.
- Explain a Vulnerability’s Impact Without Jargon. Explain the following:
  - What attackers can get out of the vulnerability
  - How attackers can exploit the vulnerability 
- Reward Communication with the Security Team
- Take the False-Positive Hit Yourself. *Don’t send unverified issues to the dev and ops teams*. Workflow: Run a report -> Filter out false positives -> Try to patch issues -> Ask Dev or Ops to fix
- Scale via Team Leads. Build relationships with team leads. Its better than trying to train all the teams who are hiring and growing faster.

### 4. Building a New Feedback Loop by Starting a Bug-Bounty Program

- Reasons for having a bug bounty program:
  - Bug bounties give you a real-time and ongoing feedback loop that highlights where your security program is succeeding and where it is failing.
  - Bug bounties provide an avenue and incentive for researchers to report serious vulnerabilities they previously might not have shared
- 2 major concerns: budget and increasing the risk of attacks
- You can start with a Hall of Fame and responsible disclosure program if you think you dont have enough budget
- The Goals of a Bug-Bounty Program
  - Incentivizing researchers to report issues to you
  - Validating where your security program is working and where it isn’t
  - Increasing attackers’ costs for vulnerability discovery and exploitation
- Launching a BBP
  - Provide Specific Guidelines and Processes
  - Record Expectations and Goal-Based Metrics
    - Number of bugs
    - Severity of each bug
    - Time to Remediation and Time to Triage
  - Inform All Teams Before the Bug-Bounty Launch
  - Review Helper Systems for Scaling Problems. (BBP brings huge traffic, be ready for that)
  - Attacks Will Begin Almost Immediately
- Communicating with Researchers. If the patch takes more time, be proactive and send updates to the researcher. This avoids the researcher having a negative feeling about the BBP.

### 5. Obtaining Better Feedback by Shifting from Penetration Testing to Attack Simulations

- Attack Simulations versus Pen-Testing
  - Pentest - enumerate a list of vulnerabilities
  - Attack simulations - provides insights into an attacker’s decision-making process
  - Just as the bug-bounty program is the feedback loop in your application security program, attack simulation is the feedback loop in your network security program
- Laying the groundwork for an Attack Simulation
  - Enumerate likely attacker goals
  - Make sure the full organization is in scope for the attack

> One of the most effective ways to mitigate the political and technical risk of running an attack simulation is having the attack team call a cutout (mutually trusted intermediary) if the team is about to do something risky

- Conducting the Attack Simulation
  - Simulate Realistic Compromise Patterns (ex: SQLi or RCE on DB)
  - Simulate Varied Attack Profiles
    - stealth level attack profile : maintain persistence and watch for a future event
    - skill level attack profile : dont care if you are detected, just grab the sensitive data and leave
  - Break the Simulation into Iterations
- Attack Simulation Output. (Have notes on why some decisions were taken and which part of the system the attackers have tested)

> The best way to continuously refine this visibility is to embrace new methods for feedback, such as attack simulations at the infrastructure level and bug bounties at the application level. 

**Ultimately, a successful modern security program is one that makes the rest of the organization security self-sufficient.**
