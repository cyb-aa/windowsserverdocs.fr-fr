---
title: Configurer le CDP et AIA Extensions sur CA1
description: Cette rubrique fait partie du guide de certificats de serveur de déploiement pour 802.1 X câblé et sans fil déploiements
manager: brianlic
ms.topic: article
ms.assetid: f77a3989-9f92-41ef-92a8-031651dd73a8
ms.prod: windows-server-threshold
ms.technology: networking
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 80f6d68816a58b2ac30010e0917ce0f816a30234
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-cdp-and-aia-extensions-on-ca1"></a>Configurer le CDP et AIA Extensions sur CA1

>S’applique à: Windows Server (canal annuel un point-virgule), Windows Server2016

Vous pouvez utiliser cette procédure pour configurer le Point de Distribution de liste de révocation de certificats (CRL) (CDP) et les paramètres d’accès des informations autorité (AIA) sur CA1.  
  
Pour effectuer cette procédure, vous devez être membre du groupe Admins du domaine.  
  
#### <a name="to-configure-the-cdp-and-aia-extensions-on-ca1"></a>Pour configurer les extensions CDP et AIA sur CA1  
  
1.  Dans le Gestionnaire de serveur, cliquez sur **outils** puis cliquez sur **autorité de Certification**.  
  
2.  Dans l’arborescence de la console Autorité de Certification, cliquez sur **corp-CA1-CA**, puis cliquez sur **propriétés**.  
  
    > [!NOTE]  
    > Le nom de votre autorité de certification est différent si vous n’avez pas nommé l’ordinateur CA1 et votre nom de domaine est différent de celui dans cet exemple. Le nom de l’autorité de certification est au format *domaine*-*NomOrdinateurAutoritéCertification*- autorité de certification.  
  
3.  Cliquez sur le **Extensions** onglet Assurez-vous que **sélectionner l’extension** est défini sur **points de Distribution CRL (CDP)**et dans le **spécifier les emplacements à partir de laquelle les utilisateurs peuvent obtenir une liste de révocation de certificats (CRL)**, procédez comme suit:  
  
    1.  Sélectionnez l’entrée `file:\/\/\\\\<ServerDNSName>\/CertEnroll\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
    2.  Sélectionnez l’entrée `http:\/\/<ServerDNSName>\/CertEnroll\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
    3.  Sélectionnez l’entrée qui commence par le chemin d’accès `ldap:\/\/CN\=<CATruncatedName><CRLNameSuffix>,CN\=<ServerShortName>`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
4.  Dans **spécifier les emplacements à partir de laquelle les utilisateurs peuvent obtenir une liste de révocation de certificats (CRL)**, cliquez sur **ajouter**. Le **ajouter un emplacement** boîte de dialogue s’ouvre.  
  
5.  Dans **ajouter un emplacement**, dans **emplacement**, type `http:\/\/pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, puis cliquez sur **OK**. Cela vous renvoie à la boîte de dialogue Autorité de certification.  
  
6.  Sur le **Extensions**, sélectionnez les cases à cocher suivantes:  
  
    -   **Inclure dans les listes de révocation. Les clients l’utilisent pour trouver les emplacements des listes CRL Delta**  
  
    -   **Inclure dans l’extension CDP des certificats émis**  
  
7.  Dans **spécifier les emplacements à partir de laquelle les utilisateurs peuvent obtenir une liste de révocation de certificats (CRL)**, cliquez sur **ajouter**. Le **ajouter un emplacement** boîte de dialogue s’ouvre.  
  
8.  Dans **ajouter un emplacement**, dans **emplacement**, type `file:\/\/\\\\pki.corp.contoso.com\/pki\/<CaName><CRLNameSuffix><DeltaCRLAllowed>.crl`, puis cliquez sur **OK**. Cela vous renvoie à la boîte de dialogue Autorité de certification.  
  
9. Sur le **Extensions**, sélectionnez les cases à cocher suivantes:  
  
    -   **Publier les listes de révocation à cet emplacement**  
  
    -   **Publier les listes CRL Delta à cet emplacement**  
  
10. Modification **sélectionner l’extension** à **les accès aux informations autorité (AIA)**et dans le **spécifier les emplacements à partir de laquelle les utilisateurs peuvent obtenir une liste de révocation de certificats (CRL)**, procédez comme suit:  
  
    1.  Sélectionnez l’entrée qui commence par le chemin d’accès `ldap:\/\/CN\=<CATruncatedName>,CN\=AIA,CN\=Public Key Services`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
    2.  Sélectionnez l’entrée `http:\/\/<ServerDNSName>\/CertEnroll\/<ServerDNSName>\_<CaName><CertificateName>.crt`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
    3.  Sélectionnez l’entrée `file:\/\/\\\\<ServerDNSName>\/CertEnroll\/<ServerDNSName>\_<CaName><CertificateName>.crt`, puis cliquez sur **supprimer**. Dans **confirmer la suppression**, cliquez sur **Oui**.  
  
11. Dans **spécifier les emplacements à partir de laquelle les utilisateurs peuvent obtenir une liste de révocation de certificats (CRL)**, cliquez sur **ajouter**. Le **ajouter un emplacement** boîte de dialogue s’ouvre.  
  
12. Dans **ajouter un emplacement**, dans **emplacement**, type `http:\/\/pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`, puis cliquez sur **OK**. Cela vous renvoie à la boîte de dialogue Autorité de certification.  
  
13. Sur le **Extensions** onglet, sélectionnez **inclure dans l’AIA des certificats émis**.  
  
14. Dans **ajouter un emplacement**, dans **emplacement**, type `file:\/\/\\\\pki.corp.contoso.com\/pki\/<ServerDNSName>\_<CaName><CertificateName>.crt`, puis cliquez sur **OK**. Cela vous renvoie à la boîte de dialogue Autorité de certification.  
  
    > [!IMPORTANT]  
    > Vérifiez que **inclure dans l’extension AIA des certificats émis** n’est pas activée.  
  
15. Lorsque vous êtes invité à redémarrer les Services de certificats ActiveDirectory, cliquez sur **non**. Vous allez redémarrer le service ultérieurement.  
  


