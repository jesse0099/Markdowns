# **Current Case.**

Sometimes, clients have special needs for their SSL certificates or already 
have a workflow that issues certificates. Whatever the case, this page describes
the process necessary to import them to ACM (AWS Certificates Manager) and
associate them with the corresponding resources.

# **Certificate Format.**

The client can send certificates as a **pair of files**, one containing the **hashed certificate** (**.pem file**), and the other, the corresponding **private key** (**.key File**).

Another format the client can use to send the certificate is a Personal Information Exchange file (**.pfx file**). For this format, the client must also send a **password** to operate over the PFX file.

## **Importing from a cert-key file pair.** 

***Requires:***

- cert.pem
- cert-key.key

*In case the cert-key.key file is protected with a passphrase; it must be provided.* 

### **Converts cert-key.key into PEM format**

***Generated files:***

- cert.pem
- no-password-cert-key.pem

ACM requires the private key to be in PEM format, so we will have to use a tool. OpenSSL is widely available and open source. It is recommended ([AWS Docs](https://aws.amazon.com/blogs/security/how-to-import-pfx-formatted-certificates-into-aws-certificate-manager-using-openssl/)).

***Using OpenSSL***

> openssl rsa -in {path_to/cert-key.key} -text > {path_to/no-password-cert-key.pem}

## **Importing from a PFX file.** 

***Requires:*** 

- cert.pfx
- password

### **Converts the PFX encoded data into PEM format**

***Generated files*:**

- cert.pem
- no-password-cert-key.pem
- ca-chain.pem (PEM file containing the root certificate of the CA.)

#### **Get no-password-cert-key.pem from PFX**

> openssl pkcs12 -in {path_to/cert.pfx} -nocerts -out {path_to/password-cert-key.pem}

The previous step generates a password-protected private key. To remove the password, run the following command.

> openssl rsa -in {path_to/password-cert-key.pem} -text > {path_to/no-password-cert-key.pem}

#### **Get cert.pem from PFX**

> openssl pkcs12 -in {path_to/cert.pfx} -clcerts -nokeys -out {path_to/cert.pem}

#### **Get ca-chain.pem from PFX**

> openssl pkcs12 -in {path_to/cert.pfx} -cacerts -nokeys -chain -out {path_to/ca-chain.pem}

## **Certificate with outdated encryption algorithm**

If and unsupported encryption algorithm error appears during any step of the above
described process, the certificate is probably encrypted with an outdated or too weak 
algorithm. Two options to import it successfully to ACM are described below.

### **OpenSSL legacy option**

If ACM supports the algorithm ([Prerequisites for importing certificates](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate-prerequisites.html)), using the ***-legacy*** option in the commands where the error is prompted should be enough, the rest of the process remains the same.

### **AWC cipher suit auto morphe**

[Supported cipher suites for auto morphe when uploading to ACM.](https://buyermls.atlassian.net/browse/DSO-1343)

## **Import cert.pem and no-password-cert-key.pem file pair to ACM.**

The cert.pem and no-password-cert-key.pem files are all you need to import the certs to ACM. The root certificate of the CA is not required. 

## **Import the cert.pem and no-password-cert-key.pem file pair to ACM when an intermediate certificate is required.**

It's true that the certificate of the CA is not always necessary, but some certificates will require an intermediate one. To determine whether it's required or not, there are some easy-to-use tools, such as:

- [sslshopper](https://www.sslshopper.com/)
- [ssllabs](https://www.ssllabs.com/ssltest/analyze.html?d=findbuyersbx.elliman.com)

 Both will provide information about the certification chain and its validity. SSLLabs offers more in-depth information, which can be useful under certain circumstances.

For example, with Entrust L1K certificates, SSLHopper and browsers might not show any issues with the certificate, but more restrictive tests, like the Facebook opengraph API, will. If an intermediate certificate is missing, these tools will show a missing link in the certification chain.

The missing intermediate certificate may or may not be present in the provided certificates files. To find the certificate CA, check the Bag Attributes in the issuer section, and once located, visit the CA's website. They often have a section with the types of certificates and their corresponding intermediate certificates. Download it and use it in the appropriate ACM section. The rest of the process remains unchanged.

- [Import from console](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate-api-cli.html#import-certificate-api:~:text=Import%20(AWS%20CLI)-,Import%20(console),-The%20following%20example)

- [Import from cli](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate-api-cli.html#import-certificate-api:~:text=choose%20Import.-,Import%20(AWS%20CLI),-The%20following%20example)

## **Associates cert with AWS Resources.**

A set of load balancers are currently managing the certificate propagation process. You can read the documentation of how to do it at: [V1-HVS Certificate](https://buyermls.atlassian.net/wiki/spaces/PER/pages/2722529304/V1-HVS+Certificate)