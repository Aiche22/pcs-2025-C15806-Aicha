# Article 1 — Autoencodeurs pour la détection d'anomalies en IRM cérébrale

> Résumé de lecture · Fondement méthodologique du prototype DAE-SSL
> Projet PCS 2025 · C15806 · Aicha Mohamed Ahid

---

## Référence

**Titre** : *Autoencoders for unsupervised anomaly segmentation in brain MR images: A comparative study*
**Auteurs** : Christoph Baur, Stefan Denner, Benedikt Wiestler, Nassir Navab, Shadi Albarqouni
**Revue** : Medical Image Analysis, vol. 69, article 101952 — 2021
**DOI** : [10.1016/j.media.2020.101952](https://doi.org/10.1016/j.media.2020.101952)
**Code** : [github.com/StefanDenn3r/Unsupervised_Anomaly_Detection_Brain_MRI](https://github.com/StefanDenn3r/Unsupervised_Anomaly_Detection_Brain_MRI)

---

## De quoi parle l'article ?

C'est l'**article de référence** pour la détection non supervisée d'anomalies (UAD) en IRM cérébrale par autoencodeurs. Il compare de façon rigoureuse les principales méthodes existantes en les plaçant sur un pied d'égalité.

Le **principe central** que pose l'article est exactement celui de notre projet : apprendre un modèle de l'anatomie normale en apprenant à compresser et reconstruire des données saines, ce qui permet de repérer les structures anormales à partir des reconstructions ratées d'échantillons potentiellement anormaux.

Autrement dit : on entraîne le modèle uniquement sur des cerveaux sains. Face à un cerveau anormal, il échoue à le reconstruire correctement — et c'est cette **erreur de reconstruction** qui signale l'anomalie.

---

## Pourquoi cette approche est intéressante

L'article met en avant deux atouts majeurs de l'approche non supervisée :

1. Elle évite le besoin de grandes quantités de données segmentées manuellement — une nécessité et un point faible de l'apprentissage profond supervisé actuel.
2. Elle permet en théorie de détecter des pathologies arbitraires, même rares, que les approches supervisées pourraient ne pas trouver.

C'est précisément l'argument de notre projet : pas besoin d'étiquettes médicales coûteuses, et capacité à repérer tout ce qui s'écarte de la normale.

---

## L'apport méthodologique

Avant cet article, la comparaison des travaux existants était faussée parce qu'ils étaient évalués sur des datasets et des pathologies différents, à des résolutions d'image différentes et avec des architectures de complexité variable.

Les auteurs corrigent cela en imposant une architecture unique, une résolution unique et les mêmes datasets pour comparer équitablement toutes les méthodes. Ils explorent aussi deux questions pratiques importantes : combien de sujets sains sont nécessaires pour modéliser la normalité, et si les approches sont sensibles au changement de domaine (domain shift).

L'étude couvre une large famille de modèles : autoencodeurs simples, variationnels (VAE), adversariaux, et génératifs comme les VAE-GAN.

---

## Limite reconnue

Les auteurs restent honnêtes sur les performances. La détection non supervisée a le potentiel de détecter des lésions de type arbitraire, mais ses performances ne sont pas comparables à celles des méthodes entièrement ou semi-supervisées. Elle sert surtout à établir une détection grossière des zones suspectes et à fournir des candidats biomarqueurs d'imagerie.

C'est une nuance importante à garder pour notre projet : un détecteur d'anomalies non supervisé est un **outil de premier filtre**, pas un diagnostic définitif.

---

## Lien avec notre projet

| Élément de l'article | Notre prototype |
|----------------------|-----------------|
| Apprendre la normale sur sujets sains | On entraîne le DAE sur les cerveaux CN |
| Erreur de reconstruction = anomalie | Notre score = ‖x − x̂‖² par sujet |
| Pas d'annotations requises | SSL sans étiquettes en entraînement |
| Autoencodeur comme modèle de base | Notre Denoising Autoencoder (MLP) |

Cet article est le **socle conceptuel direct** de notre approche : il valide scientifiquement le principe « apprendre le normal pour repérer l'anormal » que nous appliquons sur les features FreeSurfer d'OASIS-3.

---

*Résumé de lecture · Projet PCS 2025 · Aicha Mohamed Ahid*
