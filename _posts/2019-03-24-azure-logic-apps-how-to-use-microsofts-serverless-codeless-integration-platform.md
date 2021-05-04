---
title: "Azure Logic Apps: How to use Microsoft&#8217;s Serverless &#038; Codeless Integration Platform"
date: 2019-03-24 21:19:32 +00:00
author: steven
layout: post
permalink: /azure-logic-apps-how-to-use-microsofts-serverless-codeless-integration-platform/
image: /assets/img/2019/03/5-visualdesigner-getstarted.png
icon: azure
tags: azure logic-apps
---

Azure Logic Apps is a powerful **serverless** integration platform released by Microsoft in [2016](https://azure.microsoft.com/en-us/blog/announcing-azure-logic-apps-general-availability/). The definition of a \'*Logic App\'* is open to interpretation *(in my opinion)*, as the user defines what the Logic App is by integrating many different applications and services to accomplish their goal.

**No knowledge of code is required**, and **you only pay for what you use**! If a task is scheduled to run for one hour a day, you only pay for that one hour. Also, as Logic Apps are *\'serverless\'*, there is no IIS or Apache in sight.

The benefits of Azure Logic Apps:

- **No web server to configure**
- **No knowledge of code required** *(use the visual designer instead!)*
- **Use Visual Studio extension to commit to source control**
- **Hundreds of connectors to create a workflow that meets your requirements**

## Visual Designer

Under the hood, a Logic App is simply a JSON object that is interpreted by Azure to execute the applications and services in the order you configure. To use Logic Apps and compose the JSON object, Microsoft has created the ***\'visual designer\'*** to make it effortless!

{%
    include image.html
    year='2019'
    month='03'
    file='visualdesigner.png'
    alt='Visual designer'
%}

You can either use the *\'visual designer\'* in the Azure portal, or by installing the **Visual Studio extensions** [Logic App Tools for Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=VinaySinghMSFT.AzureLogicAppsToolsforVisualStudio-18551) or [Azure Logic Apps for Visual Studio Code (Preview)](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-logicapps).

## Let\'s Connect

No... I don\'t mean that kind of *connect*! What I do mean is... let\'s discuss the concept of ***\'connectors\'*** in Azure Logic Apps.

*[\'Connectors\'](https://docs.microsoft.com/en-us/azure/connectors/apis-list)* are integral to building a workflow that will accomplish your goal and can be grouped into the following categories:

- **Built-ins** *(triggers, actions, e.g. schedules, incoming HTTP requests)*
- **Control workflow** *(if statement, for loop, switch statement, etc.)*
- **Manage or manipulate data** *(parse and compose JSON, set variables)*
- **Managed API connectors** *(integrate with third-party APIs, e.g. Twitter)*
- **On-premise connectors** *(use your on-premise SQL server and/or file storage!)*
- **Integration account connectors** *(encoding, decoding and transformation)*
- **Enterprise connectors** *(enterprise-level integration)*

## Getting Started

Logic Apps can be designed in either the Azure Portal, or alternatively Visual Studio. If you work amongst a team of developers, I would recommend using Visual Studio to **take advantage of source control** to use branches, create pull requests and track changes.

For this demo, we’ll use **Visual Studio 2017** and the *‘**Azure Logic App Tools for Visual Studio 2017’*** extension.

{%
    include image.html
    year='2019'
    month='03'
    file='1-logicapptooldextension.png'
    alt='Logic app tool extension'
%}

Let’s create a new project in Visual Studio and use the ‘***Azure Resource Group’*** template:

{%
    include image.html
    year='2019'
    month='03'
    file='2-azureresourcegroup.png'
    alt='Azure resource group'
%}

Select ***‘Logic App’*** to create an empty Logic App:

{%
    include image.html
    year='2019'
    month='03'
    file='3-newlogicapp.png'
    alt='New logic app'
%}

Once the Solution Explorer is visible, use the keyboard shortcut **Ctrl+L** or the context menu to **open the Logic App in the visual designer**:

{%
    include image.html
    year='2019'
    month='03'
    file='4-openwithvisualdesigner.png'
    alt='Open with visual designer'
%}

Visual Studio will prompt you to **confirm the Azure subscription and resource group.** This is for billing and organization purposes.

{%
    include image.html
    year='2019'
    month='03'
    file='5-logicapptoresourcegroup.png'
    alt='Logic app to resource group'
%}

You will now see the **Visual Designer** in Visual Studio, just as it appears in the Azure Portal!

{%
    include image.html
    year='2019'
    month='03'
    file='5-visualdesigner-getstarted.png'
    alt='Visual designer get started'
%}

## Create the Logic App

For this demo, we’ll use the trigger **‘When a HTTP request is received’**. This is so we can use the [Postman](https://www.getpostman.com) client to trigger the Logic App using a POST request.

The purpose of this new Logic App is to create an API endpoint to insert a new note into an existing [SQL Server](https://azure.microsoft.com/en-us/services/sql-database/) database. Let’s support this functionality in the HTTP trigger request body:

{%
    include image.html
    year='2019'
    month='03'
    file='6-httptrigger.png'
    alt='HTTP trigger'
%}

We need validation to ensure a user does not submit an empty note, so let’s add a ***‘condition’*** connector. This is essentially an **if statement** that will result in either **true**, or **false.**

{%
    include image.html
    year='2019'
    month='03'
    file='7-addcontrol.png'
    alt='Add control'
%}

Let’s write an **expression** in the ***‘Choose a value’*** field of the condition to determine if the note is empty or not:

{%
    include image.html
    year='2019'
    month='03'
    file='8-addexpressionfornotelength.png'
    alt='Add expression for note length'
%}

When you are finished, the condition should look similar to:

{%
    include image.html
    year='2019'
    month='03'
    file='9-notequaltoemptystring.png'
    alt='Note equal to empty string'
%}

If the condition is **false**, we’ll use a ***‘response’*** connector to return the corresponding status code ***(***[***422***](https://httpstatuses.com/422)***)*** and a friendly error message:

{%
    include image.html
    year='2019'
    month='03'
    file='10-conditionfalseresponse.png'
    alt='Condition false response'
%}

If the condition is **true**, we’ll use the ***‘SQL Server’*** connector to insert the note into our existing database:

{%
    include image.html
    year='2019'
    month='03'
    file='11-newsqlconnector.png'
    alt='New SQL connnector'
%}

Use the built-in **expressions** and **dynamic content** to insert the note and it’s associated metadata into the database:

{%
    include image.html
    year='2019'
    month='03'
    file='12-sqlinsertrow.png'
    alt='SQL insert row'
%}

Finally, use a ‘***response’*** connector to return a ***‘Created’*** status code **(**[201](https://httpstatuses.com/201)**)**:

{%
    include image.html
    year='2019'
    month='03'
    file='13-conditiontrueresponse.png'
    alt='Condition true response'
%}

## Deploy the Logic App

Now we’re ready to **deploy to Azure** from within Visual Studio! Doing so will assign a HTTPS web address to the Logic App and allow us to trigger it using a **POST request**.

Right-click on the project and click ***‘Deploy*’** to configure the options to deploy to a resource group:

{%
    include image.html
    year='2019'
    month='03'
    file='14-deploytoazure.png'
    alt='Deploy to Azure'
%}

{%
    include image.html
    year='2019'
    month='03'
    file='15-deploytoresourcegrouo.png'
    alt='Deploy to resource group'
%}

Before you eagerly hit *‘Deploy’*, click ***‘Edit Parameters’*** to assign a meaningful, recognizable name for your new Logic App:

{%
    include image.html
    year='2019'
    month='03'
    file='16-editparameters.png'
    alt='Edit parameters'
%}

**Authorize the deployment** by entering your Microsoft credentials and the Logic App will be deployed!

{%
    include image.html
    year='2019'
    month='03'
    file='17-powershellauthenication.png'
    alt='Powershell authentication'
%}

## Executing the Logic App

Let’s see what this new Logic App is made of! If you navigate to the newly deployed Logic App in the Azure portal and click ***\'Logic app designer\'*** to open the visual designer, you will find the assigned **‘HTTP POST URL’**:

{%
    include image.html
    year='2019'
    month='03'
    file='19-getposturl.png'
    alt='Get POST URL'
%}

Firstly, let’s test if the validation is working by submitting an empty note using the [Postman](https://www.getpostman.com) client. Make sure to use a **POST request** and set the ***‘Content-Type’*** header to **‘application/json’**:

{%
    include image.html
    year='2019'
    month='03'
    file='20-postemptynote.png'
    alt='Post empty note'
%}

If you are greeted by a ***‘422 Unprocessable Entity’*** response, if works!

{%
    include image.html
    year='2019'
    month='03'
    file='21-postemptyresponse.png'
    alt='POST empty response'
%}

Lastly, let’s **insert a new note** to the existing SQL Server database:


{%
    include image.html
    year='2019'
    month='03'
    file='22-postvalidnote.png'
    alt='POST valid note'
%}

You should receive a *‘**201 Created’*** response and a message indicating the new note was created, just like an API endpoint!

{%
    include image.html
    year='2019'
    month='03'
    file='23-postvalidnoteresponse.png'
    alt='POST valid note response'
%}

Query the existing SQL Server database to view the new note:

{%
    include image.html
    year='2019'
    month='03'
    file='24-noteindb.png'
    alt='Note in database'
%}

That’s it! We created an API endpoint that is codeless, serverless and will insert any input parameter to a SQL Server database!

## View the Run History

If you navigate to the Logic App in the Azure portal, you will be able to view the **run history**. This will enable you to immediately identity if any errors occurred, and **diagnose issues** as they occur.

{%
    include image.html
    year='2019'
    month='03'
    file='25-logicapprunhistory.png'
    alt='Logic App run history'
%}

By opening an individual ***‘run’*** in the Azure portal, you will be able to identity where the issue occurred, as well as the **input**, **output** and **response time** of each connector:

{%
    include image.html
    year='2019'
    month='03'
    file='26-logicapprundetails.png'
    alt='Logic app run details'
%}