# Configuration clavier Zed — hiérarchie de référence

Document de reconstruction. Si une mise à jour de Zed casse quelque chose, ce fichier
suffit à régénérer `keymap.json` et `settings.json` de zéro.

**Base** : pas de base_keymap (défaut) + `vim_mode: true`.
Le socle (enter, tab, backspace, menus, terminal, accords `ctrl-k ctrl-*`) vient du
keymap de base — ne jamais passer en `"None"`, ce serait à réécrire entièrement.

---

## 1. Arbre LEADER — `space` (mode normal + visual)

```
LEADER = space
│
├── space          Command palette
├── tab            Buffer précédent (aller-retour, pane::AlternateFile)
├── /              Recherche globale projet
├── *              Recherche globale du mot sous le curseur
├── ;              Toggle comment (ligne en normal, sélection en visual)
├── 1..5           Aller au slot harpoon N (onglet épinglé)
│
├── q  QUIT
│   └── q          Quitter Zed
│
├── f  FILES
│   ├── f          File finder
│   ├── p          Dock projet (fichiers/dossiers)
│   ├── g          Fichiers modifiés git
│   ├── s          Save
│   ├── S          Save all
│   ├── n          New file
│   ├── r          Rename file  (révèle dans le tree + shift-r)
│   └── d          Delete file  (révèle dans le tree + shift-d)
│
├── b  BUFFERS
│   ├── b          Tab switcher (liste des buffers ouverts)
│   ├── n          Buffer suivant
│   └── p          Buffer précédent
│                  (fermeture : voir space w d)
│
├── w  WINDOWS / PANES
│   ├── h j k l    Pane gauche / bas / haut / droite
│   ├── /          Split vertical
│   ├── -          Split horizontal
│   ├── d          Fermer l'onglet courant
│   ├── o          Fermer les autres onglets et panes
│   ├── z          Zoom / dézoom du pane
│   ├── m          Fermer tous les docks
│   └── =          Égaliser les panes
│
├── m  MARKS / HARPOON
│   ├── m          Épingler / désépingler l'onglet (= ajouter au harpoon)
│   ├── a s d f g  Poser la mark globale A S D F G
│   └── A S D F G  Sauter à la mark (position exacte, backtick)
│
├── j  JUMP
│   ├── j          Go to line
│   ├── s / S      Occurrence suivante / précédente du symbole sous curseur
│   ├── f / F      Début de fonction suivant / précédent
│   ├── m / M      Fin de méthode suivante / précédente
│   ├── c / C      Section (classe, bloc top-level) suivante / précédente
│   ├── a / A      Argument suivant / précédent  (via f, et F,)
│   └── p / P      Idem, alias paramètre
│
├── l  LSP
│   ├── f          Format (sélection en visual)
│   ├── r          Rename
│   ├── d          Definition
│   ├── R          References
│   ├── i          Implementation
│   ├── t          Type definition
│   ├── a          Code action
│   └── h          Hover
│
├── g  GIT
│   ├── g          Panneau git
│   ├── c          Commit
│   ├── d          Diff
│   ├── b          Blame
│   ├── h          History (panneau git + onglet History)
│   ├── s          Status (= panneau git)
│   ├── p          Push
│   ├── P          Pull
│   └── f          Fetch
│
├── t  TERMINAL (center pane uniquement)
│   ├── t          Nouveau terminal centré
│   ├── n          Idem
│   ├── v          Terminal en split vertical
│   └── s          Terminal en split horizontal
│
└── e  ERRORS
    ├── n          Erreur suivante
    ├── p          Erreur précédente
    └── l          Liste des diagnostics
```

## 2. Arbre LEADER SECONDAIRE — `ctrl-space`

Actif là où `space` est un caractère : terminal, panneaux (projet, git, outline),
et mode insert. Même arbre, sous-ensemble utile hors éditeur.

```
ctrl-space
├── space              Command palette
├── /                  Recherche globale
├── f  f p g s S n     idem space f
├── b  b n p           idem space b
├── w  h j k l / - d o z m =   idem space w
├── g  g c d s p P f   idem space g
├── t  t n v s         idem space t
└── e  l               Liste des diagnostics
```

## 2b. Arbre LEADER dans le terminal centré, en mode vi

`shift-escape` passe le terminal en mode vi. Dans cet état `space` n'est plus un
caractère saisi, donc l'arbre leader `space` y est rejoué presque intégralement :
`space space`, `space tab`, `space /`, `space q q`, `space f *`, `space b *`,
`space w *`, `space m m`, `space 1..5`, `space g *`, `space t t/n`, `space e l`.

But : ne jamais avoir à sortir du terminal ni à toucher la souris. `shift-a`
repasse en saisie.

Les déplacements `ctrl-h/j/k/l` y sont alignés sur l'éditeur : demi-page bas et
haut, début et fin de ligne. Ils passent par `SendKeystrokes` qui rejoue les
touches comprises nativement par le mode vi du terminal (`ctrl-d`, `ctrl-u`, `0`,
`$`), sans recentrage du curseur — le terminal n'expose pas d'équivalent à `zz`.
Repli possible si ça casse : `terminal::ScrollPageDown` et `ScrollPageUp`, qui
font défiler la vue au lieu de déplacer le curseur.

Absents volontairement (sans objet hors éditeur) : `space l` (LSP), `space j`
(jump), `space ;`, `space *`, les marks `space m a..g`, et `space t v`/`t s` qui
dépendent d'un enchaînement de touches du leader éditeur.

## 3. Raccourcis hors leader

| Touche | Contexte | Action |
|---|---|---|
| `alt-h/j/k/l` | partout | Pane gauche / bas / haut / droite |
| `alt-w` | dock focus | Fermer ce dock |
| `ctrl-j` / `ctrl-k` | normal, visual | Demi-page bas / haut, curseur recentré (`ctrl-d zz` / `ctrl-u zz`) |
| `ctrl-j` / `ctrl-k` | popup complétion, code actions, signature, tous pickers | Élément suivant / précédent |
| `ctrl-d` | tab switcher | Fermer le buffer sélectionné |
| `ctrl-h` / `ctrl-l` | normal, visual, insert | Début / fin de ligne |
| `f t F T` | normal, visual | Find multiligne (continue sur les lignes suivantes) |
| `g d` | normal, visual | Go to definition |
| `Y` | normal | Yank du curseur à la fin de ligne (`y$`) |
| `shift-escape` | terminal | Basculer en mode vi |
| `shift-a` | terminal en mode vi | Repasser en mode insert |
| `ctrl-shift-5` / `ctrl-shift-6` | terminal | Split vertical / horizontal |

## 4. Raccourcis du keymap de base neutralisés

| Touche | Contexte | Valait |
|---|---|---|
| `ctrl-\`` | Workspace | Terminal docké |
| `ctrl-shift-\`` | Workspace | Nouveau terminal docké |
| `ctrl-j` | Workspace | `workspace::ToggleBottomDock` |
| `ctrl-h` | Editor mode==full | `buffer_search::DeployReplace` |
| `ctrl-l` | Editor mode==full | `editor::SelectLine` |
| `ctrl-n` | Terminal | `workspace::NewTerminal` (docké) |
| `ctrl-shift-space` | Terminal | `terminal::ToggleViMode` (déplacé sur `shift-escape`) |

---

## 5. settings.json — clés indispensables

```jsonc
{
  "vim_mode": true,
  "relative_line_numbers": "enabled",   // string, PAS un booléen
  "vim": {
    "toggle_relative_line_numbers": false,  // false = relatif aussi en insert
    "use_system_clipboard": "never",
    "highlight_on_yank_duration": 100
  },
  "experimental.theme_overrides": {
    "players": [{ "cursor": "#2e8b57ff", "background": "#2e8b57ff", "selection": "#2e8b5733" }]
  },
  "terminal": { "button": false, "working_directory": "current_project_directory" },
  "project_panel": { "dock": "right" },
  "git_panel": { "dock": "right" },
  "outline_panel": { "dock": "right" },
  "scrollbar": { "show": "never" },
  "minimap": { "show": "never" },
  "cursor_blink": false
}
```

`use_multiline_find` est **deprecated** — le find multiligne passe désormais par les
bindings `f/t/F/T` du keymap.

---

## 6. Mécanismes à connaître pour reconstruire

**Chaînage.** Zed n'accepte qu'une action par binding. Pour en enchaîner plusieurs,
`["workspace::SendKeystrokes", "touche1 touche2"]` rejoue des touches. Utilisé par :
`ctrl-j`/`ctrl-k` (scroll + `zz`), `space ;` (`gcc`), `Y` (`y$`), `space *` (`viw` +
recherche), `space f r`/`f d` (révéler + renommer/supprimer), `space t v`/`t s`
(split + terminal), `space g h` (panneau git + onglet History), `space j a/p`
(`f,` et `F,`), `space m *` (marks vim).

**Résolution des contextes.** Le contexte le plus profond gagne, et à profondeur
égale c'est la dernière déclaration. D'où trois conséquences vécues :
- Les bindings de complétion sont déclarés **après** le scroll `ctrl-j`/`ctrl-k`,
  qui porte en plus la garde `!showing_completions && !showing_code_actions`.
- Un picker avec champ de saisie est en `Picker > Editor` (voire
  `TabSwitcher > Picker > Editor`), plus profond que `Picker` — d'où les blocs
  dupliqués pour `ctrl-j`/`ctrl-k`/`ctrl-d`.
- Fermer les diagnostics a nécessité un bloc dédié en contexte `Diagnostics`.

**Affichage des raccourcis dans l'UI.** Zed cherche le binding correspondant à la
signature *exacte* de l'action. `pane::CloseActiveItem` avec `{ "close_pinned": false }`
est une signature différente de `pane::CloseActiveItem` seul : passer l'argument fait
que l'UI affiche un autre raccourci. D'où `space w d` déclaré sans argument.

**Doublons.** Deux touches sur la même action rendent l'affichage UI imprévisible.
C'est pourquoi il n'y a pas de `space b d` en plus de `space w d`.

**Terminal centré.** `workspace::NewCenterTerminal` ouvre le terminal comme onglet
ordinaire, donc `space b`/`space w` s'y appliquent. Les chemins vers le dock terminal
sont neutralisés (section 4) et le bouton masqué dans les settings.

**Marks par branche git.** Impossible via keymap — un binding ne stocke pas d'état.
Le contournement est un **worktree git par branche** (`shift-alt-w`) : chaque worktree
est un projet distinct, donc ses propres onglets épinglés restaurés automatiquement.

**Diagnostic.** `dev: open key context view` depuis la palette affiche l'arbre de
contextes actifs — c'est l'outil pour retrouver le bon contexte après une mise à jour.
L'éditeur de keymap (`zed: open keymap`) permet de chercher une action et de voir tous
ses bindings actifs, base incluse.
