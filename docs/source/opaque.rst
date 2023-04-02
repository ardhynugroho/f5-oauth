OAuth Opaque Token
====

Creating OAuth Scope 
----

Navigate to **Access  ››  Federation : OAuth Authorization Server : Scope**
   
Click **Create** button and fill in following parameters:
   
- Name: request
- Scope Name: request
- Caption: request

.. image:: img/11-new-scope.png
   
Click **Repeat** button, and create another scope below:
   
- request.headers
- request.ip
- request.user-agent
- image
- image.jpeg
- image.png
- image.svg
- image.webp

Add An OAuth Client 
----

Navigate to **Access  ››  Federation : OAuth Authorization Server : Client Application**
   
Click **Create** button and fill in following parameters:

- Name: partner-app-1
- Application Name: partner-app-1
- Caption: partner-app-1
- Grant Type: Authorization Code / Hybrid
- Redirect URI(s): https://callback
- Scopes: request; image
 
After creation, click to OAuth client name to see generated Client ID & Secret, save it to be used on later step.

.. image:: img/12-oauth-client-1.png
.. image:: img/12-oauth-client-2.png
.. image:: img/12-oauth-client-3.png

Add Oauth Resource Server (RS)
----

Navigate to **Access  ››  Federation : OAuth Authorization Server : Resource Server**

Click **Create** button and fill in following parameters:

- Name: app-1-rs
- Authentication Type: Secret

After creation, click again to RS name to see resource server ID & Secret, save it to be used on later step.

.. image:: img/13-oauth-rs-1.png

Create OAuth Profile
----

Navigate to **Access  ››  Federation : OAuth Authorization Server : OAuth Profile**

 - Name: oauth-opaque
 - Client Application: Move ``partner-app-1`` from Available to Selected
 - Resource Server: Move ``app-1-rs`` from Available to Selected

 .. image:: img/14-oauth-profile-1.png

Create Local Identity Provider (IdP)
----

Navigate to **Access  ››  Authentication : Local User DB : Instances**

Click **Create New Instance** button to create new user database instance & fill in following parameters:

- Name: demo-users
- Lockout Interval (in seconds): 600
- Lockout Threshold: 3
- Dynamic User Remove Interval (in seconds): 1800

.. image:: img/15-local-db-1.png

Add Sample User Credentials
----

Create admin & operator users from menu: **Access  ››  Authentication : Local User DB : Users**

.. image:: img/16-new-user-1.png

Create Access Policy
----

Navigate to  **Access  ››  Profiles / Policies : Access Profiles (Per-Session Policies)**

- Name: ap-oauth-as-1
- Profile Type: All
- OAuth Profile: 
- Languages: English (en)

.. image:: img/17-ap-oauth-1.png
.. image:: img/17-ap-oauth-2.png
.. image:: img/17-ap-oauth-3.png

Create Access Policy Flow
----

Edit just created policy using Visual Policy Editor (VPE)

.. image:: img/18-vpe-1.png

Add Logon > Logon Page

.. image:: img/18-vpe-3.png

Add Authentication > LocalDB Auth

.. image:: img/18-vpe-4.png

Add Authentication > OAuth Authorization

.. image:: img/18-vpe-5.png

Change OAuth Authorization end to "Allow"

.. image:: img/18-vpe-6.png

The policy flow view

.. image:: img/18-vpe-7.png

Apply Access Policy & Close the VPE

Create Virtual Server
----

Create a virtual server to serve as OAuth Authorization Server service
    
- Name: oauth_as_vs
- Destination Address/Mask: 10.1.10.70
- Service Port: 443
- HTTP Profile (Client): http
- SSL: clientssl
- Access Profile: ap-oauth-as-1
    
Testing Opaque Token Request
----

Get bearer token test using Postman

Configure Postman as ``partner-app-1`` client then click the **Get New Access Token**

.. image:: img/19-test-1.png 

Verify user credential

.. image:: img/19-test-2.png

Authorization confirmation

.. image:: img/19-test-3.png

Opaque bearer token received from OAuth AS

.. image:: img/19-test-4.png

This conclude the configuration of OAuth AS server to generate opaque token.