# Agile Methodology

## Overview

We practice Agile software development with a Scrum framework. This approach allows us to deliver value iteratively, respond to change quickly, and continuously improve our processes.

## Scrum Framework

### Roles

**Product Owner**
- Defines product vision and roadmap
- Manages and prioritizes product backlog
- Defines acceptance criteria
- Makes decisions on scope and priorities
- Available to answer team questions

**Scrum Master**
- Facilitates Scrum ceremonies
- Removes impediments
- Coaches team on Agile practices
- Protects team from distractions
- Ensures process adherence

**Development Team**
- Cross-functional and self-organizing
- Typically 5-9 members
- Collectively responsible for delivery
- Estimates and commits to work
- Delivers working software each sprint

### Sprint Cycle

We work in **2-week sprints**:

```
Week 1          Week 2
Mon-Fri         Mon-Fri
├─ Sprint Planning
├─ Daily Standups (every day)
├─ Development & Testing
├─ Sprint Review
└─ Sprint Retrospective
```

## Scrum Ceremonies

### 1. Sprint Planning

**When**: First day of sprint  
**Duration**: 2-4 hours  
**Participants**: Entire Scrum team

**Agenda:**
1. Review sprint goal
2. Review and refine backlog items
3. Team selects items for sprint
4. Break down items into tasks
5. Estimate effort
6. Commit to sprint goal

**Outputs:**
- Sprint goal
- Sprint backlog
- Task breakdown
- Team commitment

### 2. Daily Standup

**When**: Every working day  
**Duration**: 15 minutes  
**Participants**: Development team + Scrum Master

**Three Questions:**
1. What did I complete yesterday?
2. What will I work on today?
3. Are there any blockers?

**Guidelines:**
- Stand up (if in person)
- Keep it brief and focused
- Raise blockers, don't solve them
- Follow up on detailed discussions after
- Update task board during standup

### 3. Sprint Review

**When**: Last day of sprint  
**Duration**: 1-2 hours  
**Participants**: Scrum team + stakeholders

**Agenda:**
1. Demo completed work
2. Gather feedback
3. Update product backlog based on feedback
4. Discuss what was and wasn't completed
5. Review metrics (velocity, burndown)

**Guidelines:**
- Show working software, not slides
- Focus on user value delivered
- Encourage stakeholder participation
- Be honest about what wasn't completed

### 4. Sprint Retrospective

**When**: After sprint review  
**Duration**: 1-1.5 hours  
**Participants**: Scrum team only

**Format:**
1. **What went well?** (keep doing)
2. **What didn't go well?** (stop doing)
3. **What can we improve?** (start doing)
4. **Action items** (specific improvements)

**Guidelines:**
- Create safe environment
- Focus on process, not people
- Be specific and actionable
- Follow up on previous action items
- Rotate facilitator

### 5. Backlog Refinement

**When**: Mid-sprint  
**Duration**: 1-2 hours  
**Participants**: Product Owner + selected team members

**Activities:**
- Review upcoming backlog items
- Add details and acceptance criteria
- Estimate complexity
- Split large items
- Remove outdated items

## User Stories

### Format

```
As a [type of user]
I want [goal/desire]
So that [benefit/value]
```

### Example

```
As a registered user
I want to reset my password via email
So that I can regain access if I forget my password
```

### Acceptance Criteria

Use **Given-When-Then** format:

```
Given I am on the login page
When I click "Forgot Password"
Then I should see a password reset form

Given I submit a valid email address
When I click "Send Reset Link"
Then I should receive a password reset email within 5 minutes
```

### Definition of Ready

A story is ready for sprint when it has:
- [ ] Clear user story format
- [ ] Defined acceptance criteria
- [ ] Estimated by team
- [ ] Dependencies identified
- [ ] No external blockers
- [ ] Small enough to complete in one sprint

### Definition of Done

A story is done when:
- [ ] Code complete and reviewed
- [ ] Unit tests written and passing
- [ ] Integration tests passing
- [ ] Documentation updated
- [ ] Acceptance criteria met
- [ ] Deployed to staging
- [ ] Product Owner accepts

## Estimation

### Story Points

We use **Fibonacci sequence** for estimation: 1, 2, 3, 5, 8, 13, 21

**What Points Represent:**
- Complexity
- Effort
- Uncertainty

**Not Time**: Points are relative, not hours

### Planning Poker

**Process:**
1. Product Owner presents story
2. Team asks clarifying questions
3. Each member selects a card privately
4. All reveal simultaneously
5. Discuss differences
6. Re-vote if needed
7. Reach consensus

**Guidelines:**
- Don't average estimates
- Highest and lowest explain reasoning
- Use reference stories for comparison
- Re-estimate if story changes significantly

## Velocity and Capacity

### Velocity

**Definition**: Average story points completed per sprint

**Calculation:**
```
Velocity = Sum of completed story points / Number of sprints
```

**Usage:**
- Forecast future capacity
- Identify trends
- Set sustainable pace
- Plan releases

**Example:**
- Sprint 1: 25 points
- Sprint 2: 30 points
- Sprint 3: 28 points
- Average Velocity: 27.7 points

### Capacity Planning

**Factors affecting capacity:**
- Team member availability
- Holidays and PTO
- Meetings and ceremonies
- On-call duties
- Technical debt

**Calculation:**
```
Available hours = (Team size × Working days × Hours per day) 
                 - (Meetings + PTO + Other commitments)
```

## Metrics and Charts

### Burndown Chart

Tracks remaining work in sprint:
- X-axis: Days in sprint
- Y-axis: Remaining story points
- Ideal line: Linear decrease to zero
- Actual line: Real progress

**Interpretation:**
- Above ideal: Behind schedule
- Below ideal: Ahead of schedule
- Flat line: No progress

### Burnup Chart

Tracks completed work over time:
- X-axis: Sprints
- Y-axis: Story points
- Shows total scope and completed work
- Useful for release planning

### Cumulative Flow Diagram

Shows work in different states:
- Backlog
- In Progress
- In Review
- Done

**Useful for:**
- Identifying bottlenecks
- Tracking work in progress
- Spotting flow problems

## Agile Best Practices

### Maintain Sustainable Pace
- Don't overcommit
- Take regular breaks
- Avoid overtime
- Prevent burnout
- Focus on long-term productivity

### Embrace Change
- Welcome changing requirements
- Adapt plans based on feedback
- Continuous improvement
- Regular retrospectives

### Focus on Value
- Prioritize high-value features
- Deliver early and often
- Get user feedback quickly
- Avoid gold-plating

### Collaborate Constantly
- Daily communication
- Pair programming when helpful
- Shared responsibility
- Knowledge sharing

### Maintain Quality
- Don't skip tests to save time
- Refactor regularly
- Address technical debt
- Follow coding standards

## Common Challenges

### Challenge: Scope Creep
**Solution:**
- Clear acceptance criteria
- Strong Product Owner
- Say no to non-essential features
- Save new ideas for future sprints

### Challenge: Unpredictable Velocity
**Solution:**
- Estimate consistently
- Track actual vs. estimated
- Break down large stories
- Address team issues

### Challenge: Incomplete Stories
**Solution:**
- Better story breakdown
- Identify dependencies early
- Daily progress tracking
- Adjust commitment if needed

### Challenge: Low Team Morale
**Solution:**
- Regular retrospectives
- Address impediments quickly
- Celebrate successes
- Sustainable pace

### Challenge: Stakeholder Unavailability
**Solution:**
- Schedule regular reviews
- Asynchronous feedback mechanisms
- Clear communication channels
- Product Owner as proxy

## Remote Agile

### Adaptations for Remote Teams

**Daily Standups:**
- Video call with cameras on
- Use virtual board
- Time-boxed strictly
- Record for absent members

**Sprint Planning:**
- Use collaborative tools (Miro, Mural)
- Share screen for backlog review
- Digital estimation tools
- Clear written sprint goals

**Sprint Review:**
- Screen sharing for demos
- Record for stakeholders
- Interactive Q&A
- Digital feedback collection

**Retrospectives:**
- Anonymous feedback tools
- Virtual sticky notes
- Breakout rooms for discussions
- Action item tracking

### Tools for Remote Agile
- **Video**: Zoom, Teams, Meet
- **Boards**: Jira, Azure DevOps, Trello
- **Collaboration**: Miro, Mural, Figma
- **Communication**: Slack, Teams
- **Documentation**: Confluence, Notion

## Scaling Agile

For larger projects with multiple teams:

### SAFe (Scaled Agile Framework)
- Program Increment Planning
- Agile Release Trains
- Portfolio management

### LeSS (Large-Scale Scrum)
- Single Product Backlog
- Multiple teams
- Coordinated sprints

### Nexus
- Integration team
- Shared Definition of Done
- Coordinated planning

## Resources

- [Scrum Guide](https://scrumguides.org/)
- [Agile Manifesto](https://agilemanifesto.org/)
- [Mountain Goat Software](https://www.mountaingoatsoftware.com/)
- [Scrum Alliance](https://www.scrumalliance.org/)
