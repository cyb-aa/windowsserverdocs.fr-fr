---
title: Configurer les extensions CDP et AIA sur CA1
description: Cette rubrique fait partie du guide déployer des certificats de serveur pour les déploiements sans fil et câblés 802.1 X.
manager: dougkim
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.prod: windows-server
ms.technology: networking
ms.author: lizross
author: eross-msft
ms.date: 07/26/2018
ms.openlocfilehash: 6434b05d45cf9b8309fac4f6cd5cb5d6d149a7c2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318404"
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>Configurer les extensions CDP et AIA sur CA1

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Vous pouvez utiliser cette procédure pour configurer le point de distribution de liste de révocation de certificats (CDP) et les paramètres d’accès aux informations de l’autorité (AIA) sur CA1.  
  
Pour effectuer cette procédure, vous devez être membre du groupe Admins du domaine.  
  
#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>Pour configurer les extensions CDP et AIA sur CA1  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **Outils**, puis sur **Autorité de certification**.  
  
2.  Dans l’arborescence de la console autorité de certification, cliquez avec le bouton droit sur **Corp-CA1-ca**, puis cliquez sur **Propriétés**.  
  
    > [!NOTE]  
    > Le nom de votre autorité de certification est différent si vous n’avez pas nommé l’ordinateur CA1 et que votre nom de domaine est différent de celui de cet exemple. Le nom de l’autorité de certification est au format *domaine*-*nomordinateurautoritécertification*-ca.  
  
3.  Cliquez sur l’onglet **Extensions** . Assurez-vous que l' **option Sélectionner l’extension** est définie sur **point de distribution de liste de révocation de certificats (CDP)** et, dans **spécifier les emplacements à partir desquels les utilisateurs peuvent obtenir une liste de révocation de certificats**, procédez comme suit :  
  
    1.  Sélectionnez l’entrée `file://\\<ServerDNSName>\CertEnroll\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
    2.  Sélectionnez l’entrée `http://<ServerDNSName>/CertEnroll/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
    3.  Sélectionnez l’entrée qui commence par le chemin d’accès `ldap:///CN=<CATruncatedName><CRLNameSuffix>,CN=<ServerShortName>`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
4.  Dans **spécifier les emplacements à partir desquels les utilisateurs peuvent obtenir une liste de révocation de certificats**, cliquez sur **Ajouter**. La boîte de dialogue **Ajouter un emplacement** s’ouvre.  
  
5.  Dans **Ajouter un emplacement**, dans **emplacement**, tapez `http://pki.corp.contoso.com/pki/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, puis cliquez sur **OK**. Cela vous renvoie à la boîte de dialogue Propriétés de l’autorité de certification.  
  
6.  Sous l’onglet **Extensions** , activez les cases à cocher suivantes :  
  
    -   **Inclure dans les listes CRL. les clients l’utilisent pour trouver les emplacements des listes de révocation de certificats delta**  
  
    -   **Inclure dans l’extension CDP des certificats émis**  
  
7.  Dans **spécifier les emplacements à partir desquels les utilisateurs peuvent obtenir une liste de révocation de certificats**, cliquez sur **Ajouter**. La boîte de dialogue **Ajouter un emplacement** s’ouvre.  
  
8.  Dans **Ajouter un emplacement**, dans **emplacement**, tapez `file://\\pki.corp.contoso.com\pki\<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, puis cliquez sur **OK**. Cela vous renvoie à la boîte de dialogue Propriétés de l’autorité de certification.  
  
9. Sous l’onglet **Extensions** , activez les cases à cocher suivantes :  
  
    -   **Publier les listes de révocation de certificats à cet emplacement**  
  
    -   **Publier des listes de révocation de certificats delta à cet emplacement**  
  
10. Modifiez l' **option Sélectionner l’extension** en accès aux informations de l' **autorité (AIA)** , puis dans **spécifier les emplacements à partir desquels les utilisateurs peuvent obtenir une liste de révocation de certificats**, procédez comme suit :  
  
    1.  Sélectionnez l’entrée qui commence par le chemin d’accès `ldap:///CN=<CATruncatedName>,CN=AIA,CN=Public Key Services`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
    2.  Sélectionnez l’entrée `http://<ServerDNSName>/CertEnroll/<ServerDNSName>_<CaName><CertificateName>.crt`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
    3.  Sélectionnez l’entrée `file://\\<ServerDNSName>\CertEnroll\<ServerDNSName><CaName><CertificateName>.crt`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
11. Dans **Spécifiez les emplacements à partir desquels les utilisateurs peuvent obtenir le certificat pour cette autorité de certification**, cliquez sur **Ajouter**. La boîte de dialogue **Ajouter un emplacement** s’ouvre.  
  
12. Dans **Ajouter un emplacement**, dans **emplacement**, tapez `http://pki.corp.contoso.com/pki/<ServerDNSName>_<CaName><CertificateName>.crt`, puis cliquez sur **OK**. Cela vous renvoie à la boîte de dialogue Propriétés de l’autorité de certification.  
  
13. Sous l’onglet **Extensions** , sélectionnez **inclure dans l’AIA des certificats émis**.  
  
14. Lorsque vous êtes invité à redémarrer Active Directory Services de certificats, cliquez sur **non**. Vous redémarrerez le service ultérieurement.  
  

