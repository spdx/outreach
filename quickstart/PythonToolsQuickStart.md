# Quick Start Guide to generating SPDX in your Python code

## Target Audience
This guide is targeted at Python developers who want to handle SPDX documents in their code, be it parsing, creation, writing or validation.

## Scenario Overview
This guide will walk you through all important functions of the [Python spdx-tools library](https://github.com/spdx/tools-python), enabling you to incorporate them in your own code.
These will allow you to quickly parse, write and validate any given SPDX document, as well as create brand-new documents from scratch.

## Prerequisites
- Python 3.7 or above

## Example
We will use the official SPDX-2.3 examples from the SPDX specification repository, which can be found [here](https://github.com/spdx/spdx-spec/tree/development/v2.3.1/examples).
Choose whichever one you like, except for the spreadsheet formats (.xls and .xlsx). In the example below we use the "SPDXJSONExample-v2.3.spdx.json".

## Steps

### 1. Install the spdx-tools
For installation instructions, see the [README](https://github.com/spdx/tools-python#installation).

### 2. Parse a given SPDX SBOM
Parsing an SBOM with the spdx-tools is very simple:
```python
from spdx_tools.spdx.model.document import Document
from spdx_tools.spdx.parser.parse_anything import parse_file

path_to_spdx_file = "SPDXJSONExample-v2.3.spdx.json"  # adapt this to point to the location of your SBOM
document: Document = parse_file(path_to_spdx_file)
```
If the SBOM contains errors (for example missing mandatory fields) this raises an `SPDXParsingError` which will include all points in the SBOM where parsing has failed.
Else, the `parse_file()` function returns an object of type `Document`, which contains all the information of the SBOM.
You can access the creation information of the `document` like this:
```python
print(f"Parsed document name: {document.creation_info.name}")
creators_as_str = ", ".join([creator.to_serialized_string() for creator in document.creation_info.creators])
print(f"Created on {document.creation_info.created} by {creators_as_str}")
```
Information about `packages`, `files`, `snippets`, `annotations`, `relationships` and `extracted_licensing_info` is also accessible via their respective property names:
```python
list_of_all_files = document.files
print(f"The document contains {len(list_of_all_files)} files.")
if document.packages:
    first_package = document.packages[0]
    print(f"There is a package named {first_package.name} with the SPDXID {first_package.spdx_id}.")
```

### 3. Generate an SPDX SBOM
You can also create your own SBOM from scratch. Start with the creation information, which is an object of type `CreationInfo`, and fill it with values:
```python
from datetime import datetime

from spdx_tools.spdx.model import (
    Actor,
    ActorType,
    CreationInfo,
)

creation_info = CreationInfo(
    spdx_version="SPDX-2.3",
    spdx_id="SPDXRef-DOCUMENT",
    name="document name",
    data_license="CC0-1.0",
    document_namespace="https://your.namespace",
    creators=[Actor(ActorType.PERSON, "Jane Doe", "jane.doe@example.com")],
    created=datetime(2022, 1, 1),
)
```
Use this to create a `Document` object, which serves as the entry point of the SBOM:
```python
from spdx_tools.spdx.model import Document

document = Document(creation_info)
```
Depending on your use case, you can now create objects of type `Package`, `File`, `Snippet`, `Relationship`, `Annotation`, or `ExtractedLicensingInfo`
and add them to the `document`'s properties `packages`, `files`, `snippets`, `relationships`, `annotations`, or `extracted_licensing_info`, respectively.
All of these are lists, so you can add as many as you need. Note that the correctness of types is enforced, so you can't add a list of `File` objects to the `packages` property.
```python
from spdx_tools.common.spdx_licensing import spdx_licensing
from spdx_tools.spdx.model import (
    Checksum,
    ChecksumAlgorithm,
    File,
    FileType,
)

file = File(
    name="./package/file1.py",
    spdx_id="SPDXRef-File1",
    file_types=[FileType.SOURCE],
    checksums=[
        Checksum(ChecksumAlgorithm.SHA1, "d6a770ba38583ed4bb4525bd96e50461655d2758"),
        Checksum(ChecksumAlgorithm.MD5, "624c1abb3664f4b35547e7c73864ad24"),
    ],
    license_concluded=spdx_licensing.parse("GPL-2.0-only OR MIT"),
    license_info_in_file=[spdx_licensing.parse("GPL-2.0-only"), spdx_licensing.parse("MIT")],
    copyright_text="Copyright 2022 Jane Doe",
)

document.files = [file]
```
For an overview of all model classes with their properties and respective types [have a look here](https://spdx.github.io/tools-python/spdx_tools/spdx/model.html).

### 4. Validate the SPDX SBOM
You can check your document for non-conformity against [the official specification](https://spdx.github.io/spdx-spec/v2.3/) using the `validate_full_spdx_document()` function.
This returns a list of `ValidationMessage` objects, one for each invalidity. Each of these objects have a `validation_message` property which tells you about what's wrong,
and a `context` property which tells you where the problem lies. For example, you could use the validation like this:
```python
import logging
from spdx_tools.spdx.validation.document_validator import validate_full_spdx_document
from spdx_tools.spdx.validation.validation_message import ValidationMessage

validation_messages: list[ValidationMessage] = validate_full_spdx_document(document)

for message in validation_messages:
    logging.warning(message.validation_message)
    logging.warning(message.context)
```
If the list of validation messages is empty, the document is valid.

### 5. Serialize the SPDX SBOM
Once you are finished with your `Document` object, you can serialize it to either JSON, YAML, XML, Tag-Value or RDF-XML format.
Simply use the `write_file()` function and provide it with the SPDX document object and the file path.
The output format is determined by the extension found in the path (`.json`, `.yaml`, `.xml`, `.spdx`, and `.rdf.xml`, respectively).
```python
from spdx_tools.spdx.writer.write_anything import write_file

write_file(document, "my_spdx_document.spdx.json")
```

## Additional Information
- [See here](https://github.com/spdx/tools-python/tree/main/examples) for more code examples.
- [See here](https://spdx.github.io/tools-python/spdx_tools/spdx/model.html) for a full code overview of the SPDX model classes and their respective properties and types.



## Suggested Next Steps
If you find you need any additional features currently not supported by the spdx-tools library, feel free to open an issue or contribute code by creating a new pull request.
