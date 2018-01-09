# Authentication

### Header-based Authentication

> To authorize your API calls, use:

```shell
curl -H "Authorization: Token token=<YOUR_API_KEY>" "https://staging.dental-referrals.org/react4r/v2/"
```

> Expected response:

```json
{status: "success"}
```

> Make sure to replace `<YOUR_API_KEY>` with your actual API key.

Dental Referrals API uses API keys to allow access to the API. You may request an API key by emailing [office@dental-referrals.org](mailto:office@dental-referrals.org).

Every request to the API must include an API key in the header of the request as follows:

`Authorization: Token token=<YOUR_API_KEY>`

<aside class="notice">
You must replace <code>YOUR_API_KEY</code> with your personal API key.
</aside>

<aside class="notice">
Please note that API keys are not shared between staging and production environments. You must request a separate API key for each environment.
</aside>

### One API Key Per Medical Practice

Each end user (Medical Practice) must obtain their own API key that will be linked to their data on our system. This ensures no unauthorized access happens between the two separate medical practices.

<aside class="warning">
Thus any software using the Dental Referrals API must provide a way for the end user to specify their individual API key. No generic API key can be hard-coded into released software!
</aside>


### Authentication Errors
If a request fails authentication for any reason, a HTTP 401 Not Authorized response is returned. 