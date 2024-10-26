



















Voici un exemple complet utilisant **NLTK** pour illustrer les concepts de base du NLP avec des explications étape par étape. Ensuite, un problème pratique suivra, avec une solution pédagogique.

### Partie 1 : Illustration des Concepts de NLP avec NLTK

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

### Partie 2 : Problème Pratique

**Problème :**  
Vous avez un texte brut avec des informations sur les avis clients pour un produit. Le texte contient des phrases avec beaucoup de stopwords, de ponctuations et des formes verbales différentes. Le but est d'obtenir une liste de mots-clés pour comprendre les sentiments des clients (positifs ou négatifs). Utilisez NLTK pour :
1. Nettoyer le texte (tokenisation, suppression des stopwords, stemming, lemmatisation).
2. Générer une liste finale de mots-clés.

**Solution :**

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
