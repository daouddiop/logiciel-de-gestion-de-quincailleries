# Quincaillerie Pro

Gestion de quincaillerie — **une seule interface** (plus de Django Admin séparé) + fenêtre native Windows.

## Rôles

| Rôle | Accès |
|------|--------|
| **Administrateur** | Tout + Administration (users, taxes, unités, paiements, catégories) |
| **Vendeur** | Caisse, ventes, produits (lecture), clients |
| **Comptable** | Clients, ventes (lecture), paramètres entreprise |
| **Gestionnaire stock** | Produits, mouvements stock, fournisseurs |

## Windows — installation

1. Python **3.11, 3.12 ou 3.13** (+ PATH)
2. Double-clic **`install.bat`**
3. Double-clic **`start_desktop.bat`** → fenêtre native

Comptes : `admin` / `admin123` · `vendeur` / `demo123` · `stock` / `demo123` · `comptable` / `demo123`

## Build .exe + installateur

```bat
install.bat
build_windows.bat
```

Puis ouvrez **`installer.iss`** avec [Inno Setup 6](https://jrsoftware.org/isinfo.php) → Compile  
→ `installer_output\QuincailleriePro_Setup_1.0.0.exe`

## Lancement

| Script | Effet |
|--------|--------|
| `start_desktop.bat` | Fenêtre native (pywebview) |
| `start_server.bat` | Navigateur localhost:8000 |

## Modules (interface unique)

- Tableau de bord (KPIs, graphiques)
- Caisse POS + PDF ticket/facture
- Produits / stock / export Excel
- Clients / Fournisseurs
- **Administration** intégrée (users, catégories, TVA, unités, modes de paiement)
- Paramètres entreprise

## Stack

Django 5 · Bootstrap 5 · Chart.js · ReportLab · openpyxl · pywebview · SQLite


## Logs d'erreur

| Fichier | Contenu |
|---------|---------|
| `logs/error.log` | Erreurs applicatives et 500 |
| `logs/app.log` | Activité générale |
| `logs/security.log` | Alertes sécurité |
| `server.log` | Sortie du serveur (mode bureau) |

Consultation : **Administration → Logs d'erreur** (admin uniquement).  
Rotation automatique (2 Mo, plusieurs sauvegardes).


## Données persistantes

Les données ne sont **pas** dans le dossier d'installation :

| OS | Emplacement |
|----|-------------|
| Windows | `%LOCALAPPDATA%\QuincailleriePro\` |
| Linux | `~/.local/share/QuincailleriePro/` |
| macOS | `~/Library/Application Support/QuincailleriePro/` |

Contenu : `db.sqlite3`, `media/`, `logs/`, `backups/`.  
**Désinstaller l'app ne supprime pas ces fichiers.**

## Mode hors ligne

Bootstrap, icônes et Chart.js sont **embarqués** dans `static/`.  
Aucune connexion Internet n'est requise pour caisse, stock, factures PDF, etc.

## Erreurs

- Messages colorés (vert / orange / rouge) dans l'interface  
- Pages 403 / 404 / 500 dédiées  
- Journal dans `logs/error.log` + écran Administration → Logs  


## Mises à jour automatiques

1. Configurer l'URL du manifeste JSON :
   - variable d'environnement `UPDATE_MANIFEST_URL`
   - ou dans `config/settings.py`

Exemple de manifeste (`update_server/manifest.example.json`) :

```json
{
  "version": "1.2.0",
  "url": "https://votredomaine.com/QuincailleriePro_Setup_1.2.0.exe",
  "changelog": "Notes de version",
  "mandatory": false,
  "sha256": ""
}
```

2. Menu **Mises à jour** (admin) :
   - Vérifier → barre de progression au téléchargement → bouton **Installer**
