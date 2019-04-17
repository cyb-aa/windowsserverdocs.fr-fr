---
ms.assetid: 0f21951c-b1bf-43bb-a329-bbb40c58c876
title: "Erreur de réplication 1753 il ne sont aucun point de terminaison plus le mappeur de point de terminaison"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0e7412f5edc6c206888551fdc250883b5c0ced3e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 12/12/2017
---
# <a name="replication-error-1753-there-are-no-more-endpoints-available-from-the-endpoint-mapper"></a>Erreur de réplication 1753 il ne sont aucun point de terminaison plus le mappeur de point de terminaison

>S’applique à: Windows Server2016, Windows Server2012R2, Windows Server2012


<developerConceptualDocument xmlns="https://ddue.schemas.microsoft.com/authoring/2003/5" xmlns:xlink="https://www.w3.org/1999/xlink" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="https://ddue.schemas.microsoft.com/authoring/2003/5 http://clixdevr3.blob.core.windows.net/ddueschema/developer.xsd">
  <introduction>
    <para>Cette rubrique explique comment résoudre ActiveDirectory replication erreur 8524 de l’opération DSA ne peut pas continuer en raison d’une défaillance de la recherche DNS, les causes et les symptômes.</para>
    <list class="bullet">
      <listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Symptoms">Symptômes</link></para></listItem><listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Cause">Cause</link></para></listItem><listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_Resolutions">résolutions</link></para></listItem><listItem><para><link xlink:href="f7022f0d-0099-410c-8178-c654e624bc42#BKMK_MoreInfo">plus d’informations</link></para></listItem>
    </list>
  </introduction>
  <section address="BKMK_Symptoms">
    <title>Les symptômes</title>
    <content>
      <para>cet article décrit les symptômes, les causes et les étapes de résolution de d’opérations ActiveDirectory qui échouent avec l’erreur Win321753: «Il n’existe aucun point de terminaison plus le mappeur de point de terminaison.» </para>
      <list class="ordered">
        <listItem>
          <para>Rapports DCDIAG que le test de connectivité, test des réplications ActiveDirectory ou KnowsOfRoleHolders a échoué avec l’erreur 1753: «Il n’existe aucun point de terminaison plus le mappeur de point de terminaison.» </para>
          <code>Testing server: &lt;site&gt;&lt;DC Name&gt;
Starting test: Connectivity
* Active Directory LDAP Services Check
* Active Directory RPC Services Check
[&lt;DC Name&gt;] <codeFeaturedElement>DsBindWithSpnEx() failed with error 1753,
There are no more endpoints available from the endpoint mapper..</codeFeaturedElement>
Printing RPC Extended Error Info:
Error Record 1, ProcessID is &lt;process ID&gt; (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
Status is 1753: There are no more endpoints available from the endpoint mapper. Detection location is 500
NumberOfParameters is 4
Unicode string: ncacn_ip_tcp
Unicode string: &lt;source DC object GUID&gt;._msdcs.contoso.com
Long val: -481213899
Long val: 65537
Error Record 2, ProcessID is 700 (DcDiag) 
System Time is: &lt;date&gt; &lt;time&gt;
Generating component is 2 (RPC runtime)
<codeFeaturedElement>Status is 1753: There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
NumberOfParameters is 1
Unicode string: 1025
[Replications Check,&lt;DC Name&gt;] A recent replication attempt failed:
From &lt;source DC&gt; to &lt;destination DC&gt;
Naming Context: &lt;DN path of directory partition&gt;
The replication generated an error <codeFeaturedElement>(1753):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement> 
The failure occurred at &lt;date&gt; &lt;time&gt;.
The last success occurred at &lt;date&gt; &lt;time&gt;.
3 failures have occurred since the last success.
The directory on &lt;DC name&gt; is in the process.
of starting up or shutting down, and is not available.
Verify machine is not hung during boot.
</code>
        </listItem>
<listItem><para>REPADMIN. EXE de rapports que qui tentent de réplication a échoué avec le statut 1753. </para><para>Commandes REPADMIN couramment indiquer l’état 1753 incluent, mais ne sont pas limités à:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><tbody><tr><TD><list class="bullet"><listItem><para>REPADMIN /REPLSUM</para></listItem><listItem><para>REPADMIN /SHOWREPL</para></listItem></list></TD><TD><list class="bullet"><listItem><para>REPADMIN /SHOWREPS</para></listItem><listItem><para>REPADMIN /SYNCALL</para></listItem></list></TD></tr></tbody></table><para>Exemple de résultat de «REPADMIN /SHOWREPS» représentant la réplication entrante à partir de CONTOSO-DC2 à échouer CONTOSO-DC1 avec l’erreur «accès à la réplication a été refusé» est indiquée ci-dessous:</para><code>Default-First-Site-NameCONTOSO-DC1
DSA Options: IS_GC 
Site Options: (none)
DSA object GUID: b6dc8589-7e00-4a5d-b688-045aef63ec01
DSA invocationID: b6dc8589-7e00-4a5d-b688-045aef63ec01
==== INBOUND NEIGHBORS ======================================
DC=contoso,DC=com
Default-First-Site-NameCONTOSO-DC2 via RPC
DSA object GUID: 74fbe06c-932c-46b5-831b-af9e31f496b2
Last attempt @ &lt;date&gt; &lt;time&gt; failed, <codeFeaturedElement>result 1753 (0x6d9):
There are no more endpoints available from the endpoint mapper.</codeFeaturedElement>
&lt;#&gt; consecutive failure(s).
Last success @ &lt;date&gt; &lt;time&gt;.

</code></listItem><listItem><para>le <ui>vérifier la topologie de réplication</ui> commande dans les Services et Sites ActiveDirectory retourne «Il n’existe aucun point de terminaison plus le mappeur de point de terminaison.» </para><para>Avec le bouton droit sur l’objet de connexion à partir d’une contrôleur de domaine source et en choisissant <ui>vérifier la topologie de réplication</ui> échoue avec «Il n’existe aucun point de terminaison plus le mappeur de point de terminaison.» L’écran le message d’erreur est présentée ci-dessous:</para><para>texte du titre de la boîte de dialogue: vérifier la topologie de réplication</para><para>texte du message boîte de dialogue: </para><para>l’erreur suivante s’est produite lors de la tentative de contacter le contrôleur de domaine: aucun point de terminaison plus ne sont disponibles dans le mappeur de point de terminaison. </para></listItem><listItem><para>Le <ui>Répliquer maintenant</ui> commande dans les Services et Sites ActiveDirectory retourne «il n’existe aucun point de terminaison plus le mappeur de point de terminaison.» </para><para>Avec le bouton droit sur l’objet de connexion à partir d’une contrôleur de domaine source et en choisissant <ui>Répliquer maintenant</ui> échoue avec «Il n’existe aucun point de terminaison plus le mappeur de point de terminaison.» L’écran le message d’erreur est présentée ci-dessous:</para><para>texte du titre de la boîte de dialogue: Répliquer maintenant</para><para>texte du message boîte de dialogue: l’erreur suivante s’est produite lors de la tentative de synchroniser le contexte d’attribution de noms &lt;nom de partition de répertoire %&gt; à partir du contrôleur de domaine &lt;contrôleur de domaine Source&gt; au contrôleur de domaine &lt;contrôleur de domaine de Destination&gt;:</para><para>

Il n’existe aucun point de terminaison plus le mappeur de point de terminaison. </para><para>L’opération ne continuera pas</para></listItem><listItem><para>événements KCC NTDS générales NTDS ou Microsoft-Windows-ActiveDirectory_DomainService avec l’état-2146893022 sont consignés dans le journal Services d’annuaire dans l’Observateur d’événements. </para><para>Événements ActiveDirectory qui couramment indiquer l’état-2146893022 incluent, mais ne sont pas limités à:</para><table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11"><thead><tr><TD><para>ID d’événement</para></TD><TD><para>Source d’événement</para></TD><TD><para>Chaîne d’événement</para></TD></tr></thead><tbody><tr><TD><para>1655</para></TD><TD><para>NTDS Général</para></TD><TD><para>ActiveDirectory a tenté de communiquer avec le catalogue global suivant et les tentatives ont échoué.</para></TD></tr><tr><TD><para>1925</para></TD><TD><para>KCC NTDS</para></TD><TD><para>Échec de la tentative d’établir un lien de réplication pour la partition d’annuaire accessible en écriture suivante.</para></TD></tr><tr><TD><para>1265</para></TD><TD><para>KCC NTDS</para></TD><TD><para>Échec d’une tentative par le vérificateur de cohérence de connaissances (KCC) pour ajouter un accord de réplication du suivant répertoire source et la partition de contrôleur de domaine.</para></TD></tr></tbody></table></listItem>
</list>
    </content>
  </section>
  <section address="BKMK_Cause">
    <title>Cause</title>
    <content>
      <para>le diagramme ci-dessous illustre le flux de travail RPC à partir de l’inscription de l’application serveur avec le mappeur de point de terminaison RPC (EPM) à l’étape1 pour la transmission de données à partir du client RPC à l’application cliente à l’étape7. </para>
      <para>&lt;ADDS_RPCWorkflow&gt;</para>
      <para>les étapes1 à 7 carte pour les opérations suivantes:</para>
      <list class="ordered">
        <listItem>
          <para>Application Server enregistre ses points de terminaison avec le mappeur de point de terminaison RPC (EPM) </para>
        </listItem>
        <listItem>
          <para>Client effectue un appel RPC (pour un utilisateur, le compte de système d’exploitation ou une application lancé une opération) </para>
        </listItem>
        <listItem>
          <para>Client côté RPC contacts les ordinateurs cibles EPM et demander pour le point de terminaison terminer l’appel du client </para>
        </listItem>
        <listItem>
          <para>EPM de l’ordinateur serveur répond avec un point de terminaison </para>
        </listItem>
        <listItem>
          <para>côté Client RPC contacte l’application serveur </para>
        </listItem>
        <listItem>
          <para>application serveur exécute l’appel, retourne le résultat au client RPC </para>
        </listItem>
        <listItem>
          <para>côté Client RPC passe le résultat à l’application cliente</para>
        </listItem>
      </list>
      <para>1753 échec est générée par une défaillance entre les étapes #3 et 4 #. Plus précisément, erreur 1753 signifie que le client RPC (contrôleur de domaine de destination) a été en mesure de contacter le serveur RPC (contrôleur de domaine source) sur le port 135, mais le mode protégé amélioré sur le serveur RPC (contrôleur de domaine source) n’a pas pu localiser l’application RPC de vos centres d’intérêt et a renvoyé l’erreur du côté serveur 1753. La présence de l’erreur 1753 indique que le client RPC (contrôleur de domaine de destination) reçoit la réponse d’erreur côté serveur à partir du serveur RPC (source de réplication ActiveDirectory du contrôleur de domaine) sur le réseau. </para>
      <para>Incluent les causes spécifiques de l’erreur 1753: </para>
      <list class="ordered">
        <listItem>
          <para>l’application serveur jamais démarrée (autrement dit, étape #1dans le diagramme «plus d’informations» situé au-dessus a été tentée jamais). </para>
        </listItem>
        <listItem>
          <para>L’application server démarré, mais il y a un échec lors de l’initialisation qui l’a empêché de l’inscription avec le mappeur de point de terminaison RPC (autrement dit, étape #1dans le diagramme «plus d’informations» ci-dessus a été tentée mais a échoué). </para>
        </listItem>
        <listItem>
          <para>L’application server a démarré mais morts par la suite. (autrement dit, étape #1dans le diagramme «plus d’informations» ci-dessus a réussi, mais il a été annulée ultérieurement, car le serveur morts). </para>
        </listItem>
        <listItem>
          <para>L’application serveur désinscrit manuellement ses points de terminaison (semblables à 3mais intentionnelle. Mais probablement pas inclus par souci d’exhaustivité.) </para>
        </listItem>
        <listItem>
          <para>Client le RPC (contrôleur de domaine de destination) de contacter un serveur RPC différent de celui prévu en raison d’un nom pour l’erreur de mappage IP dans DNS, WINS ou hôte/Lmhosts. </para>
        </listItem>
      </list>
      <para>Erreur1753 n’est pas due à: </para>
      <list class="bullet">
        <listItem>
          <para>un manque de connectivité réseau entre le client RPC (contrôleur de domaine de destination) et le serveur RPC (contrôleur de domaine source) sur le port 135</para>
        </listItem>
        <listItem>
          <para>un manque de connectivité réseau entre le serveur RPC (contrôleur de domaine source) utilisant le port 135 et le client RPC (contrôleur de domaine de destination) via le port éphémère. </para>
        </listItem>
        <listItem>
          <para>Une non-concordance de mot de passe ou l’impossibilité de déchiffrer un paquet chiffré Kerberos par le contrôleur de domaine source</para>
        </listItem>
      </list>
      <para></para>
    </content>
  </section>
  <section address="BKMK_Resolutions">
    <title>Résolutions</title>
    <content>
      <list class="ordered">
        <listItem>
          <para>
            <embeddedLabel>Vérifiez que le service d’inscription de son service avec le mappeur de point de terminaison a démarré</embeddedLabel>
          </para>
          <para>pour Windows2000 et les contrôleurs de domaine Windows Server2003: Assurez-vous que le contrôleur de domaine source est démarré en mode normal. </para>
          <para>Pour Windows Server2008 ou Windows Server2008R2: à partir de la console du contrôleur de domaine source, démarrer les Services Gestionnaire (services.msc) et vérifiez que le <embeddedLabel>les Services de domaine ActiveDirectory</embeddedLabel> service est en cours d’exécution. </para>
        </listItem>
        <listItem>
          <para>
            <embeddedLabel>Vérifier que le client RPC (contrôleur de domaine de destination) connecté au serveur RPC prévue (contrôleur de domaine source)</embeddedLabel>
          </para>
          <para>tous les contrôleurs de domaine dans un Registre de forêt ActiveDirectory commun, un contrôleur de domaine CNAME enregistrer dans la zone _msdcs. &lt;domaine racine de forêt&gt; zone DNS, quelle que soit le domaine, ils se trouvent dans la forêt. L’enregistrement CNAME de contrôleur de domaine est dérivé de la <embeddedLabel>objectGUID</embeddedLabel> attribut de l’objet Paramètres NTDS pour chaque contrôleur de domaine. </para>
          <para>Lorsque vous effectuez des opérations basées sur la réplication, une contrôleur de domaine de destination des requêtes DNS pour l’enregistrement CNAME de contrôleurs de domaine source. L’enregistrement CNAME contient le nom complet d’ordinateur de contrôleur de domaine source qui est utilisé pour dériver de l’adresse IP des contrôleurs de domaine source via une recherche de cache du client DNS, hôte / LMHost fichier recherche, l’hôte A / AAAA enregistrement dans DNS ou WINS. </para>
          <para>Objets obsolètes Paramètres NTDS et to-IP mappages de nom incorrect - dans les fichiers de DNS, WINS, l’hôte et LMHOST peuvent provoquer le client RPC (contrôleur de domaine de destination) pour se connecter au serveur RPC incorrect (contrôleur de domaine Source). En outre, le mappage to-IP nom incorrect - peut provoquer le client RPC (contrôleur de domaine de destination) pour se connecter à un ordinateur qui n’a pas encore de l’Application de serveur RPC d’intérêt (le rôle d’ActiveDirectory dans le cas présent) est installé. (Exemple: un enregistrement hôte obsolètes pour DC2 contient l’adresse IP de DC3 ou un ordinateur membre). </para>
          <para>Vérifier correspondant à l’attribut objectGUID pour la source de contrôleur de domaine qui existe dans la destination de copie de contrôleurs de domaine ActiveDirectory le GUID de contrôleur de domaine source stocké dans la source de copie de contrôleurs de domaine d’ActiveDirectory. S’il existe une différence, utilisez repadmin /showobjmeta sur l’objet paramètres ntds pour voir celle correspond à la dernière promotion du contrôleur de domaine source (Conseil: comparer le cachet de date pour date de création de l’objet Paramètres NTDS /showobjmeta par rapport à la dernière date de la promotion dans le fichier dcpromo.log de contrôleurs de domaine source. Vous devrez peut-être utiliser le dernier / créer la date de la DCPROMO.LOG le fichier journal lui-même). Si le GUID d’objet n’est pas identique, la contrôleur de domaine susceptible de destination a un objet Paramètres NTDS obsolète pour le contrôleur de domaine source dont l’enregistrement CNAME fait référence à un enregistrement d’hôte avec un nom incorrect pour le mappage IP. </para>
          <para>Sur la contrôleur de domaine de destination, exécutez IPCONFIG//ALL afin de déterminer les serveurs DNS à l’aide de la destination du contrôleur de domaine pour la résolution de noms:</para>
          <code>c:&gt;ipconfig /all</code>
          <para>sur le contrôleur de domaine de destination, exécutez NSLOOKUP sur l’enregistrement CNAME de contrôleur de domaine complet de contrôleurs de domaine source:</para>
          <code>c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs primary DNS Server IP &gt;
c:&gt;nslookup -type=cname &lt;fully qualified cname of source DC&gt; &lt;destination DCs secondary DNS Server IP&gt;</code>
          <para>vérifier que l’adresse IP renvoyée par NSLOOKUP «propriétaire» le nom d’hôte / identité de sécurité de contrôleur de domaine source:</para>

          <code>C:&gt;NBTSTAT -A &lt;IP address returned by NSLOOKUP in the step above&gt;</code>
          <para>ou</para>
          <para>journal sur la console du contrôleur de domaine source, exécutez le «IPCONFIG» à partir de l’invite de commande et vérifiez que le contrôleur de domaine source est propriétaire de l’adresse IP renvoyée par la commande NSLOOKUP ci-dessus</para>
          <para>recherchez hôte périmé / en double à des mappages IP dans DNS</para>
          <code>NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;single label hostname of source DC&gt; &lt;secondary DNS Server IP on destination DC&gt;

NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;primary DNS Server IP on destination DC&gt;
NSLOOKUP -type=hostname &lt;fully qualified computer name of source DC&gt; &lt;secondary DNS Server IP on dest. DC&gt;</code>
<para>si les adresses IP non valides existent dans les enregistrements d’hôte, examiner si le nettoyage DNS est activé et configuré correctement. </para><para>Si les tests ci-dessus ou une trace réseau ne s’affiche une requête de nom renvoyant une adresse IP non valide, tenez compte des entrées obsolètes dans les fichiers de l’ordinateur hôte, les fichiers LMHOSTS et les serveurs WINS. Notez que les serveurs DNS peut également être configurés pour effectuer la résolution de nom de secours WINS. </para>
</listItem>
        <listItem>
          <para>
            <embeddedLabel>Vérifiez que l’application serveur (ActiveDirectory et al.) est inscrite avec le mappeur de point de terminaison sur le serveur RPC (contrôleur de domaine source)</embeddedLabel>
          </para>
          <para>ActiveDirectory utilise une combinaison de ports bien connus et inscrits de manière dynamique. Ce tableau répertorie connus ports et protocoles utilisés par les contrôleurs de domaine ActiveDirectory.</para>
          <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
            <thead>
              <tr>
                <TD>
                  <para>Application de serveur RPC</para>
                </TD>
                <TD>
                  <para>Port</para>
                </TD>
                <TD>
                  <para>TCP</para>
                </TD>
                <TD>
                  <para>UDP</para>
                </TD>
              </tr>
            </thead>
            <tbody>
              <tr>
                <TD>
                  <para>Serveur DNS</para>
                </TD>
                <TD>
                  <para>53</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Kerberos</para>
                </TD>
                <TD>
                  <para>88</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Serveur LDAP</para>
                </TD>
                <TD>
                  <para>389</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Microsoft-DS</para>
                </TD>
                <TD>
                  <para>445</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>SSL LDAP</para>
                </TD>
                <TD>
                  <para>636</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Serveur de catalogue global</para>
                </TD>
                <TD>
                  <para>3268</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
              <tr>
                <TD>
                  <para>Serveur de catalogue global</para>
                </TD>
                <TD>
                  <para>3269</para>
                </TD>
                <TD>
                  <para>X</para>
                </TD>
                <TD>
                  <para />
                </TD>
              </tr>
            </tbody>
          </table>
          <para>Les ports connus ne sont pas enregistrés avec le mappeur de point de terminaison. </para>
          <para>ActiveDirectory et autres applications également inscrire les services qui reçoivent les ports affectés dynamiquement dans la plage de ports éphémères RPC. Ces applications de serveur RPC sont affectées dynamiquement les ports TCP entre 1024 et 5000 sur les ordinateurs Windows2000 et Windows Server2003 et entre 49152 et 65535 plage sur les ordinateurs Windows Server2008 et Windows Server2008R2. Le port RPC utilisé par la réplication peut être codé en dur dans le Registre en utilisant les étapes documentées dans <externalLink><linkText>l’article 224196</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink>. ActiveDirectory continue à s’inscrire avec le mode protégé amélioré lorsque configuré pour utiliser un port codée en dur. </para>
          <para>Vérifier que l’application serveur RPC d’intérêt s’est inscrit avec le mappeur de point de terminaison RPC sur le serveur RPC (contrôleur de domaine source dans le cas de la réplication ActiveDirectory). </para>
          <para>Un certain nombre de méthodes pour accomplir cette tâche, mais une consiste à installer et exécuter PORTQRY à partir d’une invite de commandes administrateur privilégié sur la console du contrôleur de domaine à l’aide de la syntaxe de la source: </para>
          <code>c:\&gt;portquery -n &lt;source DC&gt; -e 135 &gt;file.txt</code>
          <para>dans la sortie portqry, notez les numéros de port inscrits dynamiquement par le «MS NT Active DRS Interface» (UUID = 351...) pour le <embeddedLabel>protocole ncacn_ip_tcp</embeddedLabel>. L’extrait de code ci-dessous montre un exemple de sortie portquery à partir d’un contrôleur de domaine de Windows Server2008R2 et l’UUID / paire de protocole spécifiquement utilisés par ActiveDirectory mis en surbrillance dans <embeddedLabel>gras</embeddedLabel>: </para>
          <code>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\pipe\lsass] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_np:CONTOSO-DC01[\PIPE\protected_storage] 
<codeFeaturedElement>UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_ip_tcp:CONTOSO-DC01[49156]</codeFeaturedElement> 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[49157] 
UUID: e3514235-4b06-11d1-ab04-00c04fc2dcd2 MS NT Directory DRS Interface
ncacn_http:CONTOSO-DC01[6004]</code>
          <para />
        </listItem>
        <listItem>
          <para>autres manières possible de résoudre cette erreur:</para>
          <list class="ordered">
            <listItem>
              <para>Vérifiez que le contrôleur de domaine source est démarré en mode normal et que le rôle de système d’exploitation et le contrôleur de domaine sur le contrôleur de domaine source ont entièrement démarré. </para>
            </listItem>
            <listItem>
              <para>Vérifiez que le Service de domaine ActiveDirectory est en cours d’exécution. Si le service est actuellement arrêté ou n’a pas été configuré avec des valeurs de démarrage par défaut, réinitialisez les valeurs de démarrage par défaut, redémarrez le contrôleur de domaine modifié, puis renouvelez l’opération. </para>
            </listItem>
            <listItem>
              <para>Vérifiez que l’état de service et la valeur de démarrage pour le service RPC et de recherche de RPC est correct pour la version du système d’exploitation du Client RPC (contrôleur de domaine de destination) et du serveur RPC (contrôleur de domaine source). Si le service est actuellement arrêté ou n’a pas été configuré avec des valeurs de démarrage par défaut, réinitialisez les valeurs de démarrage par défaut, redémarrez le contrôleur de domaine modifié, puis renouvelez l’opération. </para>
              <para>En outre, assurez-vous que le contexte de service correspond à des paramètres par défaut répertoriés dans le tableau suivant.</para>
              <table xmlns:caps="https://schemas.microsoft.com/build/caps/2013/11">
                <thead>
                  <tr>
                    <TD>
                      <para>Service</para>
                    </TD>
                    <TD>
                      <para>État par défaut (type de démarrage) dans Windows Server2003 et versions ultérieures </para>
                    </TD>
                    <TD>
                      <para>État par défaut (type de démarrage) dans Windows Server2000</para>
                    </TD>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <TD>
                      <para>Appel de procédure distante</para>
                    </TD>
                    <TD>
                      <para>Démarré (automatique)</para>
                    </TD>
                    <TD>
                      <para>Démarré (automatique)</para>
                    </TD>
                  </tr>
                  <tr>
                    <TD>
                      <para>Localisateur d’appel de procédure distante</para>
                    </TD>
                    <TD>
                      <para>NULL ou arrêté (manuel)</para>
                    </TD>
                    <TD>
                      <para>Démarré (automatique)</para>
                    </TD>
                  </tr>
                </tbody>
              </table>
            </listItem>
            <listItem>
              <para>Vérifiez que la taille de la plage de ports dynamique n’a pas été contrainte. La syntaxe de Windows Server2008 et Windows Server2008R2 NETSH pour énumérer la plage de ports RPC est indiquée ci-dessous:</para>
              <code>&gt;netsh int ipv4 show dynamicport tcp
&gt;netsh int ipv4 show dynamicport udp
&gt;netsh int ipv6 show dynamicport tcp
&gt;netsh int ipv6 show dynamicport udp</code>
            </listItem>
            <listItem>
              <para>vérifier que les définitions de port codée en dur définies dans la base de connaissances 224196 se situent dans la plage de ports dynamiques pour la version du système d’exploitation des contrôleurs de domaine source. </para>
              <para>Révision <externalLink><linkText>l’article 224196</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink> et assurez-vous que le port codée en dur se trouve dans la plage de ports éphémères pour la version du système d’exploitation du contrôleur de domaine source. </para>
            </listItem>
            <listItem>
              <para>Vérifiez que la clé ClientProtocols existe sous HKLM\Software\Microsoft\Rpc et contient les valeurs par 5 défaut suivantes:</para>
              <code>ncacn_http REG_SZ rpcrt4.dll
ncacn_ip_tcp REG_SZ rpcrt4.dll
<codeFeaturedElement>ncacn_nb_tcp REG_SZ rpcrt4.dll</codeFeaturedElement>
ncacn_np REG_SZ rpcrt4.dll
ncacn_ip_udp REG_SZ rpcrt4.dll</code>
            </listItem>
          </list>
        </listItem>
      </list>
    </content>
  </section>
  <section address="BKMK_MoreInfo">
    <title>Plus d’informations</title>
    <content>
      <para>
        <embeddedLabel>exemple d’un nom incorrect pour IP mappage à l’origine de l’erreur RPC1753 et-2146893022: le nom de principal cible est incorrect</embeddedLabel>
      </para>
      <para>le domaine contoso.com se compose de DC1 et DC2 avec adresse IP adresses x.x.1.1 et x.x.1.2. L’ordinateur hôte «A» / enregistrements «AAAA» pour DC2 sont inscrits correctement sur tous les serveurs DNS configurés pour DC1. En outre, le fichier HOSTS sur DC1 contient une entrée de mappage de nom d’hôte complet de DC2s à x.x.1.2 d’adresse IP. Plus tard, les modifications d’adresses IP de DC2 de X.X.1.2 X.X.1.3 et un nouvel ordinateur membre est joint au domaine avec x.x.1.2 d’adresse IP. La réplication ActiveDirectory tentatives déclenchement par le <ui>Répliquer maintenant</ui> commande dans le composant logiciel enfichable Services et Sites ActiveDirectory échoue avec l’erreur 1753, comme illustré dans la trace ci-dessous:</para>
      <code>F# SRC    DEST    Operation 
1 x.x.1.1 x.x.1.2 ARP:Request, x.x.1.1 asks for x.x.1.2
2 x.x.1.2 x.x.1.1 ARP:Response, x.x.1.2 at 00-13-72-28-C8-5E
3 x.x.1.1 x.x.1.2 TCP:Flags=......S., SrcPort=50206, DstPort=DCE endpoint resolution(135)
4 x.x.1.2 x.x.1.1 ARP:Request, x.x.1.2 asks for x.x.1.1
5 x.x.1.1 x.x.1.2 ARP:Response, x.x.1.1 at 00-15-5D-42-2E-00
6 x.x.1.2 x.x.1.1 TCP:Flags=...A..S., SrcPort=DCE endpoint resolution(135)
7 x.x.1.1 x.x.1.2 TCP:Flags=...A...., SrcPort=50206, DstPort=DCE endpoint resolution(135)
8 x.x.1.1 x.x.1.2 MSRPC:c/o Bind: UUID{E1AF8308-5D1F-11C9-91A4-08002B14A0FA} EPT(EPMP) 
9 x.x.1.2 x.x.1.1 MSRPC:c/o Bind Ack: Call=0x2 Assoc Grp=0x5E68 Xmit=0x16D0 Recv=0x16D0 
<codeFeaturedElement>10</codeFeaturedElement> x.x.1.1 x.x.1.2 EPM:Request: ept_map: NDR, DRSR(DRSR) {E3514235-4B06-11D1-AB04-00C04FC2DCD2} [DCE endpoint resolution(135)]
<codeFeaturedElement>11</codeFeaturedElement> x.x.1.2 x.x.1.1 EPM:Response: ept_map: 0x16C9A0D6 - EP_S_NOT_REGISTERED
</code>
      <para>au cadre <embeddedLabel>10</embeddedLabel>, la contrôleur de domaine de destination interroge le mappeur de point de terminaison de contrôleurs de domaine source sur le port 135 pour la classe de service de réplication ActiveDirectory UUID E351... </para>
      <para>Dans le cadre <embeddedLabel>11</embeddedLabel>, la source de contrôleur de domaine, dans ce cas, un ordinateur membre qui n’héberge pas encore le rôle de contrôleur de domaine et n’a donc pas inscrit le E351... UUID pour le service de réplication avec ses EPM local répond avec l’erreur symbolique EP_S_NOT_REGISTERED, ce qui correspond à l’erreur décimal 1753, hex 0x6d9 et convivial erreur «aucun point de terminaison plus ne sont disponibles à partir du mappeur de point de terminaison». </para>
      <para>Plus tard, l’ordinateur membre avec x.x.1.2 d’adresse IP obtient promu en tant que réplica «MayberryDC» dans le domaine contoso.com. Là encore, le <ui>Répliquer maintenant</ui> commande est utilisée pour déclencher la réplication, mais cette fois échoue avec le visuel erreur «le nom du principal cible est incorrect.» L’ordinateur dont la carte réseau est attribuée à l’adresse IP d’adresses x.x.1.2 <placeholder>est</placeholder> un contrôleur de domaine est actuellement démarré en mode normal et a inscrit le E351... le service de réplication UUID avec son mode protégé amélioré local, mais il ne possède pas l’identité de sécurité ou le nom de DC2 et ne pouvez pas déchiffrer la demande Kerberos de DC1 afin de la demande échoue désormais avec l’erreur «le nom du principal cible est incorrect». L’erreur correspond à une erreur décimal-2146893022 / hex erreur 0x80090322. </para>
      <para>Ces mappages to-IP hôte non valide - peuvent être dû à des entrées obsolètes dans hôte / fichiers lmhost, héberger un / des enregistrements AAAA dans DNS ou WINS. </para>
      <para>Résumé: cet exemple a échoué car un mappage hôte non valide - to-IP (dans le fichier hôtes dans ce cas) a provoqué la contrôleur de domaine de destination résoudre à une «source» contrôleur de domaine qui n’a pas les Services de domaine ActiveDirectory en cours d’exécution du service (ou même installés dans notre exemple), par conséquent, la réplication SPN n’a pas encore été inscrit et a renvoyé l’erreur 1753 un contrôleur de domaine source. Dans le second cas, un mappage hôte non valide - to-IP (dans le fichier d’hôte) a provoqué la destination du contrôleur de domaine pour se connecter à un contrôleur de domaine qui a enregistré l’E351... réplication SPN, mais cette source avait une identité autre nom d’hôte et de sécurité que le contrôleur de domaine source prévue afin des tentatives a échoué avec l’erreur-2146893022: le nom de principal cible est incorrect.</para>
    </content>
  </section>
  <relatedTopics>
    <externalLink>
      <linkText>Résolution des problèmes d’opérations ActiveDirectory qui échouent avec l’erreur 1753: aucun point de terminaison plus ne sont disponibles dans le mappeur de point de terminaison. </linkText>
      <linkUri>https://support.microsoft.com/kb/2089874</linkUri>
    </externalLink>
<externalLink><linkText>erreurs de mappeur de point de terminaison RPC de résolution des problèmes d’article 839880Ko à l’aide des outils de Support Windows Server2003 à partir du CD du produit</linkText><linkUri>https://support.microsoft.com/kb/839880</linkUri></externalLink>
<externalLink><linkText>Ko article vue d’ensemble du Service832017 et exigences de ports pour le système Windows Server réseau</linkText><linkUri>https://support.microsoft.com/kb/832017/</linkUri></externalLink>
<externalLink><linkText>trafic de réplication ActiveDirectory limitant Ko article 224196 et de client RPC du trafic à un port spécifique</linkText><linkUri>https://support.microsoft.com/kb/224196/</linkUri></externalLink>
<externalLink><linkText>Ko article 154596 comment configurer l’allocation de port dynamique RPC avec un pare-feu</linkText><linkUri>https://support.microsoft.com/kb/154596</linkUri></externalLink><externalLink><linkText>RPC fonctionnement</linkText><linkUri>https://msdn.microsoft.com/library/aa373935(VS.85).aspx</linkUri></externalLink><externalLink><linkText>Comment le prépare pour une connexion</linkText><linkUri>https://msdn.microsoft.com/library/aa373938(VS.85).aspx</linkUri></externalLink>
<externalLink><linkText>comment le Client établit une connexion</linkText><linkUri>https://msdn.microsoft.com/library/aa373937(VS.85).aspx</linkUri></externalLink><externalLink><linkText>enregistrement de l’Interface</linkText><linkUri>https://msdn.microsoft.com/library/aa375357(VS.85).aspx</linkUri></externalLink><externalLink><linkText>disposition le serveur sur le réseau</linkText><linkUri>https://msdn.microsoft.com/library/aa373974(VS.85).aspx</linkUri></externalLink><externalLink><linkText>les points de terminaison inscription</linkText><linkUri>https://msdn.microsoft.com/library/aa375255(VS.85).aspx</linkUri></externalLink><externalLink><linkText>à l’écoute pour les appels Client</linkText><linkUri>https://msdn.microsoft.com/library/aa373966(VS.85).aspx</linkUri></externalLink><externalLink><linkText>comment le Client établit une connexion</linkText><linkUri>https://msdn.microsoft.com/library/aa373937(VS.85).aspx</linkUri></externalLink><externalLink><linkText> ActiveDirectory de restreindre le trafic de réplication et client RPC du trafic à un port spécifique</linkText><linkUri>https://support.microsoft.com/kb/224196</linkUri></externalLink><externalLink><linkText>SPN pour un contrôleur de domaine cible dans ADDS</linkText><linkUri>https://msdn.microsoft.com/library/dd207688(PROT.13).aspx</linkUri></externalLink></relatedTopics>
</developerConceptualDocument>


