# Concepts de NLP- partie 1

### 1. **Tokenisation**
La tokenisation consiste à diviser un texte en unités plus petites appelées *tokens*. Ces tokens peuvent être des mots, des phrases ou même des caractères.

**Exercice :**
Prends la phrase suivante :
```
J'aime apprendre l'intelligence artificielle.
```
Divise cette phrase en mots (tokens). Écris chaque mot séparément.

**Réponse attendue :**
```
['J\'aime', 'apprendre', 'l\'intelligence', 'artificielle', '.']
```

### 2. **Étiquetage des données (POS tagging)**
L'étiquetage des données, ou *Part of Speech (POS) tagging*, consiste à associer à chaque mot d'une phrase sa nature grammaticale (verbe, nom, adjectif, etc.).

**Exercice :**
Utilise la même phrase :
```
J'aime apprendre l'intelligence artificielle.
```
Assigne à chaque mot son type grammatical. Exemple : "J'aime" (verbe), "apprendre" (verbe), etc.

**Réponse attendue :**
```
[('J\'aime', 'VERBE'), ('apprendre', 'VERBE'), ('l\'intelligence', 'NOM'), ('artificielle', 'ADJECTIF'), ('.', 'PONCTUATION')]
```

### 3. **Stopwords**
Les *stopwords* sont des mots très fréquents (comme "le", "de", "et") qui sont souvent ignorés dans les analyses de texte, car ils n'ajoutent pas beaucoup de sens.

**Exercice :**
Voici la phrase :
```
Le chat dort sur le canapé.
```
Supprime les mots courants ("Le", "sur", "le") qui n'ajoutent pas de signification particulière. Quels sont les mots importants à conserver ?

**Réponse attendue :**
```
['chat', 'dort', 'canapé']
```

### 4. **Lemmatisation**
La lemmatisation consiste à ramener un mot à sa forme de base, ou "lemme". Par exemple, "étudiants" devient "étudiant".

**Exercice :**
Voici quelques mots :
```
étudiants, étudié, étudierons
```
Applique la lemmatisation pour ramener ces mots à leur forme de base.

**Réponse attendue :**
```
['étudiant', 'étudier', 'étudier']
```

### 5. **Stemming**
Le *stemming* est similaire à la lemmatisation, mais il consiste à retirer les suffixes d’un mot pour en obtenir une racine. Cette approche est souvent plus brutale et peut ne pas toujours donner un mot correct.

**Exercice :**
Voici quelques mots :
```
étudiants, étudié, étudierons
```
Applique le stemming pour obtenir la racine de ces mots.

**Réponse attendue :**
```
['étudiant', 'étudi', 'étudi']
```

