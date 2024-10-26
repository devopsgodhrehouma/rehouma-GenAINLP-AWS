# NLP avec NLTK

Ce projet utilise **NLTK** pour illustrer les étapes de base du traitement du langage naturel (NLP) en Python. Il présente deux parties : une première partie avec des exemples d’étapes fondamentales de NLP, et une seconde partie avec un problème pratique qui applique ces étapes pour extraire des mots-clés d’avis clients.


---
## Partie 1 : Illustration des Concepts de NLP avec NLTK
---

```python
# Importation des modules nécessaires de la librairie NLTK
import nltk
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
from nltk.stem import PorterStemmer, WordNetLemmatizer
import string

# Téléchargement des ressources nécessaires
nltk.download('punkt')          # Pour la tokenisation
nltk.download('stopwords')      # Pour les stopwords
nltk.download('wordnet')        # Pour la lemmatisation

# Phrase exemple
phrase = "Les enfants lisent beaucoup de livres en français, et ils adorent ça ! C'était vraiment génial."

# 1. Tokenisation
tokens = word_tokenize(phrase)
print("Tokenisation:", tokens)

# 2. Suppression des stopwords
stop_words = set(stopwords.words('french'))  # Stopwords pour le français
tokens_sans_stopwords = [word for word in tokens if word.lower() not in stop_words]
print("Suppression des Stopwords:", tokens_sans_stopwords)

# 3. Stemming
stemmer = PorterStemmer()
tokens_stemmes = [stemmer.stem(word) for word in tokens_sans_stopwords]
print("Stemming:", tokens_stemmes)

# 4. Lemmatisation
lemmatizer = WordNetLemmatizer()
tokens_lemmatise = [lemmatizer.lemmatize(word) for word in tokens_sans_stopwords]
print("Lemmatisation:", tokens_lemmatise)

# 5. Normalisation des accents et des caractères spéciaux
import unicodedata
tokens_normalises = [unicodedata.normalize('NFD', word).encode('ascii', 'ignore').decode('utf-8') for word in tokens_sans_stopwords]
print("Normalisation des Accents:", tokens_normalises)

# 6. Suppression de la ponctuation
tokens_sans_ponctuation = [word for word in tokens_normalises if word not in string.punctuation]
print("Suppression de la Ponctuation:", tokens_sans_ponctuation)

# 7. Passage en minuscules
tokens_final = [word.lower() for word in tokens_sans_ponctuation]
print("Passage en Minuscules:", tokens_final)
```

### Explications étape par étape

- **Tokenisation** : Cette étape divise la phrase en unités de mots, appelés "tokens". Cela facilite l'analyse de chaque mot individuellement.
- **Suppression des Stopwords** : Les stopwords (mots courants sans valeur ajoutée) sont éliminés pour se concentrer sur les termes significatifs du texte.
- **Stemming** : Cette opération réduit chaque mot à sa racine. Par exemple, "aimant" et "aimer" deviennent "aim".
- **Lemmatisation** : Contrairement au stemming, la lemmatisation convertit chaque mot en sa forme de base tout en maintenant des mots existants.
- **Normalisation des Accents** : La normalisation transforme les caractères accentués en leur équivalent sans accent, par exemple "élève" devient "eleve".
- **Suppression de la Ponctuation** : La ponctuation est supprimée pour se concentrer uniquement sur les mots.
- **Passage en Minuscules** : Les mots sont mis en minuscules pour éviter les doublons dus à la casse (par exemple, "Livre" et "livre").

---
## Partie 2 : Problème Pratique
---

Ce problème simule une analyse d’avis client pour en extraire les mots-clés pertinents en appliquant les étapes de la Partie 1.

### Code de la Solution

```python
# Texte brut (avis client)
avis_client = "Ce produit est incroyable ! Les résultats sont vraiment visibles après une semaine d'utilisation. C'était efficace, et je suis très satisfait."

# Étapes de nettoyage et extraction des mots-clés

# Tokenisation
tokens = word_tokenize(avis_client)

# Suppression des stopwords
tokens_sans_stopwords = [word for word in tokens if word.lower() not in stop_words]

# Suppression de la ponctuation
tokens_sans_ponctuation = [word for word in tokens_sans_stopwords if word not in string.punctuation]

# Stemming
tokens_stemmes = [stemmer.stem(word) for word in tokens_sans_ponctuation]

# Lemmatisation
tokens_lemmatise = [lemmatizer.lemmatize(word.lower()) for word in tokens_stemmes]

# Résultat final : Liste des mots-clés
mots_cles = tokens_lemmatise
print("Liste des Mots-Clés:", mots_cles)
```

### Résumé de la Solution

1. **Tokenisation** : Le texte est découpé en tokens.
2. **Suppression des Stopwords et de la Ponctuation** : Les mots non informatifs et les ponctuations sont retirés.
3. **Stemming et Lemmatisation** : Les mots sont réduits et uniformisés en formes basiques pour obtenir des termes-clés.
4. **Liste des Mots-Clés** : Le résultat final est une liste de mots représentatifs de l’avis client.

Ce processus de nettoyage aboutit à une liste de mots-clés qui permet d'identifier rapidement le ressenti du client sans détails superflus.
