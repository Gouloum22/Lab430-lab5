Dyaa Abou Arida

# Rapport Labo 5

**Question 1 : Quelle réponse obtenons-nous à la requête à POST /payments ? Illustrez votre réponse avec des captures d'écran/du terminal.**

On obtient un payment id.

![alt text](/images/image-q1.png)
<p align="center">1.1 Capture d'écran de la requête</p>

---------------------------------------
**Question 2 : Quel type d'information envoyons-nous dans la requête à POST payments/process/:id ? Est-ce que ce serait le même format si on communiquait avec un service SOA, par exemple ? Illustrez votre réponse avec des exemples et captures d'écran/terminal.**

![alt text](/images/image-q2.1.png)
<p align="center">2.1 Recuperer le order_id</p>

![alt text](/images/image-q2.2.png)
<p align="center">2.2 Recuperer le payment_id</p>

![alt text](/images/image-q2.3.png)
<p align="center">2.3 Traiter le paiement</p>

![alt text](/images/image-q2.4.png)
<p align="center">2.4 Verification du paiement</p>

Dans POST /payments/process/:id, on envoie un body JSON avec les infos de la commande, comme user_id, product_id et quantity. Le payment_id est dans l’URL, donc le service sait quel paiement traiter.

Avec un service SOA classique, ce ne serait pas forcément le même format : on utiliserait souvent du SOAP/XML, plus structuré et plus lourd que le JSON utilisé ici en REST/microservices.

---------------------------------------
**Question 3 : Quel résultat obtenons-nous de la requête à POST payments/process/:id?**

La requête retourne une réponse confirmant que le paiement a été traité avec succès.

![alt text](/images/image-q2.3.png)
<p align="center">3.1 Réponse de la requete process</p>

---------------------------------------
**Question 4 : Quelle méthode avez-vous dû modifier dans log430-labo05-payment et qu'avez-vous modifiée ? Justifiez avec un extrait de code.**

---------------------------------------
**Question 5 : À partir de combien de requêtes par minute observez-vous les erreurs 503 ? Justifiez avec des captures d'écran de Locust.**

---------------------------------------
**Question 6 : Que se passe-t-il dans le navigateur quand vous faites une requête avec un délai supérieur au timeout configuré (5 secondes) ? Quelle est l'importance du timeout dans une architecture de microservices ? Justifiez votre réponse avec des exemples pratiques.**




## CI/CD

Intégration continue avec les tests: 


Déploiement continue sur la VM:


