# Current Case

Sometimes, clients have special needs for their SSL certificates, or just already 
have a workflow that issue certificates. Whathever the case, this page describe
the process necessary to import them to ACM (Amazon Certificates Manager), and
associate them with the corresponding resources.

# Certificate Format

Certificates can be send by client as a **pair of files**, one containing the **hashed certificate** (**.pem file**), and the other, the corrresponding **private key** (**.key File**).

Other format the client can use to send the certificate is in a Personal Information Exchange file (**.pfx file**). For this format the client also needs to send a **password** to operate over the PFX file.
