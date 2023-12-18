# Quick Start Guide to modifying SPDX documents using the SPDX Online Tools

## Target Audience
This guide is targeted for any consumer or producer of SPDX documents wishing to make simple modifications.

## Scenario Overview
This guide starts with an existing SPDX document in JSON form.
The existing document will be the [SPDX example in JSON format](https://github.com/spdx/spdx-spec/raw/development/v2.3.1/examples/SPDXJSONExample-v2.3.spdx.json).
We will convert the document into a spreadsheet format, make a few modifications, then convert the document back into a JSON format.

## Prerequisites
- Download the [SPDX example in JSON format](https://github.com/spdx/spdx-spec/raw/development/v2.3.1/examples/SPDXJSONExample-v2.3.spdx.json)
- Access to the [SPDX Online Tools](https://tools.spdx.org/app)

## Example
For this quick start guide, we will be using the [SPDX example in JSON format](https://github.com/spdx/spdx-spec/raw/development/v2.3.1/examples/SPDXJSONExample-v2.3.spdx.json) which is available in the SPDX specification repo under the examples folder.

## Steps

### 1. Convert the JSON file into a spreadsheet format
- Download the [SPDX example in JSON format](https://github.com/spdx/spdx-spec/raw/development/v2.3.1/examples/SPDXJSONExample-v2.3.spdx.json)
- Go to the [convert page](https://tools.spdx.org/app/convert/) of the SPDX online tools
- Select `JSON` as the from and `XLSX Spreadsheet` as the to formats
- Select the SPDX example file in JSON format downloaded above, then convert
- The file will be available for download

### 2. Edit the spreadsheet
- Edit the spreadsheet - for example, change the comment for the SPDX document
- Update the `Document Namespace` - this must be unique.  Since we created a different version, we should modify this field to distinguish it from the version we download.  We can just append "-modified" to make it different.
- Save the spreadsheet changes

### 3. Convert the spreadsheet back to JSON
- Go to the [convert page](https://tools.spdx.org/app/convert/) of the SPDX online tools
- Select `XLSX Spreadsheet` as the from and `JSON` as the to formats
- Select the spreadsheet modified above, then convert
- The file will be available for download

### 4. Verify the converted file
- Go to the [validate page](https://tools.spdx.org/app/validate/) of the SPDX online tools
- Select `JSON` as the format
- Select the JSON file downloaded from step 3 above
- Select verify
- You should get a message that the document is valid

## Additional Information
- Source for the SPDX Online Tools project is available in the [SPDX Online Tools Git repository](https://github.com/spdx/spdx-online-tools)
- Issues or improvement suggestions can be made by [submitting an issue](https://github.com/spdx/spdx-online-tools/issues)
