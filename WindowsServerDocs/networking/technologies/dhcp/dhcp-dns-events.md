---
title: Événements de journalisation DHCP pour les inscriptions d’enregistrements DNS
description: Cette rubrique fournit des informations sur les événements de journalisation du serveur DHCP dans Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: beb8c188-6fcf-4520-8825-d17f8ee9fb04
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 5d167666e632aa1a8d92de71feafc9014b66e7ce
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312608"
---
# <a name="dhcp-logging-events-for-dns-registrations"></a>Événements de journalisation DHCP pour les inscriptions DNS

>S’applique à : Windows Server (canal semi-annuel), Windows Server 2016

Les journaux des événements du serveur DHCP fournissent désormais des informations détaillées sur les échecs d’inscription DNS.

>[!NOTE]
>Dans de nombreux cas, la raison de l’échec de l’inscription des enregistrements DNS par les serveurs DHCP est qu’une zone de recherche inversée DNS\-est configurée de façon incorrecte ou n’est pas configurée du tout.

Les nouveaux événements DHCP suivants vous aident à identifier facilement le moment où les inscriptions DNS échouent en raison d’une zone de recherche inversée\-DNS incorrecte ou mal configurée.

|ID|Événement|Valeur|
|-----|--------------------|--------------------------------------------------------|
|20317|DHCPv4. ForwardRecordDNSFailure|Échec de l’enregistrement de transfert de l’enregistrement pour l’adresse IPv4% 1 et le nom de domaine complet %2 avec l’erreur %3. Cela est probablement dû au fait que la zone de recherche directe pour cet enregistrement n’existe pas sur le serveur DNS.|
|20318|DHCPv4. ForwardRecordDNSTimeout|Échec de l’enregistrement de transfert de l’enregistrement pour l’adresse IPv4% 1 et le nom de domaine complet %2 avec l’erreur %3.|
|20319|DHCPv4. PTRRecordDNSFailure|L’inscription de l’enregistrement PTR pour l’adresse IPv4% 1 et le nom de domaine complet %2 a échoué avec l’erreur %3. Cela est probablement dû au fait que la zone de recherche inversée pour cet enregistrement n’existe pas sur le serveur DNS.|
|20320|DHCPv4. PTRRecordDNSTimeout|L’inscription de l’enregistrement PTR pour l’adresse IPv4% 1 et le nom de domaine complet %2 a échoué avec l’erreur %3.|
|20321|DHCPv6. ForwardRecordDNSFailure|Échec de l’enregistrement de transfert de l’enregistrement pour l’adresse IPv6% 1 et le nom de domaine complet %2 avec l’erreur %3. Cela est probablement dû au fait que la zone de recherche directe pour cet enregistrement n’existe pas sur le serveur DNS.|
|20322|DHCPv6. ForwardRecordDNSTimeout|Échec de l’enregistrement de transfert de l’enregistrement pour l’adresse IPv6% 1 et le nom de domaine complet %2 avec l’erreur %3.|
|20323|DHCPv6. PTRRecordDNSFailure|L’inscription de l’enregistrement PTR pour l’adresse IPv6% 1 et le nom de domaine complet %2 a échoué avec l’erreur %3. Cela est probablement dû au fait que la zone de recherche inversée pour cet enregistrement n’existe pas sur le serveur DNS.|
|20324|DHCPv6. PTRRecordDNSTimeout|L’inscription de l’enregistrement PTR pour l’adresse IPv6% 1 et le nom de domaine complet %2 a échoué avec l’erreur %3.|
|20325|DHCPv4. ForwardRecordDNSError|Échec de l’inscription de l’enregistrement PTR pour l’adresse IPv4% 1 et le nom de domaine complet %2. erreur : %3 \(%4\).|
|20326|DHCPv6. ForwardRecordDNSError|Échec de l’enregistrement de transfert de l’enregistrement pour l’adresse IPv6% 1 et le nom de domaine complet %2 avec l’erreur %3 \(%4\)|
|20327|DHCPv6. PTRRecordDNSError|Échec de l’inscription de l’enregistrement PTR pour l’adresse IPv6% 1 et le nom de domaine complet %2. erreur : %3 \(%4\).|

