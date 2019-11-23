---
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: 'Erreur de réplication1396 Échec d’ouverture de session: le nom du compte cible est incorrect'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: a8af10fd54f557e4f4a2127dbd1cc178d53d93a4
ms.sourcegitcommit: 214847318401cebdcb7f1924a731b4439c9d8a24
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 11/19/2019
ms.locfileid: "71402486"
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>Erreur de réplication1396 Échec d’ouverture de session: le nom du compte cible est incorrect

>S’applique à : Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd"> <introduction>
    <para>Cet article décrit les symptômes, la cause et la résolution de l’échec de la réplication Active Directory avec l’erreur Win32 1396 : &quot;échec de l’ouverture de session : le nom du compte cible est incorrect.&quot; </para>
    <list class="bullet"> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">Symptômes</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">Provoque</link>
        </para>
      </listItem> <listItem>
        <para>
          <link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Resolutions">Résolutions</link>
        </para>
      </listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>Symptômes</title>
    <content>
      <para />
      <list class="ordered">
<listItem><para>DCDIAG signale que le test Active Directory les réplications a échoué avec l’erreur 1396 : échec d’ouverture de session : le nom du compte cible est incorrect.&quot;</para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN. EXE signale que la dernière tentative de réplication a échoué avec l’État 1396.</para><para>Les commandes REPADMIN qui citent communément l’État 1396 incluent, sans s’y limiter :</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN/ADD</para></listItem><listItem><para>REPADMIN/REPLSUM.</para></listItem><listItem><para>REPADMIN/REHOST</para></listItem><listItem><para>REPADMIN/SHOWVECTOR/LATENCY</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN/SHOWREPS</para></listItem><listItem><para>REPADMIN/SHOWREPL</para></listItem><listItem><para>REPADMIN/SYNCALL</para></listItem></list></TD></tr></tbody></table><para>L’exemple de sortie de &quot;REPADMIN/SHOWREPS&quot; illustrant la réplication entrante de CONTOSO-DC2 vers CONTOSO-DC1 échoue avec la &quot;échec de l’ouverture de session : le nom du compte cible est incorrect.&quot; erreur est indiquée ci-dessous :</para><code>Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ &lt;date&gt; &lt;time&gt; failed, <codeFeaturedElement>result 1396 (0x574):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
&lt;#&gt; consecutive failure(s).
Last success @ &lt;date&gt; &lt;time&gt;.
</code></listItem><listItem><para>La commande <ui>répliquer maintenant</ui> dans Active Directory sites et Services retourne &quot;échec de l’ouverture de session : le nom du compte cible est incorrect.&quot;</para><para>Le fait de cliquer avec le bouton droit sur l’objet de connexion à partir d’un contrôleur de source et de choisir <ui>répliquer maintenant</ui> échoue avec &quot;échec d’ouverture de session : le nom du compte cible est incorrect.&quot; le message d’erreur à l’écran est illustré ci-dessous :</para><para>Texte du titre de la boîte de dialogue :</para><para>Répliquer maintenant</para><para>Texte du message de la boîte de dialogue : </para><para>L’erreur suivante s’est produite lors de la tentative de synchronisation du contexte d’appellation &lt;chemin d’accès DNS de la partition&gt; du contrôleur de domaine &lt;contrôleur de domaine source&gt; au contrôleur de domaine &lt;contrôleur de domaine de destination&gt;: échec d’ouverture de session : le nom du compte cible est incorrect. Cette opération ne se poursuivra pas. </para></listItem><listItem><para>Les événements KCC NTDS, NTDS général ou Microsoft-Windows-ActiveDirectory_DomainService avec l’État 1396 sont consignés dans le journal des services d’annuaire dans observateur d’événements.</para><para>Active Directory événements qui citent communément l’État 1396 incluent, mais ne sont pas limités à :</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>ID d’événement</para></TD><TD><para>Source de l'événement</para></TD><TD><para>Chaîne d’événement</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Le Assistant Installation Active Directory Domain Services (dcpromo) n’a pas pu établir la connexion avec le contrôleur de domaine suivant.</para></TD></tr><tr><TD><para>1645</para><para>Cet événement répertorie le nom de principal du service en trois parties.</para></TD><TD><para>Réplication NTDS</para></TD><TD><para>Active Directory n'a pas effectué un appel de procédure distante vers un autre contrôleur de domaine car le nom principal de service désiré pour le contrôleur de domaine cible n'est pas inscrit sur le contrôleur de domaine Centre de distribution de clés qui résout les noms principaux de service.</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Active Directory Domain Services a tenté de communiquer avec le catalogue global suivant et les tentatives ont échoué.</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Le vérificateur de cohérence des connaissances a trouvé une connexion de réplication pour le service d’annuaire en lecture seule local et a tenté de le mettre à jour à distance sur l’instance de service d’annuaire suivante. L’opération a échoué. Une nouvelle tentative sera effectuée.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>KCC NTDS</para></TD><TD><para>Échec de la tentative d’établissement d’un lien de réplication pour la partition d’annuaire accessible en écriture suivante.</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>KCC NTDS</para></TD><TD><para>Échec de la tentative d’établissement d’un lien de réplication vers une partition d’annuaire en lecture seule avec les paramètres suivants.</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>NETLOGON</para></TD><TD><para> Le serveur ne peut pas enregistrer son nom dans DNS.</para></TD></tr></tbody></table></listItem><listItem><para>DCPROMO échoue avec une erreur à l’écran</para><para>Texte du titre de la boîte de dialogue :</para><para>Échec de l’installation de Active Directory</para><para>Texte du message de la boîte de dialogue :</para><para>L’opération a échoué car : le service d’annuaire n’a pas pu créer l’objet serveur pour CN = NTDS Settings, CN = ServerBeingPromoted, CN = Servers, CN = site, CN = sites, CN = Configuration, DC = contoso, DC = com sur le serveur ReplicationSourceDC.contoso.com. </para><para>Vérifiez que les informations d’identification réseau fournies disposent des droits d’accès suffisants pour ajouter un réplica. </para><para>
Échec de l’ouverture de session &quot;: le nom du compte cible est incorrect. &quot;</para><para>Dans ce cas, les ID d’événement 1645, 1168 et 1125 sont consignés sur le serveur en cours de promotion.</para></listItem><listItem><para>Mapper un lecteur à l’aide de <embeddedLabel>net use</embeddedLabel>:</para><code>C:&gt;net use z: &lt;server_name&gt;c$
System error 1396 has occurred.
Logon Failure: The target account name is incorrect.</code><para>Dans ce cas, le serveur peut également enregistrer l’ID d’événement 333 dans le journal des événements système et utiliser une grande quantité de mémoire virtuelle pour une application telle que SQL Server.</para></listItem><listItem><para>L’heure du contrôleur de l’heure est incorrecte.</para></listItem><listItem><para>Le KDC ne démarre pas sur un RODC après une restauration du compte krbtgt pour le RODC, qui avait été supprimé. Par exemple, après une restauration, l’erreur 1396 s’affiche. </para><para>
L’ID d’événement 1645 est enregistré dans le RODC. </para><para>
Dcdiag signale également une erreur indiquant qu’il ne peut pas mettre à jour le compte de contrôleur de domaine en lecture seule. </para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>Provoque</title>
    <content>
      <para />
      <list class="ordered">
        <listItem>
          <para>Le nom principal de service n’existe pas dans le catalogue global recherché par le KDC pour le compte du client qui tente de s’authentifier à l’aide de Kerberos.</para>
          <para>Dans le contexte de la réplication Active Directory, le client Kerberos est le contrôleur de domaine de destination, le KDC effectuant la recherche SPN est probablement le contrôleur de domaine de destination, mais il peut s’agir d’un contrôleur de domaine distant.</para>
        </listItem>
        <listItem>
          <para>L’utilisateur ou le compte de service qui doit contenir le nom de principal du service recherché n’existe pas dans le catalogue global recherché par le KDC pour le compte du contrôleur de domaine de destination qui tente de répliquer.</para>
          <para>Dans le contexte de la réplication Active Directory, le compte d’ordinateur DC source n’existe pas dans le catalogue global qui est recherché par le contrôleur de l’ordinateur de destination effectuant la réplication entrante.</para>
        </listItem>
        <listItem>
          <para>Le contrôleur de domaine de destination n’a pas de secret LSA pour le domaine du contrôleur de domaine source.</para>
        </listItem>
        <listItem>
          <para>Le nom de principal du service recherché existe sur un autre compte d’ordinateur que le contrôleur de périphérique source.</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Résolutions</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>Vérifiez le journal des événements du service d’annuaire sur le contrôleur de destination pour l’événement de réplication NTDS 1645 et notez les points suivants :</para>
          <para>Nom du contrôleur de l’emplacement de destination</para>
          <para>Nom de principal du service (SPN) recherché (E3514235-4B06-11D1-AB04-00C04FC2DCD2/&lt;le GUID de l’objet pour les paramètres NTDS du contrôleur de domaine source&gt;/&lt;domaine cible&amp;amp ; gt ;.&amp;amp ; lt ; TLD&amp;amp ; gt ; @&lt;domaine cible&gt;.&lt;TLD&gt;</para>
          <para>KDC utilisé par le contrôleur de domaine de destination</para>
        </listItem>
        <listItem>
          <para>À partir de la console du KDC identifié à l’étape 1, tapez : </para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>Exécutez immédiatement le test du localisateur NLTEST après une tentative de réplication qui échoue avec l’erreur 1396 sur le DC de destination. </para>
          <para>Cela doit identifier le GC par rapport auquel le KDC effectue des recherches de SPN. </para>
          <para>Le catalogue global qui fait l’objet de la recherche par le KDC peut également être capturé dans l’événement 1655 de Microsoft-Windows-ActiveDirectory_DomainService.</para>
        </listItem>
        <listItem>
          <para>Recherchez le nom de principal du service découvert à l’étape 1 sur le catalogue global découvert à l’étape 2.</para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:&quot;(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)&quot; /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>OU</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter &quot;(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)&quot; -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>Vérifiez que l’objet hôte du nom de principal du service existe.</para>
          <para>Vérifiez le chemin d’accès au DN de l’objet hôte, notamment si l’objet est tronqué/en conflit ou s’il se trouve dans le conteneur perdu et trouvé.</para>
          <para>Vérifiez que les contrôleurs de service source Active Directory le SPN de réplication est inscrit uniquement sur le compte d’ordinateur des contrôleurs de service source.</para>
          <para>Si le SPN de réplication est manquant, déterminez si le contrôleur de domaine source a inscrit son nom de principal du service auprès de lui-même, et si le nom de principal du service est manquant dans le GC utilisé par le KDC en raison d’une latence de réplication simple ou d’un échec de réplication.</para>
        </listItem>
        <listItem>
          <para>Vérifiez l’intégrité du canal sécurisé et l’intégrité de l’approbation.</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics>
    <externalLink> 
      <linkText>Dépannage des opérations de Active Directory qui échouent avec l’erreur 1396 : échec d’ouverture de session : le nom du compte cible est incorrect.</linkText> 
      <linkUri><a href="https://support.microsoft.com/kb/2183411/en-gb" data-raw-source="https://support.microsoft.com/kb/2183411/en-gb">https://support.microsoft.com/kb/2183411/en-gb</a></linkUri> 
    </externalLink>
  </relatedTopics>
</developerConceptualDocument>


