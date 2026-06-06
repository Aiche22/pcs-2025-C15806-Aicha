# Article 2 — L'apprentissage auto-supervisé en imagerie médicale (survey)

> Résumé de lecture · Cadre théorique du SSL pour le prototype
> Projet PCS 2025 · C15806 · Aicha Mohamed Ahid

---

## Référence

**Titre** : *Self-supervised learning methods and applications in medical imaging analysis: a survey*
**Auteurs** : Saeed Shurrab, Rehab Duwairi (Jordan University of Science and Technology)
**Revue** : PeerJ Computer Science — 2022
**DOI** : [10.7717/peerj-cs.1045](https://doi.org/10.7717/peerj-cs.1045)
**Pré-print** : [arXiv:2109.08685](https://arxiv.org/abs/2109.08685)

---

## De quoi parle l'article ?

C'est une **étude de synthèse (survey)** qui fait le tour des méthodes d'apprentissage auto-supervisé (SSL) appliquées à l'analyse d'images médicales. Là où l'article de Baur se concentre sur l'autoencodeur, celui-ci fournit le **cadre théorique général** du paradigme SSL dans lequel s'inscrit notre projet.

Les auteurs présentent ce travail comme la première synthèse sur les applications du SSL dans le domaine des images médicales, visant à combler le fossé entre la vision par ordinateur et l'imagerie médicale.

---

## Le problème que le SSL résout

L'article part d'un constat clé pour notre domaine : la rareté de jeux de données médicales annotées de haute qualité est un problème majeur qui freine les applications du machine learning en imagerie médicale.

Les réseaux de neurones profonds (CNN) ont énormément de paramètres à estimer, ce qui demande une grande quantité de données annotées pour un entraînement supervisé. Or, construire un jeu de données médical annoté de qualité suffisante est coûteux et chronophage.

Le SSL apporte une réponse : c'est un paradigme d'entraînement récent qui permet d'apprendre des représentations robustes **sans annotation humaine**, ce qui en fait une solution efficace à la pénurie de données médicales étiquetées.

---

## Comment fonctionne le SSL — les deux tâches

L'article structure clairement le SSL en deux étapes, exactement celles de notre pipeline :

1. **Tâche prétexte (pretext task)** — l'endroit où l'apprentissage auto-supervisé se produit réellement. Le modèle génère lui-même son signal de supervision à partir des données non étiquetées (dans notre cas : reconstruire des données propres à partir de données bruitées).

2. **Tâche aval (downstream task)** — où les représentations apprises sont réutilisées pour la vraie tâche, là où les données annotées sont limitées (classification, détection…).

Le point essentiel : du point de vue supervisé, la supervision dans le SSL est représentée par l'entraînement du modèle avec des **labels générés à partir des données elles-mêmes**.

---

## Pourquoi c'est pertinent pour notre projet

Notre Denoising Autoencoder est une instance concrète de ce cadre :

| Concept du survey | Notre prototype |
|-------------------|-----------------|
| Pénurie de données annotées | On évite d'annoter CN/MCI/AD |
| Tâche prétexte | Reconstruire x propre depuis x bruité |
| Labels auto-générés | Le bruit qu'on ajoute crée la supervision |
| Représentations réutilisables | L'encodeur pré-entraîné → fine-tuning futur |
| Tâche aval | Détection d'anomalie / classification |

L'article confirme que notre choix (le débruitage comme tâche prétexte) est une stratégie SSL reconnue, et situe notre travail dans une littérature scientifique active et bien établie.

---

## À retenir pour la présentation

Cet article justifie **pourquoi** on fait du SSL plutôt que du supervisé : en médecine, les étiquettes sont rares et chères, et le SSL permet d'apprendre des représentations utiles sans elles. C'est l'argument de fond du projet de thèse.

---

*Résumé de lecture · Projet PCS 2025 · Aicha Mohamed Ahid*
