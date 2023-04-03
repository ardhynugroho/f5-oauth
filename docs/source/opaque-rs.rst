Configuring F5 APM As Resource Server Accepting Opaque Token
====

Create OAuth Provider
----

Navigate to **Access  ››  Federation : OAuth Client / Resource Server : Provider**

Create new OAuth provider with following parameters:

- Name: f5-apm-opaque
- Type: F5
- Use Auto JWT: disabled/clear
- Authentication URI: https://10.1.10.70/f5-oauth2/v1/authorize
- Token URI: https://10.1.10.70/f5-oauth2/v1/token
- Token Validation Scope URI: https://10.1.10.70/f5-oauth2/v1/introspect
- UserInfo Request URI: https://10.1.10.70/f5-oauth2/v1/userinfo

Server IP ``10.1.10.70`` is OAuth AS server address used in this lab. 
It is actually VS listener address in the same F5 instance.

Click **Save** to save the changes

.. image: img/201-add-oauth-provider-1.png

Create OAuth Resource Server Profile
----

Navigate to **Access  ››  Federation : OAuth Client / Resource Server : OAuth Server** and click **Create** button.

Configure following parameters:

- Name: app-1
- Mode: Resource Server
- Type: F5
- Oauth Provider: f5-apm-opaque
- DNS Resolver: f5-aws-dns
- Resource Server ID: Input the ID obtained from step in :ref:`register oauth rs`
- Resource Server Secret: Input the secret obtained from step in :ref:`register oauth rs`
- Resource Server's ServerSSL Profile Name: serverssl

.. image: img/202-oauth-resource-server-1.png

Create Access Profile for Resource Server
----

Navigate to **Access  ››  Profiles / Policies : Access Profiles (Per-Session Policies)**

Click **Create** button and configure following parameters:

- Name: app-1-ap
- Profile Type: OAuth-Resource Server
- OAuth Profile: oauth-opaque
- Languages: English (en)

.. image: img/204-oauth-ap-1.png
.. image: img/204-oauth-ap-2.png

Then edit policy flow detils in VPE [#]_

.. [#] Visual Policy Editor

Change flow ending from **Deny** to **Allow** then apply & close VPE.

.. image: img/204-oauth-ap-3.png

Create Per-Request Policy Profile for Resource Server
----

Navigate to **Access  ››  Profiles / Policies : Per-Request Policies** then clik **Create** button.

Configure following parameters:

- Name: app-1-prp
- Policy Type: All
- Incomplete Action: Deny
- Languages: Move ``English (en)`` from Factory Builtin to Accepted Languages

.. image: img/205-oauth-prp-1.png

Click **Finished** to save the changes

Edit the policy flow using VPE.

1. Click **Add New Subroutine** give it a name, example: ``Scope Check``
2. Click the (+) sign to add process, select ``Authentication > OAuth Scope Management`` and click **Add Item**
3. Click the new process box, and configure following parameters:

    - Token Validation: External
    - Server: /Common/app-1
    - Scope Request: /common/F5ScopeRequest

4. Save the changes
5. Click (+) sign after **Start** then add ``Scope Check`` Subroutine

    .. image: img/205-oauth-prp-3.png

The policy check flow should be like this

.. image: img/205-oauth-prp-5.png

Attach Access & Per-Request Policy Profile
----

Edit the virtual server that serve ``app-1`` application.
Scroll down to **Access Policy** and configure following profile to the virtual server

.. image: img/206-access-policy-1.png

Test The configuration
----

