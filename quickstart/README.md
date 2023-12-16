# Quick Start Resources

This directory contains resources to help a specific tooling ecosystem or community get started quickly in producing and/or consuming SPDX.
Since the requirements and usage for SPDX varies, each quick start will have unique information that is focused on the objects of the ecosystem or community.

## Current Quick Starts

| Ecosystem or Community | Quick Start Document | Example(s) | Video | Contact |
|--|--|--|--|--|
| Java Library | [JavaToolsQuickStart.md](JavaToolsQuickStart.md) | [java-tools-quickstart-files.zip](java-tools-quickstart-files.zip) | N/A | @goneall |
| Maven | [MavenQuickStart.md](MavenQuickStart.md) | [Full Maven Example](https://github.com/spdx/spdx-maven-plugin/blob/master/src/it/advanced/pom.xml) | N/A | @goneall |
| [Python Library](https://github.com/spdx/tools-python) | [PythonToolsQuickStart.md](PythonToolsQuickStart.md) | [Python Tools Examples](https://github.com/spdx/tools-python/tree/main/examples) | N/A | @armintaenzertng |
| [Yocto](https://www.yoctoproject.org/) | [Creating a Software Bill of Materials in Yocto](https://docs.yoctoproject.org/dev-manual/sbom.html) | N/A | [Automated SBoM generation with OpenEmbedded and the Yocto Project](https://youtu.be/Q5UQUM6zxVU) | @JPEWdev |
| FOSSA | [FOSSAQuickStart.md](FOSSAQuickStart.md) | | | product@fossa.com |
| GitHub | [Exporting a Software Bill of Materials for your Repository](https://docs.github.com/en/code-security/supply-chain-security/understanding-your-software-supply-chain/exporting-a-software-bill-of-materials-for-your-repository) | N/A | | [Ask Github Community](https://github.com/orgs/community/discussions) or [Github Support](https://support.github.com/)
| General SPDX | [Modifying SPDX documents using the SPDX Online Tools](OnlineToolsQuickStart.md) | N/A | | [Create an issue in online tools repo](https://github.com/spdx/spdx-online-tools/issues)

## Upcoming Quick Starts

| Ecosystem or Community | Volunteer | Estimated Date |
|--|--|--|
| Gradle | @goneall | August, 2023 |
| Open SBOM Generator | @goneall | September, 2023 |

## Contributing

If you have a suggestion for a quick start guide, please create a pull request and add it to the Upcoming Quick Starts in this README file.
If you would like to contribute to an upcoming quick start, please create a pull request to add your name to the volunteers in the Upcoming Quick Starts table and contact any existing volunteers to coordinate.


## Format of Quick Start Guides

Each quick start guide should be complete enough for someone familiar with the ecosystem or community can easily complete the quick start.

An example should be used throughout the guide.  It is suggested that the Acme example used in the SPDX 3.0 spec is used as a base with any additional classes, profiles, or properties added as required for that community.

A typical quick start guide would have the following sections:
- Target Audience - who is this quick start for?
- Scenario Description - What is the goal, expected outcomes from following the quick start guide? e.g. "At the end of this quick start guide, you will be able to ..."
- Prerequisites - Any tooling required? Any knowledge pre-requisites? Would another quick start be helpful to run through first?
- Example - Introduce the example that will be used throughout the quick start
- Steps - What are the specific steps to produce or consume an SPDX document?
- Questions? - Where to go if you have any questions
- Suggested next steps - Any further reading, further quick-starts, or just "join our SPDX community"




