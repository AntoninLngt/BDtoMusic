# BD TO MUSIC

BD TO MUSIC est un projet de génération d’ambiances sonores à partir de planches de bande dessinée, basé sur l’apprentissage multimodal et la génération dans des espaces latents.

Notebook principal :  
[Voir mon Notebook](./Projet_BD.ipynb)

---

## Objectif

Apprendre une correspondance entre les caractéristiques visuelles et sémantiques des planches de BD et les caractéristiques audio (rythme, énergie, timbre, ambiance), afin de générer un son cohérent à partir d’une image.

---

## Principe général

Le système repose sur trois modèles :

- Un VAE entraîné pour encoder / décoder les planches de BD
- Un VAE entraîné pour encoder / décoder les audios (via spectrogrammes)
- Un GAN conditionné par l’espace latent BD, entraîné pour générer l’espace latent audio

Pipeline final :

Planche de BD  
→ encodeur VAE BD  
→ GAN  
→ décodeur VAE audio  
→ audio généré

---

## Architecture

### Étape 1 : Apprentissage des représentations

BD :
- Entraînement d’un VAE (beta-VAE ou VAE hiérarchique)
- Extraction de features : structure des cases, texte, couleurs

Audio :
- Transformation des fichiers .wav en spectrogrammes
- Entraînement d’un VAE audio
- Extraction de features : rythme, énergie, descripteurs spectraux
- Embeddings audio via CLAP

Les features audio sont apprises directement depuis l’espace latent pour éviter le décodage pendant l’entraînement du GAN.

### Étape 2 : Alignement BD ↔ Audio

- Utilisation d’un CycleGAN pour apprendre le mapping entre espaces latents BD et audio
- Contraintes utilisées :
  - adversarial
  - cycle consistency
  - identity
  - contraintes structurelles entre features BD et audio
  - contraintes sémantiques via CLAP

---

## Datasets

BD :
- eBDtheque

Audio :
- FSD50K

---

## Outils et concepts

- VAEs (beta-VAE, hiérarchiques)
- GAN conditionné / CycleGAN
- Extraction de features audio (librosa, STFT, MFCC)
- Détection de structure de BD (YOLO)
- Alignement multimodal via CLIP / CLAP

---

## Statut

Projet de recherche exploratoire  
Datasets non appariés  
Génération d’ambiances sonores
