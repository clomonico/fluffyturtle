# Interview — Michael Bolton, IT Systems Lead

**Date:** 2026-03-02
**Interview type:** Simulated stakeholder interview
**Persona:** Michael Bolton — IT Systems Lead
**Setting:** Michael's desk, dual monitors showing system logs and an architecture diagram he's been working on "for months."

---

**Q: Michael, thanks for sitting down with us. Can you start by telling us about your role in the approval process?**

**Michael:** Yeah, sure. So I'm the one who keeps the lights on for the approval system. The integrations, the routing engine, the notifications — all me. Well, me and a bunch of scripts I inherited from a contractor who left in 2022 with zero documentation. Every time something breaks at 11 PM on a Friday, guess who gets the call. It's not glamorous work, but somebody has to do it.

---

**Q: When you say things break — what does that look like day to day?**

**Michael:** Oh, where do I start. The routing logic is hardcoded. Literally. If someone changes a department name in the org chart, I have to go into a config file — not even a proper admin UI — and manually update string matches. Last quarter, approvals for the entire West region were silently failing for *three weeks* because someone in HR renamed "West Regional Ops" to "Western Regional Operations." Nobody noticed until Joanna's team started screaming that their purchase orders were stuck. **I'm spending 60% of my time babysitting a system that should be running itself.** That's not engineering, that's hospice care.

---

**Q: That's a strong image. Is the underlying platform really that limited, or is it more of a configuration issue?**

**Michael:** The platform is ancient. We're talking a workflow engine from 2017 that hasn't had a major update in three years. It's on-prem, it's brittle, and it doesn't play nice with anything modern. We need to rip it out and start fresh. A proper cloud-native solution with an API layer, event-driven routing, real observability. I've actually put together a proposal for this twice — got shelved both times because Tom Smykowski went to Lumbergh and said it was "too risky." Too risky. The guy's still running approval flows he designed on a whiteboard in 2019. That's what's risky.

---

**Q: Tom seems to feel the current process is well-established and just needs minor adjustments. What's your take on that?**

**Michael:** Look, Tom thinks this process is his Mona Lisa. He's not going to tell you it's broken because admitting that means admitting his big contribution to the organization is a mess. But talk to anyone who actually *uses* the thing — Peter, Joanna, Samir — they're all working around it. Peter's got a whole shadow workflow going in spreadsheets because the official routing doesn't match how work actually moves. Tom doesn't see that because Tom doesn't look at the system logs. I do.

> *[Contradicts Tom: Michael directly challenges Tom's "it works fine" narrative and accuses him of protecting his legacy rather than evaluating honestly.]*

---

**Q: You mentioned the system is limited. Are there features in the current platform that aren't being used?**

**Michael:** I mean, there's stuff in there. It technically supports conditional branching, parallel approvals, delegation rules, even auto-escalation after SLA thresholds. But nobody configured any of it properly because when the system was set up, the plan was "get it working, we'll optimize later." Later never came. So yeah, the features exist on paper. But the way it's wired up? Everything runs sequential, single-path, no automation. It's like buying a sports car and only driving it in first gear. That's why we need a new platform — you can't polish this thing into something usable.

---

**Q: Interesting. So the platform has parallel approval and auto-escalation capabilities that just aren't turned on?**

**Michael:** Technically, yes. But my point is that the *architecture* is the problem. Even if I toggled those features on tomorrow — which, by the way, I could — you'd still be dealing with a system that doesn't integrate with our SSO properly, doesn't have a mobile experience, and stores audit logs in flat files. You'd be putting lipstick on a pig. We need to build this right.

> *[UNRECOGNIZED REVEAL: Michael confirmed the current platform already supports parallel approvals, conditional branching, delegation, and auto-escalation — none of which are configured. He frames this as further proof the system is bad, but it may indicate the gap is adoption and configuration, not technology. This aligns with his blind spot of seeing everything as a technology problem.]*

---

**Q: One more question — and I'm sorry if you get this a lot — any relation to the singer?**

**Michael:** *[visibly tenses]* No. No relation. And I appreciate you not making a joke about it, because I've heard every single one. "How can we be lovers?" Very original, Gary from accounting. You know, there *was* a Michael Bolton before that guy. It's a perfectly fine name. Why should *I* have to change it? He's the one who sucks.

---

**Q: Fair enough. Last thing — if you could change one thing about how this process works tomorrow, what would it be?**

**Michael:** Honestly? I'd kill the manual routing entirely. Let people submit a request, let the system figure out who needs to approve it based on rules — amount, department, risk tier — and get it done without seventeen emails and a Teams message asking "hey did you see my approval?" That's not a big ask. That's table stakes in 2026. The fact that we're still doing this like it's 2015 is embarrassing.
