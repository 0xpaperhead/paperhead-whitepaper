# System Architecture & Metrics

This document outlines the technical architecture and data metrics used by the Paperhead AI Trading Agent.

## System Flow Diagram
![Agent System Flow](https://i.imgur.com/4rRGIRN.png)
  

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
 

 ### Trending Tokens APIs
 1.  **Solana Tracker Trending API**: `https://data.solanatracker.io/tokens/trending`
	-  Gets the top 100 trending tokens based on transaction volume in the specified timeframe
	-  This will return the following:
	```json
	[
        {
            "token": {
                "name": "FartCat",
                "symbol": "Fartcat",
                "mint": "3dk9CNre8tmv6bbNXd5F6dgkNnEzsyQ7sPhVT8kKpump",
                "uri": "https://ipfs.io/ipfs/QmNNkjiD8tEcjop3eYy5FDFmohtcJykEiF848pHRaEt1oh",
                "decimals": 6,
                "hasFileMetaData": true,
                "createdOn": "https://pump.fun",
                "description": "",
                "image": "https://image.solanatracker.io/proxy?url=https%3A%2F%2Fipfs.io%2Fipfs%2FQmWDZ9fyPTmRThJKdc7Yf6Za4D6FbYopPjkvq3DwRPa42K",
                "showName": true,
                "twitter": "https://x.com/AndyAyrey/status/1925868039410528290",
                "website": "https://www.infinitebackrooms.com/dreams/conversation-1747996802-scenario-opus-3-meet-4-txt",
                "creation": {
                    "creator": "6yrCgiRyeAyY3fTFBRbCV4vX5KHUZWC5xwgYLoAtmvCd",
                    "created_tx": "58MoisbMFjgbMD6zaptnbFvXXLZbGFu2Cpt5AyQnEQytoqzhzmqyf7LKxzaMfhJr5fqbRrpS4EcUWeM1EiSdfQKg",
                    "created_time": 1747998309
                }
            },
            "pools":
            [
                {
                    "liquidity": {
                        "quote": 1248.23870711,
                        "usd": 364165.2834303389
                    },
                    "price": {
                        "quote": 0.00003543462808397693,
                        "usd": 0.005168907720113244
                    },
                    "tokenSupply": 999832114.612585,
                    "lpBurn": 100,
                    "tokenAddress": "3dk9CNre8tmv6bbNXd5F6dgkNnEzsyQ7sPhVT8kKpump",
                    "marketCap": {
                        "quote": 35428.67912771314,
                        "usd": 5168039.936038139
                    },
                    "decimals": 6,
                    "security": {
                        "freezeAuthority": null,
                        "mintAuthority": null
                    },
                    "quoteToken": "So11111111111111111111111111111111111111112",
                    "market": "pumpfun-amm",
                    "lastUpdated": 1749960170435,
                    "createdAt": 1747998603486,
                    "txns": {
                        "buys": 97574,
                        "total": 185428,
                        "volume": 23976153,
                        "sells": 87854
                    },
                    "deployer": null,
                    "poolId": "AE1RrYTUpKWV18kLxY2kfLCq7mZBVovZUnuBEWRvHTyY"
                }
            ],
            "events": {
                "1m": { "priceChangePercentage": 11.739560004960783 },
                "5m": { "priceChangePercentage": 12.058989597956032 },
                "15m": { "priceChangePercentage": 53.53709926051653 },
                "30m": { "priceChangePercentage": 71.77699643282355 },
                "1h": { "priceChangePercentage": 173.7750244597893 },
                "2h": { "priceChangePercentage": 241.0241547732657 },
                "3h": { "priceChangePercentage": 129.55243425955757 },
                "4h": { "priceChangePercentage": 56.56369544200442 },
                "5h": { "priceChangePercentage": 61.501291528003364 },
                "6h": { "priceChangePercentage": 174.1901767654027 },
                "12h": { "priceChangePercentage": 4331.713499010888 },
                "24h": { "priceChangePercentage": 4669.7715925521625 }
            },
            "risk": {
                "rugged": false,
                "risks": [
                    {
                        "name": "Top 10 Holders",
                        "description": "Top 10 holders own more than 20.36% of the total supply.",
                        "value": "20.36%",
                        "level": "danger",
                        "score": 5000
                    }
                ],
                "score": 4
            },
            "buysCount": 1464,
            "sellsCount": 1579
        }
    ]
	```
	-  **Trending Score**: Based on transaction volume and activity across multiple timeframes
	-  **Price Momentum**: Multiple timeframe price change percentages (1m to 24h) for trend analysis
	-  **Risk Assessment**: Built-in risk scoring and rug pull detection
	-  **Liquidity Analysis**: Pool liquidity and market cap data for trade feasibility
	-  **Buy/Sell Pressure**: Real-time buy vs sell transaction counts and volume

2.  **Moralis Trending Tokens API**: `https://deep-index.moralis.io/api/v2.2/tokens/trending?chain=solana&limit=100`
	-  Gets the top 100 trending tokens on Solana chain with comprehensive market data
	-  This will return the following:
	```json
	[
		{
			"chainId": "solana",
			"tokenAddress": "CfJ58KZpVvPm5ketxbUMmRMHZh41AWZh9qx8r9cspump",
			"name": "doodles",
			"uniqueName": "doodles-821143262",
			"symbol": "DOOD",
			"decimals": 6,
			"logo": "https://d23exngyjlavgo.cloudfront.net/solana_CfJ58KZpVvPm5ketxbUMmRMHZh41AWZh9qx8r9cspump",
			"usdPrice": 0.000655483894313,
			"createdAt": 1739745029,
			"marketCap": 655478,
			"liquidityUsd": 56983,
			"holders": 24667,
			"pricePercentChange": {
				"1h": -0.33801255044203293,
				"4h": -0.48742981106028005,
				"12h": 1.1810803769845877,
				"24h": 7.537889198641526
			},
			"totalVolume": {
				"1h": 434764,
				"4h": 1729031,
				"12h": 9100448,
				"24h": 10568778
			},
			"transactions": {
				"1h": 9589,
				"4h": 34103,
				"12h": 128212,
				"24h": 145532
			},
			"buyTransactions": {
				"1h": 4684,
				"4h": 18119,
				"12h": 71017,
				"24h": 80921
			},
			"sellTransactions": {
				"1h": 4905,
				"4h": 15984,
				"12h": 57195,
				"24h": 64611
			},
			"buyers": {
				"1h": 3937,
				"4h": 14699,
				"12h": 52021,
				"24h": 58690
			},
			"sellers": {
				"1h": 3368,
				"4h": 8955,
				"12h": 25689,
				"24h": 28200
			}
		}
	]
	```
	-  **Market Data**: Real-time USD price, market cap, and liquidity information
	-  **Holder Analysis**: Total holder count for community strength assessment
	-  **Volume Tracking**: Multi-timeframe volume data (1h, 4h, 12h, 24h)
	-  **Transaction Analysis**: Detailed buy/sell transaction counts and ratios
	-  **User Activity**: Unique buyer and seller counts for market participation metrics