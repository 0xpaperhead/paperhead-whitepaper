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

4.  **Fear and Greed Index**: We will fetch the crypto fear and greed index from the following endpoint: `https://crypto-fear-greed-index2.p.rapidapi.com/index?limit=2`
	-  This endpoint is updated daily, with `limit=2` returning today and yesterday's data
	-  This will return the following:
	```json
	{
		"1749859200": {
			"value": "63",
			"value_classification": "Greed"
		},
		"1749945600": {
			"value": "60",
			"value_classification": "Greed",
			"time_until_update": "72800"
		}
	}
	```
	-  **Market Sentiment Score**: Values range from 0-100 (Extreme Fear to Extreme Greed)
	-  **Daily Comparison**: Compare today vs yesterday to identify sentiment shifts
	-  **Classification Mapping**: Extreme Fear (0-24), Fear (25-49), Neutral (50), Greed (51-74), Extreme Greed (75-100)
 
5.  **Listing/Delisting Activity**: We will fetch cryptocurrency listing and delisting data from major exchanges using the following endpoint: `https://crypto-news51.p.rapidapi.com/api/v1/crypto/listing_delisting?sort_by=published`
	-  This endpoint provides real-time data on trading pair additions and removals across major crypto exchanges
	-  This will return the following:
	```json
	[
		{
			"title": "HTX to Open Trading for SQD (Subsquid) at 09:00 (UTC) on June 14, 2025",
			"source": "htx",
			"symbol": "SQD/USDT",
			"content": "HTX opens trading for Subsquid, offering both spot and grid trading pairs against USDT. Users are reminded to exercise caution due to potential market volatility.",
			"category": "listing",
			"date": "2025-06-14T09:00:00Z",
			"published_date": "2025-06-14T03:39:00Z"
		}
	]
	```
	-  **Exchange Activity Score**: Number of new listings vs delistings indicates market expansion/contraction
	-  **Token Momentum**: New listings often correlate with increased trading volume and price movement
	-  **Risk Assessment**: Delisting announcements can signal potential price volatility or regulatory concerns
 