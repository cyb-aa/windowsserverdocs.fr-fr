---
ms.assetid: 4ae26970-e42e-4e69-887a-b16d2f8d0695
title: Configurer les ordinateurs clients pour qu’ils approuvent le serveur de Fédération de comptes
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 8c047f69cb0cb2db57ea33697c49e53a212fe823
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359863"
---
# <a name="configure-client-computers-to-trust-the-account-federation-server"></a>Configurer les ordinateurs clients pour qu’ils approuvent le serveur de Fédération de comptes

Pour que les ordinateurs clients puissent accéder correctement aux applications fédérées à l’aide de Services ADFS \(AD FS @ no__t-1, vous devez d’abord configurer les paramètres d’Internet Explorer sur chaque ordinateur client afin que le navigateur approuve le compte. serveur de Fédération. Vous pouvez effectuer cette opération manuellement ou par le biais de stratégie de groupe, en fonction de vos préférences administratives, en effectuant l’une des procédures suivantes.  
  
## <a name="configuring-internet-explorer-settings-manually"></a>Configuration manuelle des paramètres d’Internet Explorer  
Vous pouvez utiliser la procédure suivante pour configurer manuellement les paramètres Internet Explorer de chaque utilisateur afin de prendre en charge la Fédération par le biais de AD FS. Si plusieurs utilisateurs utilisent un seul ordinateur, effectuez cette procédure plusieurs fois, une fois pour chaque profil utilisateur.  
  
Pour effectuer cette procédure, connectez-vous en tant qu’utilisateur qui accédera aux applications fédérées. Il s’agit d’un paramètre profile @ no__t-0specific. Par conséquent, vous devez ajouter manuellement ce paramètre pour chaque profil qui existe sur un ordinateur spécifique.  
  
#### <a name="to-manually-configure-client-computers-to-trust-the-account-federation-server"></a>Pour configurer manuellement les ordinateurs clients afin qu’ils approuvent le serveur de Fédération de comptes  
  
1.  Sur l’ordinateur client, démarrez Internet Explorer.  
  
2.  Dans le menu **Outils** , cliquez sur **Options Internet**.  
  
3.  Sous l’onglet **sécurité** , cliquez sur l’icône **Intranet local** , puis sur **sites**.  
  
4.  Cliquez sur **avancé**, puis dans **ajouter ce site Web à la zone**, tapez le nom de domaine complet System \(DNS @ no__t-3 Name of the Account Federation Server \(for exemple, https : @no__t -5\/fs1.fabrikam.com @ no__t-7, puis cliquez sur **Ajouter.** .  
  
5.  Cliquez sur **OK** à trois reprises.  
  
## <a name="configuring-internet-explorer-settings-by-using-grouppolicy"></a>Configuration des paramètres d’Internet Explorer à l’aide de stratégie de groupe  
Pour la plupart des déploiements, nous vous recommandons d’utiliser stratégie de groupe pour transmettre les paramètres Internet Explorer appropriés à chaque ordinateur client.  
  
Pour effectuer cette procédure, vous devez au minimum appartenir au groupe **Admins du domaine** ou administrateurs de l' **entreprise**ou à un groupe équivalent dans Active Directory Domain Services \(AD DS @ no__t-3.  Passez en revue les détails sur l’utilisation des comptes et des appartenances aux groupes appropriés dans les\/ \( [groupes locaux et de domaine par défaut](https://go.microsoft.com/fwlink/?LinkId=83477) http :\/Go.Microsoft.com\/fwlink\/? LinkId\=83477\).   
  
#### <a name="to-configure-client-computers-to-trust-the-account-federation-server-by-using-grouppolicy"></a>Pour configurer les ordinateurs clients de sorte qu’ils approuvent le serveur de Fédération de compte à l’aide de stratégie de groupe  
  
1.  Sur un contrôleur de domaine dans la forêt de l’organisation partenaire de compte, démarrez le composant logiciel enfichable de **gestion de stratégie de groupe** @ no__t-1Dans.  
  
2.  Recherchez l’objet stratégie de groupe approprié \(GPO @ no__t-1, Right @ no__t-2click, puis cliquez sur **modifier**.  
  
3.  Dans l’arborescence de la console, ouvrez **Configuration utilisateur @ no__t-1Preferences @ no__t-2Windows paramètres @ no__t-3Internet Explorer maintenance**, puis cliquez sur **sécurité**.  
  
4.  Dans le volet d’informations, double @ no__t-0click **zones de sécurité et**contrôle d’accès au contenu.  
  
5.  Sous **zones de sécurité et confidentialité**, cliquez sur **Importer les paramètres actuels des zones de sécurité et de confidentialité**, puis cliquez sur **modifier les paramètres**.  
  
6.  Cliquez sur **Intranet local**, puis sur **sites**.  
  
7.  Dans **ajouter ce site Web à la zone**, tapez le nom DNS complet du serveur de Fédération de comptes \(Pour exemple, https : @no__t -2\/fs1.fabrikam.com @ no__t-4, cliquez sur **Ajouter**, puis sur **Fermer**.  
  
8.  Cliquez deux fois sur **OK** pour appliquer ces modifications à stratégie de groupe.  
  
