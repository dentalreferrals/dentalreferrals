# Change Log

Date | Author          | Description
---- |-----------------| ------------
<nobr>25 Oct 2022</nobr> | Oleg Shevtsov   | Added new patient_gp_code field to `prepare_form` and `save_draft` 
<nobr>4 Oct 2022</nobr> | Serhiy Pustovit | New kinds of files that can be uploaded were added. Such as `image/jpg`, `image/bmp`, `image/tiff`, `image/png`, `text/rtf`, `application/pdf`, `application/msword` - for doc files, and `application/vnd.openxmlformats-officedocument.wordprocessingml.document` - for docx files.
<nobr>1 Sep 2020</nobr> | Serhiy Pustovit | `patient_dob` now is required field for `prepare_form` and `save_draft` endpoints. 422 error can be returned on invalid DOB. Fixed typos.
<nobr>2 Jul 2018</nobr> | Irina Zayats    | Added new `system_type` - `SE`. For [Prepare Form](#prepare-form) API method, if `SE` is passed as `system_type` then `form_type` parameter becomes optional and will allow the user to select the form type in the browser once the prepared form URL is opened. This does not apply to the [Save Draft](#save-draft) API method.
<nobr>9 Jan 2018</nobr> | Irina Zayats    | Initial version of documentation.