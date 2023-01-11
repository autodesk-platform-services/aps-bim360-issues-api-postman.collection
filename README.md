# Postman Collection for BIM 360 Issues API 

[![Postman](https://img.shields.io/badge/Postman-v8.11-orange.svg)](https://www.getpostman.com/)

[![Issue API of BIM 360 Version 2](https://img.shields.io/badge/bim360%20issue%20api-v2-yellowgreen)](https://aps.autodesk.com/en/docs/bim360/v1/overview/field-guide/issues/)

![Beginner](https://img.shields.io/badge/Level-Beginner-green.svg)
[![License](https://img.shields.io/:license-MIT-blue.svg)](http://opensource.org/licenses/MIT)

## Description

This repository provides a [postman](https://www.getpostman.com/downloads/) collection that demonstrates the usage of [BIM360 Issues Version 2 (v2)](https://aps.autodesk.com/en/docs/bim360/v1/overview/field-guide/issues/) .  

The API supports **3 legged token** only. This collection takes **[Inheriting auth](https://learning.getpostman.com/docs/postman/sending-api-requests/authorization/#inheriting-auth)** to apply 3-legged token to every endpoint in the collection automatically
 

## Setup

1.  **Autodesk Platform Services (APS) Account**: Learn how to create an APS Account, activate the subscription and create an app by [this tutorial](http://aps.autodesk.com/tutorials/#/account/). Ensure to select API Type **BIM 360**. Get APS _client id_, _client secret_ and  _callback url_. Please register APS app with the _callback url_ as 

    ```https://www.getpostman.com/oauth2/callback```

2. **BIM 360 account and project**: must be an Account Admin to add the app integration. [Learn about provisioning](https://aps.autodesk.com/blog/bim-360-docs-provisioning-forge-apps). Make a note of the __account name__

3. Create a project in BIM 360 or reuse existing project. Make a note of the __project name__

4. Follow the [product help document of BIM 360](https://knowledge.autodesk.com/support/bim-360/learn-explore/caas/CloudHelp/cloudhelp/ENU/BIM360D-Administration/files/About-Project-Admin/BIM360D-Administration-About-Project-Admin-about-issues-html-html.html) to create some test entities, e.g.
    -  common issues
    -  pushpin issues with document
    -  create some custom attributes definitions and attach them to issues. 
    -  create some comments with issues
    -  add some attachments to issues (either with documents from visible folder or from a local uploaded file)
    -  create some custom issue types and root causes. specify them with issues

5. (Optional) Create a couple of assets in Assets module. This is to test fetching/creating reference with Issues and Assets.

6.  Clone this repository or download it. We recommend to install [GitHub Desktop](https://desktop.github.com/). To clone it via a command line, use the following in (**Terminal** on MacOSX/Linux, and **Git Shell** on Windows):

    ```git clone https://github.com/Autodesk-Forge/forge-autodesk.docs.issue.api-postman.collection```

7. Import the collection and environment files to Postman

    <p align="center"><img src="./help/collection.png" width="400" ></p>  

8. In environment, input _client id_, _client secret_, _account name_ and _project name_.

   <p align="center"><img src="./help/apiref-env.png" width="600" ></p>  

9. In the context menu of collection >> select **Edit**, switch to the tab **Authorization**, click **Get New Access Token**, and input the variables as below:

   - Grant Type: ``Authorization Code``
   - Callback URL:  ``https://www.getpostman.com/oauth2/callback``
   - Auth URL:  ``https://developer.api.autodesk.com/authentication/v1/authorize``
   - Access Token URL:  ``https://developer.api.autodesk.com/authentication/v1/gettoken``

   - Client ID: ``{{client_id}}``
   - Client Secret: ``{{client_secret}}``
   - Scope: ``data:read data:write``
   - Client Authentication: ``Send client credentials body``

   <p align="center"><img src="./help/apiref-oauth2.png" width="600" ></p> 
 
 10. Click **Get New Access Token**. It will direct to login Autodesk user account login. After it succeeds, the token will be generated. Click **Use Token**.  
   
## API Test

### API References
Assume the steps to Setup above have been followed and the access token is successfully generated. Run the scripts in Run First. It will get account (hub) id, project id and one user id (for an assignee), location id, one document urn (to attachan issue). Run the endpoints under **API References** folder in Postman collection. Follow [API References](https://aps.autodesk.com/en/docs/bim360/v1/reference/http/issues-v2-users-me-GET/) to verify if the APIs work well. Try to change the parameters in various scenarios to see how it works. Check UI if it works well with items created or updated using API. For example,  creating/patching new issues and adding new comments.
    

### Download Attachment
To test downloading attachment files, run scripts in **Download Attachment**. It assumes one issue is available with a few attachments. 
  - Step-01 will return information of the first attachment. If urnType is dm, the urn pattern is like a base64 encoded string. The post script will transform it to a version urn. If the urnType is **oss**, it is a storage urn already. The post script will get bucket key and object key directly. 
  - if urnType is **dm**, run step-02 to get a storage urn by Data Management API. Finally get bucket key and object key. 
  - Step-03 is to get S3 signed url
  - Step-04 is to download the file by the signed url

### Upload Local Attachment
To test uploading local files as attachments, run scripts in **Upload Local Attachment**. It assumes at least one issue is available.

 - Step-01 creates an attachment to the issue with urnType = **oss**. It will create the storage in the invisible folder for the issue in Docs
 - Step-02 will generate S3 signed url for uploading by Data Management API
 - Step-03 is to upload the local binary file by the signed url
 - Step-04 is to complete the uploading by Data Management API
 - Step-05 is to notify the server to associate the file with the issue.

 To verify if the attachment is attached successfully, follow the steps in Test R. 

### Test Reference by Relationship API
In BIM 360, the reference of issue with assets or rfis is managed by Relationships API, which is a common component across different modules in BIM 360. To work with **Issue>>References**, you can use [Relationship API](https://aps.autodesk.com/en/docs/acc/v1/reference/http/relationship-service-v2-search-relationships-GET/). This sample Postman collection includes one sample usage. For more detail about Relationships API, please take a look [at this blog](https://aps.autodesk.com/blog/bim-360acc-relationships-api). 

  -  Step-01 is to get supported Relationships. You can use this stop to check entity types that you can set relationships with BIM 360 issues.
  -  The 02 step is to get one asset id by [Asset API](https://aps.autodesk.com/en/docs/BIM360/v1/reference/http/assets-assets-v2-GET/)
  -  The 03 step is add reference between one issue and one asset
  -  The 04 step get all referenced assets of one issue

## Documentaions

- [Field Guide](https://aps.autodesk.com/en/docs/bim360/v1/overview/field-guide/issues)
- [Migration Guide](https://aps.autodesk.com/en/docs/bim360/v1/overview/migration-guides/Issues_v1_to_v2): outlines and details of comparison of version 1 and version 2
- [Retrieve Container ID of Issue](https://aps.autodesk.com/en/docs/bim360/v1/tutorials/issuesv2/retrieve-container-id)
- [Retrieve Issues Data](https://aps.autodesk.com/en/docs/bim360/v1/tutorials/issuesv2/retrieve-issues-v2) 
- [Create Issues Data](https://aps.autodesk.com/en/docs/bim360/v1/tutorials/issuesv2/create-issues-v2)
-  [Attach BIM 360 Document Management Files to Issues](https://aps.autodesk.com/en/docs/bim360/v1/tutorials/issuesv2/attach-BIM-360-files-v2)
- [Retrieve Issue Custom Attributes Mapped to an Issue Type](https://aps.autodesk.com/en/docs/bim360/v1/tutorials/issuesv2/retrieve-custom-attr-issues-v2)
- [Render Document-related (Pushpin) Issues and RFIs in Your App](https://aps.autodesk.com/en/docs/bim360/v1/tutorials/pushpins/retrieve-pushpin-v2)
- [Create Document-related (Pushpin) Issues and RFIs in Your App](https://aps.autodesk.com/en/docs/bim360/v1/tutorials/pushpins/create-pushpin-v2) 
- [Upload Local Attachment](https://aps.autodesk.com/en/docs/bim360/v1/tutorials/issuesv2/attach-local-attachment-issues-v2)

## Blogs
- [Autodesk Platform Services (APS) Blog](https://aps.autodesk.com/blog)
- [Field of View](https://fieldofviewblog.wordpress.com/), a BIM focused blog

## License

This sample is licensed under the terms of the [MIT License](http://opensource.org/licenses/MIT). Please see the [LICENSE](LICENSE) file for full details.

## Written by

Xiaodong Liang [@coldwood](https://twitter.com/coldwood), Developer Advocate and Support team 