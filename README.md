# Steps for creating a custom SSL certificate for use with Azure Services 

**Objective**</br>
Using the openssl tool, these are the steps from beginning to end to create a custom SSL certificate for use with Azure Services such as App Services, Application Gateway, etc...

**Certificate Signing Request**

A CSR or Certificate Signing request is a block of encoded text that is given to a Certificate Authority when applying for an SSL Certificate. It is usually generated on the server where the certificate will be installed and contains information that will be included in the certificate such as the organization name, common name (domain name), locality, and country. It also contains the public key that will be included in the certificate. A private key is usually created at the same time that you create the CSR, making a key pair.

1. Generate CSR with openssl

Hint: [Use Azure CloudShell!](https://docs.microsoft.com/en-us/azure/cloud-shell/overview)
<pre lang="...">
openssl req -new -newkey rsa:2048 -nodes -out [domainname].csr -keyout [domainname].key -subj "/C=[country]/ST=[state]/L=[city]/O=[org]/CN=[*.domain.com]"
</pre>

2. Request certificate from Certificate Authority

No documentation for this step as it varies by CA. Visit CA website for specific instructions.

3. Concat certificate chain
If your certificate authority gives you multiple certificates in the certificate chain, you need to merge the certificates in order. The order of your certificates should follow the order in the certificate chain, beginning with your certificate and ending with the root certificate
<pre lang="...">
cat [Base].crt [Intermediate...].crt [Root]> [outfile].crt
</pre>

4. Create pfx file

The PKCS#12 or PFX format is a binary format for storing the server certificate, any intermediate certificates, and the private key into a single file

Note you will be prompted for a password upon execution of the command below. This password must be used when applying the certificate to services in Azure and should be as highly confidential
<pre lang="...">
openssl pkcs12 -export -out [domain].pfx -inkey [domain].key -in [domain].crt
</pre>

<br/>
# createCustomCertificateforAzure
