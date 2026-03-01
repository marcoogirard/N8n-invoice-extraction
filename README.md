# 🧾 Automatisation Comptable – Extraction de Factures PDF/Image vers Google Sheets

## 📌 Description

Ce workflow N8n automatise entièrement la saisie comptable à partir de factures au format PDF ou image (JPG, PNG, JPEG).  
Il scanne un dossier Google Drive toutes les heures, extrait les données clés grâce à l'IA (Google Gemini), détecte les doublons, et enregistre les informations directement dans un Google Sheets de comptabilité.

---

## ⚙️ Fonctionnement du workflow

```
[Schedule Trigger - toutes les heures]
         ↓
[Google Drive] → Scan du dossier de factures à traiter
         ↓
[Loop] → Traitement fichier par fichier
         ↓
[Switch] → Détection du type de fichier
    ├── PDF/PDF → Extraction texte native
    └── JPG/PNG/JPEG → Analyse visuelle par Gemini (OCR IA)
         ↓
[Google Gemini 2.5 Flash] → Extraction structurée des données
         ↓
[Vérification doublon dans Google Sheets]
    ├── Doublon détecté → Déplacement dans "Factures erreurs/doublons"
    └── Nouvelle facture → Enregistrement dans Google Sheets
              ↓
         [Déplacement dans "Factures traitées"]
              ↓
         [Renommage automatique du fichier]
         ex: 15/01/2025-Fournisseur-245.50€
```

---

## 🤖 Données extraites par l'IA

Pour chaque facture, le workflow extrait automatiquement :

| Champ | Description |
|---|---|
| `type_facture` | Débit (achat) ou Crédit (vente) |
| `entite_principale` | Nom du fournisseur ou du client |
| `date` | Date de la facture (JJ/MM/AAAA) |
| `numero_facture` | Numéro unique de la facture |
| `montant_ht` | Montant Hors Taxes |
| `montant_tva` | Montant de la TVA |
| `montant_ttc` | Montant Toutes Taxes Comprises |
| `devise` | Devise (ex: EUR) |

---

## 🛠️ Stack technique

- **N8n** – Orchestration du workflow
- **Google Gemini 2.5 Flash** – OCR et extraction intelligente des données (vision IA)
- **Google Drive API** – Lecture, déplacement et renommage des fichiers
- **Google Sheets API** – Saisie comptable automatisée
- **Service Account Google** – Authentification sécurisée sans intervention manuelle

---

## 📁 Structure Google Drive attendue

```
📂 Factures à traiter/         ← Déposer les factures ici
📂 Factures traitées/          ← Factures traitées avec succès (renommées)
📂 Factures erreurs_doublons/  ← Factures en erreur ou doublons détectés
```

---

## 📊 Structure Google Sheets attendue

Le Google Sheets doit contenir les colonnes suivantes :

`Date Comptabilité` | `Numéro facture` | `Date Facture` | `Entité` | `Valeur HT` | `Valeur TTC` | `TVA` | `Débit/Crédit` | `Catégorie` | `Sous-Catégorie` | `Autre commentaire`

---

## 🚀 Installation & Configuration

### Prérequis
- Une instance N8n (self-hosted ou cloud)
- Un compte Google Cloud avec les APIs Drive et Sheets activées
- Un Service Account Google avec accès aux dossiers Drive et au Google Sheets
- Une clé API Google Gemini (Google AI Studio)

### Étapes
1. Importer le fichier `Extraction_Factures_PDF_Image_vers_Excel.json` dans N8n
2. Reconfigurer les credentials :
   - **Google Gemini API** → Remplacer par votre clé API
   - **Google Service Account** → Remplacer par votre compte de service
3. Mettre à jour les IDs Google Drive :
   - ID du dossier source (factures à traiter)
   - ID du dossier "Factures traitées"
   - ID du dossier "Factures erreurs/doublons"
4. Mettre à jour l'ID du Google Sheets cible
5. Activer le workflow ✅

---

## 💡 Points forts

- **Zéro saisie manuelle** – Les factures sont traitées automatiquement toutes les heures
- **Multi-format** – Gère les PDF natifs et les images scannées (OCR IA)
- **Détection des doublons** – Évite les doubles saisies comptables
- **Renommage automatique** – Les fichiers sont renommés au format `date-entité-montant`
- **Gestion des erreurs** – Les fichiers problématiques sont isolés automatiquement

---

## 📄 Licence

Projet personnel – Usage libre pour inspiration ou adaptation.
