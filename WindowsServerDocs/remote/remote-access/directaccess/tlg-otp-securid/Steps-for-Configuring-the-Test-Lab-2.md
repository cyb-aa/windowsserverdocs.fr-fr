---
title: Étapes de configuration du laboratoire de test
description: Cette rubrique fait partie du Guide de laboratoire de Test - démontrer DirectAccess avec l’authentification OTP et RSA SecurID pour Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a40183c-afd1-43ca-b306-05745640a37d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0607506f2b6dd49284e6b377fb87da4f731eb94d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67283083"
---
# <a name="steps-for-configuring-the-test-lab"></a>Étapes de configuration du laboratoire de test

>S'applique à : Windows Server (canal semi-annuel), Windows Server 2016

Les étapes suivantes décrivent comment configurer l’infrastructure d’accès à distance, de configurer le serveur d’accès à distance et le client et de tester la connectivité DirectAccess depuis les sous-réseaux du réseau domestique et Internet.  
  
Dans ce guide de laboratoire de test, vous allez générer un accès à distance avec l’environnement d’OTP en effectuant les étapes suivantes :  
  
-   [ÉTAPE 1 : Terminer la Configuration DirectAccess](assetId:///4dbf877f-02fb-439b-907a-f5b3f1d8afa6). Toutes les étapes dans le [Guide de laboratoire de Test : Montrez l’installation du serveur DirectAccess unique avec un environnement mixte IPv4 et IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [ÉTAPE 2 : Configurer APP1](assetId:///c1bb590f-91d4-4ed5-bceb-b0e36eabd4ff). Configurer APP1 avec des modèles de certificat OTP pour une utilisation par EDGE1.  
  
-   [ÉTAPE 3 : Configurer DC1](assetId:///904a6edc-a771-45ed-9630-a34a680bb522). Vérifiez le que nom d’utilisateur Principal est défini sur DC1.  
  
-   [ÉTAPE 7 : Installer et configurer RSA](assetId:///baa4c28c-add7-42e2-8afd-ccc7a559406a). Installer et configurer le serveur RSA, un rayon et un secret à usage unique et configurer des EDGE1 pour le secret à usage unique.  
  
-   [ÉTAPE 11 : Vérifier l’intégrité du secret à usage unique sur EDGE1](assetId:///3b397a4a-8478-47f2-a932-9e8e048c14ba). Assurez-vous que l’état de l’OTP est intègre sur le serveur d’accès à distance.  
  
-   [ÉTAPE 8 : Tester la connectivité DirectAccess à partir du sous-réseau de réseau domestique](assetId:///ba1652a6-0692-4add-91ca-34a84956ba14). Tester la fonctionnalité DirectAccess OTP derrière un périphérique NAT.  
  
-   [ÉTAPE 10 : Tester la connectivité DirectAccess à partir d’Internet](assetId:///321149eb-5f23-4a0b-b8fb-1244540126e9). Tester la connectivité des clients DirectAccess à partir d’Internet.  
  
-   [ÉTAPE 12 : La configuration de capture instantanée](assetId:///8a51ed3c-9c32-402f-85d1-617ce46845b4). À l’issue de ce laboratoire de test, prenez un instantané de l’utilisation de DirectAccess avec la configuration de secret à usage unique afin que vous pouvez y revenir plus tard pour tester des scénarios supplémentaires.  
  


