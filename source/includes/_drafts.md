# Drafts API

## Save Draft

> Prepare an Adult Restorative Referral form with pre-populated patient data:

```shell
curl -H "Authorization: Token token=<YOUR_API_KEY>" --data "form_type=AdultRestorativeForm&patient_title=Mr&patient_first_name=John&patient_last_name=Doe&patient_identifier=1" "https://staging.dental-referrals.org/react4r/v2/save_draft"
```

> The above command returns JSON structured as follows:

```json
{
    "status":"success",
    "draft_id":11047,
    "urn":"TST0000001",
    "ptguid":"NDJhNDJhNTJlOTZmOTE5YTdmYTc="
}
```

This API method allows to save drafts of medical forms within <a href="https://app.dental-referrals.org" target="_blank">Dental Referrals Web Application</a>, linking these drafts to a user's account that is tied to the API key.

### HTTP Request

`POST <API_ENDPOINT>/save_draft`

### Request Parameters

Exactly the same request parameters are available as for the [Prepare Form](#prepare-form) API method.

### Response Format

Parameter | Type | Description
--------- | ---- | ------------
status | String | One of: `success` or `error`.
draft_id | Numeric | The unique numeric reference of the draft within <a href="https://app.dental-referrals.org" target="_blank">Dental Referrals Web Application</a>.
urn | String | `URN` code of the referral that will be associated with this record once the draft is submitted by the user.
ptguid | String | `PTGUID` (`Patient GUID`) value that was created as a `Base64` hash of the `API KEY` + `patient_identifier` request parameter. This parameter will only be present in the response if patient_identifier was provided to the [Save Draft](#save-draft) request.