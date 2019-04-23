Il existe de nombreuses façons de configurer la résolution de nom pour le domaine de fabric. Un moyen simple consiste à configurer une zone DNS redirecteur conditionnel pour l’infrastructure. Pour configurer cette zone, exécutez les commandes suivantes dans une console Windows PowerShell avec élévation de privilèges sur un serveur DNS de fabric. Remplacez les noms et adresses dans la syntaxe Windows PowerShell ci-dessous autant que nécessaire pour votre environnement. Ajouter des serveurs maîtres pour les nœuds de SGH supplémentaires.

```
Add-DnsServerConditionalForwarderZone -Name 'bastion.local' -ReplicationScope "Forest" -MasterServers <IP addresses of HGS server>
```

<!-- Appears in guarded-fabric-configuring-fabric-dns-ad.md and guarded-fabric-configuring-fabric-dns.md and set-up-hgs-for-always-encrypted-in-sql-server.md
-->    