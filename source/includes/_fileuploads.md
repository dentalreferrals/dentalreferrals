# Uploading Files

Both [Prepare Form](#prepare-form) and [Save Draft](#save-draft) API methods allow an optional `fileupload_id` parameter that allows attaching files to the referral. In order to attach files, they must be pre-uploaded before making a call to [Prepare Form](#prepare-form) or [Save Draft](#save-draft) API methods - so that when a medical form page opens, the user can see the files that were already uploaded.

**The steps needed to upload files along with referral data are:**

1. Make a call to [Prepare Fileupload](#prepare-fileupload) API method and receive an ID of newly created `fileupload` entity in response.

2. Use the `<FILEUPLOAD_ID>` obtained in step 1 to upload actual files to the server.

3. Supply the `<FILEUPLOAD_ID>` as the `fileupload_id` parameter to the [Prepare Form](#prepare-form) and [Save Draft](#save-draft) API methods.

## Prepare Fileupload

> Create a fileupload entity, to which further uploads will be linked:

```shell
curl -H "Authorization: Token token=<YOUR_API_KEY" "https://staging.dental-referrals.org/react4r/v2/prepare_fileupload"
```

> The above command returns JSON structured as follows:

```json
{
	"status":"success",
	"fileupload_id":396
}
```

Prepares a fileupload entity to link further uploads to. The 396 is the `FILEUPLOAD_ID` to be used in the call to [Add File](#add-file) API method.

### HTTP Request

`GET|POST <API_ENDPOINT>/prepare_fileupload`

### Request Parameters

Does not require any request parameters.

### Response Format

Parameter | Type | Description
--------- | ---- | ------------
status | String | One of: `success` or `error`
filupload_id | Numeric | The `FILEUPLOAD_ID` to be used in the call to [Add File](#add-file) API method


## Add File

> Upload a file and link it to a previously prepared fileupload enpoint:

```shell
curl -H "Authorization: Token token=<YOUR_API_KEY>" -H "Upload-Content-Type:image/png" -H "Upload-Filename:email_sample.png" -H "Upload-Kind:radiograph" -H "Upload-Date:29/01/2016" -v -F file=@/home/user/file.png "https://staging.dental-referrals.org/react4r/v2/fileupload/396/add_file"
```

With the `FILEUPLOAD_ID` obtained from [Prepare Fileupload](#prepare-fileupload) API call, this method allows to upload files and linked them all to the same `FILEUPLOAD_ID`.

### HTTP Request

`POST <API_ENDPOINT>/fileupload/<FILEUPLOAD_ID>/add_file`

### Allowed Headers:

Header | Required | Default | Description
------ | -------- | ------- | ------------
`Upload-Content-Type` | Yes | | MIME type of the uploaded file.
`Upload-Filename`| Yes | | Name of the original file, with extension.
`Upload-Kind` | No | `other` | Type of the upload. At the moment, only 2 values are allowed: `radiograph` and `other`.
`Upload-Date`| Required if `Upload-Kind` set to `radiograph` | | Date when the radiograph has been taken, in `DD/MM/YYYY` format.

### Request Parameters

Does not require any request parameters.

### Response Format

Parameter | Type | Description
--------- | ---- | ------------
status | String | One of: `success` or `error`

## Tying Everything Together

Now that files are uploaded to the server and tied to a single `<FILEUPLOAD_ID>`, simply use that `<FILEUPLOAD_ID>` as the value for `fileupload_id` parameter in [Prepare Form](#prepare-form) and [Save Draft](#save-draft) API methods.

