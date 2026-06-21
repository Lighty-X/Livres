# 📚 Biblio — Ma bibliothèque partagée

PWA de gestion de bibliothèque avec scanner ISBN, hébergée sur Supabase + Vercel/Netlify.

---

## 🚀 Mise en place (15 min)

### 1. Créer le projet Supabase (gratuit)

1. Va sur [supabase.com](https://supabase.com) → **New project**
2. Note ton **URL** et ta **clé anon** (Settings → API)
3. Dans l'éditeur SQL de Supabase, colle et exécute ce script :

```sql
CREATE TABLE books (
  id          UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  isbn        TEXT,
  title       TEXT,
  author      TEXT,
  publisher   TEXT,
  year        TEXT,
  status      TEXT DEFAULT 'à lire',
  notes       TEXT,
  cover_url   TEXT,
  added_by    TEXT,
  created_at  TIMESTAMPTZ DEFAULT now()
);

-- Autoriser la lecture/écriture sans authentification
-- (protège via URL+clé anon partagée entre tes téléphones)
ALTER TABLE books ENABLE ROW LEVEL SECURITY;

CREATE POLICY "Accès libre" ON books
  FOR ALL USING (true) WITH CHECK (true);
```

### 2. Héberger l'app (gratuit)

**Option A — Vercel (recommandé)**
```bash
npm i -g vercel
cd bookshelf/
vercel --prod
```
→ Tu obtiens une URL `https://biblio-xxx.vercel.app`

**Option B — Netlify**
- Glisse le dossier `bookshelf/` sur [app.netlify.com/drop](https://app.netlify.com/drop)
- URL immédiate, zéro config

**Option C — GitHub Pages**
```bash
git init && git add . && git commit -m "init"
gh repo create biblio --public --push
# Activer Pages dans les Settings du repo
```

### 3. Premier lancement

1. Ouvre l'URL sur ton téléphone
2. Saisis ton URL Supabase + clé anon + ton prénom
3. Clique **Connecter** → c'est mémorisé sur cet appareil
4. Même procédure sur chaque téléphone

> Les configs sont stockées en `localStorage` de chaque appareil.
> Tous les téléphones partagent la même base Supabase → liste toujours à jour.

---

## 📱 Installer comme app (PWA)

**iOS** : Safari → Partager → "Sur l'écran d'accueil"
**Android** : Chrome → menu ⋮ → "Installer l'application"

---

## 🔍 Scanner ISBN

- **Caméra** : onglet "Ajouter" → Scanner avec la caméra → cadre le code-barres
- **Manuel** : saisis directement les 13 chiffres ISBN
- Les métadonnées (titre, auteur, couverture) viennent d'**Open Library** (gratuit, sans clé)

---

## 📂 Structure

```
bookshelf/
├── index.html    ← toute l'app (HTML + CSS + JS)
└── manifest.json ← rend l'app installable (PWA)
```

---

## 🛠 Personnalisation facile

| Ce que tu veux changer | Où |
|---|---|
| Statuts de lecture | Cherche `à lire`, `en cours`, `lu` |
| Champs du formulaire | Section `<!-- ADD FORM -->` |
| Couleurs | Variables CSS `:root { --accent: ... }` |
| Langue de l'interface | Textes en clair dans le HTML |

---

## 🔒 Sécurité

La clé `anon` de Supabase est publique par design — elle donne accès à ta table via les politiques RLS. Pour une vraie auth multi-utilisateurs, active Supabase Auth et adapte la politique RLS.
