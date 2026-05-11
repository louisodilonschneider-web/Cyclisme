# 🚴 VéloTrack — Plateforme de Suivi Cycliste

> Pédalez. Progressez. Performez.

Application web de suivi d'entraînement cycliste, **multi-appareils** (téléphone, tablette, ordinateur), avec synchronisation cloud Firebase en temps réel.

---

## 📱 Accès multi-appareils

VéloTrack fonctionne sur **tous vos appareils** sans installation :

| Appareil | Accès | Navigation |
|----------|-------|------------|
| 📱 Téléphone | Ouvrir `index.html` dans le navigateur | Barre de navigation en bas + menu hamburger |
| 📟 Tablette | Ouvrir `index.html` dans le navigateur | Sidebar iconique réduite |
| 💻 Ordinateur | Ouvrir `index.html` dans le navigateur | Sidebar complète avec labels |

Pour synchroniser vos données entre appareils, créez un compte dans l'application (email + mot de passe). Vos données sont alors automatiquement synchronisées via Firebase Firestore.

---

## ☁️ Configuration Firebase (déjà intégrée)

Le projet Firebase `cyclisme-54026` est déjà configuré dans l'application.

| Paramètre | Valeur |
|-----------|--------|
| Nom du projet | cyclisme-54026 |
| Project ID | cyclisme-54026 |
| Auth Domain | cyclisme-54026.firebaseapp.com |
| Firestore | Base de données (default) |

### Services utilisés
- Firebase Authentication — connexion par email/mot de passe
- Cloud Firestore — base de données temps réel multi-appareils
- Persistance hors-ligne — fonctionne sans connexion, se resynchronise automatiquement

---

## 🔐 Règles Firestore à configurer

⚠️ ACTION REQUISE : les règles actuelles bloquent tout (`allow read, write: if false`).

### Dans la console Firebase → Firestore → Règles

Remplacez par :

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/{document=**} {
      allow read, write: if request.auth != null
                         && request.auth.uid == userId;
    }
  }
}
```

Chaque utilisateur ne peut accéder qu'à ses propres données.

**Étapes :**
1. console.firebase.google.com → projet cyclisme-54026
2. Firestore Database → onglet Règles
3. Remplacer le code → Publier

---

## 🔑 Activer l'authentification Email/Mot de passe

⚠️ ACTION REQUISE

1. Console Firebase → Authentication
2. Onglet Sign-in method
3. Email/Password → Activer → Enregistrer

---

## 🗄️ Structure Firestore

```
users/
  {uid}/
    workouts/   {id}/ → date, type, distance, elevation, speed, hr, rpe, notes…
    events/     {id}/ → name, start, end, days[], notes…
    injuries/   {id}/ → zone, severity, status, log[]…
    gpx/        {id}/ → name, date…
```

Note : les photos (base64) restent en localStorage uniquement — trop lourdes pour Firestore (limite 1 Mo/doc). Seules les données textuelles/numériques sont synchronisées.

---

## 🌐 Hébergement optionnel (Firebase Hosting)

Pour une URL fixe accessible partout :

```bash
npm install -g firebase-tools
firebase login
firebase init hosting
firebase deploy --only hosting
```

URL : https://cyclisme-54026.web.app

---

## 📊 Taux de préparation — 6 piliers, 100 points

| Pilier | Pts | Critère |
|--------|-----|---------|
| A — Endurance de base | 25 | Volume hebdo moyen ≥ 30% de la dist./jour de l'événement |
| B — Sortie longue | 20 | Meilleure sortie ≥ 80% de l'étape la plus longue |
| C — Dénivelé cumulé | 20 | Total D+ réalisé ≥ 1,5× le D+ total de l'événement |
| D — Capacité D+/sortie | 15 | Meilleure sortie D+ ≥ 75% de l'étape la plus exigeante |
| E — Régularité | 10 | ≥ 12 sorties qualitatives sur 4 semaines |
| F — Gestion de charge | 10 | Pas de surcharge ; réduction de volume si J-21 |

---

## 🏁 Événements programmés

### Le Bretzel de Diamant
- 2 jours : 63 km + 140 km — Alsace / Vosges
- GPX fourni

### La Méditerranéenne — Atlantique → Méditerranée

| J | Étape | Dist. | D+ | Cols |
|---|-------|-------|----|------|
| 1 | St-Jean-de-Luz → Larrau | 110 km | 2 600 m | St-Ignace · Burdincurutcheta · Bagargiak |
| 2 | Larrau → Argelès-Gazost | 122 km | 2 900 m | Marie Blanque · Aubisque · Soulor |
| 3 | Argelès-Gazost → Arreau | 80 km | 2 500 m | Tourmalet · Aspin |
| 4 | Arreau → Bagnères-de-Luchon | 48 km | 1 500 m | Azet · Peyresourde |
| 5 | Bagnères-de-Luchon → Seix | 94 km | 2 300 m | Menté · Portet d'Aspet · Port de la Core |
| 6 | Seix → Ax-les-Thermes | 100 km | 2 300 m | Col d'Agnes · Port de Lers · Corniches |
| 7 | Ax-les-Thermes → Prades | 90 km | 2 400 m | Pailhères 2001m · Moulis · Jau |
| 8 | Prades → Collioure | 98 km | 1 100 m | Palomère · Fourtou · Llauro |

Total : ~742 km — ~17 600 m D+

---

## ✅ Checklist de mise en route

- [ ] Ouvrir index.html dans un navigateur
- [ ] Firebase Auth : activer Email/Mot de passe dans la console
- [ ] Firestore Règles : appliquer les règles du README
- [ ] Créer un compte dans l'app (barre latérale)
- [ ] Point vert visible = synchronisation active
- [ ] Ouvrir sur un 2ème appareil → même compte → données identiques ✅

---

## 💾 Données & synchronisation

| Mode | Stockage | Multi-appareils |
|------|----------|-----------------|
| Sans compte | localStorage (local) | Non |
| Avec compte Firebase | localStorage + Firestore | Oui, temps réel |

- Point vert dans le header mobile = sync active
- Fonctionne hors-ligne, synchronise à la reconnexion
- Export/Import JSON disponible dans la sidebar

---

*VéloTrack — Pédalez. Progressez. Performez.*
