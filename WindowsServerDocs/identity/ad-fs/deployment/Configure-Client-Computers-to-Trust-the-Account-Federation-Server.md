---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: "Configurer les ordinateurs clients pour approuver le serveur de fédération de comptes"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfb086c8177e72c074ac5b5b1a38aac49c4082c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>Configurer les ordinateurs clients pour approuver le serveur de fédération de comptes

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Afin que les ordinateurs clients puissent accéder avec succès les applications fédérées à l’aide d’Active Directory Federation Services \(AD FS\), vous devez d’abord configurer les paramètres d’Internet Explorer sur chaque ordinateur client afin que le navigateur approuve le serveur de fédération de comptes. Vous pouvez cela manuellement ou via la stratégie de groupe, selon votre préférence d’administration, en effectuant l’une des procédures suivantes.  
  
## <a name="configuring-internet-explorer-settings-manually"></a>La configuration des paramètres d’Internet Explorer manuellement  
Vous pouvez utiliser la procédure suivante pour configurer manuellement les paramètres d’Internet Explorer de chaque utilisateur pour prendre en charge la fédération AD FS. Si plusieurs utilisateurs utilisent un seul ordinateur, effectuez cette procédure plusieurs fois: une fois pour chaque profil utilisateur.  
  
Pour effectuer cette procédure, ouvrez une session en tant que l’utilisateur qui accèdent aux applications fédérées. Il s’agit d’un paramètre profile\ spécifique. Par conséquent, elle nécessite que vous ajoutez manuellement ce paramètre pour chaque profil qui existe sur un ordinateur spécifique.  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>Pour configurer manuellement les ordinateurs clients pour approuver le serveur de fédération de comptes  
  
1.  Sur l’ordinateur client, démarrez Internet Explorer.  
  
2.  Sur le **outils** menu, cliquez sur **Options Internet**.  
  
3.  Sur le **sécurité** , cliquez sur le **intranet Local** icône, puis cliquez sur **Sites**.  
  
4.  Cliquez sur **avancé**et dans **ajouter ce site Web à la zone**, tapez le nom \(DNS\) système de nom de domaine complet du serveur de fédération compte \ (par exemple, https:\/\/fs1.fabrikam.com\), puis cliquez sur **ajouter**.  
  
5.  Cliquez sur **OK** trois fois.  
  
## <a name="configuring-internet-explorer-settings-by-using-group-policy"></a>Configuration des paramètres d’Internet Explorer à l’aide de stratégie de groupe  
La plupart des déploiements, nous vous recommandons d’utiliser Stratégie de groupe pour transmettre les paramètres d’Internet Explorer appropriés à chaque ordinateur client.  
  
L’appartenance au groupe **Admins du domaine** ou **administrateurs de l’entreprise**, ou équivalent, dans les Services de domaine Active Directory \(AD DS\) est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide des comptes appropriés et les appartenances au groupe [locaux et groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \ (http:///\/ go.microsoft.com\/fwlink\ /? LinkId\ = 83477\).   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-group-policy"></a>Pour configurer les ordinateurs clients pour approuver le serveur de fédération de comptes à l’aide de stratégie de groupe  
  
1.  Sur un contrôleur de domaine dans la forêt de l’organisation partenaire de compte, démarrez le **gestion des stratégies de groupe** logiciel enfichable.  
  
2.  Trouver le \(GPO\) objet de stratégie de groupe approprié, clic droit et puis cliquez sur **modifier**.  
  
3.  Dans l’arborescence de la console, ouvrez **utilisateur configuration ordinateur\\préférences\\paramètres Settings\\Internet Explorer Maintenance**, puis cliquez sur **sécurité**.  
  
4.  Dans le volet détails, double-cliquant sur **Zones de sécurité et contrôle d’accès au contenu**.  
  
5.  Sous **Zones de sécurité et confidentialité**, cliquez sur **importer les actuels des zones de sécurité et les paramètres de confidentialité**, puis cliquez sur **modifier les paramètres**.  
  
6.  Cliquez sur **intranet Local**, puis cliquez sur **Sites**.  
  
7.  Dans **ajouter ce site Web à la zone**, tapez le nom DNS complet du serveur de fédération compte \ (par exemple, https:\/\/fs1.fabrikam.com\), cliquez sur **ajouter**, puis cliquez sur **fermer**.  
  
8.  Cliquez sur **OK** deux fois pour appliquer ces modifications à la stratégie de groupe.  
  
