# System Architecture & Metrics

This document outlines the technical architecture and data metrics used by the Paperhead AI Trading Agent.

## System Flow Diagram
![Agent System Flow](https://i.imgur.com/LMMUsjr.png)
  

## Measurable Metrics
The agent utilizes various data sources to make informed trading decisions. Below are the key metrics and data streams that feed into the decision-making algorithm:

### News Sentiment Analysis
 1.  **Topic Generation**: Agent dynamically generates a list of relevant topics such as: `solana`, `rwa`, `rwas`, `airdrop`, `airdrops`, `memecoins`, `defi`, `nft`, `gaming`, `ai`, etc.

2.  **News Volume Scoring**: For each topic, fetch articles from `https://crypto-news51.p.rapidapi.com/api/v1/crypto/articles/search?title_keywords={topic}&page=1&limit=100&time_frame=24h&format=json`

	-  **Popularity Score**: Number of articles returned (max 100) determines topic popularity score (0-100 scale)

3.  **Sentiment Weight**: We will fetch overall 24h and 48h crypto news sentiment from the following endpoint: `https://crypto-news51.p.rapidapi.com/api/v1/crypto/sentiment?interval={interval}`
	-  This will return the following:
	```json
	{ 
		"interval": "24h",
		"total": 1597,
		"counts": {
			"positive": 451, 
			"neutral": 842, 
			"negative": 304
		},
		"percentages": { 
			"positive": 28.24, 
			"neutral": 52.72, 
			"negative": 19.04 
		}
	}
	```
-  **Trend Analysis**: Compare current 24h scores with previous periods to identify trending topic
 