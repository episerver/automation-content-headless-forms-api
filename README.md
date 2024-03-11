# Overview

This repository contains Headless Form Automation testing scripts in Postman

It is owned and maintained by the Addons QA team.

## Main Folders overview

| Folder                           | Description                                                                                                               |
|:---------------------------------|:--------------------------------------------------------------------------------------------------------------------------|
| HeadlessForm                     | Contains scripts for Headless Forms Service API testing. This includes endpoinds for: Get Forms, Submit Form  and Get Forms Submission.|                                   
| Data                             | Contains test data file for testing                                                                                       |
                                                                                   

## Newman and htmlextra reporter
Newman is a command-line collection runner for Postman. It allows you to effortlessly run and test a Postman collection directly from the command-line
### Installation
- Refer to link [https://confluence.sso.episerver.net/download/attachments/2861024248/Install%20Newman%20for%20Postman%20to%20generate%20html%20report.docx?version=1&modificationDate=1652927482393&api=v2]  to install newman and newman-reporter-htmlextra

### Sample commands to run script for Headless Forms
You can run specific script from command line. Following are sample commands.
#### Run all test of Headless Forms collection, including Get Forms, Submit Form and Get Forms Submission
###### Definition:
    newman run “FormHeadless v1.0.postman_collection.json” -r htmlextra

#### Run test for GET forms API
###### Code:
    newman run “FormHeadless v1.0.postman_collection.json” --folder "Get Forms" -r htmlextra
    
#### Run test for Submit forms and Get forms submission
###### Code:
    newman run “FormHeadless v1.0.postman_collection.json” --folder "Form submission" -r htmlextra

* Note: You can specify the test folder to save time with option --folder "folderName"

## Run script for Headless Forms 
### Preparation for test site
- Get AlloyMvcTemplates and install Headless Forms Service.
- Pull latest code of https://github.com/episerver/automation-content-headless-forms-api
- Import file automation-content-headless-forms-api\Data\Headless.bak to the database(Using SQL Server >=2019)
- Add code to any Controller class (for example StartPageController.cs) to get form guid automatically:
###### Code:
      [Route("/cms/GetContentGuid")]
      // Get content guid (key) /cms/GetContentGuid?id=1
      public string GetContentGuid(int id)
      {
          var contentRepository = ServiceLocator.Current.GetInstance<ContentRepository>();
          var contentLink = new ContentReference(id);
          var content = contentRepository.Get<IContent>(contentLink);
          if (content != null)
              return content.ContentGuid.ToString().Replace("-", "").ToUpper();
          return "No content found";
      }
- Build and deploy site.

### Postman script for Headless Form
Postman script is stored in folder automation-content-headless-forms-api/FormHeadless/

- Import script to Postman
- Copy Data\files to your Working directory which is defined on Postman. These folder contains upload files that some scripts used.
- From Collection variables, update these varriables corresponding to the site: url, username, password.
- Run script with Newman.


