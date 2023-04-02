OAuth Opaque Token
====

1. Create Scope

   **Access  ››  Federation : OAuth Authorization Server : Scope**
   
   - request
   - request.headers
   - request.ip
   - request.user-agent
   - image
   - image.jpeg
   - image.png
   - image.svg
   - image.webp

#. Add OAuth client

   **Access  ››  Federation : OAuth Authorization Server : Client Application**
   
   *Parameters:*
   
   - Name:
   - Application Name
   - Caption
   - Grant Type: Authorization Code / Hybrid
   - Redirect URI(s): https://callback
   - Scopes:
    
   After create, click to client name to see generated Client ID & Secret

#. Add Oauth resource server (RS)

   **Access  ››  Federation : OAuth Authorization Server : Resource Server**

   - Name: http-bin_rs
   - Authentication Type: Secret
   - Finished
   
   After create, click again to RS name to see resource server ID & secret

#. Create OAuth service profile
   
   **Access  ››  Federation : OAuth Authorization Server : OAuth Profile**

   - Name:
   - Client Application: Move from Available to Selected
   - Resource Server: Move from Available to Selected

#. Create IDP using local-db

   **Access  ››  Authentication : Local User DB : Instances**

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
