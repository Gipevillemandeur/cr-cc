# CR-CC — Compte Rendu Conseil de Classe

Application web gratuite pour les parents délégués, permettant de rédiger et envoyer le compte rendu du conseil de classe.

## Fonctionnalités

- 🔐 Accès sécurisé par classe avec code
- ✍️ Formulaire de saisie guidé
- 👁️ Aperçu PDF en temps réel
- 🔗 Relecture collaborative entre deux parents
- 📨 Envoi automatique par email
- 💾 Sauvegarde automatique
- 📱 Compatible mobile
- ⚙️ Panneau d'administration complet

## Installation

### 1. Créer un projet Firebase

1. Aller sur [firebase.google.com](https://firebase.google.com)
2. Créer un nouveau projet
3. Activer **Firestore Database** (mode test)
4. Créer une application Web et copier les clés de configuration

### 2. Configurer l'application

Dans `index.html` et `admin.html`, remplacer la section `firebaseConfig` par vos propres clés Firebase.

### 3. Déployer sur GitHub Pages

1. Créer un nouveau repo GitHub
2. Uploader tous les fichiers
3. Activer GitHub Pages (Settings → Pages → main branch)

### 4. Configurer via le panneau admin

1. Ouvrir `votre-site/admin.html`
2. Mot de passe par défaut : `admin123` (à changer immédiatement !)
3. Configurer l'établissement, les classes, les profs et la direction

### 5. Configurer l'envoi email (optionnel)

Pour l'envoi automatique des PDF par email, déployer le script `google-apps-script.js` sur Google Apps Script et renseigner l'URL dans le panneau admin.

## Structure des données Firebase

```
config/
  etablissement/    → Nom, académie, email, logos, mot de passe admin
  classes/          → Classes avec profs et codes d'accès
  direction/        → Membres de la direction
```

## Licence

Open source — libre d'utilisation et de modification.
