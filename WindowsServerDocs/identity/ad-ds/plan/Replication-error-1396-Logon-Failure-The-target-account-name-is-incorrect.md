---
ms.assetid: 399a8bbe-3375-4bb0-b55b-5f46e7050028
title: "Erreur de réplication Échec d’ouverture de session 1396 le nom du compte cible est incorrect"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 84799de26e1260f914d9b959357d5eed6fef62f6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="replication-error-1396-logon-failure-the-target-account-name-is-incorrect"></a>Erreur de réplication Échec d’ouverture de session 1396 le nom du compte cible est incorrect

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>Cet article décrit les symptômes, cause et comment résoudre la réplication ActiveDirectory échoue avec l’erreur Win321396: «échec d’ouverture de session: nom du compte cible est incorrect.» </para><list class="bullet"><listItem><para><link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Symptoms">Symptômes</link></para></listItem><listItem><para><link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Causes">entraîne</link></para></listItem><listItem><para><link xlink:href="d3a01966-74c9-4c49-ba11-354b9acf7519#BKMK_Resolutions">résolutions</link></para></listItem></list>
  </introduction>
  <section address="BKMK_Symptoms">
              
    <title>Symptoms</title>
    <content>
      <para />
      <list class="ordered">
<listItem><para>DCDIAG reports that the Active Directory Replications test has failed with error 1396: Logon failure: The target account name is incorrect."</para><code>Testing server: &lt;Site name&gt;&lt;DC Name&gt;
Starting test: Replications
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: CN=&lt;DN path of naming context&gt;
<codeFeaturedElement>The replication generated an error (1396):
Logon Failure: The target account name is incorrect.</codeFeaturedElement>
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
XX failures have occurred since the last success</code></listItem><listItem><para>REPADMIN.EXE reports that the last replication attempt has failed with status 1396.</para><para>REPADMIN commands that commonly cite the 1396 status include but are not limited to:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /ADD</para></listItem><listItem><para>REPADMIN /REPLSUM</para></listItem><listItem><para>REPADMIN /REHOST</para></listItem><listItem><para>REPADMIN /SHOWVECTOR /LATENCY</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>Exemple de résultat de «REPADMIN /SHOWREPS» représentant la réplication entrante à partir de CONTOSO-DC2 à CONTOSO-DC1 échoue avec le «Échec d’ouverture de session: le nom du compte cible est incorrect.» erreur est présentée ci-dessous::</para><ph x="2">< code > par défaut-Premier-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Options de Site: (aucun)
GUID d’objet DSA: b6dc8589-7e00-4a5d-b688-045aef63ec01
invocationID DSA: b6dc8589-7e00-4a5d-b688-045aef63ec01
=== voisins entrants ===
contrôleur de domaine = contoso, DC = com
par défaut-Premier-Site-NameCONTOSO-DC2 via RPC
GUID d’objet DSA: 74fbe06c-932c-46b5-831b-af9e31f496b2
de la dernière tentative @ et lt; date et gt; et lt; heure et gt; n’a pas pu, le résultat de < codeFeaturedElement > 1396 (0x574):
Échec d’ouverture de session: nom du compte cible est incorrect. < / codeFeaturedElement >
& lt; #& gt; échecs consécutifs. 
De la dernière réussite @ et lt; date et gt; et lt; heure et gt;. 
< / Code >< / listItem ></ph><listItem><para>le <ui>Répliquer maintenant</ui> commande dans les Services et Sites ActiveDirectory retourne «Échec d’ouverture de session: nom du compte cible est incorrect.» </para><para>Avec le bouton droit sur l’objet de connexion à partir d’une contrôleur de domaine source et en choisissant <ui>Répliquer maintenant</ui> échoue avec «échec d’ouverture de session: nom du compte cible est incorrect.» L’à l’écran le message d’erreur est présentée ci-dessous:</para><para>texte du titre de la boîte de dialogue:</para><para>Répliquer maintenant</para><para>texte du message boîte de dialogue: </para><para>l’erreur suivante s’est produite lors de la tentative de synchroniser le contexte d’attribution de noms &lt;le chemin d’accès de partition DNS&gt; à partir du contrôleur de domaine &lt;de source de contrôleur de domaine&gt; au contrôleur de domaine &lt;du contrôleur de domaine de destination&gt;: échec d’ouverture de session: nom du compte cible est incorrect. Cette opération ne peut pas continuer. </para></listItem><listItem><para>Événements KCC NTDS générales NTDS ou Microsoft-Windows-ActiveDirectory_DomainService avec l’état 1396 sont consignés dans le journal Services d’annuaire dans l’Observateur d’événements. </para><para>Événements ActiveDirectory qui couramment indiquer l’état 1396 incluent, mais ne sont pas limités à:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>ID d’événement</para></TD><TD><para>Source d’événement</para></TD><TD><para>Chaîne d’événement</para></TD></tr></thead><tbody><tr><TD><para>1125</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>L’Assistant domaine ActiveDirectory Services Installation (Dcpromo) n’a pas pu établir de connexion avec le contrôleur de domaine suivant.</para></TD></tr><tr><TD><para>1645</para><para>cet événement répertorie le SPN trois parties.</para></TD><TD><para>Réplication NTDS</para></TD><TD><para>ActiveDirectory n’a pas effectué un appel de procédure distante (RPC) vers un autre contrôleur de domaine car le nom principal de service désiré (SPN) pour le contrôleur de domaine de destination n’est pas inscrit sur le contrôleur de domaine centre de Distribution de clés (KDC) qui résout le SPN.</para></TD></tr><tr><TD><para>1655</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Les Services de domaine ActiveDirectory a tenté de communiquer avec le catalogue global suivant et les tentatives ont échoué.</para></TD></tr><tr><TD><para>2847</para></TD><TD><para>Microsoft-Windows-ActiveDirectory_DomainService</para></TD><TD><para>Le vérificateur de cohérence trouve une connexion de réplication pour le service d’annuaire en lecture seule local et a tenté de mettre à jour à distance sur l’instance de service d’annuaire suivant. L’opération a échoué. Il sera tentée à nouveau.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>KCC NTDS</para></TD><TD><para>Échec de la tentative d’établir un lien de réplication pour la partition d’annuaire accessible en écriture suivante.</para></TD></tr><tr><TD><para>1926</para></TD><TD><para>KCC NTDS</para></TD><TD><para>La tentative d’établir un lien de réplication pour une partition d’annuaire en lecture seule avec les paramètres suivants a échoué.</para></TD></tr><tr><TD><para>5781</para></TD><TD><para>ACCÈS RÉSEAU</para></TD><TD><para> Le serveur ne peut pas enregistrer son nom dans DNS.</para></TD></tr></tbody></table></listItem><listItem><para>DCPROMO échoue avec une erreur à l’écran</para><para>texte du titre de la boîte de dialogue:</para><para>ActiveDirectory Installation n’a pas pu</para><para>texte du Message de la boîte de dialogue:</para><para>l’opération a échoué car: le Service d’annuaire n’a pas pu créer l’objet serveur pour CN = NTDS Settings, CN = ServerBeingPromoted, CN = Servers, CN = sites, CN = Sites, CN = Configuration, DC = contoso, DC = com sur le serveur ReplicationSourceDC.contoso.com. </para><para>Vérifiez que les informations d’identification réseau fournies ont des droits d’accès suffisants pour ajouter un réplica.</para><para> «Échec d’ouverture de session: nom du compte cible est incorrect.» </para><para>Dans ce cas, l’ID d’événement 1645 et 1168 1125 sont enregistrés sur le serveur qui est promu. </para></listItem><listItem><para>Mapper un lecteur en utilisant <embeddedLabel>commande net use</embeddedLabel>:</para><ph x="13">< code > c: et gt; net use z: et lt; nom_du_serveur et gt; c$
système erreur 1396 s’est produite.
 Échec d’ouverture de session: Nom du compte cible est incorrect. < / code ></ph><para>dans ce cas, le serveur peut également la journalisation 333 d’ID d’événement dans le journal des événements système et utiliser une grande quantité de mémoire virtuelle pour une application, telles que SQLServer. </para></listItem><listItem><para>Heure du contrôleur de domaine est incorrect. </para></listItem><listItem><para>Le KDC ne démarre pas sur un RODC après une restauration du compte krbtgt pour le RODC, qui avait été supprimé. Par exemple, après une restauration, erreur 1396 s’affiche. </para><para>1645 d’ID d’événement est consigné sur le RODC. </para><para>Dcdiag signale également une erreur qu’il ne peut pas mettre à jour le compte krbtgt RODC.</para></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Causes">
    <title>entraîne</title>
    <content>
      <para />
      <list class="ordered">
        <listItem>
          <para>le SPN n’existe pas sur le catalogue global recherché KDC de la part du client tentent de s’authentifier à l’aide de Kerberos.</para>
          <para>dans le cadre de la réplication ActiveDirectory, le client Kerberos est la destination du contrôleur de domaine, le KDC effectuer la recherche du nom principal de service est probablement la destination du contrôleur de domaine lui-même, mais peut être un contrôleur de domaine distant.</para>
        </listItem>
        <listItem>
          <para>le compte d’utilisateur ou un service qui doit contenir le service de nom principal recherchées n’existe pas sur le catalogue global recherché par le KDC pour le compte de tentative de réplication du contrôleur de domaine de destination.</para>
          <para>dans le cadre de la réplication ActiveDirectory, la compte d’ordinateur du contrôleur de domaine source n’existe pas sur le catalogue global recherché par le contrôleur de domaine pour le compte de la réplication entrante performances de contrôleur de domaine de destination. </para>
        </listItem>
        <listItem>
          <para>la contrôleur de domaine de destination ne dispose pas d’un secret LSA pour le domaine de contrôleurs de domaine source.</para>
        </listItem>
        <listItem>
          <para>le SPN recherchées existe sur un compte d’ordinateur autre que le contrôleur de domaine source.</para>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Résolutions</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>Vérifiez le journal des événements Service d’annuaire sur le contrôleur de domaine de destination pour l’événement NTDS réplication 1645 et notez les points suivants:</para>
          <para>le nom de la destination du contrôleur de domaine</para>
          <para>le SPN recherchées (E3514235-4B06-11D1-AB04-00C04FC2DCD2 /&lt;objet guid de l’objet Paramètres NTDS de contrôleurs de domaine source de&gt;/&lt;domaine cible&gt;.&lt; domaine de premier niveau&gt;@&lt;domaine cible&gt;. &lt;tld&gt;</para>
          <para>le KDC utilisé par la contrôleur de domaine de destination</para>
        </listItem>
        <listItem>
          <para>à partir de la console du KDC identifié à l’étape1, tapez: </para>
          <code>nltest /dsgetdc &lt;forest root DNS domain name &gt; /gc</code>
          <para>exécuter le test du localisateur NLTEST immédiatement après une tentative de réplication échoue avec l’erreur 1396 sur le contrôleur de domaine de destination. </para>
          <para>Cela doit identifier ce catalogue global qui exécute des recherches SPN sur le KDC. </para>
          <para>Le catalogue global recherchés par le contrôleur de domaine Kerberos peuvent également être capturé dans l’événement Microsoft-Windows-ActiveDirectory_DomainService1655. </para>
        </listItem>
        <listItem>
          <para>Recherche le SPN découverts à l’étape1dans le catalogue global découvert à l’étape2. </para>
          <code>C:&gt;repadmin /showattr Server_Name DC=corp,DC=contoso,dc=com &lt;GC used by KDC&gt; &lt;DN path of forest root domain&gt; /filter:"(serviceprincipalname=&lt;SPN cited in the NTDS Replication event 1645&gt;)" /gc /subtree /atts:cn,serviceprincipalname</code>
          <para>Ou</para>
          <code>C:&gt;dsquery * forestroot -scope subtree -filter "(serviceprincipalname=E3514235-4B06-11D1-AB04-00C04FC2DCD2/65cead9f-4949-46a3-a49a-f1fbfe13d2b3*)" -attr * -s Server_Name.europe.corp.contoso.com</code>
          <para>Vérifiez que l’objet ordinateur hôte pour le SPN existe. </para>
          <para>Vérifier le chemin d’accès de nom unique pour l’objet hôte notamment si l’objet est CNF / conflit tronqué ou se trouve dans le conteneur LostAndFound. </para>
          <para>Vérifier que la source de contrôleurs de domaine ActiveDirectory Replication SPN est enregistrée uniquement sur la contrôleurs de domaine de compte d’ordinateur source. </para>
          <para>Si la réplication SPN est manquante, déterminez si le contrôleur de domaine source a inscrit son nom principal de service avec lui-même et si le SPN est manquant dans le catalogue global utilisé par le KDC en raison de la latence de réplication simple ou d’un échec de réplication. </para>
        </listItem>
        <listItem>
          <para>Vérifiez l’intégrité de canal sécurisé et d’intégrité de confiance.</para>
        </listItem>
      </list>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>des opérations de résolution des problèmes ActiveDirectory qui échouent avec l’erreur 1396: échec d’ouverture de session: nom du compte cible est incorrect.</linkText>
      <linkUri>https://support.microsoft.com/kb/2183411/en-gb</linkUri>
    </externalLink>
  </relatedTopics>
</developerConceptualDocument>


