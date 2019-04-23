---
title: Configurer les extensions CDP et AIA sur CA1
description: Cette rubrique fait partie du guide des certificats de serveur de déploiement pour les déploiements de sans fil et câblé à 802.1 X
manager: dougkim
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.date: 07/26/2018
ms.openlocfilehash: 2bdd03636d1cbbcf5b0355358cbedeb63f97af1b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834220"
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>Configurer les extensions CDP et AIA sur CA1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour configurer le Point de Distribution de liste de révocation de certificats (CRL) (CDP) et les paramètres d’accès d’informations d’autorité (AIA) sur CA1.  
  
Pour effectuer cette procédure, vous devez être membre du groupe Admins du domaine.  
  
#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>Pour configurer les extensions CDP et AIA sur CA1  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Autorité de certification**.  
  
2.  Dans l’arborescence de la console Autorité de Certification, cliquez sur **corp-CA1-CA**, puis cliquez sur **propriétés**.  
  
    > [!NOTE]  
    > Le nom de votre autorité de certification est différent si vous n’avez pas nommé l’ordinateur CA1 et votre nom de domaine est différent de celui dans cet exemple. Le nom de l’autorité de certification est au format *domaine*-*NomOrdinateurAutoritéCertification*- CA.  
  
3.  Cliquez sur l’onglet **Extensions** . Vérifiez que **sélectionner une extension** a la valeur **points de Distribution CRL (CDP)**, puis, dans le **spécifier les emplacements à partir de laquelle les utilisateurs peuvent obtenir une liste de révocation de certificats (CRL)**, procédez comme suit :  
  
    1.  Sélectionnez l’entrée `file://\\<ServerDNSName>\CertEnroll\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
    2.  Sélectionnez l’entrée `https://<ServerDNSName>/CertEnroll/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
    3.  Sélectionnez l’entrée qui commence par le chemin d’accès `ldap:///CN=<CATruncatedName><CRLNameSuffix>,CN=<ServerShortName>`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
4.  Dans **spécifier les emplacements à partir de laquelle les utilisateurs peuvent obtenir une liste de révocation de certificats (CRL)**, cliquez sur **ajouter**. Le **ajouter un emplacement** boîte de dialogue s’ouvre.  
  
5.  Dans **ajouter un emplacement**, dans **emplacement**, type `https://pki.corp.contoso.com/pki/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, puis cliquez sur **OK**. Cela vous ramène à la boîte de dialogue Autorité de certification.  
  
6.  Sur le **Extensions** , sélectionnez les cases à cocher suivantes :  
  
    -   **Inclure dans les listes de révocation. Les clients l’utilisent pour trouver les emplacements CRL Delta**  
  
    -   **Inclure dans l’extension CDP des certificats émis**  
  
7.  Dans **spécifier les emplacements à partir de laquelle les utilisateurs peuvent obtenir une liste de révocation de certificats (CRL)**, cliquez sur **ajouter**. Le **ajouter un emplacement** boîte de dialogue s’ouvre.  
  
8.  Dans **ajouter un emplacement**, dans **emplacement**, type `file://\\pki.corp.contoso.com\pki\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, puis cliquez sur **OK**. Cela vous ramène à la boîte de dialogue Autorité de certification.  
  
9. Sur le **Extensions** , sélectionnez les cases à cocher suivantes :  
  
    -   **Publier les listes de révocation sur cet emplacement**  
  
    -   **Publier les listes CRL Delta sur cet emplacement**  
  
10. Modification **sélectionner une extension** à **les accès aux informations autorité (AIA)**, puis, dans le **spécifier les emplacements à partir de laquelle les utilisateurs peuvent obtenir une liste de révocation de certificats (CRL)**, faire les éléments suivants :  
  
    1.  Sélectionnez l’entrée qui commence par le chemin d’accès `ldap:///CN=<CATruncatedName>,CN=AIA,CN=Public Key Services`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
    2.  Sélectionnez l’entrée `https://<ServerDNSName>/CertEnroll/<ServerDNSName>_<CaName><CertificateName>.crt`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
    3.  Sélectionnez l’entrée `file://\\<ServerDNSName>\CertEnroll\<ServerDNSName><CaName><CertificateName>.crt`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
11. Dans **spécifier les emplacements à partir de laquelle les utilisateurs peuvent obtenir le certificat pour cette autorité de certification**, cliquez sur **ajouter**. Le **ajouter un emplacement** boîte de dialogue s’ouvre.  
  
12. Dans **ajouter un emplacement**, dans **emplacement**, type `https://pki.corp.contoso.com/pki/<ServerDNSName>_<CaName><CertificateName>.crt`, puis cliquez sur **OK**. Cela vous ramène à la boîte de dialogue Autorité de certification.  
  
13. Sur le **Extensions** onglet, sélectionnez **Include dans l’AIA des certificats émis**.  
  
14. Lorsque vous êtes invité à redémarrer les Services de certificats Active Directory, cliquez sur **non**. Vous allez de redémarrer le service ultérieurement.  
  

