
# AI Trading Agent Workdoc
This is a to-do side paper

  

**Created On**: June 01, 2025
**Version:** 1.0
  

## 1. Introduction
This document is a WIP for to-do technicals tasks updated often and reflective of a bi-weekly completion goal. This is a granular snapshop of the roadmap. This document is necessary for the community to observe where they stand on the completion of technical tasks and gives a better idea of timelines.

## 2. To-do

- [x] create a public workdoc breaks down the technical to-do bi-weekly (side paper)  `this stream`
- [ ] update the website to reflect this technical workdoc/todo `this stream`
- [ ] fix chart (complete) `this stream`
	- [ ] data rendering
	- [ ] overlay UI
	- [ ] Highlight graph data is only mock data, actual agent performance will only start to be tracked when the agent is active and reporting allocations over time
- [ ] make readme more accurate for [paperhead-trading-agent](https://github.com/0xpaperhead/paperhead-trading-agent) `this stream`
- [ ] add React Query for organized caching and fetching of the agent data `next stream`
- [ ] correct the agent app's metadata and favicon `next stream`
- [ ] publish the agent on [agent.paperhead.io](https://paperhead.io) `next stream`
- [ ] decide privy vs dynamic (user-session generation)
- [ ] Implement a robust encryption library to handle pool wallet creation, storage, and access
- [ ] implement supabase database for the following:
	- [ ] user creation upon wallet connection
	- [ ] create a shared-custodian user pool upon wallet connection
	- [ ] agent logs
	- [ ] user interactions (user logs?)
- [ ] Implement Redis for secure key export and single-request session logic
- [ ] Implement $PAPERHEAD as currency on agent.paperhead.io
	- [ ] Accept the user's inputted $PAPERHEAD amount and trigger a wallet transaction to the shared-custodial agent-user pool	
- [ ] Implement a listener for the transaction completion/success to initiate agent permission to use the funds and start allocating based on currently held assets proportionally.
- [ ] Implement user controls on the front-end