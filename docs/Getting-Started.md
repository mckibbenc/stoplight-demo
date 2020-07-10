# Detection on Demand
FireEye offers a best-in-class virtual execution engine in many of its core products, including our Network Security, Email Security, and File Analysis solutions. Now our customers can interact with and consume those capabilities directly via a scalable and performant web service. Use the new RESTful API to submit files for malware analysis, search hash values for past analysis results, get full reports for your file submissions, and integrate into your existing toolsets and workflows.

Let's get started! 

## Prerequisites
Before you use this API, you must first do the following:
* [Sign up for FireEye's Detection On Demand on the AWS Marketplace](https://aws.amazon.com/marketplace/pp/B07XSMKK41)
* [Grab your API Key](https://mvxdashboard.marketplace.apps.fireeye.com/)

That's it!

## FAQs
Here are some questions that were recently asked by testers using the service. We will do our best to update the FAQs as common questions come in. Don't forget you can ask questions in our [Developer Community](https://community.fireeye.dev)

### How Much Does This Service Cost?
Tiered pricing breakdowns are available in the [AWS Marketplace](https://aws.amazon.com/marketplace/pp/B07XSMKK41).

### What Files Can I Submit?
You can upload any file as long as it meets the file size requirement, which is currently 32 MB. There are no limitations on file extensions (or lack thereof).

### How is Maliciousness Determined?
The FireEye Malware Protection System features dynamic, real-time analysis for advanced malware using our patent-pending, multi-flow Multi-Vector Virtual Execution (MVX) engine. The MVX engine captures and confirms zero-day and targeted APT attacks by detonating suspicious files, web objects, and email attachments within instrumented virtual machine environments.

The MVX engine performs multi-flow analysis to understand the full context of an advanced targeted attack. Stateful attack analysis is critical to trigger analysis of the entire attack lifecycle, from the initial exploit to data exfiltration. This is why point products that focus on a single attack object (such as malware executable (EXE), dynamic linked library (DLL), or portable document format (PDF) file types) will miss the vast majority of advanced attacks, because they are blind to the full attack lifecycle.

The Detection On Demand service is built on top of [our MVX engine](https://www.threatprotectworks.com/MVX-engine.asp), and the results are returned to you via a report.

### Can I See the Scan's Progress?
When you submit a file, you will receive a `report_id` value. Use this key to make a `GET` request to `/reports/{report_id}` to see the progress of your file submission. The current best practice with our API is to implement polling and request the report every 1-2 minutes. Webhooks will be implemented in future iterations.

### What is the rate limit for the API?
Rate-limiting is a per-account (not per-API) basis. You are limited to 100 requests/minute for posting new files, 300 requests/minute for retrieving reports, and 200 requests/minute for retrieving hashes.

## Examples
Looking for some inspiration? Here are a few good ideas!

### Corporate Communications
Our Technical Partnerships manager, Christopher Unick, shows how easy it can be to include the API in your Slack workspace. Chris uses Detection On Demand to scan files added to his workspace and lets users know whether the file is malicious, protecting his users from unwanted malware attacks.  [Click here to see how this is done](https://fireeye.dev/zapier-integration).

### SOC Analysts
While helping to remediate threats in IT, SOC analysts may come across MD5 hashes in their browser. The Detection On Demand team designed a Google Chrome plug-in that allows you to find any MD5 hashes on a webpage and run them through Detection On Demand for analysis. Optionally, you can also upload local files through the Chrome plug-in. Check out the Chrome plug-in in the FireEye Market, [here](https://fireeye.market/apps/255060).

## Endpoints
If you are looking for the Detection On Demand API docs, you can find them [here](https://fireeye.dev/apis). Our API docs are all written in the OpenAPI 3.0.0 specification and include everything you need to start making requests to the API. In this section, you will find related information that supports the API doc. Generally, if you have a question beyond what the API doc shows, then you should find the answer here.

### Files
Today the `/files` collection only allows a single operation: creating a new file with a `POST` to `/files`. You can send files to the Detection On Demand API, but **please note that you cannot delete them**.

#### Filesize Limitations
Currently, files of 32MB or less are supported.

#### Supported File Extensions
There are no limitations on files in regards to extensions (or lack thereof). You can upload any file as long as it meets the file size requirements.

#### File Retention
You cannot delete files once they have been sent to the Detection On Demand service. Files found not to be malicious will automatically be removed, while files found to be malicious will be kept for further analysis.

### Reports
Today, the `/reports` collection allows a single operation: retrieving reports with a `GET` request from `/reports/{report_id}`. Reports are tied to the report_id that is returned when you submit a file to the `/files` endpoint. There are some important things to know about reports before using the Detection On Demand service:
* Today, you do not have access to the `/reports` collection, so you cannot get a list of all reports run by your API key or account. If you want to look up reports later, you must store your report IDs for future lookups.
* Reports, by default, include only high-level information. If you want to see the full report, you must pass the `extended=true` parameter.

#### Report Definition
The report response object schema has been fully defined in our API docs. If you would like to understand what fields in the report mean, please refer to the API docs.

#### OS Change Reports
OS change reports describe the changes that your file made when it was run in different operating systems. To understand the OS Change report, please refer to the API docs.

#### Supported Operating Systems
Currently, your submitted files will be executed in the following environments:
 * Windows 10
 * Windows 7
 * Windows XP
 * CentOS 7.2
 * Mac OS X 11.2

### Hashes
Today, the `/hashes` collection only allows a single operation: retrieving hash reports with a `GET` request from `/hashes/{hash_id}`, where `{hash_id}` is an MD5 hash. Currently, only MD5 hashes are supported, but plans to support other hashes are in the pipeline.

#### Hash Response
Hash responses are similar but not identical to reports. To understand the hash response schema, please refer to the API docs.
