# Welcome to Put-Options-Yield-Explorer hackathon
December 23, 2021

A weekend hackathon inspiration came to be to write a tool to enable me to browse the put-options premiums on some of my favorite stocks to see if I can collect some premiums while I wait for the prices to come down to my entry buy range. 

**[Disclaimer: This is purely for me to mess around with coding and deployment and this is not financial advice and any purported financial tool for the public.  This is solely written for myself**]

## Screenshots/Previews


## Cash-secured put options
If you don't know what put options are, just search the term on your favorite search engine for a proper explanation.  
For **TL;DR;**, here is a simplistic explanation.

Let says ABCD is currently trading at $10/share, you then offer an assurance that you will buy this stock for $8 in the next 3 months.  Therefore you are providing downside protection in the event the price drops under $8.  You are paid a premium for providing that insurance.  

There are 2 main outcomes.  

 1. The price after 3 months remains higher than $8.  At this point, no one will bother exercising your option and you just keep the premium and nothing else happens.  This is what you want.
 2. Another scenario is that the price drops to $7 and now someone exercises the options and you are now compelled to buy the stock from them at $8.  You just lost $1 because the current price is only $7.  You still keep the original premium because that is paid to you the moment you sell the option.

Ok ok, this is not a financial blog but rather more about programming so that is all I have to say for this section

## Server

For the server-side of things, I started by looking at what free financial api is available out there.  I quickly decided to use the Yahoo Finance Python API.  It is free, easy to use, has decent documentation and will do exactly what I need for this project. 

For this application, I just needed a very small server that will make some basic api calls and then packaging the results into an easily consumable format in json.  Immediately, I ruled out a full blown server and instead just leveraged a serverless solution by [Vercel](http://vercel.com) .

#### REST endpoints

GET /api/meta?ticker=rklb
	The first endpoint is to get the basic meta data like the available expiration dates and 3-yr historical stock price.
	
	{
		ticker: string;
		price: number;	
		expirationDates: string[];
		historicalPrices: PriceData[];
	}
	
GET /api/put-options?ticker=rklb&expiry=01-07-2022
	The second endpoint is to get the detail put-option chain for the requested expiration date.
	
    {
		strike: number;
		offset: number;
		percentage: number;
		lastPrice: number;
		openInterest: number;
	}
	
## UI
For the UI, I just went with my comfort zone of Angular.  I am using Angular 13 for this.  Not a whole lot to say but I am hosting this on Vercel and using chart-ng2 charting package which has always served me well for visualization.

## Source code
Python server code: https://github.com/hujanais/options-yield

Angular 13 UI code : https://github.com/hujanais/options-yield-ui
