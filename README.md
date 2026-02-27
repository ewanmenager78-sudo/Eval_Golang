# Eval_Golang

1 — Analyser un seul fichier

1. Tapez `1` puis Entrée.
2. Appuyez sur Entrée pour utiliser `config.txt` (créé automatiquement s'il n'existe pas), ou tapez le chemin d'un autre fichier `.txt`.
3. Le programme affiche les stats du fichier (taille, lignes, mots).
4. Tapez un mot-clé pour filtrer les lignes → deux fichiers sont créés : `out/filtered.txt` et `out/filtered_not.txt`.
5. Entrez un nombre N → les N premières lignes sont sauvées dans `out/head.txt` et les N dernières dans `out/tail.txt`.

2 — Analyser plusieurs fichiers

1. Tapez `2` puis Entrée.
2. Entrez le chemin d'un dossier contenant des fichiers `.txt` (ex: `data/`).
3. Le programme analyse tous les `.txt` trouvés et génère automatiquement :
   - `out/report.txt` — stats de chaque fichier
   - `out/index.txt` — liste rapide nom/taille/date
   - `out/merged.txt` — tous les fichiers fusionnés en un seul

3 — Analyser un article Wikipedia

1. Tapez `3` puis Entrée.
2. Entrez le nom de l'article (ex: `Go_(langage)` ou `Tour_Eiffel`).
3. Le programme télécharge l'article et affiche le nombre de lignes et de mots.
4. Tapez un mot-clé pour compter les lignes qui le contiennent.
5. Le contenu brut est sauvegardé dans `out/wiki_<nom_article>.txt`.

4 — ProcessOps (gestion des processus)

1. Tapez `4` puis Entrée.
2. Choisissez une sous-option :
   - `1` **Lister** : entrez un nombre N, affiche les N premiers processus actifs.
   - `2` **Filtrer** : entrez un mot, affiche les processus dont le nom le contient.
   - `3` **Kill** : entrez un PID, confirmez avec `yes` pour terminer le processus.
   - `4` **Retour** au menu principal.

5 — Quitter

Tapez `5` pour fermer le programme.

Tous les fichiers générés sont dans le dossier `out/`, créé automatiquement.

 Choix techniques du code

`bufio.Reader` pour les saisies**
Permet de lire les entrées utilisateur ligne par ligne sans bloquer le programme.

`bufio.Writer` pour les fichiers**
Les écritures sont mises en mémoire tampon avant d'être envoyées d'un coup sur le disque, c'est plus performant que d'écrire ligne par ligne directement.

`os.Stat()` avant chaque opération fichier**
On vérifie que le fichier ou dossier existe avant d'essayer de l'ouvrir, pour éviter des erreurs silencieuses.

`runtime.GOOS` dans ProcessOps**
Le programme détecte automatiquement si on est sur Windows, Linux ou macOS et adapte la commande système à utiliser (`tasklist` ou `ps`).

`http.Client` avec timeout pour Wikipedia**
Un timeout de 15 secondes évite que le programme reste bloqué indéfiniment si le réseau ne répond pas.

`io.ReadAll()` pour Wikipedia**
Lit tout le contenu de la réponse HTTP en une seule fois en mémoire, plus simple que de lire chunk par chunk quand la taille reste raisonnable.

`strings.Fields()` pour compter les mots**
Découpe une chaîne sur tous les espaces/tabulations d'un coup, plus fiable que de splitter sur un simple espace.

`strings.Contains()` + `strings.ToLower()` pour la recherche**
On met tout en minuscules avant de comparer pour que la recherche soit insensible à la casse.

`strconv.Atoi()` pour valider les PID**
Avant de tenter quoi que ce soit, on vérifie que l'entrée est bien un nombre entier valide.

Confirmation avant kill**
On demande `yes` explicitement avant de tuer un processus pour éviter toute suppression accidentelle.

