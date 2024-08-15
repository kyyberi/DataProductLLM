# Prompts used

We use vanilla ChatGPT 4o and following prompts. These prompt captures the customer needs from fileflies recorded transripts (prefereably we have 3 cases) and summary it creates. 

The questions used in the customer interview are structured, but always the customer is not able to answer to all, at least not in the first attempt. Thus it is expected that the process has to be iterative and new interview happens after each time we have generated the outcomes: data product spec, data contract and commercial agreement. 


## Prompt 1: Capture data needs

Get the data needs from customer dialog. This will be used also in generating as code monitoring rules for data quality. 

## Prompt 2: Generate SLA

Generate ODPS SLA object based on customer dialog.  

**Prompt:**

https://chatgpt.com/share/f740a411-429c-4a0d-8b5d-74baa6425f1a 

**Sample output:** 

This SLA takes into account the specific requirements discussed, such as the need for high data accuracy, daily updates, and support availability during business hours. The objectives reflect the customer's expectations for data quality and support.

```yaml
SLA:
  - dimension: uptime
    displaytitle:
      - en: Uptime
    objective: 99
    unit: percent
    
  - dimension: latency
    displaytitle:
      - en: Data Update Latency
    objective: 1440
    unit: minutes

  - dimension: dataAccuracy
    displaytitle:
      - en: Data Accuracy
    objective: 98
    unit: percent

  - dimension: dataCompleteness
    displaytitle:
      - en: Data Completeness
    objective: 98
    unit: percent

  - dimension: updateFrequency
    displaytitle:
      - en: Update Frequency
    objective: 1440
    unit: minutes

  - dimension: errorRate
    displaytitle:
      - en: Error Rate
    objective: 0.2
    unit: percent

  - dimension: timeToRepair
    displaytitle:
      - en: Time to Repair
    objective: 12
    unit: hours

  - dimension: supportAvailability
    displaytitle:
      - en: Support Availability
    objective: 12
    unit: hours

  - dimension: endOfSupport
    displaytitle:
      - en: End of Support
    objective: 01/01/2025
    unit: date

support:
  phoneNumber: '+971508976456'
  phoneServiceHours: 'Mon-Fri 9am-9pm (GMT)'
  email: support@opendataproducts.org
  emailServiceHours: 'Mon-Fri 9am-9pm (GMT)'
  documentationURL: 'https://opendataproducts.org/documentation'

```

## Prompt 3: Pricing

Data Product Pricing Plans prompt: https://chatgpt.com/share/ebcac00e-49e3-4d46-84e2-889133eaa8a9 


```yaml
You are acting as product manager. Based on the dialog with customer, draft YAML pricing plans according to Open Data Product Specification. Provide just the PricingPlans component as YAML.  Make all plans to offer API access only. Schema of the Pricing plans object is available at https://opendataproducts.org/v3.0/schema/odps.yaml. Follow the Schema!

Here is an example of typical plans. Do not follow it precisely, take the customers needs into account and give enough variance in the plans:
pricingPlans:
  en:
  - name: Premium subscription 1 year
    priceCurrency: EUR
    price: 50.00
    billingDuration: year
    unit: recurring
    maxTransactionQuantity: unlimited
    offering:
      - High Quality Pets data
      - Unlimited transactions
      - Billed annually 
  - name: Premium Package Monthly
    priceCurrency: EUR
    price: 5.00
    billingDuration: month
    unit: recurring
    maxTransactionQuantity: unlimited
    offering:
      - High Quality Pets data
      - Unlimited transactions
      - Billed monthly 
  - name: Freemium Package
    priceCurrency: EUR
    price: 0.00
    billingDuration: month
    unit: recurring
    maxTransactionQuantity: 1000
    offering:
      - High Quality Pets data
      - Free to use, no cost at all!
      - Fair amount of transactions for testing and small business 
  - name: Revenue sharing
    priceCurrency: percentage
    price: 5.50
    billingDuration: month
    unit: revenue-sharing
    maxTransactionQuantity: 20000
    offering:
      - High Quality Pets data
      - No upfront fee
      - Billed monthly

DIALOG: 

Add here the transcript....

```

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


## Prompt 4: Generate Data Quality rules

## Prompt 5: Generate Data Contract

## Prompt x: Summary the results 

This prompt writes a summary of the results. This can be used in communicating with the customer. 