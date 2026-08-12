# Configuration clavier Zed — référence

Document de reconstruction : si une mise à jour de Zed casse quelque chose,
ce fichier suffit à régénérer `keymap.json` et `settings.json` de zéro.

**Base** : `base_keymap` par défaut + `vim_mode: true`.
Ne jamais passer en `"None"` — le socle (enter, tab, backspace, menus,
terminal, accords `ctrl-k ctrl-*`) serait entièrement à réécrire.

---

## 1. Arbre LEADER — `space` (normal + visual)

```
LEADER = space
│
├── space          Command palette
├── tab            Buffer précédent (aller-retour)
├── /              Recherche globale projet
├── *              Recherche globale du mot sous le curseur
├── ;              Toggle comment (ligne en normal, sélection en visual)
├── 1..5           Aller au slot harpoon N (onglet épinglé)
│
├── q q            Quitter Zed
│
├── f  FILES
│   ├── f          File finder
│   ├── p          Dock projet
│   ├── g          Fichiers modifiés git
│   ├── s          Save (sans formatage — voir §5)
│   ├── S          Save all
│   ├── n          New file
│   ├── r          Rename file
│   └── d          Delete file
│
├── b  BUFFERS
│   ├── b          Tab switcher
│   ├── n / p      Buffer suivant / précédent
│   └── r          Recharger le fichier depuis le disque
│
├── w  WINDOWS / PANES
│   ├── h j k l    Pane gauche / bas / haut / droite
│   ├── /          Split vertical
│   ├── -  ou  _   Split horizontal
│   ├── d          Fermer l'onglet courant
│   ├── D          Fermer les autres onglets et panes
│   ├── z          Zoom / dézoom du pane
│   ├── m          Fermer tous les docks
│   └── =          Égaliser les panes
│
├── m  MARKS / HARPOON
│   ├── t          Épingler / désépingler l'onglet
│   ├── m          Poser / retirer un bookmark
│   ├── a          Bookmark avec libellé
│   ├── e          Modifier le libellé
│   ├── l          Liste des bookmarks
│   ├── c          Effacer tous les bookmarks
│   └── n / p      Bookmark suivant / précédent (recentré)
│
├── j  JUMP        (tous recentrés)
│   ├── j          Go to line
│   ├── f / F      Début de fonction suivant / précédent
│   ├── m / M      Fin de méthode suivante / précédente
│   ├── c / C      Section suivante / précédente
│   ├── a / A      Argument suivant / précédent
│   └── p / P      Idem, alias paramètre
│
├── l  LSP
│   ├── f          Format (sélection en visual)
│   ├── r          Rename
│   ├── d          Definition (recentré)
│   ├── R          References
│   ├── i          Implementation (recentré)
│   ├── t          Type definition (recentré)
│   ├── a          Code action
│   └── h          Hover
│
├── g  GIT
│   ├── g          Panneau git, onglet Changes
│   ├── h          Panneau git, onglet History
│   ├── c          Commit
│   ├── d          Diff
│   ├── b          Branch
│   ├── B          Blame
│   ├── s / u      Stage / unstage le hunk et passer au suivant
│   ├── p          Push
│   ├── P          Force push
│   ├── F          Pull
│   └── f          Fetch
│
├── t  TERMINAL (center pane uniquement)
│   ├── t  ou  n   Nouveau terminal centré
│   ├── v          Terminal en split vertical
│   └── s          Terminal en split horizontal
│
├── e  ERRORS
│   ├── n / p      Erreur suivante / précédente (recentré)
│   └── l          Liste des diagnostics
│
└── o  OUTLINE
    ├── o          Outline du buffer
    └── p          Panneau outline
```

## 2. LEADER SECONDAIRE — `ctrl-space`

Actif là où `space` est un caractère : terminal, panneaux, mode insert.
Sous-ensemble utile hors éditeur : `space`, `/`, `f f p g s S n`,
`b b n p`, `w h j k l / - _ d D z m =`, `g g c d p P F f`, `t t n v s`, `e l`.

## 3. LEADER dans le terminal centré, en mode vi

`escape` bascule le terminal en mode vi, `shift-a` revient en saisie. Dans cet
état `space` redevient un leader et l'arbre est rejoué : `space`, `tab`, `/`,
`q q`, `f *`, `b b n p`, `w *`, `m t`, `1..5`, `g *`, `t t/n`, `e l`.

`ctrl-h/j/k/l` y rejouent `0`, `ctrl-d`, `ctrl-u`, `$` — pas de recentrage,
le mode vi du terminal n'a pas d'équivalent à `zz`.

Absents volontairement : `space l` (LSP), `space j`, `space o`, `space ;`,
`space *`, les bookmarks, et `space t v` / `t s` qui dépendent d'un
enchaînement du leader éditeur.

## 4. Hors leader

| Touche | Contexte | Action |
|---|---|---|
| `alt-h/j/k/l` | éditeur, panneaux | Redimensionner le pane |
| `alt-w` | dock focus | Fermer ce dock |
| `ctrl-j` / `ctrl-k` | normal, visual | Demi-page bas / haut, recentré |
| `ctrl-j` / `ctrl-k` | complétion, code actions, signature, pickers | Suivant / précédent |
| `ctrl-d` | tab switcher | Fermer le buffer sélectionné |
| `ctrl-h` / `ctrl-l` | normal, visual, insert | Début / fin de ligne |
| `ctrl-o` / `ctrl-i` | normal, visual | Jumplist arrière / avant, recentré |
| `n` / `N` | normal, visual | Occurrence suivante / précédente, recentré |
| `f t F T` | normal, visual | Find multiligne |
| `g d` | normal, visual | Go to definition, recentré |
| `Y` | normal | Yank jusqu'à la fin de ligne (`y$`) |
| `shift-space` | éditeur, panneaux | Ouvrir l'entrée / le fichier réel |
| `escape` | terminal | Basculer en mode vi |
| `shift-a` | terminal en mode vi | Repasser en saisie |
| `ctrl-shift-5` / `6` | terminal | Split vertical / horizontal |

**Panneau git** : `s` / `S` stage fichier / tout, `u` / `U` unstage,
`d` discard avec confirmation.

## 5. Raccourcis du keymap de base neutralisés

| Touche | Contexte | Valait |
|---|---|---|
| `ctrl-\`` / `ctrl-shift-\`` | Workspace | Terminal docké |
| `ctrl-j` | Workspace | `ToggleBottomDock` |
| `ctrl-o` | Workspace | `OpenFiles` |
| `ctrl-h` | Editor mode==full | `DeployReplace` |
| `ctrl-l` | Editor mode==full | `SelectLine` |
| `ctrl-i` | Editor mode==full | `ShowSignatureHelp` |
| `ctrl-n` | Terminal | `NewTerminal` (docké) |
| `ctrl-shift-space` | Terminal | `ToggleViMode` (déplacé sur `escape`) |
| `g space` | VimControl | `OpenExcerpts` (déplacé sur `shift-space`) |
| `space` | Panneau git | `ToggleStaged` |
| `alt-y` / `shift-alt-y` | Editor | `StageAndNext` / `UnstageAndNext` |
| `space w q` | helix, hors éditeur | `CloseActiveItem` (remplacé par `space w d`) |
| `space w d` | helix | `SplitDown` |

---

## 6. settings.json — clés indispensables

```jsonc
{
  "vim_mode": true,
  "relative_line_numbers": "enabled",     // string, PAS un booléen
  "vim": {
    "toggle_relative_line_numbers": false,  // false = relatif aussi en insert
    "use_system_clipboard": "never",
    "highlight_on_yank_duration": 100
  },
  "theme_overrides": {
    "<nom du thème actif>": {
      "editor.active_line_number": "#FF9E3Bff",  // roninYellow kanagawa
      "editor.line_number": "#54546Dff",
      "vim.yank.background": "#FF9E3B44"
    }
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

Le **gras** sur les numéros de ligne n'est pas possible : les overlays de
thème n'acceptent une graisse que dans `syntax`, pas sur les couleurs d'UI.

`use_multiline_find` est deprecated — le find multiligne passe par les
bindings `f/t/F/T`.

---

## 7. Mécanismes

**Recentrage.** `["action::Sequence", [<action>, "editor::ScrollCursorCenter"]]`.
Préférer cette forme à `SendKeystrokes` : elle nomme l'action directement, donc
rien à revérifier après une mise à jour. Elle échoue en revanche sur les actions
asynchrones — `vim::MoveToNext` (`*` et `#`) n'a jamais pu être recentré, ni par
`Sequence` ni par `SendKeystrokes`. Repli universel si besoin :
`"vertical_scroll_margin": 999` dans les settings.

**Chaînage de touches.** `["workspace::SendKeystrokes", "t1 t2"]` rejoue des
frappes. Réservé aux touches littérales (`f ,`, `g c c`, `y $`) et aux
enchaînements multi-vues (`space t v`, `space f r`, `space g g`). Une touche
peut se rejouer elle-même : Zed utilise alors la définition de priorité
immédiatement inférieure au lieu de boucler.

**Résolution des contextes.** Le plus profond gagne ; à profondeur égale, la
dernière déclaration. Conséquences vécues :
- Les bindings de complétion sont déclarés après le scroll `ctrl-j`/`ctrl-k`,
  qui porte en plus la garde `!showing_completions && !showing_code_actions`.
- Un picker avec champ de saisie est en `Picker > Editor`, voire
  `TabSwitcher > Picker > Editor` — d'où les blocs dupliqués.
- `Diagnostics` n'hérite pas de `VimControl` : tout y est redéclaré.
- Pour neutraliser un binding de base, reprendre son **contexte exact**, sinon
  le `null` ne l'emporte pas.

**Affichage UI.** Zed cherche la signature exacte de l'action.
`pane::CloseActiveItem` avec `{ "close_pinned": false }` est une signature
différente de l'action nue : passer l'argument fait afficher un autre
raccourci. Et deux touches sur une même action rendent l'affichage
imprévisible — d'où l'absence de `space b d` à côté de `space w d`.

**Sauvegarde.** `workspace::SaveWithoutFormat` au lieu de `Save` : le formatage
à la sauvegarde écrase le buffer de redo et fait perdre les changements
annulés (discussion Zed #31021, fermée comme non actionnable). Le formatage
reste manuel sur `space l f`. `:w` en mode vim repasse par le chemin à risque.

**Marks par branche git.** Impossible via keymap. Utiliser un **worktree git
par branche** (`shift-alt-w`) : chaque worktree est un projet distinct, donc
ses propres onglets épinglés restaurés automatiquement.

**Diagnostic.** `dev: open key context view` affiche l'arbre des contextes
actifs. `zed: open keymap` permet de chercher une action et de voir tous ses
bindings, base incluse. L'autocomplétion dans `keymap.json` est plus fiable
que la liste publiée des actions.
