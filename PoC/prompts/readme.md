# Prompts used

We use vanilla ChatGPT 4o and following prompts. These prompt captures the customer needs eventually from fileflies recorded transripts (prefereably we have 3 cases) and summary it creates. In the beginning we use synthetic transcripts as input. 

The questions used in the customer interview are structured, but always the customer is not able to answer to all, at least not in the first attempt. Thus it is expected that the process has to be iterative and new interview happens after each time we have generated the outcomes: data product spec, data contract and commercial agreement. 

We have 6 prompts with predefined outputs: 

1. **Capture data needs.** 
      
      OUTPUTS as YAML: 
      - Schema of the data product (for Data Product API for consumption), 
      - Data description: databases and fields to use for generating data contract and DQ rules as code,  
      - needed SQLs to access data for pipeline generation. 

      INPUTS:
      - customer dialog

2. **Generate SLA.** 

      OUTPUTS as YAML: 
      - SLA object in ODPS format

      INPUTS: 
      - customer dialog
      - Provider fixed details for support: email service hours, phone support hours, phone number, email.

3. **Generate Pricing plans.** 

      OUTPUTS as YAML: 
      - Pricing object in ODPS format

      INPUTS: 
      - customer dialog

4. **Generate Data Contract skeleton.**

      OUTPUTS as YAML: 
      - Data Contract in ODCS format

      INPUTS: 
      - customer dialog

5. **Generate Data Quality object**

      OUTPUTS as YAML: 
      - YAML 

      INPUTS: 
      - Data Description from prompt 1

5. **Generate Licensing. Outputs as YAML:**
      - Licensing object in ODPS format
6. **Generate Commercial Agreement. Outputs as YAML:**
      - Commercial Agreement containing Licencing, Pricing, agreed DQ and SLA levels according to ODPS. 
7. **Summarize of the results. Outputs as text:** 
      - a summary of the results. Summarize the customer case and needs. Then explain the data product matching the needs and contributing the customer goals.

## Prompt 1: Capture data needs

Get the data needs from customer dialog. In this prompt we also need available data metadata in order to match customer needs and offering. 

The result should function as an input to DQ rules generation. One of the aims is to generate data quality "as code" checks with tools like SodaCL. Ask AI is a tool available to generate production-ready SodaCL checks. Of course other tools like MonteCarlo must be supported in the future. 

The result should also enable creation of Schema for the data product. Schema will be used in the API exposing data product to customer. By default now in this PoC data products are offered and consumed via API, not direct SQL access. It also assumed here that data product provider has standardized the offering API eg. have one REST API for all the data products. In the result Schema defines the content of this specific data product in the API. 

**Prompt:**

```yaml
You are acting as product manager. Based on the dialog with customer, identify customer data needs as fields and output YAML description of those, include only database level description. Also generate SQL to access data.  Finally, generate Schema for the needed data. Schema will be used in REST API offering the data product to the customer. 


DIALOG: 
Role 1: Data Product Seller (Interviewer)

Question 1: Purpose of the Data Product

"Thank you for meeting with me today. To start off, could you please describe the purpose of the data product you're looking to acquire? Specifically, what is the context in which this data product will be used? Are you aiming to support decision-making processes, enhance operations, or something else? Also, can you elaborate on the specific value you expect to create with this data product? For example, do you anticipate that it will help save costs, increase sales, or reduce risks? Any estimates on the financial impact?"
Role 2: Customer (Respondent)

"Thank you for asking. We're looking to use this data product in the context of enhancing our customer segmentation strategy for our e-commerce platform. The main purpose is to improve the personalization of our marketing campaigns, which in turn should increase customer engagement and boost sales. The data product will be central to decision-making, particularly in analyzing customer behavior and predicting purchasing trends.

In terms of value, we expect this data product to drive a 15% increase in conversion rates over the next year. This could potentially translate to an additional $50000  in revenue. Moreover, by better targeting our campaigns, we also anticipate a reduction in marketing costs by about 10%, which would amount to approximately $500 in savings."
Role 1: Data Product Seller (Interviewer)

"That's great to hear. Who do you see as the typical users of this data product? Are they primarily analysts, or do you foresee other roles utilizing this data? Additionally, can you explain what specific gap you aim to fill with this data product?"
Role 2: Customer (Respondent)

"The primary users will be our data analysts and marketing teams. The analysts will use the data to develop models and derive insights, while the marketing teams will apply these insights to craft targeted campaigns. The gap we aim to fill is the lack of real-time, granular customer data that can help us personalize our marketing efforts more effectively. Currently, our data is too aggregated, which leads to generic campaigns that don't resonate as well with our customers."
Role 1: Data Product Seller (Interviewer)

Question 2: What is the Content?

"Moving on to the content of the data, what specific data points or types of data are you looking for? Are there any particular formats or structures you require the data in? And do you have any specific standards, regulatory requirements, or benchmarks that the data must meet?"
Role 2: Customer (Respondent)

"We're looking for data that includes detailed customer profiles, purchase history, browsing behavior, and demographic information such as age, gender, location, and income level. The data should also include behavioral indicators, like time spent on the site, click-through rates, and abandoned cart details.

As for the format, a CSV or an API that we can easily integrate into our existing systems would be ideal. In terms of standards, the data needs to comply with GDPR regulations since we operate in Europe. It should also meet internal benchmarks for data completeness and accuracy, with at least 95% accuracy in demographic information and no more than 2% missing data points."
Role 1: Data Product Seller (Interviewer)

Question 3: Data Quality and SLA

"Understood. Regarding data quality, what level of accuracy and reliability do you require for this data? How frequently should the data be updated or refreshed? What is your tolerance for data latency, and are there any other data quality dimensions that require attention? Lastly, do you have specific service-level agreements (SLAs) in mind for data delivery and support, such as channels and support hours?"
Role 2: Customer (Respondent)

"We need the data to be highly accurate—at least 98% reliability for critical data points like purchase history and demographic details. The data should be refreshed daily to ensure we are working with the most up-to-date information.

Regarding latency, we can tolerate up to 24 hours of delay in data updates, but anything beyond that would start to impact our operations. We also need the data to be consistent and free from duplication, with a low error rate. In terms of SLAs, we expect 99% uptime for data access, with support available during our business hours, which are 9 AM to 9 PM GMT. We would also appreciate a dedicated support channel for urgent issues."
Role 1: Data Product Seller (Interviewer)

Question 4: Pricing

"Thank you for that detailed information. Now, let’s discuss pricing. What budget do you have allocated for data acquisition? Are you open to different pricing models, such as a subscription model, a one-time purchase, or a usage-based pricing model? Would you consider a freemium model to test the data before committing fully?"
Role 2: Customer (Respondent)

"We have no clear annually for data acquisition. We are open to a subscription model, especially if it includes regular updates and support. A usage-based model could also work, provided it aligns with our expected data usage patterns. We would definitely be interested in a freemium model, allowing us to test the data quality and relevance before making a full commitment."
```

**Sample output:** 


```yaml

Schema as object

type: "object"
properties:
  customer_profiles:
    type: "object"
    properties:
      customer_id:
        type: "string"
        description: "Unique identifier for each customer"
      name:
        type: "string"
        description: "Full name of the customer"
      age:
        type: "integer"
        description: "Age of the customer"
      gender:
        type: "string"
        description: "Gender of the customer"
      location:
        type: "string"
        description: "Location of the customer, including city and country"
      income_level:
        type: "string"
        description: "Approximate income level of the customer"
      email:
        type: "string"
        description: "Email address of the customer"

  purchase_history:
    type: "object"
    properties:
      purchase_id:
        type: "string"
        description: "Unique identifier for each purchase"
      customer_id:
        type: "string"
        description: "Unique identifier for each customer"
      product_id:
        type: "string"
        description: "Unique identifier for each product"
      purchase_date:
        type: "string"
        format: "date-time"
        description: "Date and time of purchase"
      purchase_amount:
        type: "number"
        format: "float"
        description: "Total amount spent on the purchase"

  browsing_behavior:
    type: "object"
    properties:
      customer_id:
        type: "string"
        description: "Unique identifier for each customer"
      session_id:
        type: "string"
        description: "Unique identifier for each browsing session"
      time_spent:
        type: "integer"
        description: "Total time spent on the site during the session"
      pages_visited:
        type: "array"
        items:
          type: "string"
        description: "List of pages visited during the session"
      click_through_rate:
        type: "number"
        format: "float"
        description: "Ratio of clicks to impressions"
      abandoned_cart:
        type: "boolean"
        description: "Indicates whether the cart was abandoned (true/false)"

  demographic_information:
    type: "object"
    properties:
      customer_id:
        type: "string"
        description: "Unique identifier for each customer"
      age:
        type: "integer"
        description: "Age of the customer"
      gender:
        type: "string"
        description: "Gender of the customer"
      location:
        type: "string"
        description: "Location of the customer, including city and country"
      income_level:
        type: "string"
        description: "Approximate income level of the customer"


```


## Prompt 2: Generate SLA

Generate ODPS SLA object based on customer dialog.  This SLA takes into account the specific requirements discussed, such as the need for high data accuracy, daily updates, and support availability during business hours. The objectives reflect the customer's expectations for data quality and support.

**Prompt:**

```yaml
You are acting as product manager. Based on the dialog with customer, draft SLA  according to Open Data Product Specification. Provide just the SLA component as YAML.  Schema of the SLA object is available at https://opendataproducts.org/v3.0/schema/odps.yaml. Follow the Schema!

Here is an example of typical SLA object. Do not follow it precisely, take the customers needs into account and give enough variance in the result:


SLA:
  - dimension: latency
    displaytitle:
      - en: Latency
    objective: 100
    unit: milliseconds
  
  - dimension: uptime
    displaytitle:
      - en: Uptime
    objective: 99
    unit: percent
    
  - dimension: responseTime
    objective: 200
    unit: milliseconds
    

  - dimension: updateFrequency
    objective: 30
    unit: minutes
    
  - dimension: errorRate
    objective: 0.1
    unit: percent

  - dimension: endOfSupport
    objective: 01/01/2025 # dd/mm/yyyy
    unit: date

  - dimension: endOfLife
    objective: 01/03/2025 # dd/mm/yyyy
    unit: date

  - dimension: timeToDetect
    objective: 60
    unit: minutes

  - dimension: timeToNotify
    objective: 120
    unit: minutes

  - dimension: timeToRepair
    objective: 24
    unit: hours

  - dimension: emailResponseTime
    objective: 12
    unit: hours

  support:
      phoneNumber: '+971508976456'
      phoneServiceHours: 'Mon-Fri 8am-4pm (GMT)'
      email: support@opendataproducts.org
      emailServiceHours: 'Mon-Fri 8am-4pm (GMT)'
      documentationURL: ''


DIALOG: 
Role 1: Data Product Seller (Interviewer)

Question 1: Purpose of the Data Product

"Thank you for meeting with me today. To start off, could you please describe the purpose of the data product you're looking to acquire? Specifically, what is the context in which this data product will be used? Are you aiming to support decision-making processes, enhance operations, or something else? Also, can you elaborate on the specific value you expect to create with this data product? For example, do you anticipate that it will help save costs, increase sales, or reduce risks? Any estimates on the financial impact?"
Role 2: Customer (Respondent)

"Thank you for asking. We're looking to use this data product in the context of enhancing our customer segmentation strategy for our e-commerce platform. The main purpose is to improve the personalization of our marketing campaigns, which in turn should increase customer engagement and boost sales. The data product will be central to decision-making, particularly in analyzing customer behavior and predicting purchasing trends.

In terms of value, we expect this data product to drive a 15% increase in conversion rates over the next year. This could potentially translate to an additional $50000  in revenue. Moreover, by better targeting our campaigns, we also anticipate a reduction in marketing costs by about 10%, which would amount to approximately $500 in savings."
Role 1: Data Product Seller (Interviewer)

"That's great to hear. Who do you see as the typical users of this data product? Are they primarily analysts, or do you foresee other roles utilizing this data? Additionally, can you explain what specific gap you aim to fill with this data product?"
Role 2: Customer (Respondent)

"The primary users will be our data analysts and marketing teams. The analysts will use the data to develop models and derive insights, while the marketing teams will apply these insights to craft targeted campaigns. The gap we aim to fill is the lack of real-time, granular customer data that can help us personalize our marketing efforts more effectively. Currently, our data is too aggregated, which leads to generic campaigns that don't resonate as well with our customers."
Role 1: Data Product Seller (Interviewer)

Question 2: What is the Content?

"Moving on to the content of the data, what specific data points or types of data are you looking for? Are there any particular formats or structures you require the data in? And do you have any specific standards, regulatory requirements, or benchmarks that the data must meet?"
Role 2: Customer (Respondent)

"We're looking for data that includes detailed customer profiles, purchase history, browsing behavior, and demographic information such as age, gender, location, and income level. The data should also include behavioral indicators, like time spent on the site, click-through rates, and abandoned cart details.

As for the format, a CSV or an API that we can easily integrate into our existing systems would be ideal. In terms of standards, the data needs to comply with GDPR regulations since we operate in Europe. It should also meet internal benchmarks for data completeness and accuracy, with at least 95% accuracy in demographic information and no more than 2% missing data points."
Role 1: Data Product Seller (Interviewer)

Question 3: Data Quality and SLA

"Understood. Regarding data quality, what level of accuracy and reliability do you require for this data? How frequently should the data be updated or refreshed? What is your tolerance for data latency, and are there any other data quality dimensions that require attention? Lastly, do you have specific service-level agreements (SLAs) in mind for data delivery and support, such as channels and support hours?"
Role 2: Customer (Respondent)

"We need the data to be highly accurate—at least 98% reliability for critical data points like purchase history and demographic details. The data should be refreshed daily to ensure we are working with the most up-to-date information.

Regarding latency, we can tolerate up to 24 hours of delay in data updates, but anything beyond that would start to impact our operations. We also need the data to be consistent and free from duplication, with a low error rate. In terms of SLAs, we expect 99% uptime for data access, with support available during our business hours, which are 9 AM to 9 PM GMT. We would also appreciate a dedicated support channel for urgent issues."
Role 1: Data Product Seller (Interviewer)

Question 4: Pricing

"Thank you for that detailed information. Now, let’s discuss pricing. What budget do you have allocated for data acquisition? Are you open to different pricing models, such as a subscription model, a one-time purchase, or a usage-based pricing model? Would you consider a freemium model to test the data before committing fully?"
Role 2: Customer (Respondent)

"We have no clear annually for data acquisition. We are open to a subscription model, especially if it includes regular updates and support. A usage-based model could also work, provided it aligns with our expected data usage patterns. We would definitely be interested in a freemium model, allowing us to test the data quality and relevance before making a full commitment."
```

**Sample output:** 

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


## Prompt 4: Generate Data Contract

## Prompt 5: Generate Commercial Agreement

## Prompt x: Summary the results 

This prompt writes a summary of the results. This can be used in communicating with the customer. Summarize the customer case and needs. Then explain the data product matching the needs and contributing the customer goals. 