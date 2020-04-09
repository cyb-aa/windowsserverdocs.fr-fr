---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: Configurer les ordinateurs clients pour qu’ils approuvent le serveur de Fédération de comptes
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5022be852ab72f57c90d957cb1111f10e9b03a90
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854942"
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>Configurer les ordinateurs clients pour qu’ils approuvent le serveur de Fédération de comptes

Pour que les ordinateurs clients puissent accéder correctement aux applications fédérées à l’aide de Services ADFS \(AD FS\), vous devez d’abord configurer les paramètres d’Internet Explorer sur chaque ordinateur client afin que le navigateur approuve le serveur de Fédération de compte. Vous pouvez effectuer cette opération manuellement ou par le biais de stratégie de groupe, en fonction de vos préférences administratives, en effectuant l’une des procédures suivantes.  
  
## <a name="configuring-internet-explorer-settings-manually"></a>Configuration manuelle des paramètres d’Internet Explorer  
Vous pouvez utiliser la procédure suivante pour configurer manuellement les paramètres Internet Explorer de chaque utilisateur afin de prendre en charge la Fédération par le biais de AD FS. Si plusieurs utilisateurs utilisent un seul ordinateur, effectuez cette procédure plusieurs fois, une fois pour chaque profil utilisateur.  
  
Pour effectuer cette procédure, connectez-vous en tant qu’utilisateur qui accédera aux applications fédérées. Il s’agit d’un profil\-paramètre spécifique. Par conséquent, vous devez ajouter manuellement ce paramètre pour chaque profil qui existe sur un ordinateur spécifique.  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>Pour configurer manuellement les ordinateurs clients afin qu’ils approuvent le serveur de Fédération de comptes  
  
1.  Sur l’ordinateur client, démarrez Internet Explorer.  
  
2.  Dans le menu **Outils** , cliquez sur **Options Internet**.  
  
3.  Sous l’onglet **sécurité** , cliquez sur l’icône **Intranet local** , puis sur **sites**.  
  
4.  Cliquez sur **avancé**, puis dans **ajouter ce site Web à la zone**, tapez le nom de domaine complet DNS (domain Name System) \(DNS\) nom du serveur de Fédération de comptes \(par exemple, https :\/\/FS1.fabrikam.com\), puis cliquez sur **Ajouter**.  
  
5.  Cliquez sur **OK** à trois reprises.  
  
## <a name="configuring-internet-explorer-settings-by-using-grouppolicy"></a>Configuration des paramètres d’Internet Explorer à l’aide de stratégie de groupe  
Pour la plupart des déploiements, nous vous recommandons d’utiliser stratégie de groupe pour transmettre les paramètres Internet Explorer appropriés à chaque ordinateur client.  
  
Pour effectuer cette procédure, il est nécessaire d’appartenir au minimum au groupe **Admins du domaine** ou administrateurs de l' **entreprise**, ou à un groupe équivalent, dans Active Directory Domain Services \(AD DS\).  Passez en revue les détails sur l’utilisation des comptes et des appartenances aux groupes appropriés dans les [groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) \(http :\/\/Go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-grouppolicy"></a>Pour configurer les ordinateurs clients de sorte qu’ils approuvent le serveur de Fédération de compte à l’aide de stratégie de groupe  
  
1.  Sur un contrôleur de domaine dans la forêt de l’organisation partenaire de compte, démarrez le\-du composant logiciel enfichable de **gestion de stratégie de groupe** dans.  
  
2.  Recherchez l’objet stratégie de groupe approprié \(objet de stratégie de groupe\), à droite\-cliquez dessus, puis cliquez sur **modifier**.  
  
3.  Dans l’arborescence de la console, ouvrez **Configuration utilisateur\\préférences\\paramètres Windows\\maintenance Internet Explorer**, puis cliquez sur **sécurité**.  
  
4.  Dans le volet d’informations, double\-cliquez sur **zones de sécurité et**contrôle d’accès au contenu.  
  
5.  Sous **zones de sécurité et confidentialité**, cliquez sur **Importer les paramètres actuels des zones de sécurité et de confidentialité**, puis cliquez sur **modifier les paramètres**.  
  
6.  Cliquez sur **Intranet local**, puis sur **sites**.  
  
7.  Dans **ajouter ce site Web à la zone**, tapez le nom DNS complet du serveur de Fédération de comptes \(par exemple, https :\/\/FS1.fabrikam.com\), cliquez sur **Ajouter**, puis sur **Fermer**.  
  
8.  Cliquez deux fois sur **OK** pour appliquer ces modifications à stratégie de groupe.  
  
