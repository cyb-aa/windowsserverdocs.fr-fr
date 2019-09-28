---
title: ÉTAPE 11 configurer le déploiement multisite
description: 'Cette rubrique fait partie du Guide de laboratoire de test : illustrer un déploiement multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cbdeb1d-5f7c-4360-bcc1-ab40d3cd8040
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3a80697665eeb67c2dda0d4d25201c7d02ed0c7e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404804"
---
# <a name="step-11-configure-the-multisite-deployment"></a>ÉTAPE 11 configurer le déploiement multisite

>S'applique à : Windows Server (Canal semi-annuel), Windows Server 2016

Pour configurer un déploiement multisite, apportez des modifications à l’Assistant Configuration de l’accès à distance en cours sur EDGE1, activez la fonctionnalité multisite, puis ajoutez 2-EDGE1 en tant que deuxième point d’entrée.  
  
- Configurer l’accès à distance sur EDGE1  
  
- Activer la configuration multisite sur EDGE1  
  
- Ajouter 2-EDGE1 en tant que deuxième point d’entrée  
  
## <a name="configDA"></a>Configurer l’accès à distance sur EDGE1  
  
1.  Dans l’écran d' **Accueil** , tapez**RAMgmtUI. exe**, puis appuyez sur entrée. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la Console de gestion de l'accès à distance, cliquez sur **Configuration**.  
  
3.  Dans le volet central de la console, dans la zone **étape 2 : serveur d’accès à distance** , cliquez sur **modifier**.  
  
4.  Cliquez sur **configuration du préfixe**. Dans la page **configuration du préfixe** , dans **préfixes IPv6 de réseau interne**, entrez **2001 : DB8:1 ::/64 ; 2001 : DB8:2 ::/64**. Dans le **préfixe IPv6 attribué aux ordinateurs clients DirectAccess**, entrez **2001 : DB8:1 : 1000 ::/64**, cliquez sur **suivant**, puis sur **Terminer**.  
  
5.  Dans le volet central de la console, dans la zone **étape 3 : serveurs d’infrastructure** , cliquez sur **modifier**.  
  
6.  Cliquez sur **liste de recherche de suffixes DNS**. Dans la page **liste de recherche de suffixes DNS** , assurez-vous que la case à cocher **configurer les clients DirectAccess avec la liste de recherche de suffixes client DNS** est activée et que les suffixes de domaine **Corp.contoso.com** et **corp2.Corp.contoso.com** Affichez la liste des **suffixes de domaine à utiliser** , cliquez sur **suivant**, puis sur Terminer.  
  
7.  Dans le volet central de la console, cliquez sur **Terminer**.  
  
8.  Dans la boîte de dialogue vérification de l' **accès à distance** , vérifiez les paramètres de configuration, puis cliquez sur **appliquer**. Dans la boîte de dialogue **Application des paramètres de l’Assistant Configuration de l’accès à distance**, cliquez sur **Fermer**.  
  
9. Dans le volet **tâches** , cliquez sur **Actualiser les serveurs d’administration**, puis cliquez sur **Fermer** lorsque vous avez terminé.  
  
## <a name="EnabledMultisite"></a>Activer la configuration multisite sur EDGE1  
  
1.  Dans la console Gestion de l’accès à distance, dans le volet **tâches** , cliquez sur **activer multisite**.  
  
2.  Dans l’Assistant activer le déploiement multisite, dans la page **avant de commencer** , cliquez sur **suivant**.  
  
3.  Dans la page **nom du déploiement** , dans nom du **déploiement multisite**, tapez **contoso**, dans **premier nom du point d’entrée**, tapez **Edge1-site**, puis cliquez sur **suivant**.  
  
4.  Sur la page **sélection du point d’entrée** , cliquez sur affecter des points d' **entrée automatiquement, puis autorisez les clients à sélectionner manuellement**, puis cliquez sur **suivant**.  
  
5.  Dans la page **équilibrage de charge global** , cliquez sur **non, n’utilisez pas l’équilibrage de charge global**, puis cliquez sur **suivant**.  
  
6.  Sur la page **prise en charge du client** , cliquez sur **autoriser les ordinateurs clients exécutant Windows 7 à accéder à ce point d’entrée**, puis cliquez sur **Ajouter**.  
  
7.  Dans la boîte de dialogue **Sélectionner des groupes** , dans **Entrez les noms des objets à sélectionner**, tapez **Win7_Clients_Site1**, cliquez sur **OK**, puis sur **suivant**.  
  
8.  Sur la page Paramètres de l' **objet de stratégie de groupe du client** , cliquez sur **suivant**.  
  
9. Sur la page **Résumé** , cliquez sur **valider**.  
  
10. Dans la boîte de dialogue **activation du déploiement multisite** , cliquez sur **Fermer** , puis dans l’Assistant activer le déploiement multisite, cliquez sur **Fermer**.  
  
## <a name="AddEP"></a>Ajouter 2-EDGE1 en tant que deuxième point d’entrée  
  
1.  Dans la console Gestion de l’accès à distance, dans le volet **tâches** , cliquez sur **Ajouter un point d’entrée**.  
  
2.  Dans l’Assistant Ajouter un point d’entrée, sur la page **Détails du point d’entrée** , dans serveur d' **accès à distance**, tapez **2-Edge1.corp2.Corp.contoso.com**, dans nom du **point d’entrée**, tapez **2-Edge1-site**, puis cliquez sur **suivant**.  
  
3.  Dans la page **topologie du réseau** , cliquez sur **Edge**, puis sur **suivant**.  
  
4.  Dans la page **nom du réseau ou adresse IP** , dans **tapez le nom public ou l’adresse IP utilisée par les clients pour se connecter au serveur d’accès à distance**, tapez **2-Edge1.contoso.com**, puis cliquez sur **suivant**.  
  
5.  Dans la page **cartes réseau** , assurez-vous que la **carte externe** est **Internet**, que la **carte interne** est **2-corpnet**, que le certificat est **CN = 2-Edge1.contoso.com**, puis cliquez sur **suivant**.  
  
6.  Dans la **page Configuration du préfixe** , dans **préfixe IPv6 attribué aux ordinateurs clients DirectAccess**, tapez **2001 : DB8:2 : 2000 ::/64**, puis cliquez sur **suivant**.  
  
7.  Sur la page **prise en charge du client** , cliquez sur **autoriser les ordinateurs clients exécutant Windows 7 à accéder à ce point d’entrée**, puis cliquez sur **Ajouter**.  
  
8.  Dans la boîte de dialogue **Sélectionner des groupes** , dans **Entrez les noms des objets à sélectionner**, tapez **Win7_Clients_Site2**, cliquez sur **OK**, puis sur **suivant**.  
  
9. Sur la page Paramètres de l' **objet de stratégie de groupe du client** , cliquez sur **suivant**.  
  
10. Sur la page Paramètres de l' **objet de stratégie de groupe du serveur** , cliquez sur **suivant**.  
  
11. Sur la page **Résumé** , cliquez sur **valider**.  
  
12. Dans la boîte de dialogue Ajout d’un **point d’entrée** , cliquez sur **Fermer** , puis dans l’Assistant Ajouter un point d’entrée, cliquez sur **Fermer**.  
  


