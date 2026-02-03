# Communication

Effective communication is the foundation of successful remote and hybrid teams. This guide outlines our communication practices, tools, and expectations at Solution8.

## 🛠️ Communication Channels & Tools

### Primary Tools

**Slack** - Real-time messaging
- **Use for**: Quick questions, team coordination, casual conversation
- **Response time**: Within a few hours during work hours
- **After hours**: No expectation of immediate response
- **Status**: Use status to indicate availability (in meeting, focus time, away)

**Email** - Formal communication
- **Use for**: External communication, formal announcements, documentation that needs permanence
- **Response time**: Within 24 hours on business days
- **Not for**: Urgent matters or team coordination

**Confluence/Wiki** - Documentation
- **Use for**: Living documentation, project plans, architectural decisions, runbooks
- **Best practices**: Keep organized, link liberally, update regularly
- **Ownership**: Document owners responsible for keeping current

**Zoom/Meet** - Video conferencing
- **Use for**: Meetings, pair programming, complex discussions
- **Etiquette**: Camera on when possible, mute when not speaking, use virtual backgrounds if needed
- **Recording**: Ask permission, store in shared location

**GitHub** - Code collaboration
- **Use for**: Code reviews, technical discussions, issue tracking
- **Response time**: Pull requests reviewed within 24 hours
- **Tone**: Professional and constructive

**Jira** - Work tracking
- **Use for**: Tracking work items, sprints, dependencies
- **Update**: Keep tickets current for team visibility
- **Detail**: Provide context for future readers

### Channel Organization

**Slack Channel Structure:**

**#general** - Company-wide announcements and discussions

**#engineering** - Engineering-wide updates and discussions

**#team-[name]** - Team-specific channels (e.g., #team-platform, #team-mobile)

**#project-[name]** - Temporary project channels

**#topic-[name]** - Interest-based channels (e.g., #topic-frontend, #topic-devops)

**#random** - Non-work conversation and fun

**#wins** - Celebrate successes

**#help-[category]** - Get help (e.g., #help-aws, #help-debugging)

**Channel Best Practices:**
- Use threads to keep conversations organized
- Use @channel sparingly (urgent, everyone needs to see)
- Use @here for online people only
- Direct message for private or 1-on-1 matters
- Post in public channels when possible (knowledge sharing)

## 📋 Communication Guidelines

### Asynchronous Communication (Default)

We're a distributed team across time zones. Most communication should work asynchronously.

**Async-Friendly Practices:**

**Over-communicate context**
- Don't assume others have the same background
- Link to relevant documents, issues, or conversations
- Explain the "why" not just the "what"

**Make it self-service**
- Write messages that don't require follow-up questions
- Provide all relevant information upfront
- Include screenshots, logs, or examples

**Document decisions**
- Don't let important decisions live only in chat
- Summarize outcomes in persistent locations
- Use ADRs for architectural decisions

**Example of Good Async Message:**
```
Hey team! 👋

I'm proposing we switch from REST to GraphQL for our mobile API.

Context: Mobile team is making 20+ API calls per screen load, 
causing performance issues on slower connections.

Proposal: Implement GraphQL gateway that lets clients request 
exactly what they need.

Tradeoffs: 
✅ Reduce over-fetching, faster mobile experience
✅ Better developer experience with type safety
❌ Learning curve for team
❌ Additional infrastructure complexity

Details in this doc: [link]

Please review and share thoughts by EOD Friday. 
I'll synthesize feedback and make a decision next Monday.

Questions? Drop them in the thread below 👇
```

**Example of Poor Async Message:**
```
Should we use GraphQL?
```

### Synchronous Communication (When Needed)

Some situations benefit from real-time interaction.

**Use Sync For:**
- Complex discussions requiring back-and-forth
- Brainstorming and ideation
- Building relationships and trust
- Urgent or time-sensitive matters
- Difficult conversations (feedback, conflicts)

**Sync Best Practices:**
- Schedule in advance when possible
- Share agenda beforehand
- Document outcomes afterward
- Record if others might benefit (with permission)
- Be respectful of time zones

## 🎯 Meeting Best Practices

### Before the Meeting

**For Organizers:**
- [ ] Clear objective and desired outcome defined
- [ ] Agenda shared at least 24 hours in advance
- [ ] Only essential participants invited
- [ ] Pre-reads or materials shared early
- [ ] Appropriate time allocated (25 or 50 minutes)

**For Participants:**
- [ ] Review agenda and materials
- [ ] Come prepared to contribute
- [ ] Notify organizer if you can't attend
- [ ] Suggest alternative if you're not essential

### During the Meeting

**Facilitator Responsibilities:**
- Start on time (even if people are late)
- Review objectives and agenda
- Keep discussion on track (use parking lot)
- Ensure everyone has a chance to contribute
- Capture action items and decisions
- End on time

**Participant Responsibilities:**
- Be present (avoid multitasking)
- Contribute actively
- Listen generously
- Use "raise hand" feature for turn-taking
- Mute when not speaking

**Virtual Meeting Etiquette:**
- Camera on when possible (builds connection)
- Mute when not speaking (reduces noise)
- Use chat for questions/links without interrupting
- Blur or use virtual background if needed
- Ensure good audio quality (use headphones)

### After the Meeting

**Within 24 hours:**
- [ ] Publish notes in shared location
- [ ] Send summary to participants (and those who should be informed)
- [ ] Create tickets for action items
- [ ] Upload recording if applicable

**Meeting Notes Template:**
```
Meeting: [Title]
Date: [Date]
Attendees: [Names]
Objectives: [What we wanted to accomplish]

Key Discussions:
- [Topic 1 summary]
- [Topic 2 summary]

Decisions Made:
- [Decision 1 and rationale]
- [Decision 2 and rationale]

Action Items:
- [ ] [Task 1] - Owner: @name - Due: date
- [ ] [Task 2] - Owner: @name - Due: date

Parking Lot (to address later):
- [Topic 1]
- [Topic 2]

Next Steps:
[What happens next]
```

## 💬 Async vs Sync Communication

### Decision Framework

**Choose Async When:**
- ✅ Information sharing or updates
- ✅ Non-urgent questions
- ✅ Multiple time zones involved
- ✅ People need time to think
- ✅ Documentation is important
- ✅ Can wait hours/days for response

**Choose Sync When:**
- ✅ Complex problem requiring collaboration
- ✅ High-bandwidth discussion needed
- ✅ Building relationships
- ✅ Urgent time-sensitive issues
- ✅ Nuanced or emotional topics
- ✅ Brainstorming and ideation

**Hybrid Approach:**
Many situations benefit from both:
1. Share context async (doc, message)
2. Meet sync to discuss and decide
3. Document decision async

## 🎤 Feedback Culture

Feedback is a gift that helps us grow. We give it frequently, kindly, and constructively.

### Giving Feedback

**SBI Model (Situation-Behavior-Impact):**
- **Situation**: Describe when and where
- **Behavior**: Describe specific, observable action
- **Impact**: Explain the effect it had

**Example:**
"During yesterday's standup (situation), you interrupted Sarah twice while she was speaking (behavior). This made it hard for her to complete her updates and may have made her feel like her input wasn't valued (impact)."

**Best Practices:**
- ✅ Be timely (don't let issues fester)
- ✅ Be specific (vague feedback isn't actionable)
- ✅ Be kind (assume positive intent)
- ✅ Focus on behavior (not personality)
- ✅ Make it actionable (what should change?)
- ✅ Deliver privately (praise public, criticize private)

**Avoid:**
- ❌ Sandwich method (insincere)
- ❌ Comparing to others
- ❌ Generalizations ("you always...")
- ❌ Waiting for performance reviews

### Receiving Feedback

**How to Respond:**
- Listen fully before responding
- Assume good intent
- Ask clarifying questions
- Thank the person (even if you disagree)
- Reflect before reacting
- Decide what to do with it

**It's OK to:**
- Disagree with feedback (after considering it)
- Ask for time to process
- Request specific examples
- Seek additional perspectives

### Feedback Channels

**360 Reviews** (Annually)
- Structured feedback from peers, managers, and direct reports
- Focus on growth and development

**1-on-1s** (Weekly/Bi-weekly)
- Regular opportunity for feedback in both directions
- Safe space for coaching and course-correction

**Retrospectives** (Every sprint)
- Team-level feedback on processes and collaboration
- Focus on continuous improvement

**Ad-hoc** (Anytime)
- Don't wait for formal processes
- Deliver timely, specific feedback when it's fresh

## 🤝 Conflict Resolution

Conflict is natural. How we handle it defines our culture.

### Healthy Conflict

**Good conflict:**
- ✅ Focuses on ideas, not people
- ✅ Seeks the best outcome
- ✅ Respectful and professional
- ✅ Based on data and reasoning
- ✅ Results in better decisions

**Unhealthy conflict:**
- ❌ Personal attacks or blame
- ❌ Hidden agendas
- ❌ Power plays
- ❌ Avoiding the real issue
- ❌ Undermining decisions after they're made

### Addressing Conflict

**Step 1: Direct Conversation**
- Talk directly to the person involved
- Assume positive intent
- Use "I" statements ("I felt..." not "You made me...")
- Focus on specific behaviors and impact
- Seek to understand their perspective

**Step 2: Involve a Mediator**
If direct conversation doesn't resolve it:
- Ask your manager or a neutral third party to facilitate
- Approach with openness to resolution
- Focus on finding common ground

**Step 3: Escalate if Necessary**
For serious issues:
- Involve HR/People Ops
- Document the situation objectively
- Follow company policies and procedures

### Preventing Conflict

**Proactive Practices:**
- Clarify roles and responsibilities
- Set clear expectations
- Communicate frequently
- Address small issues before they grow
- Build relationships and trust
- Assume positive intent

## 📞 Communication Norms by Situation

### Code Reviews
- **Tone**: Constructive and educational
- **Timeliness**: Respond within 24 hours
- **Format**: Written, in GitHub
- **Detail**: Explain reasoning, suggest alternatives
- **Respect**: Appreciate the author's effort

### Incidents/Outages
- **Channel**: Dedicated Slack channel + incident ticket
- **Updates**: Frequent (every 15-30 min)
- **Tone**: Clear, factual, no blame
- **Follow-up**: Blameless postmortem, action items

### Announcements
- **Major changes**: Email + Slack + all-hands meeting
- **Team updates**: Team channel + standups
- **Individual updates**: 1-on-1s or direct message

### Asking for Help
- **Public first**: Ask in appropriate channel (others benefit)
- **Provide context**: What you're trying to do, what you've tried
- **Show effort**: Demonstrate you've attempted to solve it
- **Be patient**: People respond when they can

## 🌍 Cross-Cultural Communication

We're a global team. Cultural awareness enhances communication.

**Be Aware Of:**
- Time zones (use world clock, schedule fairly)
- Language differences (favor clear, simple language)
- Communication styles (direct vs. indirect)
- Holidays and working days
- Cultural norms around feedback, hierarchy, etc.

**Best Practices:**
- Be patient with non-native speakers
- Avoid idioms and slang
- Confirm understanding (especially for critical topics)
- Rotate meeting times to share time zone burden
- Learn about teammates' cultures

## ✅ Communication Checklist

**Before communicating, ask:**
- [ ] What's the goal of this communication?
- [ ] Who needs to receive this?
- [ ] What's the best channel/format?
- [ ] Is async or sync more appropriate?
- [ ] Have I provided enough context?
- [ ] Is the message clear and actionable?
- [ ] Is the tone appropriate?

---

**Remember**: Over-communicate in remote environments. What feels like too much context is usually just right.

**Questions about communication practices?** Reach out to your team lead or check the #help-communication channel.
