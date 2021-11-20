---
title: OAuth2.0 Explained Simple
layout: default.liquid
---
#### John Mason - 20-11-2021

## OAuth2.0 Explained Simple

OAuth 2.0 is a security standard which lets one application access data from another application without sharing your credentials. 

### Key Terminology
**Resource Owner**: User that gives permission to another application to access another application's data.

**Client**: Application that wants to access another application's data on the Resource Owner's behalf.

**Authorization Server**: Application the Client is trying to get the data from. 

**Resource Server**: The API the Client wants to access.

**Scope**: Data the Client is trying to access or the actions the Client wants to perform in the Authorization Server on behalf of the Resource Owner. 

**Redirect URI/Callback URL**: Once the Resource Owner has given permission to the Client to access the data it wants from the Authorization Server, the server will redirect the Resource Owner back to the Client via the Rediect URI/Callback URL. 

**Client ID**: Authorization Server uses this to identify the Client. 

**Client Secret**: A password shared between the Authorization Server and the Client. The Authorization Server uses the Secret to validate the request came from the Client. 

**Response Type**: Type of data sent back to the Client by the Authorization Server. 

**Consent**: The Resource Owner giving consent to the Authorization Server that they wish to allow access to the data requested in the Scope by the Client. 

**Authorization Code**: Sent to the Client once the Authorization Server has recieved consent from the Resource Owner. 

**Access Token**: Authorization Server sends the Access Token to the Client who can then send the Access Token to the Resource Server in exchange for the data or action requested to be performed. 

### Example
Person A (Resource Owner) goes onto a website (Client) and the website want's to load their Gmail inbox. 

The website redirects Person A's browser to get their Consent from the Authorization Server sending the Client ID, Redirect URI, Response Type and Scope. The Authorization Server will establish a relationship with the Client by sharing the Client ID and Client Secret for exchanges and if the Client ID given can be verified against the Client Secret the relationship is established. The scope will be the data the Client wants access too. The Response Type will be the Authorization Code. Once Person A has given consent to the Client for the requested Scope the Redirect URI will be used by the Authorization Server to redirect Person A back to the website and sends the Authorization Code to the Client.

The Client now will send the Authorization code to the Authorization Server in exchange for an Access Token. The Authorization code's life is short lived so the Client may need to request a new Authorization code. If the Authorization code hasn't expired the Client can send a request to the Authorization Server for an Access Token. The Access Token can then by sent by the Client to the Resource Server in exchange for the data or action. The Access Token would typically have a longer life than the authorization code and once it's expired a new one will need to be requested. 

In this example once the website has got an Access Token it can request the Gmail inbox of Person A from the Resource Server since it now has been given authorized access from Person A. Note that no password was exchanged between the Client and the Resource Owner. The authorization is handled by the Authorization Server.