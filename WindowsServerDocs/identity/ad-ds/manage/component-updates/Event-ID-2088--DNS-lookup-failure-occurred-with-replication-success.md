---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: "ID d’événement 2088 - Échec de la recherche DNS s’est produite avec la réplication"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4f223f075775f942f83a1962da28a77e85e89aa0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>ID d’événement 2088: Une défaillance de la recherche DNS s’est produite avec la réplication

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012

    
    <developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
      <introduction>
    <para>When a destination domain controller running Windows Server 2003 with Service Pack 1 (SP1) receives Event ID 2088 in the Directory Service event log, attempts to resolve the globally unique identifier (GUID) in the alias (CNAME) resource record to an IP address for the source domain controller failed. However, the destination domain controller tried other means to resolve the name and succeeded by using either the fully qualified domain name (FQDN) or the NetBIOS name of the source domain controller. Although replication was successful, the Domain Name System (DNS) problem should be diagnosed and resolved. </para>
    <para>The following is an example of the event text: </para>
    <code>Log Name: Directory Service

    Source: Microsoft-Windows-ActiveDirectory_DomainService
    Date: 3/15/2008  9:20:11 AM
    Event ID: 2088
    Task Category: DS RPC Client 
    Level: Warning
    Keywords: Classic
    User: ANONYMOUS LOGON
    Computer: DC3.contoso.com
    Description:
    Active Directory could not use DNS to resolve the IP address of the 
    source domain controller listed below. To maintain the consistency 
    of Security groups, group policy, users and computers and their passwords, 
    Active Directory Domain Services successfully replicated using the NetBIOS 
    or fully qualified computer name of the source domain controller. 

Configuration DNS incorrecte peut-être affecter les autres opérations essentielles sur les ordinateurs membres, les contrôleurs de domaine ou serveurs d’applications dans cette forêt ActiveDirectory Domain Services, y compris l’authentification de connexion ou l’accès aux ressources réseau. 

Vous devez immédiatement résoudre cette erreur de configuration DNS pour que ce contrôleur de domaine peut résoudre l’adresse IP du contrôleur de domaine source à l’aide de DNS. 

Nom d’un autre serveur: nom d’hôte DNS de l’échec de DC1: 4a8717eb-8e58-456 c-995a-c92e4add7e8e._msdcs. contoso.com 

Remarque: Par défaut, jusqu'à 10échecs DNS sont affichés pour une période de 12heures donné, même si plus de 10échecs se produisent.  Pour vous connecter à tous les événements d’échec individuels, définissez les tests de diagnostic suivantes valeur de Registre sur 1: 

Chemin d’accès du Registre: HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22DS RPC Client 

Action de l’utilisateur: 

1) Si le contrôleur de domaine source n’est plu fonctionne ou de système d’exploitation a été réinstallé avec un nom d’ordinateur différent ou NTDSDSA objet GUID, supprimer les métadonnées du contrôleur de domaine source avec ntdsutil.exe, à l’aide de la procédure décrite dans l’article correctif 216498. 

2) Vérifiez que le contrôleur de domaine source ActiveDirectory est en cours d’exécution et qu’il est accessible sur le réseau en tapant «net affichage \\&lt;nom du contrôleur de domaine source&gt;» ou «ping &lt;nom du contrôleur de domaine source&gt;». 

3) Vérifiez que le contrôleur de domaine source utilise un serveur DNS valide pour les services DNS et que le contrôleur de domaine source un enregistrement d’hôte et CNAME de l’enregistrement sont correctement inscrit, à l’aide de la version de DNS amélioré de DCDIAG.EXE de https://www.microsoft.com/dns 

DCDiag//test: dns 

4) Vérifiez que ce contrôleur de domaine de destination utilise un serveur DNS valide pour les services DNS, en exécutant la version améliorée du DNS de DCDIAG.EXE commande sur la console du contrôleur de domaine de destination, comme suit: 

DCDiag//test: dns 

5) Pour une analyse plus approfondie de DNS échecs erreur voir Ko824449: https://support.microsoft.com/?kbid=824449 

Valeur d’erreur de données supplémentaire: 11004 le nom demandé est valide, mais aucune donnée du type demandé n’introuvable</code>
  </introduction>
  <section>
    <title>Diagnostic</title>
    <content>
      <para>échec pour résoudre le nom de contrôleur de domaine source à l’aide de l’enregistrement de ressource alias (CNAME) dans DNS peut être dû à des erreurs de configuration DNS ou des retards de propagation des données DNS.</para>
    </content>
  </section>
  <section>
    <title>Résolution</title>
    <content>
      <para>continuer le DNS de test comme décrit dans «<link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">ID d’événement 2087: Échec de la recherche DNS a provoqué l’échec de la réplication</link>.»</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


