---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: "Meilleures pratiques pour la sécurisation d’ActiveDirectory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: ecef0c173677d379524189b1769d4721ab0774a8
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 07/03/2017
---
# <a name="best-practices-for-securing-active-directory"></a>Meilleures pratiques pour la sécurisation d’ActiveDirectory

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

Ce document fournit d’un praticien et contient un ensemble de techniques pratiques pour aider les responsables informatiques à protéger un environnement ActiveDirectory d’entreprise. ActiveDirectory joue un rôle essentiel dans l’infrastructure informatique et garantit l’harmonie et la sécurité des différentes ressources réseau dans un environnement global interconnecté. Les méthodes présentées reposent largement sur l’expérience de l’organisation MicrosoftInformation Security and risque Management (ISRM), qui est responsable de la protection des ressources de MicrosoftIT et les autres Divisions d’entreprise Microsoft, en plus de conseiller un certain nombre de clients MicrosoftGlobal 500.  
  
-   [Préambule](https://technet.microsoft.com/library/dn487451.aspx)  
  
-   [Accusés de réception](https://technet.microsoft.com/library/dn487445.aspx)  
  
-   [Résumé](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [Introduction](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [Voies de compromis](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [Comptes intéressantes pour le vol d’informations d’identification](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [Ce qui réduit la Surface d’attaque ActiveDirectory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [Implémentation de modèles d’administration de privilèges](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [Implémentation des hôtes d’administration sécurisés](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [Sécurisation des contrôleurs de domaine contre les attaques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [Surveillance des signes de compromission d’ActiveDirectory](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [Recommandations concernant la stratégie d’audit](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [Planification des compromis](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [Maintenance d’un environnement plus sécurisé](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [Annexes](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [Annexe b: comptes privilégiés et groupes dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Annexe c: comptes protégés et groupes dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Annexe d: sécurisation des comptes d’administrateur intégrés dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [Annexe e: sécurisation des groupes d’administrateurs de l’entreprise dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [Annexe f: sécurisation des groupes d’administrateurs de domaine dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [Annexeg: Sécurisation des groupes d’administrateurs dans ActiveDirectory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [Annexeh: Sécurisation des comptes administrateur locaux et groupes](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [AnnexeI Création de gestion des comptes pour les comptes protégés et groupes dans ActiveDirectory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [Annexel: Événements à surveiller](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [Annexe m: liens vers des documents et lecture recommandée](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


