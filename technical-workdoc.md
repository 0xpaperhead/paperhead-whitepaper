# AI Trading Agent Workdoc
This is a to-do side paper

  

**Created On**: June 01, 2025
**Version:** 1.1.3

## 1. Introduction

This document serves as a detailed technical breakdown and progress tracker for the Paperhead AI Trading Agent development. While the main [roadmap](README.md#6-roadmap) provides high-level quarterly milestones and strategic direction, this technical workdoc offers granular, actionable tasks with specific completion targets.

**Purpose:**
- **Transparency**: Provides the community with real-time visibility into development progress and task completion status
- **Accountability**: Establishes clear, measurable goals for each development cycle
- **Timeline Clarity**: Gives community members a better understanding of what to expect and when
- **Live Stream Alignment**: Tasks are organized around live streaming sessions, allowing the community to watch development happen in real-time

**Update Cycle:**
This document is updated frequently and reflects tasks intended to be completed over the following 2 weeks. Tasks are marked with completion status and organized by streaming sessions (`this stream`, `next stream`) to align with the live development approach that defines the Paperhead project.

This granular approach ensures that both the community and development team have a clear understanding of immediate priorities, technical challenges, and progress toward the larger vision outlined in the main roadmap.

## 2. Versioning Logic

This document follows a semantic versioning approach to track progress and changes:

- **Major version (X.0.0)**: Complete document restructures, major milestone completions, or significant scope changes
- **Minor version (1.X.0)**: New sections added, significant content updates, or structural improvements to the document  
- **Patch version (1.0.X)**: Task completion updates, small corrections, or minor progress tracking changes

**Example:** When tasks are marked as completed or small updates are made to reflect development progress, the patch version is incremented (e.g., 1.0 → 1.0.1). This helps the community track development momentum and easily see when new progress has been made since their last visit.

## 3. To-do

- [x] create a public workdoc breaks down the technical to-do bi-weekly (side paper)
- [x] update the website to reflect this technical workdoc/todo
- [x] fix chart (complete)
	- [x] data rendering
	- [x] overlay UI
	- [x] Highlight graph data is only mock data, actual agent performance will only start to be tracked when the agent is active and reporting allocations over time
- [x] make readme more accurate for [paperhead-trading-agent](https://github.com/0xpaperhead/paperhead-trading-agent)
- [x] correct the agent app's metadata and favicon
- [x] publish the agent on [agent.paperhead.io](https://paperhead.io)
- [x] add React Query for organized caching and fetching of the agent data
- [x] make popup graph state separate from the main graph load (so it doesn't close when the timerange changes) `this stream`
- [ ] decide privy vs dynamic (user-session generation) `next stream`
- [ ] Implement a robust encryption library to handle pool wallet creation, storage, and access `next stream`
- [ ] implement supabase database for the following:
	- [ ] db initialization, connection, types `next stream`
	- [ ] user creation upon wallet connection
	- [ ] create a shared-custodian user pool upon wallet connection
	- [ ] agent logs
	- [ ] user interactions (user logs?)
- [ ] Implement Redis for secure key export and single-request session logic
- [ ] Implement $PAPERHEAD as currency on agent.paperhead.io
	- [ ] Accept the user's inputted $PAPERHEAD amount and trigger a wallet transaction to the shared-custodial agent-user pool	
- [ ] Implement a listener for the transaction completion/success to initiate agent permission to use the funds and start allocating based on currently held assets proportionally.
- [ ] Implement user controls on the front-end