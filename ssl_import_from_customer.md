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

> openssl rsa -in {path_to/cert.pfx} -nocerts -out {path_to/no-password-cert-key.pem}

#### **Get cert.pem from PFX**

> openssl pkcs12 -in {path_to/cert.pfx} -clcerts -nokeys -out {path_to/cert.pem}

#### **Get ca-chain.pem from PFX**

> openssl pkcs12 -in {path_to/cert.pfx} -cacerts -nokeys -chain -out {path_to/ca-chain.pem}

## **Import cert.pem and no-password-cert-key.pem file pair to ACM.**

The cert.pem and no-password-cert-key.pem files are all you need to import the certs to ACM. The root certificate of the CA is not required.

- [Import from console](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate-api-cli.html#import-certificate-api:~:text=Import%20(AWS%20CLI)-,Import%20(console),-The%20following%20example)

- [Import from cli](https://docs.aws.amazon.com/acm/latest/userguide/import-certificate-api-cli.html#import-certificate-api:~:text=choose%20Import.-,Import%20(AWS%20CLI),-The%20following%20example)

## **Associates cert with AWS Resources.**

A set of load balancers are currently managing the certificate propagation process. You can read the documentation of how to do it at: [V1-HVS Certificate](https://buyermls.atlassian.net/wiki/spaces/PER/pages/2722529304/V1-HVS+Certificate)