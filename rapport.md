Dyaa Abou Arida

# Rapport Labo 5

**Question 1 : Quelle réponse obtenons-nous à la requête à POST /payments ? Illustrez votre réponse avec des captures d'écran/du terminal.**

On obtient un payment id.

![alt text](/images/image-q1.png)

<p align="center">1.1 Capture d'écran de la requête</p>

---

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

---

**Question 3 : Quel résultat obtenons-nous de la requête à POST payments/process/:id?**

La requête retourne une réponse confirmant que le paiement a été traité avec succès.

![alt text](/images/image-q2.3.png)

<p align="center">3.1 Réponse de la requete process</p>

---

**Question 4 : Quelle méthode avez-vous dû modifier dans log430-labo05-payment et qu'avez-vous modifiée ? Justifiez avec un extrait de code.**

J’ai modifier la méthode update_status_to_paid(payment_id) dans le microservice log430-labo5-payment, car c’est elle qui traite le paiement et met son statut à payé.

Code modifié:

    def update_status_to_paid(payment_id: int):
        """Update payment status to paid in MySQL"""
        if not payment_id:
            raise ValueError("Vous devez indiquer un ID de paiement.")

        session = get_sqlalchemy_session()

        try:
            # Find the payment by ID
            payment = session.query(Payment).filter(Payment.id == payment_id).first()

            if not payment:
                raise ValueError(f"Aucun paiement trouvé avec l'ID {payment_id}")

            # Update the payment status
            payment.is_paid = True
            session.commit()

            response = requests.put(
                "http://api-gateway:8080/store-manager-api/orders",
                json={
                    "order_id": payment.order_id,
                    "is_paid": True
                },
                headers={"Content-Type": "application/json"},
                timeout=5
            )

            response.raise_for_status()

            return {
                "payment_id": payment_id,
                "order_id": payment.order_id,
                "is_paid": True
            }

---

**Question 5 : À partir de combien de requêtes par minute observez-vous les erreurs 503 ? Justifiez avec des captures d'écran de Locust.**

Les erreurs 503 apparaissent lorsque le nombre de requêtes dépasse la limite configurée dans KrakenD, soit environ 200 requêtes par minute pour POST /store-manager-api/orders. Dans Locust, avec 100 utilisateurs, j’ai observé des erreurs 503 massives : 3613 échecs sur 4041 requêtes, avec environ 50 RPS, soit plus de 3000 requêtes/minute. Cela confirme que le rate limiting de KrakenD bloque les requêtes dépassant la limite.

![alt text](/images/image-q5.1.png)

<p align="center">5.1 Nombre de requêtes</p>

![alt text](/images/image-q5.2.png)

<p align="center">5.2 Capture des charts locust</p>

![alt text](/images/image-q5.3.png)

<p align="center">5.3 Erreur 503 Locust</p>

---

**Question 6 : Que se passe-t-il dans le navigateur quand vous faites une requête avec un délai supérieur au timeout configuré (5 secondes) ? Quelle est l'importance du timeout dans une architecture de microservices ? Justifiez votre réponse avec des exemples pratiques.**

## CI/CD

Intégration continue avec les tests:

Déploiement continue sur la VM:
