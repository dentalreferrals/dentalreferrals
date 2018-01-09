# Webhooks

When a referral is submitted to Dental Referrals Web Application, all medical forms are exported to PDF format and attached to the referral. The export to PDF happens in background queue after the referral is submitted and so isn't immediatelly available to the user. In order to assist the with collection of exported PDF forms, the [Prepare Form](#prepare-form) and [Save Draft](#save-draft) API methods accept a parameter called `files_ready_webhook_url` - a URL to trigger when the PDF forms become available for referrals submitted through the API.

If `files_ready_webhook_url` is provided during a call to [Prepare Form](#prepare-form) or [Save Draft](#save-draft) API method, when the PDF files become available, the system will send a POST request to the specified URL with the following payload in JSON format: 

`{
    "urn":"<URN_OF_REFERRAL>", 
    "files": ["<URL_OF_THE_PDF_FORM_1>", "<URL_OF_THE_PDF_FORM_2>", ... ]
}`
### Retry policy
If the webhook URL fails or returns a response code other than 200, we will retry 3 more times - in an hour, in 4 hours, and then in a day.

### Accessing PDF exports
In order to fetch the attachments using URLs that are sent to the `files_ready_webhook_url`, include an `Authorization: Token token=<YOUR_API_KEY>` header along with your request.
