# Updating From Earlier Versions

## Updating from Version 1 of the API

### API Endpoints

We removed the `/refadmin/` portion from all our application URLs and the staging endpoint is now served via `HTTPS`. 

**So, the staging API endpoint used to be:**

`http://staging.dental-referrals.org/refadmin/react4r/v1/`

**and now becomes:**

`https://staging.dental-referrals.org/react4r/v2/`

<br>

**The production API endpoint used to be:**

`https://app.dental-referrals.org/refadmin/react4r/v1/`

**and now becomes:**

`https://app.dental-referrals.org/react4r/v2/`

## Authentication

We are moving the authentication from URL parameter to a header-based model.

And so the old way of authentication:

`curl http://staging.dental-referrals.org/refadmin/react4r/v1/available_forms?api_key=<YOUR_API_KEY>`

Now becomes:

`curl -H "Authorization: Token token=<YOUR_API_KEY>" https://staging.dental-referrals.org/react4r/v2/available_forms`

This way of passing the API KEY in the header of the request applies to all API V2 methods.

## API Method Changes

Previously existing API methods are backward-compatible and should just continue to work once URLs are changed and authentication mechanism updated:

- [List Available Forms](#list-available-forms)
- [Referral Status](#referral-status)
- [Batch Referral Status](#batch-referral-status)
- [Referral Status By Patient](#referral-status-by-patient)
- [Prepare Form](#prepare-form)
- [Save Draft](#save-draft)

The only other change is that [Save Draft](#save-draft) and [Prepare Form](#prepare-form) methods now support an optional `fileupload_id` parameter.

In addition to the existing methods, 2 new API methods have been introduced as a part of [Fileupload Functionality](#uploading-files).

