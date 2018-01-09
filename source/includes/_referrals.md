# Referrals API


## Referral Status

> Check the status of a single Referral using a URN code:

```shell
curl -H "Authorization: Token token=<YOUR_API_KEY>" "https://staging.dental-referrals.org/react4r/v2/referral_status?urn=TST0000001"
```

> The above command returns JSON structured as follows:

```json
{
    "status":"success",
    "referral_status":"Sent to: St Clements Dental Suite - Mr E Harunani",
    "referrer_name":"Nitin Joshi",
    "referral_reason":"Minor Oral Surgery",
    "referral_files":["LINK_1","LINK_2"]
}

```

Use this API method to check the status of one specific referral that:

1. has been submitted through this API
2. has been submitted using the API key linked to the same user that the API key for this request

### HTTP Request

`GET <API_ENDPOINT>/referral_status`

### Request Parameters

Parameter | Type | Required | Description
--------- | ---- | ---------| -----------
urn | String | One of: `urn`, `ptguid` is required | `URN` code of the referral to return the status for.
ptguid | String | One of: `urn`, `ptguid` is required | `PTGUID` (`Patient GUID`) code of the referral to return the status for.

### Response Format

Parameter | Type | Description
--------- | ---- | ------------
status | String | One of: `success` or `error` - indicates whether your API request is successful or not.
referral_urn | String | Unique 10-symbol identifier of the referral within Dental Referrals online system.
referral_status | String | The current status of this referral in Dental Referrals online system.
referral_name | String | Name of the referring dentist.
referral_reason | String | The reason this referral was submitted.
referral_files | Array of Strings | Unique 10-symbol identifier of the referral within Dental Referrals online system.





## Batch Referral Status

> Check the status of a batch of recent referrals

```shell
curl -H "Authorization: Token token=<YOUR_API_KEY>" "https://staging.dental-referrals.org/react4r/v2/batch_referral_status?count=2"
```

> The above command returns JSON structured as follows:

```json
{
    "status":"success",
    "referral_data": 
    [
        {
        "referral_urn":"TST0000001", 
        "referral_status":"Entered on System", 
        "referrer_name":"Jane Doe", 
        "referral_reason":"Restorative", 
        "referral_files":["LINK_1","LINK_2"]
        },

		{
		"referral_urn":"TST0000002",
		"referral_status":"Sent to: St Clements Dental Suite", 
		"referrer_name":"Jane Doe", 
		"referral_reason":"Restorative", 
		"referral_files":["LINK_1"]
		}
	]
}
```

Use this API method to check the status of up to 100 referrals most recently submitted through the API using the given API key.

### HTTP Request

`GET <API_ENDPOINT>/batch_referral_status`

### Request Parameters

Parameter | Type | Required | Value Bounds | Description
--------- | ---- | ---------| ------ | ------------
count | Numeric | Yes | If larger than 100, will be set to 100 anyway | The number of most recent referrals to return statuses for.

### Response Format

Parameter | Type | Description
--------- | ---- | ------------
status | String | One of: `success` or `error` - indicates whether your API request is successful or not.
referral_data | JSON Array | A JSON array of referral statuses, each status - a JSON object with the following fields:<br>`referral_urn` - Unique 10-symbol identifier of the referral within Dental Referrals online system.<br>`referral_status` - The current status of this referral in Dental Referrals online system.<br>`referrer_name` - Name of the referring dentist.<br>`referral_reason` - The reason this referral was submitted<br>`referral_files` - an array of URLs to PDF files for this referral.



## Referral Status By Patient

> Check the status of all referrals that belong to a patient, using PTGUID

```shell
curl -H "Authorization: Token token=<YOUR_API_KEY>" "https://staging.dental-referrals.org/react4r/v2/patient_referrals?count=2&ptguid=NDJhNDJhNTJlOTZmOTE5YTdmYTc="
```

> The above command returns JSON structured as follows:

```json
{
    "status":"success",
    "referral_data": 
    [
        {
        "referral_urn":"TST0000001", 
        "referral_status":"Entered on System", 
        "referrer_name":"Jane Doe", 
        "referral_reason":"Restorative", 
        "referral_files":["LINK_1","LINK_2"]
        },

		{
		"referral_urn":"TST0000002",
		"referral_status":"Sent to: St Clements Dental Suite", 
		"referrer_name":"Jane Doe", 
		"referral_reason":"Restorative", 
		"referral_files":["LINK_1"]
		}
	]
}
```

Use this API method to check the status of most recent referrals submitted for a given `PTGUID` (which is created as a `Base64` of an API key and `patient_identifier` parameter used in [Prepare Form](#prepare-form) and [Save Draft](#save-draft) API calls.)

### HTTP Request

`GET <API_ENDPOINT>/patient_referrals`

### Request Parameters

Parameter | Type | Required | Default | Value Bounds | Description
--------- | ---- | ---------| ------- |------------- | ------------
ptguid | String | Yes | | | `PTGUID` (`Patient GUID`) code of the referral to return the status for.
count | Numeric | No | 100 | If larger than 100, will be set to 100 anyway | Maximum number of recent patient's referrals to return statuses for.

### Response Format

Parameter | Type | Description
--------- | ---- | ------------
status | String | One of: `success` or `error` - indicates whether your API request is successful or not.
referral_data | JSON Array | A JSON array of referral statuses, each status - a JSON object with the following fields:<br>`referral_urn` - Unique 10-symbol identifier of the referral within Dental Referrals online system.<br>`referral_status` - The current status of this referral in Dental Referrals online system.<br>`referrer_name` - Name of the referring dentist.<br>`referral_reason` - The reason this referral was submitted<br>`referral_files` - an array of URLs to PDF files for this referral.
