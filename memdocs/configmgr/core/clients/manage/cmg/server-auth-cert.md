---
title: CMG server authentication certificate
titleSuffix: Configuration Manager
description: The CMG uses HTTPS for secure client communication over the public internet. You can get a certificate from a public provider, or issue one from your public key infrastructure (PKI).
ms.date: 09/28/2020
ms.prod: configuration-manager
ms.technology: configmgr-client
ms.topic: how-to
ms.assetid: 9d11e590-5df8-4a49-b1c0-0883b8969cfc
author: aczechowski
ms.author: aaroncz
manager: dougeby
---

# CMG server authentication certificate

*Applies to: Configuration Manager (current branch)*

The first step when you set up a cloud management gateway (CMG) is to get the server authentication certificate. The CMG creates an HTTPS service to which internet-based clients connect. The server requires a server authentication certificate to build the secure channel. You can acquire a certificate for this purpose from a public provider, or issue it from your public key infrastructure (PKI).

When you create the CMG in the Configuration Manager console, you provide this certificate. The common name (CN) of this certificate defines the service name of the CMG.

> [!NOTE]
> You may need additional certificates for clients and management points. These certificates are covered in the third step of the CMG setup process, [Configure client authentication](configure-authentication.md).

A reminder of some CMG terminology that's used in this article:

- **Service name**: The common name (CN) of the CMG server authentication certificate. Clients and the CMG connection point site system role communicate with this service name. For example, `GraniteFalls.contoso.com` or `GraniteFalls.cloudapp.net`.

- **Deployment name**: The first part of the service name plus the Azure location for the cloud service deployment. For example, `GraniteFalls.cloudapp.net`. The cloud service manager component of the service connection point uses this name when it deploys the CMG in Azure. The deployment name is always in an Azure domain.

## Choose the certificate type

First, decide where you want to get the certificate. There are several factors to consider.

Clients must trust the CMG server authentication certificate to establish the HTTPS channel with the CMG service. There are two methods to accomplish this trust:

1. Use a certificate from a public and globally trusted certificate provider. For example, but not limited to, DigiCert, Thawte, or VeriSign.

    - Windows clients include trusted root certificate authorities (CAs) from these providers. By using a certificate issued by one of these providers, your clients automatically trust it.

    - There's a cost associated with this certificate, which is specific to the provider.

1. Use a certificate issued by an enterprise CA from your public key infrastructure (PKI).

    - Most enterprise PKI implementations add the trusted root CAs to Windows clients. For example, if you use Active Directory Certificate Services with group policy. If you issue the CMG server authentication certificate from a CA that your clients don't automatically trust, add the CA trusted root certificate to internet-based clients.

        - You can also use Configuration Manager certificate profiles to provision certificates on clients. For more information, see [Introduction to certificate profiles](../../../../protect/deploy-use/introduction-to-certificate-profiles.md).

        - If you plan to [install the Configuration Manager client from Intune](../../../../comanage/how-to-prepare-Win10.md#install-the-configuration-manager-client), you can also use Intune certificate profiles to provision certificates on clients. For more information, see [Configure a certificate profile](../../../../../intune/protect/certificates-configure.md).

    - Your organization may have an internal cost to issue certificates, but there are generally no external costs associated with this certificate.

> [!IMPORTANT]
> Before you get this certificate, make sure the service name is globally unique for the cloud service and storage account. Also make sure the name uses supported characters. For more information, see [Globally unique name](#globally-unique-name).

### Summary comparison of certificate types

|  | Public provider | Enterprise PKI |
|---------|---------|---------|
| **Client trust** | Trusted in Windows by default | Automatic with some implementations, otherwise need to deploy |
| **Cost** | Yes | Not typical |
| **Service name example** | `GraniteFalls.contoso.com` | `GraniteFalls.contoso.com` or `GraniteFalls.cloudapp.net`|
| **DNS CNAME required** | Yes | No for Azure domain service name (`GraniteFalls.cloudapp.net`) |

> [!NOTE]
> The CMG server authentication certificate supports wildcards. Some certificate authorities issue certificates using a wildcard character for the service name prefix. For example, `*.contoso.com`. Some organizations use wildcard certificates to simplify their PKI and reduce maintenance costs.<!--491233-->
>
> For more information on how to use a wildcard certificate with a CMG, see [Set up a CMG](setup-cloud-management-gateway.md#set-up-a-cmg).<!--SCCMDocs issue #565-->

## Globally unique name

This certificate requires a globally unique name to identify the service in Azure. Before you request a certificate, confirm that the Azure _deployment name_ you want is unique. For example, `GraniteFalls.cloudapp.net`.

1. Sign in to the [Azure portal](https://portal.azure.com).

1. From the Azure portal home page, select **Create a resource** under Azure services.

1. Search for **Cloud service**. Select **Create**.

1. In the **DNS name** field, type the prefix you want, for example `GraniteFalls`. The interface reflects whether the domain name is available or already in use by another service.

    > [!IMPORTANT]
    > Don't create the service in the portal, just use this process to check the name availability.

If you also enable the CMG for content, confirm that it's also a unique Azure storage account name. If the CMG cloud service name is unique, but the storage account isn't, Configuration Manager fails to provision the service in Azure. Repeat the above process in the Azure portal with the following changes:

- Search for **Storage account**.

- Test your name in the **Storage account name** field.

> [!IMPORTANT]
> The DNS name prefix, for example `GraniteFalls`, should be 3 to 24 characters long, and only use alphanumeric characters. Don't use special characters, like a 
> dash (`-`).<!-- SCCMDocs#1080 -->

## Issue the certificate

The CMG server authentication certificate supports the following configurations:

- 2048-bit or 4096-bit key length

- This certificate supports key storage providers for certificate private keys (v3). For more information, see [CNG v3 certificates overview](../../../plan-design/network/cng-certificates-overview.md).

### <a name="bkmk_serverauthpublic"></a> Use a public provider certificate

A third-party certificate provider can't create a certificate for an Azure domain like `cloudapp.net`, because Microsoft owns those domains. You can only get a certificate issued for a domain you own. The main reason for acquiring a certificate from a third-party provider is that your clients already trust that provider's root certificate.

The specific process to get this certificate varies by provider. For more information, contact your third-party certificate provider.

For the web server certificate common name (CN):

- You've made sure the _deployment name_ is [globally unique](#globally-unique-name) in Azure for the cloud service and storage account. For example, `GraniteFalls.cloudapp.net`.

- To determine the _service name_, append the _deployment name_ prefix (`GraniteFalls`) to your organization's domain name (`contoso.com`).

- Use this _service name_ for the certificate common name (CN). For example, `GraniteFalls.contoso.com`.

Next, you need to [create a DNS CNAME alias](#create-a-dns-cname-alias).

### <a name="bkmk_serverauthpki"></a> Use an enterprise PKI certificate

Issuing a web server certificate from your organization's PKI varies by product. The instructions for [Deploying the service certificate for cloud-based distribution points](../../../plan-design/network/example-deployment-of-pki-certificates.md#BKMK_clouddp2008_cm2012) are for Active Directory Certificate Services. This process generally applies for the CMG server authentication certificate.

For the web server certificate common name (CN):

- You've made sure the _deployment name_ is [globally unique](#globally-unique-name) in Azure for the cloud service and storage account. For example, `GraniteFalls.cloudapp.net`.

- To determine the _service name_, you have two options:

  - Use your domain name. Append the _deployment name_ prefix (`GraniteFalls`) to your organization's domain name (`contoso.com`). For example, `GraniteFalls.contoso.com`. For this option, you also need to [create a DNS CNAME alias](#create-a-dns-cname-alias).

  - Use the Azure deployment name. This option doesn't require a DNS CNAME alias. For example:

    - For the Azure public cloud: `GraniteFalls.cloudapp.net`.

    - For the Azure US Government cloud: `GraniteFalls.usgovcloudapp.net`.

- Use this _service name_ for the certificate common name (CN).

## Create a DNS CNAME alias

If the CMG service name uses your organization's domain name (`GraniteFalls.contoso.com`), you need to create a DNS canonical name record (CNAME). This alias maps the _service name_ to the _deployment name_.

Create a CNAME record in your organization's public DNS. The CMG service in Azure and all clients that use it need to resolve the service name. For example:

- Contoso names their CMG **GraniteFalls**.

- The deployment name in Azure is `GraniteFalls.cloudapp.net`.

- In Contoso's public DNS `contoso.com` namespace, the DNS administrator creates a new CNAME record for the service name `GraniteFalls.contoso.com` to the Azure deployment name, `GraniteFalls.cloudapp.net`.

When you create the CMG, while the certificate has `GraniteFalls.contoso.com` as the CN, Configuration Manager only extracts the service name prefix, for example: **GraniteFalls**. It appends this prefix to the Azure service domain (`cloudapp.net`) to create the deployment name. For example, `GraniteFalls.cloudapp.net`. The CNAME alias in the DNS namespace for your domain (`contoso.com`) maps together these two FQDNs.

The Configuration Manager client policy includes the CMG service name, `GraniteFalls.contoso.com`. The client resolves the service name via the CNAME alias to the deployment name, `GraniteFalls.cloudapp.net`. It then can resolve the IP address of the deployment name to communicate with the service in Azure.<!--SCCMDocs issue #565-->

## Next steps

Continue your CMG setup by configuring Azure Active Directory (Azure AD):
  
> [!div class="nextstepaction"]
> [Configure Azure AD](configure-azure-ad.md)
