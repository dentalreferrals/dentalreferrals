# Prepared Forms API

A central use-case for the API is to transition patient data from an external system into medical forms inside <a href="https://app.dental-referrals.org" target="_blank">Dental Referrals Web Application</a> while saving the effort of copy-pasting the data between two systems.

When used in a desktop application, a usual integration workflow consist of 2 steps:

1. Make a request to Dental Referrals API, passing patient data in the body of the request. In response, receive a unique URL to a medical form page inside <a href="https://app.dental-referrals.org" target="_blank">Dental Referrals Web Application</a> - that URL when opened will have all the submitted data pre-populated into its fields.

2. Use user's default browser or a built-in web view to open the unique medical form URL received in step 1 for user to submit the rest of the referral data.

<aside class="notice">
   The URL of the prepared form contains a unique token so that when the URL is opened in the browser for the first time, it authenticates the user that's linked to the API key for a 24-hour in browser session. The authentication token expires on first use, so if the URL is copy-pasted into a different browser, the system will as the user to authenticate.
</aside>

## List Available Forms

```shell
curl -H "Authorization: Token token=<YOUR_API_KEY>" "https://staging.dental-referrals.org/react4r/v2/available_forms"
```

> The above command returns a JSON array of form types:

```json
["AdultRestorativeForm","ChildOralCareForm"]
```

This API method lists all medical form types that are enabled for a given API key. One of these form types must be used for subsequent call to [Prepare Form](#prepare-form) API method.

### HTTP Request

`GET <API_ENDPOINT>/available_forms`

### Request Parameters

Does not require any input parameters.

### Response Format

The response is an array of medical form types. See example response in the right side pane.



## Prepare Form

> Prepare an Adult Restorative Referral form with pre-populated patient data:

```shell
curl -H "Authorization: Token token=<YOUR_API_KEY>" --data "form_type=AdultRestorativeForm&patient_title=Mr&patient_first_name=John&patient_last_name=Doe&patient_identifier=1&patient_dob=10/10/2010" "https://staging.dental-referrals.org/react4r/v2/prepare_form"
```

> The above command returns JSON structured as follows:

```json
{
    "status":"success",
    "prepared_form_url":"https://staging.dental-referrals.org/prepared_forms/show/d090e3dd-614f-47f3-8bc7-e403c1a2936e", 
    "urn":"TST0000001", 
    "ptguid":"NDJhNDJhNTJlOTZmOTE5YTdmYTc="
}
```

This API method prepares a medical form of a given type by pre-populating patient data into corresponding fields and assigning a unique URL to this pre-populated form.

### HTTP Request

`POST <API_ENDPOINT>/prepare_form`

### Request Parameters

Parameter | Type | Required | Default | Description
--------- | ---- | ---------| ------- | -----------
system_type | String | No | WD | Identifies which system is using the API. One of: `WD`, `WT`, `PD`, `PC`, `DN`, `DT`, `SE`.
form_type | String | Yes* | | Medical form type. Must be one of the types returned by [Available Forms](#list-available-forms) API method. <br><br>* If you are passing `system_type`=`SE` then this parameter is optional (the system will allow the user to select the form type in the browser once the prepared form URL is opened.) For all other system types, `form_type` parameter is required. 
patient_title | String | No | | Patient's title. Must be one of: `Mr`, `Mrs`, `Miss`, `Master`, `Dr`.
patient_first_name | String | No | | Patient's first name.
patient_last_name | String | No | | Patient's last name.
patient_dob | String | Yes || Patient’s date of birth, in the following format: `DD/MM/YYYY`.
patient_postcode | String | No || Patient's postcode.
patient_gp_code | String | No || GMP's code.
patient_address_line_1 | String | No || Patient's address line 1. If provided, will be concatenated with the other `address_line_x` parameters into a single address field on the medical form.
patient_address_line_2 | String | No || Patient's address line 2. If provided, will be concatenated with the other `address_line_x` parameters into a single address field on the medical form.
patient_address_line_3 | String | No || Patient's address line 3. If provided, will be concatenated with the other `address_line_x` parameters into a single address field on the medical form.
patient_town | String | No | | Patient's town or city.
patient_mobile_tel | String | No || Patient's mobile phone number.
patient_home_tel | String | No || Patient's home phone number.
patient_work_tel | String | No || Patient's work phone number.
patient_phone_number | String | No || If patient's phone number is present, but it is not clear if it's home, work, or mobile number, the phone number should go into this field.
permission_sms | Numeric | No || One of: `0`, `1`. The `1` means that we have permission to contact the patient on their mobile number via SMS.
nhs_number | Numeric | No || Patient's NHS Number.
gender | String | No || Patient’s gender. One of: `M`, `MALE`, `F`, `FEMALE`.
patient_gp_name | String | No || Patient's general practitioner's name. 
patient_gp_postcode | String | No || Patient's general practitioner's postcode. 
patient_gp_street_address | String | No || Patient's general practitioner's street address. 
patient_gp_town | String | No || Patient's general practitioner's city or town.
referrer_gdc | String | No || Referrer's GDC number.
referrer_name | String | No || Referrer's name.
urgent | Numeric | No || If set to `1`, the resulting medical form will have the Care Type set to `Urgent`, otherwise `Routine`.
medical_history_drugs | Text | No || List of patient's current medications in free text format.
medical_history_text | Text | No || Medical alert information or additional medical history information, in free text form.
medical_forms_as_single_file | Numeric | No || If set to `1`, the exported PDF medical forms will be merged into one file when notifying the `files_ready_webhook_url`. Check out [Webhooks](#webhooks) for more.
files_ready_webhook_url | Text | No || A callback URL to trigger when the PDF forms become available for this referral. For payload format, please see [Webhooks](#webhooks) section.
patient_identifier | String || No | Internal patient identifier within your system. If provided, this will be appended to your `API key`, out of which a `Base64` hash will be produced and stored as `PTGUID` (`Patient GUID`) within our system. This value is returned as part of the response and can later be used to query the status of submitted referral, same as the `URN` value.
fileupload_id | Numeric | No || An ID of the fileupload entity with pre-uploaded files to be tied to this referral. For more on fileuploads, refer to [Uploading Files](#uploading-files).

### Response Format

Parameter | Type | Description
--------- | ---- | ------------
status | String | One of: `success` or `error`.
prepared_form_url | String | The URL to the web page with a prepared medical form, where patient data supplied in the request is pre-filled into corresponding fields within the form.
urn | String | `URN` code of the referral that will be associated with this record once the prepared form is submitted by the user.
ptguid | String | `PTGUID` (`Patient GUID`) value that was created as a `Base64` hash of the `API KEY` + `patient_identifier` request parameter. This parameter will only be present in the response if patient_identifier was provided to the [Prepare Form](#prepare-form) request.

