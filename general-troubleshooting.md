---
css: toc
layout: page
title: General Troubleshooting
permalink: /common/
---

- Do not remove this line (it will not be displayed)
{:toc}

## Missing or outdated root CA certificates

A program or application may not have the root CA certificate needed to verify a server's SSL certificate. This can happen if the root CA certificate is missing or outdated, or if the program is looking for the certificate in the wrong location.

The root CA certificates of the biggest root CA authorities like Microsoft, Google, DigiCert, GlobalSign, VeriSign etc. are typically updated automatically by operating system updates. However in case you need add the certificate manually - maybe it is the one missing by your own company - follow these steps:

### Windows

To update root CA certificates using an UI, follow these steps:

- Open the Microsoft Management Console (MMC) by pressing Windows key + R, typing "mmc" and hitting Enter.
- From the File menu, select Add/Remove Snap-in, and then select Certificates from the Available snap-ins list and click Add.
- Choose the "Computer Account" option and click Next.
- Select "Local Computer" and click Finish.
- Expand the "Trusted Root Certification Authorities" folder and right-click on "Certificates" to select "All Tasks" and then "Import".
- Follow the prompts to import the new root CA certificate file.

To update root CA certificates using the command-line, follow these steps:

- Open the Command Prompt (cmd) or PowerShell (powershell/pwsh) as an administrator.
- Run the following command to import the new root CA certificate file:

  ```shell
  certutil -addstore -f "Root" <path to certificate file>
  ```

  Replace `<path to certificate file>` with the path to the new root CA certificate file.

To update intermediate certificates using an UI, follow these steps:

- Open the MMC and select Certificates from the Available snap-ins list.
- Choose the "Computer Account" option and select "Local Computer".
- Expand the "Intermediate Certification Authorities" folder and right-click on "Certificates" to select "All Tasks" and - then "Import".
- Follow the prompts to import the new intermediate certificate file.

To update intermediate certificates using the command-line, follow these steps:

- Open the Command Prompt (cmd) or PowerShell (powershell/pwsh) as an administrator.
- Run the following command to import the new intermediate certificate file:

  ```shell
  certutil -addstore -f "CA" <path to certificate file>
  ```

  Replace `<path to certificate file>` with the path to the new intermediate certificate file.

### macOS

To update root CA certificates in macOS using an UI, follow these steps:

- Open the "Keychain Access" app from the Applications > Utilities folder.
- Select "System" from the Keychains list.
- Select "Certificates" from the Category list.
- Drag and drop the new root CA certificate file into the Keychain Access window.
- Double-click on the imported certificate, expand the "Trust" section, and ensure that the "Always Trust" option is - selected.

To update root CA certificates in macOS using the command-line, follow these steps:

- Open the Terminal app.
- Run the following command to import the new root CA certificate file:

  ```shell
  sudo security add-trusted-cert -d -r trustRoot -k "/Library/Keychains/System.keychain" <path to certificate file>
  ```

  Replace `<path to certificate file>` with the path to the new root CA certificate file.

Note: The `sudo` command is used to run the security command with administrator privileges.

To update intermediate certificates, follow these steps:

- Open the Keychain Access app and select "System" from the Keychains list.
- Select "Certificates" from the Category list.
- Locate the expired or missing intermediate certificate and delete it.
- Drag and drop the new intermediate certificate file into the Keychain Access window.
- Double-click on the imported certificate, expand the "Trust" section, and ensure that the "Always Trust" option is - selected.

### Linux (Ubuntu)

To update root CA certificates in Ubuntu Linux, follow these steps:

- Install the ca-certificates package by running the following command in the terminal: `sudo apt-get install ca-certificates`
- Copy the new root CA certificate file to the `/usr/local/share/ca-certificates/` directory.
- Run the following command in the terminal to update the certificate store: `sudo update-ca-certificates`

To update intermediate certificates, follow these steps:

- Copy the new intermediate certificate file to the `/usr/local/share/ca-certificates/` directory.
- Run the sudo `update-ca-certificates` command to update the certificate store.

Note: The exact steps may vary depending on the Linux distribution and version.

## SSL certificate chain issues

A server's SSL certificate may be missing one or more intermediate certificates in its chain, which are needed to establish trust between the root CA and the server's certificate.

## Certificate revocation

The SSL certificate presented by the server may have been revoked by the issuing CA, or may have expired, leading to trust issues.

First check the SSL certificate trust chain like described [here](#ssl-certificate-chain-issues). When the certificate is expired, you might have to update the certificate store of the operating system (like shown [here](#missing-or-outdated-root-ca-certificates)) or the certificate store of your program like shown [here](/{{site.baseurl}}/#want-a-solution-for-the-error).

An actual SSL certificate revocation might occur for several reasons like:

1. Compromised private key: If the private key used to generate the SSL certificate is compromised or stolen, the certificate must be revoked to prevent unauthorized access to sensitive data.

2. Misissued certificate: If the SSL certificate was issued incorrectly or contains incorrect information, it may need to be revoked to prevent further security issues.

3. Certificate expiration: SSL certificates have an expiration date, and if the certificate is not renewed or replaced before the expiration date, it must be revoked.

4. Change in domain ownership: If the ownership of the domain associated with the SSL certificate changes, the certificate may need to be revoked to ensure the security of the new domain owner.

5. Breach of CA policies: If a certificate authority (CA) violates industry standards or security policies, its certificates may be revoked to protect users' security and trust.

SSL certificate revocation is an important mechanism for maintaining the integrity and security of online communication, and is used to prevent unauthorized access to sensitive data and protect users from security threats.

## Self-Signed certificates

To resolve the issue when using a self-signed SSL certificate, the certificate needs to be manually added to the trusted certificates list on the client-side. This will enable the program or application to establish a secure and encrypted connection with the server using the self-signed SSL certificate.

However, it's important to note that relying on self-signed certificates for secure communication is generally discouraged, as it can leave users vulnerable to man-in-the-middle attacks and other security risks. In production environments, it's recommended to use SSL certificates issued by trusted third-party CAs to ensure the highest level of security and trust.
