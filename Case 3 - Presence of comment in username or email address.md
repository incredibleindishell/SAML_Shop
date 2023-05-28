
### Presence of comment in username or email address - Web application 
<hr>

When a user has HTML comment present in his/her username or email address, after successfull login the IDP will return SAML response containing Name identifier having HTML comment in it.

Sometime, during the processing of SAML response, the web application strip out the HTML comment from it which leads to user account takeover.

#### Attack Scenario
<hr>

Let's assume, we have a below mentioned user email registered with IDP:

    admin<!--b0x-->@test.com

When IDP will return SAML response to the web application with below mentioned Subject:

    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">admin@test.com&lt;!--b0x--&gt;</NameID>
    
The web application on server-side will strip out the HTML comment `&lt;!--b0x--&gt;` and resultant will be:

    <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">admin@test.com</NameID>

