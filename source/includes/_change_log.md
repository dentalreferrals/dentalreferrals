# Change Log

Date | Author | Description
---- | ------ | ------------
<nobr>1 Sep 2020</nobr> | Serhiy Pustovit | `patient_dob` now is required field for `prepare_form` and `save_draft` endpoints. 422 error can be returned on invalid DOB. Fixed typos.
<nobr>2 Jul 2018</nobr> | Irina Zayats | Added new `system_type` - `SE`. For [Prepare Form](#prepare-form) API method, if `SE` is passed as `system_type` then `form_type` parameter becomes optional and will allow the user to select the form type in the browser once the prepared form URL is opened. This does not apply to the [Save Draft](#save-draft) API method.
<nobr>9 Jan 2018</nobr> | Irina Zayats | Initial version of documentation.