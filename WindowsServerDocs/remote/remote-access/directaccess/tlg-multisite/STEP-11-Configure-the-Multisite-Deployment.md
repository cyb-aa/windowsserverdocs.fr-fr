---
title: ÉTAPE 11 configurer le déploiement Multisite
description: 'Cette rubrique fait partie du Guide de laboratoire de Test : illustrer un déploiement Multisite DirectAccess pour Windows Server 2016'
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cbdeb1d-5f7c-4360-bcc1-ab40d3cd8040
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2dc70d9c89b19a3e38d7f6cd682f047530f87cc6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839520"
---
# <a name="step-11-configure-the-multisite-deployment"></a>ÉTAPE 11 configurer le déploiement Multisite

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Pour configurer un déploiement multisite, apporter des modifications à l’Assistant de configuration de l’accès à distance en cours sur EDGE1, activer la fonctionnalité multisite, puis ajouter 2-EDGE1 qu’un deuxième point d’entrée.  
  
- Configurer l’accès à distance sur EDGE1  
  
- Activer la configuration multisite sur EDGE1  
  
- Ajouter 2-EDGE1 qu’un deuxième point d’entrée  
  
## <a name="configDA"></a>Configurer l’accès à distance sur EDGE1  
  
1.  Sur le **Démarrer** , tapez**RAMgmtUI.exe**, puis appuyez sur ENTRÉE. Si la boîte de dialogue **Contrôle de compte d'utilisateur** s'affiche, vérifiez que l'action affichée est celle que vous voulez, puis cliquez sur **Oui**.  
  
2.  Dans la Console de gestion de l'accès à distance, cliquez sur **Configuration**.  
  
3.  Dans le volet central de la console, dans le **étape 2 : serveur accès à distance** zone, cliquez sur **modifier**.  
  
4.  Cliquez sur **Configuration du préfixe**. Sur le **Configuration du préfixe** page **les préfixes IPv6 de réseau interne**, entrez **2001:db8:1 :: / 64 ; 2001:db8:2 :: / 64**. Dans **préfixe IPv6 attribué aux ordinateurs clients DirectAccess**, entrez **2001:db8:1:1000 :: / 64**, cliquez sur **suivant**, puis cliquez sur **Terminer** .  
  
5.  Dans le volet central de la console, dans le **étape 3 : serveurs d’Infrastructure** zone, cliquez sur **modifier**.  
  
6.  Cliquez sur **liste de recherche de suffixes DNS**. Sur le **liste de recherche de suffixes DNS** page, assurez-vous que le **clients de configurer DirectAccess avec la liste de recherche de suffixes DNS client** case à cocher est activée et que le **corp.contoso.com** et **corp2.corp.contoso.com** suffixes de domaine apparaissent dans le **suffixes de domaine à utiliser** , cliquez sur **suivant**, puis cliquez sur Terminer.  
  
7.  Dans le volet central de la console, cliquez sur **Terminer**.  
  
8.  Sur le **révision d’accès à distance** boîte de dialogue, passez en revue les paramètres de configuration, puis cliquez sur **appliquer**. Dans la boîte de dialogue **Application des paramètres de l’Assistant Configuration de l’accès à distance**, cliquez sur **Fermer**.  
  
9. Dans le **tâches** volet, cliquez sur **actualiser les serveurs d’administration**, puis cliquez sur **fermer** issue.  
  
## <a name="EnabledMultisite"></a>Activer la configuration multisite sur EDGE1  
  
1.  Dans la Console de gestion des accès à distance, dans le **tâches** volet, cliquez sur **Multisite activer**.  
  
2.  Dans l’Assistant Activer le déploiement Multisite, sur le **avant de commencer** , cliquez sur **suivant**.  
  
3.  Sur le **nom du déploiement** page **nom du déploiement Multisite**, type **Contoso**, dans **premier point d’entrée nom**, type **Edge1-Site**, puis cliquez sur **suivant**.  
  
4.  Sur le **sélection de Point d’entrée** , cliquez sur **affecter automatiquement des points d’entrée, et permettent aux clients de sélectionner manuellement**, puis cliquez sur **suivant**.  
  
5.  Sur le **équilibrage de charge Global** , cliquez sur **non, n’utilisez pas l’équilibrage de charge global**, puis cliquez sur **suivant**.  
  
6.  Sur le **prise en charge Client** , cliquez sur **autoriser les ordinateurs qui exécutent 7 Windows pour accéder à ce point d’entrée client**, puis cliquez sur **ajouter**.  
  
7.  Sur le **sélectionner des groupes** boîte de dialogue **Entrez les noms des objets à sélectionner**, type **Win7_Clients_Site1**, cliquez sur **OK**, puis cliquez sur **Suivant**.  
  
8.  Sur le **paramètres de stratégie de groupe Client** , cliquez sur **suivant**.  
  
9. Sur le **Résumé** , cliquez sur **valider**.  
  
10. Sur le **activer le déploiement Multisite** boîte de dialogue, cliquez sur **fermer** puis cliquez sur l’Assistant Activer le déploiement Multisite, **fermer**.  
  
## <a name="AddEP"></a>Ajouter 2-EDGE1 qu’un deuxième point d’entrée  
  
1.  Dans la Console de gestion des accès à distance, dans le **tâches** volet, cliquez sur **ajouter un Point d’entrée**.  
  
2.  Dans la zone Ajouter un Point d’entrée Assistant, sur le **détails de Point d’entrée** page **serveur d’accès à distance**, type **2-edge1.corp2.corp.contoso.com**, dans **entrée nom du point de**, type **Edge1-2-Site**, puis cliquez sur **suivant**.  
  
3.  Sur le **topologie de réseau** , cliquez sur **Edge**, puis cliquez sur **suivant**.  
  
4.  Sur le **nom réseau ou l’adresse IP** page **Type dans le nom public ou une adresse IP utilisée par les clients pour se connecter au serveur d’accès à distance**, type **2-edge1.contoso.com**, et puis cliquez sur **suivant**.  
  
5.  Sur le **cartes réseau** page, assurez-vous que le **adaptateur externe** est **Internet**, le **carte réseau interne** est **2 Réseau d’entreprise -**, le certificat est **CN = 2-edge1.contoso.com**, puis cliquez sur **suivant**.  
  
6.  Sur le **Configuration du préfixe** page **préfixe IPv6 attribué aux ordinateurs clients DirectAccess**, type **2001:db8:2:2000 :: / 64**, puis cliquez sur **suivant** .  
  
7.  Sur le **prise en charge Client** , cliquez sur **autoriser les ordinateurs qui exécutent 7 Windows pour accéder à ce point d’entrée client**, puis cliquez sur **ajouter**.  
  
8.  Sur le **sélectionner des groupes** boîte de dialogue **Entrez les noms des objets à sélectionner**, type **Win7_Clients_Site2**, cliquez sur **OK**, puis cliquez sur **Suivant**.  
  
9. Sur le **paramètres de stratégie de groupe Client** , cliquez sur **suivant**.  
  
10. Sur le **paramètres de stratégie de groupe serveur** , cliquez sur **suivant**.  
  
11. Sur le **Résumé** page, cliquez sur **valider**.  
  
12. Sur le **Point d’entrée Ajout** boîte de dialogue, cliquez sur **fermer** puis cliquez sur l’Assistant Ajout d’un Point d’entrée, **fermer**.  
  


