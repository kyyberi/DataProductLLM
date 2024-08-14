# Prompts used

We use vanilla ChatGPT 4o and following prompts.

## Prompt 1: Capture customer needs from Fireflies data

This prompt captures the customer needs from fileflies recorded transripts (prefereably we have 3 cases) and summary it creates. 
The questions used in the customer interview are structured, but always the customer is not able to answer to all, at least not in the first attempt. Thus it is expected that the process has to be iterative and new interview happens after each time we have generated the outcomes: data product spec, data contract and commercial agreement. 


**Sample output:** 


**Prompt:**


## Prompt 2: 

## Prompt 3: Pricing

Data Product Pricing Plans prompt: https://chatgpt.com/share/ebcac00e-49e3-4d46-84e2-889133eaa8a9 

**Sample output:**

```yaml
pricingPlans:
  en:
  - name: Enterprise Subscription
    priceCurrency: USD
    price: 120000.00
    billingDuration: year
    unit: recurring
    maxTransactionQuantity: unlimited
    offering:
      - API access to detailed customer profiles
      - Daily data refresh
      - 99% uptime guarantee
      - Dedicated support channel during business hours
      - GDPR-compliant data
      - Integration support
      
  - name: Professional Package
    priceCurrency: USD
    price: 12000.00
    billingDuration: month
    unit: recurring
    maxTransactionQuantity: unlimited
    offering:
      - API access to customer profiles and behavior data
      - Weekly data refresh
      - 98% data accuracy guarantee
      - Support during business hours
      - GDPR-compliant data
      
  - name: Freemium Access
    priceCurrency: USD
    price: 0.00
    billingDuration: month
    unit: recurring
    maxTransactionQuantity: 5000
    offering:
      - API access to a limited dataset of customer profiles
      - Monthly data refresh
      - Fair usage limit for testing and small-scale operations
      - GDPR-compliant data
```

```yaml
pricingPlans:
  en:
  - name: Enterprise Subscription Annual
    priceCurrency: USD
    price: 120000.00
    billingDuration: year
    unit: recurring
    maxTransactionQuantity: unlimited
    offering:
      - High-Accuracy Customer Data
      - Daily Data Refresh
      - 98% Data Reliability
      - GDPR Compliance
      - Dedicated Support Channel (9 AM to 9 PM GMT)
      - 99% Uptime Guarantee

  - name: Enterprise Subscription Monthly
    priceCurrency: USD
    price: 11000.00
    billingDuration: month
    unit: recurring
    maxTransactionQuantity: unlimited
    offering:
      - High-Accuracy Customer Data
      - Daily Data Refresh
      - 98% Data Reliability
      - GDPR Compliance
      - Dedicated Support Channel (9 AM to 9 PM GMT)
      - 99% Uptime Guarantee

  - name: Freemium Package
    priceCurrency: USD
    price: 0.00
    billingDuration: month
    unit: recurring
    maxTransactionQuantity: 2000
    offering:
      - Limited Access to Customer Data
      - Weekly Data Refresh
      - 95% Data Reliability
      - GDPR Compliance
      - Access for Testing and Small Campaigns

  - name: Usage-Based Pricing
    priceCurrency: USD
    price: 0.50
    billingDuration: month
    unit: per-transaction
    maxTransactionQuantity: unlimited
    offering:
      - High-Accuracy Customer Data
      - Daily Data Refresh
      - 98% Data Reliability
      - GDPR Compliance
      - Pay as You Go
```


## Prompt 4:

## Prompt 5:

## Prompt 6:

## Prompt 7:

## Prompt 8: