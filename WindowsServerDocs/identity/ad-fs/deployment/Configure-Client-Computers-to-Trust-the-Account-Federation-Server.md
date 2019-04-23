---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: Configurer des ordinateurs clients pour approuver le serveur de fédération de compte
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: bdfb086c8177e72c074ac5b5b1a38aac49c4082c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886750"
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>Configurer des ordinateurs clients pour approuver le serveur de fédération de compte

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Pour que les ordinateurs clients puissent accéder correctement des applications fédérées à l’aide d’Active Directory Federation Services \(AD FS\), vous devez d’abord configurer les paramètres Internet Explorer sur chaque ordinateur client afin que le navigateur fait confiance le serveur de fédération de compte. Procéder manuellement ou via la stratégie de groupe, en fonction de vos préférences d’administration, en effectuant l’une des procédures suivantes.  
  
## <a name="configuring-internet-explorer-settings-manually"></a>Configuration des paramètres d’Internet Explorer manuellement  
Vous pouvez utiliser la procédure suivante pour configurer manuellement les paramètres d’Internet Explorer de chaque utilisateur pour prendre en charge la fédération via AD FS. Si plusieurs utilisateurs utilisent un seul ordinateur, effectuez cette procédure plusieurs fois : une fois pour chaque profil utilisateur.  
  
Pour effectuer cette procédure, connectez-vous en tant que l’utilisateur qui accéderont aux applications fédérées. Il s’agit d’un profil\-paramètre spécifique. Par conséquent, requiert que vous ajoutez manuellement ce paramètre pour chaque profil existe sur un ordinateur spécifique.  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>Pour configurer manuellement les ordinateurs clients pour approuver le serveur de fédération de compte  
  
1.  Sur l’ordinateur client, démarrez Internet Explorer.  
  
2.  Sur le **outils** menu, cliquez sur **Options Internet**.  
  
3.  Sur le **sécurité** , cliquez sur le **intranet Local** icône, puis cliquez sur **Sites**.  
  
4.  Cliquez sur **avancé**et dans **ajouter ce site Web à la zone**, tapez le système de nom de domaine complet \(DNS\) nom du serveur de fédération de compte \(, par exemple, https :\/\/fs1.fabrikam.com\), puis cliquez sur **ajouter**.  
  
5.  Cliquez sur **OK** à trois reprises.  
  
## <a name="configuring-internet-explorer-settings-by-using-grouppolicy"></a>Configuration des paramètres d’Internet Explorer à l’aide de stratégie de groupe  
Pour la plupart des déploiements, nous vous recommandons d’utiliser la stratégie de groupe pour pousser les paramètres d’Internet Explorer appropriés à chaque ordinateur client.  
  
L’appartenance au **Admins du domaine** ou **administrateurs de l’entreprise**, ou équivalent, dans les Services de domaine Active Directory \(AD DS\) est la condition minimale requise pour effectuer cette procédure.  Examinez les informations relatives à l’aide de comptes appropriés et les appartenances au groupe [locaux et les groupes de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/go.microsoft.com\/fwlink\/? LinkId\=83477\).   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-grouppolicy"></a>Pour configurer les ordinateurs clients pour approuver le serveur de fédération de compte à l’aide de stratégie de groupe  
  
1.  Sur un contrôleur de domaine dans la forêt de l’organisation partenaire de compte, démarrez le **Group Policy Management** aligner\-dans.  
  
2.  Rechercher l’objet de stratégie de groupe approprié \(GPO\), à droite\-cliquez dessus, puis cliquez sur **modifier**.  
  
3.  Dans l’arborescence de la console, ouvrez **Configuration utilisateur\\préférences\\Windows paramètres\\Maintenance Internet Explorer**, puis cliquez sur **sécurité**.  
  
4.  Dans le volet de détails, double-cliquez\-cliquez sur **Zones de sécurité et des classifications de contenu**.  
  
5.  Sous **Zones de sécurité et confidentialité**, cliquez sur **importer les actuels de zones de sécurité et les paramètres de confidentialité**, puis cliquez sur **modifier les paramètres**.  
  
6.  Cliquez sur **intranet Local**, puis cliquez sur **Sites**.  
  
7.  Dans **ajouter ce site Web à la zone**, tapez le nom DNS complet du serveur de fédération de compte \(, par exemple, https :\/\/fs1.fabrikam.com\), cliquez sur **ajouter**, puis cliquez sur **fermer**.  
  
8.  Cliquez sur **OK** deux fois pour appliquer ces modifications à la stratégie de groupe.  
  
