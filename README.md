# Force Spike integration Manual & API
Manual for integration with Force Spike service

## Overview 

Force spike currently offers service of **[Price delver](https://www.forcespike.com/price-delver.html)** - Accurate and up-to-date prices and price history from all your major competitors, comparison sites and marketplaces. Quickly respond to any competitor offers and mitigate their advantage.

This document describes basic functionality of Price delver and how to integrate with this service.

## Data flow

Price delver works in following steps:
1. **Insert your products** - Price delver reads your Google Merchant Center product feed and fetches products from it.
2. **Define competitors** - You can define competitors you want Price Delver to regular check for price of your products. You can define both direct competitors or aggregators, which contain multiple competitors.
3. **Get Prices** - Price Delver get your competitors prices and saves them in service.
4. **Return all prices** - Price delver returns prices to your system via API. Currently you can call our API to return prices of all competitors for all your products.
5. **Return suggested prices** - Price delver will calculate prices based on your rules and return them for all the products together with all prices of all your competitors.

![Price Delver Data Flow](./img/force_spike_flow.png?raw=true)

## API Specification

> 🚧 Swagger version of this documentation will be ready soon. We apologize for inconvenience. 

### Base URL

All production endpoints are located on https://api.forcespike.com/v1/

Sandbox environment is available on https://sandbox-api.forcespike.com/v1/

### Authentication 

Force spike uses standard [Basic HTTP Authentication](https://en.wikipedia.org/wiki/Basic_access_authentication) for all operations.

Credentials can be obtained / changed via our support.

### 1. Insert your products

Price delver fetches your feed. We expect the feed to be in the format defined in [Google Merchant Feed specification](https://support.google.com/merchants/answer/7052112?hl=en)

In case our service will fail to read your feed 3 consecutive times our customer support will contact your administrator contact.

### 4&5. Get prices

`GET /products`

Returns all products available in Price Delver with all available prices. Endpoint can be called without limitation, but prices are updated based on your plan / contract (usually 24h).

Example:

```
Status: 200 OK
Content-Type: application/json

{
  "products": {
    "product": [
      {
        "active": true,
        "id": 0,
        "productName": "string",
        "dateAdded": "yyyy-MM-dd HH:mm:ss",
        "dateModified": "yyyy-MM-dd HH:mm:ss",
        "customerId": "string",
        "gtinIds": {
          "EAN": "sting",
          "ISBN": "sting",
          "GTIN": "sting"
        },
        "lastChecked": "yyyy-MM-dd HH:mm:ss",
        "prices": {
          "maxPrice": 100,
          "minPrice": 10,
          "recomendedPrice": 50,
          "currency": "EUR"
        },
        "competitors": {
          "competitor": [
            {
              "active": true,
              "id": 0,
              "competitorName": "string",
              "competitorUrl": "https://www.competitor.com/",
              "competitorProductName": "string",
              "competitorProductId": "string",
              "competitorProductUrl": "https://www.competitor.com/competitor-product-url",
              "lastChecked": "yyyy-MM-dd HH:mm:ss",
              "prices": {
                "price": 50,
                "currency": "EUR"
              }
            }
          ]
        },
        "aggregators": {
          "aggregator": [
            {
              "active": true,
              "id": 0,
              "aggregatorName": "string",
              "aggregatorUrl": "https://www.aggregator.com/",
              "aggregatorProductName": "string",
              "aggregatorProductId": "string",
              "aggregatorProductUrl": "https://www.aggregator.com/aggregator-product-url",
              "lastChecked": "yyyy-MM-dd HH:mm:ss",
              "prices": {
                "maxPrice": 100,
                "minPrice": 10,
                "currency": "EUR"
              },
              "aggregatorCompetitor": [
                {
                  "id": 0,
                  "aggregatorCompetitorName": "string",
                  "aggregatorCompetitorUrl": "https://www.aggregatorcompetitor.com/",
                  "prices": {
                    "price": 50,
                    "currency": "EUR"
                  }
                }
              ]
            }
          ]
        }
      }
    ]
  }
}
