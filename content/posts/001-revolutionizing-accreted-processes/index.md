+++
title = "Revolutionizing Accreted Processes"
date = 2026-01-09
toc = true
+++

I recently came across Bryan Cantrill's talk [The Complexity of Simplicity](https://www.youtube.com/watch?v=Cum5uN2634o) in which he categorizes systems based on their complexity and discusses their qualities and risks. ![System taxonomy](taxonomy.png)

I'm stripping away a ton of nuance here, but briefly:
* constructed systems are large and susceptible to scope creep 
* rebellious systems are built as a sort of minimalistic, "surely we can do better than this" reaction to constructed systems
* accreted systems are ones which accumulate tech debt or suffer from poor/unprincipled maintenance
* revolutionary systems are "clean sheet of paper" attempts to innovate on a problem with an elegant abstraction

Again, that's a simplification and you should definitely go watch the talk in its entierty.

Since I've spent the majority of the past 5 years thinking about automating operational processes in Azure's Network, I couldn't help but see this chart through a process-oriented lens. To parallel, constructed processes are born by converting cross-org stakeholder input into 100-page word docs, rebellious processes might skirt the standard a bit and try to get something done quickly in the name of producing value. When we start tacking appendages onto processes to support other workloads, work tracking, integrations, etc it can easily become accreted. And when we realize a single person can't explain a process end-to-end anymore we might dare to revolutionize it.

<br />

## Web of Complexity
I'm sure all companies have their fair share of accreted systems and processes. It's easy for this to happen as turnover deletes tribal knowledge, original intents become obfuscated, and features of dubious value are preserved for fear of breaking things. Despite these challenges, as Bryan points out, accreted systems do work *well enough*. We just end up tangled in a "web of complexity." Once that web is in place, it's not too hard to tack on a little bit more functionality. In fact, it's a bit of death spiral as each addition adds to the burden of revolutionizing. The value calculation weighs heavily in favor of just digging the hole a little bit deeper - you can still reach the ladder from here.

The "complexity" we might have to manage when trying to automate some process can look like:
- a ticket has to be created with the title just-so and be kept in a queue for the duration of the process
- you can track the progress of your request by querying this table
- the source of truth for the current state is shaky so you should cross reference with these other sources maintained by other teams and the systems they own
- a dashboard is populated by a particular query, so data has to be ingested to this table (which you need to request permission to)

All of that to perform a single "step" of the process. Now if you have multiple systems that need to perform this step, you have to make sure they're all performing the same incantations in the same order. If we need to change one of the interactions (we want to create tickets in a different queue or read data from a regional table), we have to  carry out that update everywhere.

## Untangle Ourselves
*todo*

### How did we get here?
It's important to clarify this isn't a value judgement of anyone or their services. I'm sure we can all agree this end result sucks. Bryan points out these accreted systems are "ones that no one would design from first principles." There is a history to the process and the services that carry out that process. No one ever decided this is the way it should be - rather we started with some process and changes were made here and there until we arrive at the amalgamation we have today. And those changes likely had good intentions too, maybe even resulting from findings of an RCA. 

So, let’s put the index fingers away and do our best to understand the full process and why each aspect of it is performed.  no finger pointing, we must take stock oDocument the current process and who owns which parts. Figure out what purpose all of the intermediate steps serve; which appendages are necessary, which are vestigial? Someone from each interested party must weigh in so it's known which breadcrumbs of the process they care about. The process was allowed to become decentralized and its implementation was allowed to leak, so we have to give a best-effort attempt at understanding the extent of the leakage. 

There is another frustrating problem here that likely every organization encounters: we don't know the full history? Historical decisions, which if documented, might live in OneNote, or Sharepoint, or someone's personal OneDrive folder, or ADO Wiki, or a Loop document. Oh and you don't have permission to view it. If the decisions weren't documented then you have to hope the tribal knowledge survived attrition and human forgetfulness.

Even if the history is not intact, it's important to get a snapshot of the current process so any radical changes can still maintain correctness.

### Is this a Revolution or a Rebellion
Bryan's taxonomy outlines both revolutionary systems and rebellious systems. I was eager to label what I want to carry out as "revolutionary" - maybe I hold my intentions in too high regard. But looking back at the qualities ascribed to the different types of systems, I do see a little bit of this and a little bit of that in what I'm trying to attain. 
A little bit of this, a little bit of that. The analogy here between internal processes and Bryan's talk isn't perfect. Bryan discusses discrete systems (NoSQL is discrete from and a rebellion of SQL) whereas I'm looking at how can we metamorphize some process. So there are some aspects of rebelling against the old process ("do we need to keep all of these steps?") and revolutionizing it at the same time (cleaning up the interface, not leaking implementation to clients). There is both the rejection of what came before and the concerted effort to innovate on the operational task the process address.

### To Arms
The current process is unanimously agreed to be subpar, we understand the scope of the process and how it grew to become this, and we have an idea of what sort of interface we would like to expose to clients instead of forcing them to re-enact the song and dance. Great. Now we can build something well-intentioned that "[sparks joy](https://konmari.com/marie-kondo-rules-of-tidying-sparks-joy/?srsltid=AfmBOoqwHLhW_yV3Zorbrp14-kJMrGbi_0qjk83QzqX_MUnFmiGIpj-A)."

For this particular process, that meant a microservice. We write a service solely responsible for owning the business-logic of the process and provide an API that can untangle itself from the implementation details of the process. E.g. some basic CRUD operations might be to submit a request that the process be carried out on an entity, check the current status of the process, or request the process be rolled back. Clients are happier because they can talk *intent* to the microservice. "I want to start the process for this network entity" is what the automation service wants to do as opposed to "I want to start the process, so I must create a ticket in this queue and ingest this data into a table and...".

Okay, admittedly, "turn a decentralized process into a microservice" is not a revolutionary suggestion. (TODO)

### Scrape the Cruft
If we need to change the process, we can change the implementation of the microservice. Clients everywhere automatically start perform the new process without changing 

Now that it's centralized, the process can be easily changed


### Defending Against the Ouroboros of Software Accretion
*cool we have something nice and new. what principles do we adopt to prevent it from sucking*

### Integrating LLMs
When you hear "automating operational processes" you should probably be thinking of LLM integration. We have spent a few years tinkering with various services that seek to unburden our engineers from operational load. I won't go into those projects here, but one thing we have experienced is an agent's ability to carry out some task is weak when it is operating amongst services in the bottom-left quadrant of Bryan's chart. And I don't blame the technology - if you gave me a system prompt and our internal APIs, I probably couldn't `curl` everything in the right order with the right payloads either. If we want to enable effective agentic operations, we have to shift our system complexity to the right. The funny bit of this is we won't simplify complexity for ourselves but if you tell C-suite it's needed to enable agentic workloads - green light!

When you provide a thoughtfully-crafted abstraction, it's pleasant no matter the interface - REST, GRPC, or MCP. 



\* \* \*




Have to survey the current state of the process and how/why different systems interact with it to maintain correctness

Not starting with an attempt to be yet another service involved in the process but rather a way to abstract away parts of the accreted process for your ends. You get to interact with the revolutionary system and the system deals with the accreted processes until they can all be killed/consumed by the revolutionary process. 

Scope of a process has become fractured and then siloed. Need to provide a central microservice that clients interested in that process can interface with. Then the process itself can be revised and consolidated without leaking the process changes to clients

Operational task we need to perform frequently in a number of different systems, and the specifics of the task change in non-trivial ways over time. Rather than maintain implementations across systems, centralize into a microservice and make the other systems clients of that service. Client services then get more ergonomic, intent-based implementations of the operational task. 

Re llm integration: can maybe plop an agent into accreted systems and use more complexity to deal with it (elaborate prompts, tailored task management). In doing so you externalize complexity 
