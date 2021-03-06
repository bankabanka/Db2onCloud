---

copyright:
  years: 2014, 2021
lastupdated: "2021-03-05"

keywords: 

subcollection: Db2onCloud

---

<!-- Attribute definitions --> 
{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:pre: .pre}

# Migrating data to IBM Cloud
{: #migration}

You can load data from a data file in a delimited format (CSV or TXT) on a local network or in an object store (Amazon S3 or IBM Cloud Object Storage) to {{site.data.keyword.Db2_on_Cloud_long}}. You can even migrate your data from an on-premises system.
{: shortdesc}

## Loading data from object store
{: #cos}

To load data from Amazon S3, select one of the following methods:
  * {{site.data.keyword.Db2_on_Cloud_short}} web console. **Load > Amazon S3**. 
  * External Tables directly. The following is an example SQL statement:

    ```
    INSERT INTO <table-name> SELECT * FROM EXTERNAL '<mys3file.txt>' USING
      (CCSID 1208 s3('s3.amazonaws.com', 
      '<S3-access-key-ID>',
      '<S3-secret-access-key>', 
      '<my_bucket>'
         )
      )      
    ```

To load data from IBM Cloud Object Storage by using External Tables directly, the following is an example SQL statement:

```
INSERT INTO <table-name> SELECT * FROM EXTERNAL '<mys3file.txt>' USING
  (CCSID 1208 s3('s3-api.us-geo.objectstorage.softlayer.net', 
  '<S3-access-key-ID>',
  '<S3-secret-access-key>', 
  '<my_bucket>'
     )
  )      
```

For IBM Cloud Object Storage, to create HMAC credentials when creating new service credentials, specify {"HMAC:true"} in the *Add Inline Configuration Parameters* field.
{: note}

## Migrating large amounts of data from on-premises system
{: #onprem}

<!--
To migrate 25 TB of data and greater: see [IBM Cloud Mass Data Migration](#mdms)
-->

To migrate your data from an on-premises system, choose one of the following methods depending on the size of your data set:
* Less than 25 TB of data: [IBM Lift CLI](#lift)
* 25 TB of data and greater: [IBM Cloud Mass Data Migration](#mdms)

### Lift CLI
{: #lift}

Lift CLI support is coming soon.

The following workaround can be used to perform the function similar to a **lift put** command.

- *url* = your console URL
- *crn* = service instance CRN
- *user* = Db2 user
- *password* = Db2 password

The following example code snippets are in `curl` and `jq`:

1. Retrieve your token:

   ```
   token=`curl -s -X POST https://$url/dbapi/v4/auth/tokens -H 'content-type: application/json' -H "x-deployment-id: $crn" -d "{\"userid\": \"$user\", \"password\": \"$password\"}" | jq -r '.token'`
   ```
   {: codeblock}

2. If you have a `test.csv` file in your local path and you want to upload it to {{site.data.keyword.Db2_on_Cloud_short}}:

   ```
   curl -s -X POST https://$url/dbapi/v4/home_content/ -H "authorization: Bearer $token" -H "x-deployment-id: $crn" -H "Content-Type: multipart/form-data" -F 'test.csv=@test.csv'
   ```
   {: codeblock}

For code snippet examples in other programming languages, see [{{site.data.keyword.Db2_on_Cloud_short}} API](https://cloud.ibm.com/apidocs/db2-on-cloud/db2-on-cloud-v4#introduction){: external}.

<!--
The Lift CLI is an application that you can use without charge to migrate your data to the {{site.data.keyword.Bluemix_notm}} from the various data sources listed in Table 1. 

| Target database on IBM Cloud | Data source |
|------------------------------|-------------|
| {{site.data.keyword.Db2_on_Cloud_long_notm}}   | IBM Db2 |
|                              | Oracle Database |
|                              | Microsoft SQL Server |
|                              | CSV file format |
{: caption="Table 1. Migration data sources" caption-side="top"}

To download and install Lift CLI, see: [Download Lift CLI](https://www.lift-cli.cloud.ibm.com/#download){:external}.

For step-by-step instructions about migrating your data to the {{site.data.keyword.Bluemix_notm}} by using Lift CLI, see: [Migrate data to {{site.data.keyword.Db2_on_Cloud_long_notm}}](https://www.lift-cli.cloud.ibm.com/#docs){:external}.
-->

### IBM Cloud Mass Data Migration
{: #mdms}

A fast, simple, secure way to physically transfer terabytes to petabytes of data to the {{site.data.keyword.Bluemix_notm}}. Mass Data Migration is a mobile storage device with 120 TB of usable storage capacity to accelerate moving data to the {{site.data.keyword.Bluemix_notm}}. Overcome common transfer challenges like high costs, long transfer times, and security concerns – all in a single service.

![View of the Mass Data Migration device](images/mdms.svg "View of the Mass Data Migration device"){: caption="Figure 1. View of the Mass Data Migration device" caption-side="bottom"}

For more information about the Mass Data Migration device, see: [Getting started tutorial](/docs/mass-data-migration?topic=mass-data-migration-getting-started-tutorial){:external}.

