# Quick Start Guide to generating SPDX in your Java code

## Target Audience
This guide is targeted at Java developers who want to handle SPDX documents in their code, be it parsing, creation, writing or validation.

## Scenario Overview
This guide will walk you through all important functions of the [SPDX Java Library](https://github.com/spdx/spdx-java-library), enabling you to incorporate them in your own code.
These will allow you to quickly parse, write and validate any given SPDX document, as well as create brand-new documents from scratch.

## Prerequisites
- Java 8 or above (note: using RDF as a serialization format requires Java 11 or above)
- Maven

## Example
For this quick start guide, we will be using the project resulting from the [Maven in 5 minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) "getting started" exercise.
This is a basic project with just enough information to compile.

You can, of course, follow this guide using one of your own projects, just substitute your POM file for the examples and make the corresponding changes.

We will use the official SPDX-2.3 JSON example from the SPDX specification repository, which can be found [here](https://github.com/spdx/spdx-spec/blob/development/v2.3.1/examples/SPDXJSONExample-v2.3.spdx.json).

## Steps

### 1. (Optional) Maven in 5 Minutes
- Run through all the steps in the [Maven in 5 minutes](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) "getting started"
- Check to make sure your project builds successfully
- You POM file should look like:

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
 
  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>
 
  <properties>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>
 
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```

### 2. Update the POM file to the supported compiler version

Change:

```
  <properties>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>
```

to:

```
  <properties>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
```

### 3. Add the SPDX Java Library and SPDX Jackson Store as dependencies

Change:

```
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
```

to:

```
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.12</version>
      <scope>test</scope>
    </dependency>
    <dependency>
      <groupId>org.spdx</groupId>
      <artifactId>java-spdx-library</artifactId>
      <version>1.1.7</version>
    </dependency>
    <dependency>
      <groupId>org.spdx</groupId>
      <artifactId>spdx-jackson-store</artifactId>
      <version>1.1.7</version>
    </dependency>
  </dependencies>
```

The `java-spdx-library` is the main library you will be using in the quick start guide.

The `spdx-jackson-store` is the library that supports the parsing of the JSON files.

### 4. Parse the SPDX Example

- Download the [SPDX JSON Example](https://github.com/spdx/spdx-spec/blob/development/v2.3.1/examples/SPDXJSONExample-v2.3.spdx.json) to the root of the project directory
- Add the following import code to your Java application source file (App.java if you are using the Maven quickstart):

```
import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStream;

import org.spdx.jacksonstore.MultiFormatStore;
import org.spdx.jacksonstore.MultiFormatStore.Format;
import org.spdx.jacksonstore.MultiFormatStore.Verbose;
import org.spdx.library.InvalidSPDXAnalysisException;
import org.spdx.library.model.SpdxDocument;
import org.spdx.storage.ISerializableModelStore;
import org.spdx.storage.simple.InMemSpdxStore;
```

- add the following code to the main method:

```
        ISerializableModelStore store = new MultiFormatStore(	// This will "store" the deserialized JSON file
        		new InMemSpdxStore(),	// Store the JSON file in memory - the parameter allows for other, more robust, storage
				Format.JSON_PRETTY,		// Specify the JSON format - this store also supports XML and YAML
				Verbose.COMPACT);		// Specify how verbose when serializing
        String documentUri = null;		// The Document URI is a unique key for every deserialized SPDX document in the store
        try (InputStream is = new FileInputStream("SPDXJSONExample-v2.3.spdx.json")) {
        	documentUri = store.deSerialize(is, false);	// This deserializes the JSON file - it returns the document URI
		} catch (IOException e) {
			System.out.println("I/O error reading the SPDX Json file");
			System.exit(-1);
		} catch (InvalidSPDXAnalysisException e) {
			System.out.println("Error parsing the SPDX JSON file: "+e.getMessage());
			System.exit(-1);
		}
        try {
			SpdxDocument doc = new SpdxDocument(	// the SPDX document is a model object representing the serialized SPDX content in the file
					store,							// the store where we deserialized the JSON file
					documentUri,					// the URI of the document we wish to access
					null,							// an optional copy manager can be provided if working with more than one store
					false);							// we the document already exists, we don't want to create it
			System.out.println("Successfully deserialized " + documentUri);
		} catch (InvalidSPDXAnalysisException e) {
			System.out.println("Error accessing SPDX document: "+e.getMessage());
			System.exit(-1);
		}
```

Running the app, you should see the following:

```
Successfully deserialized http://spdx.org/spdxdocs/spdx-example-444504E0-4F89-41D3-9A0C-0305E82C3301
```

### 5. Work with the SPDX model

Now that we have a document to work with, let's see what we can do.

Properties for the `doc` object created above can simply be referenced using that model object.  
If we want to get the creation information we can just call `doc.getCreationInfo()`.
This returns another model object `CreationInfo` which also has properties we can get and set.

If we want to print out all the creators of the document, we can add the following code:

```
			System.out.println("Following are the creators:");
			for (String creator:doc.getCreationInfo().getCreators()) {
				System.out.println("\t"+creator);
			}
```

The model objects generally follow the [SPDX specification object model](https://github.com/spdx/spdx-spec/blob/development/v2.3.1/model/SPDX-UML-Class-Diagram.jpg) unless there is a naming conflict with the Java reserved words.

See the [JavaDocs](https://spdx.github.io/Spdx-Java-Library/) for documentation on the model.


### 6. Creating an SPDX document from scratch

- Creating a document from scratch requires the following imports:

```
import java.text.SimpleDateFormat;
import java.util.Arrays;
import java.util.Date;
import java.util.List;

import org.spdx.library.InvalidSPDXAnalysisException;
import org.spdx.library.SpdxConstants;
import org.spdx.library.Version;
import org.spdx.library.model.license.LicenseInfoFactory;
import org.spdx.library.model.SpdxDocument;
import org.spdx.library.model.SpdxFile;
import org.spdx.library.model.enumerations.ChecksumAlgorithm;
import org.spdx.library.model.license.AnyLicenseInfo;
import org.spdx.storage.ISerializableModelStore;
import org.spdx.storage.IModelStore;
import org.spdx.storage.simple.InMemSpdxStore;
```

- You will need a unique document URI - we'll just modify the example document URI, but you may want to use
the recommended method of generating a globally unique ID programmatically:

```
		String newDocumentUri = "http://spdx.org/spdxdocs/spdx-example2-444504E0-4F89-41D3-9A0C-0305E82CCCCC";
```

- Add the following code to create an empty document:

```
			IModelStore inMemStore = new InMemSpdxStore();
			String newDocumentUri = "http://spdx.org/spdxdocs/spdx-example2-444504E0-4F89-41D3-9A0C-0305E82CCCCC";
			SpdxDocument myDoc = new SpdxDocument(
					inMemStore,						// here we'll just use a very simple in memory store which doesn't support serialization
					newDocumentUri,					// the URI of the document - must be globally unique
					null,							// an optional copy manager can be provided if working with more than one store
					true);							// this time, we want to create it
			System.out.println("Successfully created " + newDocumentUri);
```

- We now need to add the required fields to make this a valid SPDX document:

```
			myDoc.setCreationInfo(				// Set the required creationInfo
					myDoc.createCreationInfo(	// All model objects have a set of convenience methods to create 
												// other model objects using the same model store and document URI
							Arrays.asList(new String[] {"Tool: Sample App"}), // creators
							new SimpleDateFormat(SpdxConstants.SPDX_DATE_FORMAT).format(new Date())));
												// creation date - note that SpdxConstants has several useful constant values
			myDoc.setSpecVersion(Version.CURRENT_SPDX_VERSION); // the Version class has constants defined for all supported SPDX spec versions
			myDoc.setName("My Document");
			// The LicenseInfoFactory contains some convenient static methods to manage licenses including
			// a license parser.  Note that we have to pass in the model store and document URI so that the
			// license is created in the same store.
			myDoc.setDataLicense(LicenseInfoFactory.parseSPDXLicenseString("CC0-1.0", inMemStore, newDocumentUri, null));
			// We need something for the document to describe, we'll create an SPDX file
			AnyLicenseInfo apacheLicense = LicenseInfoFactory.parseSPDXLicenseString("Apache-2.0", inMemStore, newDocumentUri, null);
			SpdxFile file = myDoc.createSpdxFile(
					SpdxConstants.SPDX_ELEMENT_REF_PRENUM + "44",
					"./myfile/name", 
					apacheLicense, 
					Arrays.asList(new AnyLicenseInfo[] {apacheLicense}),
					"Copyright me, 2023",
					myDoc.createChecksum(ChecksumAlgorithm.SHA1, "2fd4e1c67a2d28fced849ee1bb76e7391b93eb12"))
					.build();  // The more complex model objects follows a builder pattern
			myDoc.getDocumentDescribes().add(file);
```

### 7. Validate the SPDX SBOM

You can check your document for non-conformity against [the official specification](https://spdx.github.io/spdx-spec/v2.3/) using the `verify()` method on any model object.  

Calling `verify()` on the SPDX document will verify all the elements described by the document - both directly and indirectly.

Below is a snippet of code which will verify the above created SPDX document and print out any warnings:

```
			List<String> warnings = myDoc.verify();
			if (warnings.isEmpty()) {
				System.out.println("My doc is valid");
			} else {
				System.out.println("Verification failed for the following reason(s):");
				for (String warning:warnings) {
					System.out.println("\t"+warning);
				}
			}
```


### 8. Serialize the SPDX SBOM
Once you are finished with your `SpdxDocument` object, you can serialize it to either JSON, YAML, XML, Tag-Value or RDF-XML format.

If you already have your SPDX document using a model store that supports the serialization, you just need to call the `serialize()` method.  The code below will serialize the same document we just deserialized from JSON:

```
import java.io.FileOutputStream;
import java.io.OutputStream;
```
...

```
			try (OutputStream os = new FileOutputStream("SPDXJSONExample-v2.3-copy.spdx.json")) {
	        	store.serialize(documentUri, os);
			} catch (IOException e) {
				System.out.println("I/O error writing the SPDX Json file");
				System.exit(-1);
			} catch (InvalidSPDXAnalysisException e) {
				System.out.println("Error parsing the SPDX JSON file: "+e.getMessage());
				System.exit(-1);
			}
```

If you want to serialize a document in a different format or if it is stored in a model store that doesn't support serialization, you can create a different model store and copy the document before serializing.

Below is an example where we serialize the document we created:

```
			ModelCopyManager copyManager = new ModelCopyManager();  // the copy manager handles copying between model store
			myDoc.setCopyManager(copyManager); 	// we need to set the copymanager in the document we're copying from
			ISerializableModelStore storeForSerialization = new MultiFormatStore(new InMemSpdxStore(),
					Format.JSON_PRETTY,	Verbose.COMPACT);			// A new model store just to serialize the JSON file
			SpdxDocument docToSerialize = new SpdxDocument(
					storeForSerialization,			// a model store that supports serialization
					newDocumentUri,					// we'll use the same URI as myDoc since we're making a copy
					copyManager,					// the copy manager must be the same as the one we set in myDoc
					true);
			docToSerialize.copyFrom(myDoc);			// This will copy the document to the new model store
			
			try (OutputStream os = new FileOutputStream("my-doc.spdx.json")) {
				storeForSerialization.serialize(newDocumentUri, os);
			} catch (IOException e) {
				System.out.println("I/O error writing the SPDX Json file");
				System.exit(-1);
			} catch (InvalidSPDXAnalysisException e) {
				System.out.println("Error parsing the SPDX JSON file: "+e.getMessage());
				System.exit(-1);
			}
			System.out.println("Serialized SPDX document to my-doc.spdx.json");
		} catch (InvalidSPDXAnalysisException e) {
			System.out.println("Error accessing SPDX document: "+e.getMessage());
			System.exit(-1);
		}
```

## Additional Information

- [java-tools-quickstart-files.zip](java-tools-quickstart-files.zip) Source files used as examples in this quick start
- [JavaDocs](https://spdx.github.io/Spdx-Java-Library/) for the SPDX Java Library
- [Spdx-Java-Library GitHub repository](https://github.com/spdx/Spdx-Java-Library)
- [Implementing SPDX Model Stores](https://github.com/spdx/Spdx-Java-Library/blob/master/README-SPI-MODEL-STORE.md)
- [SPDX Specification](https://github.com/spdx/spdx-spec)
- [spdx-java-jackson-store](https://github.com/spdx/spdx-java-jackson-store) which implements JSON, YAML, and XML serializations
- [spdx-java-tag-value-store](https://github.com/spdx/spdx-java-tagvalue-store) which implements TAG/Value serialization
- [spdx-java-rdf-store](https://github.com/spdx/spdx-java-rdf-store) which support the RDF serialization formats (RDF/XML, RDF/Turtle, JSON-LD)
- [spdx-java-spreadsheet-store](https://github.com/spdx/spdx-java-spreadsheet-store) which supports serializing SPDX to spreadsheets (XLS and XLSX)

## Suggested Next Steps

- Read through the [JavaDocs](https://spdx.github.io/Spdx-Java-Library/) to better understand the model
- Investigate the different serialization formats available
- Contribute to the community - contributions to the libraries and spec are welcome!
