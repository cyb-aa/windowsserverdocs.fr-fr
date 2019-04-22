---
ms.assetid: e2651dc8-4b31-4cd8-a400-3b8123890210
title: Meilleures pratiques pour la sécurisation d'Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 972def668634e794908a3ff2933d038ae38be5d6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817080"
---
# <a name="best-practices-for-securing-active-directory"></a>Meilleures pratiques pour la sécurisation d'Active Directory

>S'applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Ce document fournit la perspective d’un praticien et contient un ensemble de techniques pratiques pour aider les responsables informatiques à protéger un environnement Active Directory d’entreprise. Active Directory joue un rôle essentiel dans l'infrastructure informatique et garantit l'harmonie et la sécurité des différentes ressources réseau dans un environnement global interconnecté. Les méthodes présentées reposent largement sur l’expérience de l’organisation Microsoft Information Security and Risk Management (ISRM), qui est responsable de la protection des ressources de Microsoft IT et les autres départements de Microsoft, en plus de conseiller un nombre de clients Microsoft Global 500 est sélectionné.  
  
-   [Résumé](../../../ad-ds/manage/component-updates/Executive-Summary.md)  
  
-   [Introduction](../../../ad-ds/manage/component-updates/Introduction.md)  
  
-   [Voies de compromis](../../../ad-ds/plan/security-best-practices/Avenues-to-Compromise.md)  
  
-   [Comptes attrayants pour le vol d’informations d’identification](../../../ad-ds/plan/security-best-practices/Attractive-Accounts-for-Credential-Theft.md)  
  
-   [En réduisant la Surface d’attaque Active Directory](../../../ad-ds/plan/security-best-practices/Reducing-the-Active-Directory-Attack-Surface.md)  
  
-   [Implémentation de modèles d’administration de moindre privilège](../../../ad-ds/plan/security-best-practices/Implementing-Least-Privilege-Administrative-Models.md)  
  
-   [Implémentation des hôtes d’administration sécurisés](../../../ad-ds/plan/security-best-practices/Implementing-Secure-Administrative-Hosts.md)  
  
-   [Sécurisation des contrôleurs de domaine contre les attaques](../../../ad-ds/plan/security-best-practices/Securing-Domain-Controllers-Against-Attack.md)  
  
-   [Surveillance d’Active Directory pour rechercher des signes de compromission](../../../ad-ds/plan/security-best-practices/Monitoring-Active-Directory-for-Signs-of-Compromise.md)  
  
-   [Recommandations de stratégie d’audit](../../../ad-ds/plan/security-best-practices/Audit-Policy-Recommendations.md)  
  
-   [Planification des compromis](../../../ad-ds/plan/security-best-practices/Planning-for-Compromise.md)  
  
-   [Maintenance d’un environnement plus sécurisé](../../../ad-ds/plan/security-best-practices/Maintaining-a-More-Secure-Environment.md)  
  
-   [Annexes](../../../ad-ds/plan/security-best-practices/Appendices.md)  
   
-   [Annexe b : Comptes privilégiés et groupes dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-B--Privileged-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Annexe C : Comptes protégés et groupes dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory.md)  
  
-   [Annexe d : Sécurisation des comptes d’administrateur intégré dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-D--Securing-Built-In-Administrator-Accounts-in-Active-Directory.md)  
  
-   [Annexe e : Sécurisation des groupes administrateurs d’entreprise dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-E--Securing-Enterprise-Admins-Groups-in-Active-Directory.md)  
  
-   [Annexe f : Sécurisation des groupes d’administrateurs de domaine dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-F--Securing-Domain-Admins-Groups-in-Active-Directory.md)  
  
-   [Annexe g : Sécurisation des groupes d’administrateurs dans Active Directory](../../../ad-ds/plan/security-best-practices/Appendix-G--Securing-Administrators-Groups-in-Active-Directory.md)  
  
-   [Appendix h : Sécurisation des comptes des administrateurs locaux et groupes](../../../ad-ds/plan/security-best-practices/Appendix-H--Securing-Local-Administrator-Accounts-and-Groups.md)  
  
-   [Annexe i : Création de gestion de comptes pour les comptes protégés et groupes dans Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md)   
  
-   [Annexe l : Événements à surveiller](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md)  
  
-   [Annexe m : Liens vers des documents et lecture recommandée](../../../ad-ds/manage/Appendix-M--Document-Links-and-Recommended-Reading.md)  
  


