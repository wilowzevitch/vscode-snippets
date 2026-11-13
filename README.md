# Snippets VSCode

Collection personnelle de snippets VSCode, organisée **un fichier par techno**.

| Fichier | Techno | Snippets |
|---|---|---|
| [`dolibarr.code-snippets`](dolibarr.code-snippets) | Dolibarr (PHP, jQuery) | 15 |
| [`react.code-snippets`](react.code-snippets) | React (composants, hooks, tests) | 10 |
| [`redux.code-snippets`](redux.code-snippets) | Redux classique et Redux Toolkit | 6 |
| [`wordpress.code-snippets`](wordpress.code-snippets) | WordPress (thèmes, plugins, CPT, REST) | 21 |

---

## Installation

### Globale (tous les projets)

1. `Ctrl/Cmd + Maj + P` → **Snippets: Configure Snippets**
2. Choisir **New Global Snippets file…**, le nommer (par exemple `dolibarr`) puis coller le contenu du fichier correspondant.

Ou copier directement les fichiers `.code-snippets` dans le dossier des snippets utilisateur :

| OS | Chemin |
|---|---|
| Linux | `~/.config/Code/User/snippets/` |
| macOS | `~/Library/Application Support/Code/User/snippets/` |
| Windows | `%APPDATA%\Code\User\snippets\` |

Les snippets utilisateur sont inclus dans **Settings Sync**.

### Par projet

Copier le fichier voulu dans le dossier `.vscode/` du dépôt. Les snippets ne sont alors disponibles que dans ce projet, ce qui est pratique pour partager ceux de Dolibarr avec l'équipe.

---

## Utilisation

Taper le préfixe (par exemple `dol:ajax`) puis `Tab` ou `Entrée`.

- `Tab` passe d'un placeholder au suivant.
- Un même placeholder répété (par exemple `theme_name`) est **synchronisé** : on le saisit une fois.
- Certains placeholders sont des **menus déroulants** (`GET|POST|PUT|DELETE`, `error|warning`…).
- Le curseur final (`$0`) est placé à l'endroit où écrire le code métier.
- Si les suggestions n'apparaissent pas, forcer avec `Ctrl + Espace`.

Chaque snippet est limité à un langage (`scope`) pour ne pas polluer les autres types de fichiers.

---

## Convention de nommage

Les préfixes suivent le format **`techno:nom`**.

| Techno | Préfixe |
|---|---|
| Dolibarr | `dol:` |
| React | `react:` |
| Redux | `redux:` |
| WordPress | `wp:` |

Les noms composés sont en `snake_case` (`dol:module_setup_page`, `wp:theme_section`). Les snippets React reprennent la camelCase historique (`react:classComponent`).

---

## Dolibarr

Écrits pour **Dolibarr 19 ou plus**. `GETPOSTINT` n'existe pas en v18 : tant que l'instance n'est pas migrée, le remplacer par `GETPOST('id', 'int')` dans `dol:ajax` et `dol:page`.

| Préfixe | Description |
|---|---|
| `dol:page` | Squelette de page de module (chargement de `main.inc.php`, droits, actions, vue) |
| `dol:module_setup_page` | Page de configuration d'un module (en-tête GPL, onglets, page « À propos ») |
| `dol:ajax` | Endpoint AJAX avec réponse JSON `status: ok\|error` |
| `dol:hook` | Classe de hook `actions_xxx.class.php` |
| `dol:sql_select` | Construction d'un `SELECT` (entité, cast `int`, `$db->escape()`) |
| `dol:query` | Exécution de `$sql` et récupération des lignes (contexte API REST : `RestException`) |
| `dol:transaction` | Transaction `begin` / `commit` / `rollback` |
| `dol:migration` | Fichier de migration CLI avec `up()` / `down()` |
| `dol:extrafield` | Ajout d'un extrafield (à utiliser dans une migration) |
| `dol:popup_jquery` | Popup jQuery UI contenant un formulaire POST |
| `dol:formconfirm` | Boîte de confirmation native (`Form::formconfirm`) |
| `dol:ajax_js` | Appel `$.ajax` correspondant à `dol:ajax` |
| `dol:toast_js` | Notification JS (`$.jnotify`) |
| `dol:toast_php` | Notification PHP (`setEventMessages`) |
| `dol:const_set` | Sauvegarde d'une constante de module (`dolibarr_set_const`) |

**À vérifier selon la version** : la signature de `ExtraFields::addExtraField` (nombreux arguments positionnels) a évolué entre les versions.

---

## React

Écrits pour **React 16.8 ou plus** (hooks). L'import `import React from 'react'` est conservé partout, car il est obligatoire avant le nouveau JSX transform (React 16.14 / 17). Aucun snippet n'utilise `propTypes` ni `defaultProps` : les valeurs par défaut passent par la destructuration.

| Préfixe | Description |
|---|---|
| `react:component` | Composant fonction minimal |
| `react:fc` | Composant fonction avec SCSS, `useState` et `useEffect` |
| `react:fcRedux` | Composant fonction avec `useSelector` / `useDispatch` (react-redux ≥ 7.1) |
| `react:classComponent` | Composant classe connecté à Redux (`connect`) |
| `react:classComponentPlain` | Composant classe sans Redux |
| `react:errorBoundary` | Error boundary (seul cas où une classe reste obligatoire) |
| `react:hookFetch` | Hook de fetch JSON avec `loading`, `error` et abandon au démontage |
| `react:hookDebounce` | Hook de debounce d'une valeur |
| `react:context` | Context, Provider mémoïsé et hook protégé |
| `react:test` | Squelette Testing Library |

**Prérequis tests** : `@testing-library/react` en version 12 ou moins pour React 16/17. `toBeInTheDocument` nécessite `@testing-library/jest-dom`.

---

## Redux

| Préfixe | Description |
|---|---|
| `redux:actions` | Constante de type et action creator (classique) |
| `redux:reducer` | Reducer avec `switch` et état initial (classique) |
| `redux:thunk` | Action asynchrone `REQUEST` / `SUCCESS` / `FAILURE` (nécessite `redux-thunk`) |
| `redux:selector` | Selector mémoïsé (`reselect`) |
| `redux:slice` | `createSlice` (Redux Toolkit) |
| `redux:asyncThunk` | `createAsyncThunk` avec les `extraReducers` en commentaire (Redux Toolkit) |

**Compatibilité** : les versions récentes de react-redux (9.x, à ma connaissance) exigent React 18. Sur React 16, rester en 7.x ou 8.x et vérifier la version de Redux Toolkit compatible.

---

## WordPress

Les snippets sont prévus pour du PHP **procédural** avec des fonctions préfixées par `theme_name_`. Les clés de CPT, les noms de fonctions et de variables doivent donc rester en `snake_case`, sans tiret.

### Thème

| Préfixe | Description |
|---|---|
| `wp:theme_setup` | `after_setup_theme` (supports, menus, textdomain) |
| `wp:enqueue` | Styles et scripts versionnés par `filemtime` (`'strategy'` : WP ≥ 6.3) |
| `wp:nav_menu` | Affichage d'un menu enregistré |

### Customizer

| Préfixe | Description |
|---|---|
| `wp:theme_customizer` | Fonction `customize_register` |
| `wp:theme_panel` | Panneau du Customizer |
| `wp:theme_section` | Section + champs générés par boucle |
| `wp:theme_control_image` | Contrôle média (stocke un ID de pièce jointe) |
| `wp:theme_control_color` | Sélecteur de couleur |
| `wp:mod` | Affichage échappé d'une valeur (`get_theme_mod`) |

### Contenu

| Préfixe | Description |
|---|---|
| `wp:custompost` | Custom Post Type |
| `wp:taxonomy` | Taxonomie personnalisée |
| `wp:metabox` | Meta box (nonce, droits, sanitization) |
| `wp:shortcode` | Shortcode avec attributs |
| `wp:query` | Boucle `WP_Query` |

### Backend

| Préfixe | Description |
|---|---|
| `wp:plugin` | En-tête de plugin, constantes, hooks d'activation |
| `wp:ajax` | Handler `admin-ajax` (nonce, JSON) |
| `wp:ajax_js` | Appel `fetch` correspondant à `wp:ajax` |
| `wp:rest_route` | Route REST (`permission_callback` obligatoire depuis WP 5.5) |
| `wp:settings_page` | Page de réglages via la Settings API |
| `wp:cron` | Événement planifié WP-Cron |
| `wp:transient` | Cache avec transient |

**Notes**

- Penser à réenregistrer les permaliens (Réglages → Permaliens) après avoir ajouté ou modifié un CPT.
- Les labels sont en français en dur, sans `__()`.
- `wp:theme_section` utilise `wp_kses_post` comme sanitize callback : sortir ces valeurs avec `wp_kses_post()` (c'est ce que fait `wp:mod`).

---

## Ajouter ou modifier un snippet

Les fichiers `.code-snippets` sont du **JSON avec commentaires** (JSONC) : les `// …` sont autorisés.

```jsonc
"Nom affiché": {
    "scope": "php",
    "prefix": "techno:nom",
    "body": [
        "ligne 1",
        "\t${1:placeholder}",
        "\\$variable_php"
    ],
    "description": "Techno - Description courte"
}
```

Règles à respecter :

- **`$` littéral** : à échapper en `\\$` dans le JSON (soit `\$` pour la syntaxe de snippet). Un simple `\$` est un échappement JSON invalide.
- **Backslash littéral** (balises Doxygen `\file`, `\brief`) : à écrire `\\file`.
- **Placeholders** : `${1:nom}` ; un même numéro répété est synchronisé. Choix : `${2|a,b,c|}`. Curseur final : `$0`.
- **Pas de `}` ni de `$`** dans la valeur par défaut d'un placeholder sans l'échapper.
- **`scope`** : toujours renseigné et sans espace (`"php,html"`).
- **Pas de contenu spécifique à un projet** : pas de nom de module, de client ou d'email en dur dans le corps. Utiliser un placeholder.
- **Valeurs par défaut** : un placeholder ne doit jamais, tel quel, produire du code invalide.

### Vérifier un fichier

```bash
python3 - << 'EOF'
import json, re, sys
path = "wordpress.code-snippets"
txt = "\n".join(l for l in open(path, encoding="utf-8") if not l.strip().startswith("//"))
data = json.loads(txt)
for name, s in data.items():
    for line in s["body"]:
        if re.search(r'(?<!\\)\$(?!\{|0)', line):
            print("$ non échappé :", name, "→", line)
print(len(data), "snippets OK")
EOF
```

Ce script détecte les erreurs de JSON et les `$` non échappés. Il ne remplace pas un test dans VSCode.
