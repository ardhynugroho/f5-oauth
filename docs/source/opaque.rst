OAuth Opaque Token
====

#. Creating OAuth scope from menu: **Access  ››  Federation : OAuth Authorization Server : Scope**
   
   Click **Create** button and fill in following parameters:
   
   - Name: request
   - Scope Name: request
   - Caption: request
   
   Click **Repeat** button, and create another scope below:
   
   - request.headers
   - request.ip
   - request.user-agent
   - image
   - image.jpeg
   - image.png
   - image.svg
   - image.webp

#. Add an OAuth client from menu: **Access  ››  Federation : OAuth Authorization Server : Client Application**
   
   Click **Create** button and fill in following parameters:
   
   - Name: partner-app-1
   - Application Name: partner-app-1
   - Caption: partner-app-1
   - Grant Type: Authorization Code / Hybrid
   - Redirect URI(s): https://callback
   - Scopes: request; image
    
   After creation, click to OAuth client name to see generated Client ID & Secret, save it to be used on later step.

#. Add Oauth resource server (RS) from menu: **Access  ››  Federation : OAuth Authorization Server : Resource Server**

   Click **Create** button and fill in following parameters:
   
   - Name: app-1-rs
   - Authentication Type: Secret
   
   After creation, click again to RS name to see resource server ID & Secret, save it to be used on later step.

#. Create OAuth service profile from menu: **Access  ››  Federation : OAuth Authorization Server : OAuth Profile**

   - Name: oauth-opaque
   - Client Application: Move ``partner-app-1`` from Available to Selected
   - Resource Server: Move ``app-1-rs`` from Available to Selected

#. Create IDP using local-db from menu: **Access  ››  Authentication : Local User DB : Instances**

   Click **Create New Instance** button to create new user database instance & fill in following parameters:
   
   - Name: demo-users
   - Lockout Interval (in seconds): 600
   - Lockout Threshold: 3
   - Dynamic User Remove Interval (in seconds): 1800

#. Add new users
   
   **Access  ››  Authentication : Local User DB : Users**

#. Create access policy
   
   **Access  ››  Profiles / Policies : Access Profiles (Per-Session Policies)**

   - Name: ap_authserver_1
   - Profile Type: All
   - OAuth Profile: 
   - Languages: English (en)

#. Edit just created policy using Visual Policy Editor (VPE)

   - Add Logon > Logon Page
   - Add Authentication > LocalDB Auth
   - Add Authentication > OAuth Authorization
   - Change OAuth Authorization end to "Allow"

   Apply Access Policy & Close the VPE

#. Create VS
    
   - Name: oauth_as_vs
   - Destination Address/Mask: 10.1.10.70
   - Service Port: 443
   - HTTP Profile (Client): http
   - SSL: clientssl
   - Access Profile: ap_authserver_1
    
#. Test get bearer token
    
   test using Postman

#. Test using python

   - ``apt update; apt install -y python3-pip python3-venv``
