# 🧠 Détection d'anomalies cérébrales par IRM — DAE-SSL

**Projet Python & Calcul Scientifique — PCS 2025 — C15806**
**Aicha Mohamed Ahid** · Doctorat Informatique

> Prototype d'un framework d'apprentissage auto-supervisé (SSL) basé sur un autoencodeur débruiteur (DAE), pour détecter automatiquement des anomalies cérébrales à partir de données de neuroimagerie — **sans étiquettes médicales**.

---

## 🎯 Le projet en une phrase

Un programme Python qui **apprend à quoi ressemble un cerveau sain**, puis repère les cerveaux qui s'en écartent — en se basant sur l'erreur de reconstruction d'un autoencodeur, sans qu'on lui fournisse jamais les diagnostics pendant l'entraînement.

---

## 📌 Pourquoi ce projet ?

- La maladie d'Alzheimer est souvent détectée **trop tard**, quand les lésions sont avancées.
- Faire annoter chaque IRM par un médecin est **coûteux et lent**.
- L'IRM révèle des **changements structurels mesurables** avant les symptômes.

👉 **Objectif** : repérer un cerveau « différent de la normale » de façon **automatique et non supervisée**.

---

## 🗂️ Données : OASIS-3

Base publique d'IRM cérébrales. Chaque sujet est résumé par ~90 mesures **FreeSurfer** (volumes de régions, épaisseurs corticales) rangées dans un tableau.

| Groupe | Signification | Sujets | Rôle |
|--------|--------------|--------|------|
| **CN** | Cognitively Normal | ~400 | la « normale » apprise |
| **MCI** | Mild Cognitive Impairment | ~200 | stade intermédiaire |
| **AD** | Alzheimer's Disease | ~150 | cible de détection |

> ⚠️ Les étiquettes (CN/MCI/AD) ne sont **jamais utilisées à l'entraînement** — seulement pour évaluer le modèle à la fin.

---

## 🧩 L'idée : le Denoising Autoencoder (DAE)

Un **autoencodeur** compresse une donnée puis la reconstruit. Le **denoising** (débruitage) consiste à abîmer volontairement l'entrée, puis à demander au modèle de retrouver la version propre — ce qui le force à comprendre la structure des données au lieu de recopier.

```
Entrée x (90 mesures)
    ↓ corrupt(x) — masking 30% + bruit gaussien N(0, 0.1)
Encodeur : 90 → 256 → 128 → 64   (espace latent)
Décodeur : 64 → 128 → 256 → 90   (reconstruction x̂)
    ↓
Loss : MSE(x̂, x_propre)          ← compare vs original, pas vs bruité
    ↓
Score d'anomalie : ‖x − x̂‖²      ← élevé pour AD, bas pour CN
```

**Analogie** : comme un mécanicien qui connaît le son d'un bon moteur et repère instantanément un bruit anormal — même une panne jamais entendue.

---

## ⚙️ Workflow du projet

```
1. Charger les données  →  2. Explorer & nettoyer  →  3. Normaliser (StandardScaler)
        →  4. Construire le modèle  →  5. Entraîner (SSL)
        →  6. Calculer le score d'anomalie  →  7. Visualiser (loss, scores, t-SNE)
```

Tout est implémenté dans **un seul notebook Google Colab** (~350 lignes Python), exécutable sur GPU T4 gratuit.

---

## 🛠️ Outils utilisés

| Outil | Rôle |
|-------|------|
| **PyTorch** | Construction et entraînement du modèle (autoencodeur) |
| **pandas** | Chargement et nettoyage du tableau de données CSV |
| **NumPy** | Calculs numériques sur les tableaux de features |
| **scikit-learn** | Normalisation (StandardScaler), split train/test, AUC |
| **Matplotlib / seaborn** | Visualisation : courbes de loss, distributions, t-SNE |

---

## 📊 Résultats préliminaires (données d'exemple)

> Le pipeline fonctionne de bout en bout. Ces chiffres proviennent de **données synthétiques** servant à valider le code ; la validation sur le vrai OASIS-3 est la prochaine étape.

| Métrique | Valeur | Lecture |
|----------|--------|---------|
| AUC-ROC | ~0.71 | 0.5 = hasard, 1.0 = parfait |
| Score moyen CN | 0.08 | cerveaux sains |
| Score moyen MCI | 0.19 | intermédiaires |
| Score moyen AD | 0.31 | malades |

✅ Le score d'anomalie **monte bien des sains vers les malades** — exactement le comportement attendu, alors que le modèle n'a jamais vu les diagnostics.

---

## 🔭 Prochaines étapes

1. Tester sur le **vrai dataset OASIS-3** et mesurer l'AUC réelle
2. Ajouter les **covariables** (âge, sexe, volume intracrânien) pour corriger les biais → approche CVAE
3. **Fine-tuning** supervisé CN/MCI/AD avec l'encodeur pré-entraîné gelé
4. **Transfert** vers ADNI pour vérifier la généralisation
5. **Interprétabilité** : identifier les régions qui pèsent le plus (SHAP)

---

## 📁 Structure du dépôt

```
pcs-2025-C15806-Aicha/
├── PRESENTATION.md                      # ce fichier
├── README.md
├── requirements.txt
├── notebook/
│   └── DAE_SSL_BrainAnomaly_OASIS3.ipynb
├── docs/
│   ├── article_1_Baur_2021.md           # autoencodeurs pour anomalies cérébrales
│   ├── article_2_Shurrab_2022.md        # survey SSL en imagerie médicale
│   └── resume_livre.md
└── presentation/
    └── Presentation_simplifiee_C15806.pptx
```

---

## 📚 Cadre de la thèse

Ce prototype s'inscrit dans le projet de thèse :
**« A Deep Self-Supervised Learning Framework Based on Autoencoders for Brain Health Assessment from Neuroimaging Data »**

Deux articles fondateurs sont résumés dans `docs/` :
- **Baur et al. (2021)** — autoencodeurs pour la détection non supervisée d'anomalies en IRM cérébrale (le socle méthodologique)
- **Shurrab & Duwairi (2022)** — survey de l'apprentissage auto-supervisé en imagerie médicale (le cadre théorique du SSL)

---

*Projet PCS 2025 · Aicha Mohamed Ahid · Superviseure : Dr. Salima Hassairi*
