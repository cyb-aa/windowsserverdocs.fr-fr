---
ms.assetid: 0fd7b6aa-3e50-45a3-a3a6-56982844363e
title: ID d’événement 2088-échec de la recherche DNS avec succès de la réplication
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d51cbcc93a8decbcb72a1e91854a09345507511d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71368908"
---
# <a name="event-id-2088-dns-lookup-failure-occurred-with-replication-success"></a>ID d’événement 2088 : Une défaillance de la recherche DNS s’est produite avec la réplication

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

    
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

Une configuration DNS non valide peut affecter les autres opérations essentielles sur les ordinateurs membres, les contrôleurs de domaine ou les serveurs d’applications de cette forêt Active Directory Domain Services, y compris l’authentification d’ouverture de session ou l’accès aux ressources réseau. 

Vous devez immédiatement résoudre cette erreur de configuration DNS afin que ce contrôleur de domaine puisse résoudre l’adresse IP du contrôleur de domaine source à l’aide de DNS. 

Nom du serveur de remplacement : échec de DC1 nom d’hôte DNS : 4a8717eb-8e58-456c-995a-c92e4add7e8e. _msdcs. contoso. com 

Remarque : par défaut, seuls 10 échecs au maximum sont affichés pour une période de 12 heures donnée, même si plus de 10 échecs se produisent.  Pour consigner tous les événements d’échec individuels, définissez la valeur de registre de diagnostics suivante sur 1 : 

Chemin du Registre : client RPC HKLM\System\CurrentControlSet\Services\NTDS\Diagnostics\22 DS 

Action de l’utilisateur : 

1) Si le contrôleur de domaine source n’est plus opérationnel ou que son système d’exploitation a été réinstallé avec un autre nom d’ordinateur ou GUID d’objet NTDSDSA, supprimez les métadonnées du contrôleur de domaine source avec Ntdsutil. exe, en suivant les étapes décrites dans l’article MSKB 216498. 

2) Confirmez que le contrôleur de domaine source exécute Active Directory et qu’il est accessible sur le réseau en tapant « net view \\&lt;nom du contrôleur de domaine source&gt;» ou « ping &lt;nom du contrôleur de domaine source&gt;». 

3) Vérifiez que le contrôleur de domaine source utilise un serveur DNS valide pour les services DNS et que l’enregistrement d’hôte et l’enregistrement CNAMe du contrôleur de domaine source sont correctement enregistrés, à l’aide de la version améliorée DNS de DCDIAG. EXE disponible sur <https://www.microsoft.com/dns> 

Dcdiag/test : DNS 

4) Vérifiez que ce contrôleur de domaine de destination utilise un serveur DNS valide pour les services DNS en exécutant la version améliorée DNS de DCDIAG. EXE sur la console du contrôleur de domaine de destination, comme suit : 

Dcdiag/test : DNS 

5) Pour une analyse plus poussée des échecs d’erreurs DNS, consultez KB 824449 : <https://support.microsoft.com/?kbid=824449> 

Valeur d’erreur de données supplémentaires : 11004 le nom demandé est valide, mais aucune donnée du type demandé n’a été trouvée</code> </introduction>
  <section>
    <content>de 
    <title>diagnostic</title> 
      <para>L’échec de la résolution du nom du contrôleur de domaine source à l’aide de l’enregistrement de ressource alias (CNAMe) dans DNS peut être dû à des problèmes de configuration DNS ou de retards dans la propagation des données DNS.</para>
    </content>
  </section>
  <section>
    <title>Résolution</title>
    <content>
      <para>Effectuez les tests DNS comme décrit dans &quot;<link xlink:href="85b1d179-f53e-4f95-b0b8-5b1c096a8076">ID d’événement 2087 : échec de la recherche DNS provoquant l’échec de la réplication</link>.&quot;</para>
    </content>
  </section>
  <relatedTopics />
</developerConceptualDocument>


