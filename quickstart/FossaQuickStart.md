
# **Quick Start Guide for Generating SPDX Documents with FOSSA**

## **Target Audience**

This guide is intended for both technical and non-technical users who wish to generate SPDX documents. FOSSA’s SBOM tool supports a wide range of programming languages and ecosystems. Visit our [docs for the full list](https://docs.fossa.com/docs/supported-languages). 

## **Scenario Overview**

This guide will explain how you can import your project into FOSSA’s SBOM tool. It will then demonstrate how you can generate an SPDX document in the tag/value and/or JSON formats.

## **Prerequisites**

You must have a FOSSA account to follow the steps in this guide (you can generate SPDX documents with either a free or premium account).

## **Example**

In this guide, we will generate an SPDX document that describes the example application [my_recipe_book](https://github.com/CortezFrazierJr/my_recipe_book). 

## **Steps**

Below, we’ll show you how to generate an SPDX document in two different ways: 



* With the FOSSA web app 
* With the FOSSA CLI (best for technical users)

## **Generating an SPDX Document with the FOSSA Web App**



1. Sign up for a free FOSSA account: [https://app.fossa.com/auth/register](https://app.fossa.com/auth/register)
2. Click the “Add Projects” button to import your project. Select the “Quick Import” option, and connect your Version Control System account to FOSSA
3. After connecting your VCS provider account, you should see a list of your repositories. To import, simply select _Import All_ for all the repositories or select specific ones and click _Import_.
4. In our example, once we’ve successfully imported [My Recipe Book](https://github.com/CortezFrazierJr/my_recipe_book), we’ll click on it from our “Projects” dashboard. We’ll then select the “Generate SBOM Report” from the “Actions” menu on the right side of the screen
5. Now, we’ll customize our SBOM. In our example, we’ve selected the SPDX JSON format. &lt;>
6. Once our SBOM is generated, we can either download and distribute the report, or we can use FOSSA to host it. In our example, we’ve downloaded a JSON file and [added it to our GitHub repo](https://github.com/CortezFrazierJr/my_recipe_book/blob/main/sampleSPDX.json).

## **Generating an SPDX Document with the FOSSA CLI**
 


1. Sign up for a free FOSSA account: [https://app.fossa.com/auth/register](https://app.fossa.com/auth/register)
2. Click the “Add Projects” button to import your project. Select the “Integrate Locally (CLI)” option. Then, click the “View Guide” button for instructions on installation and setting up your API key
3. Once you’ve completed installation and set up your API key, you’ll run the command `fossa analyze` to scan your project
4. To generate an SPDX document, you’ll input the command 

    ```
    export FOSSA_API_KEY=XXXXXXXX (needs to be a full API key) 
    fossa analyze && fossa report attribution --format spdx-json > file.json

    ```


## **Questions**

Email product@fossa.com, or reach out to us on [LinkedIn](https://www.linkedin.com/company/fossa) or [Twitter](https://twitter.com/getfossa).

## **Suggested Next Steps**

Once you’ve signed up for a free FOSSA account, consider viewing our docs page for more information on importing projects and generating SBOMs:

[Getting Started](https://docs.fossa.com/docs/getting-started)

[Generating SBOMs](https://docs.fossa.com/docs/generating-reports)

We also recommend viewing the short video: “[How to Generate SBOMs with FOSSA”](https://www.youtube.com/watch?v=0bzBtYRGx8c&t)
