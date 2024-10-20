# **Partie 1 : Analyse, nettoyage et préparation d’un texte pour un chatbot**

### **Contexte :**
Les chatbots modernes nécessitent une prétraitement complet du texte pour pouvoir répondre de manière efficace aux utilisateurs. Cela inclut le nettoyage du texte, la gestion des *stopwords*, la normalisation par la lemmatisation, l’analyse grammaticale avec le *POS tagging*, et même la reconnaissance des entités nommées. 

Dans cette pratique, nous allons concevoir un pipeline complet de traitement du langage naturel, en simulant un environnement pour un chatbot. Le texte sera nettoyé, analysé et transformé pour qu'il puisse être utilisé dans des applications de chatbot ou d'assistant virtuel.

### **Texte à analyser :**
```
Bonjour, je m'appelle Pierre et je travaille chez OpenAI. Pouvez-vous m'aider à réserver une table dans un restaurant à Paris pour 19h ce soir ?
```

### **Étapes à suivre :**

1. **Étape 1 :** Installation des bibliothèques nécessaires  
   Avant de commencer, assure-toi d’avoir installé **NLTK** et les ressources nécessaires pour cette pratique. Utilise la commande suivante si NLTK n'est pas déjà installé :
   ```bash
   pip install nltk
   ```

   Ensuite, dans le programme, nous devons télécharger les ressources dont nous aurons besoin, y compris les modèles de stopwords, de lemmatisation, de POS tagging, et d'entités nommées.

   ```python
   import nltk
   nltk.download('punkt')
   nltk.download('stopwords')
   nltk.download('averaged_perceptron_tagger')
   nltk.download('wordnet')
   nltk.download('maxent_ne_chunker')
   nltk.download('words')
   ```

2. **Étape 2 :** Tokenisation du texte  
   La **tokenisation** consiste à diviser le texte en unités appelées tokens (mots ou phrases). Dans un chatbot, cette étape est cruciale pour isoler les éléments importants.

   ```python
   from nltk.tokenize import word_tokenize

   texte = "Bonjour, je m'appelle Pierre et je travaille chez OpenAI. Pouvez-vous m'aider à réserver une table dans un restaurant à Paris pour 19h ce soir ?"

   # Tokenisation
   tokens = word_tokenize(texte, language='french')
   print("Tokens :", tokens)
   ```

3. **Étape 3 :** Suppression des *stopwords* et nettoyage de la ponctuation  
   Il est essentiel de supprimer les mots courants (comme "je", "et", "à") qui n’ajoutent pas de valeur particulière à l’analyse du texte. Nous allons également éliminer les signes de ponctuation.

   ```python
   from nltk.corpus import stopwords
   import string

   # Chargement des stopwords français
   stop_words = set(stopwords.words('french'))

   # Suppression des stopwords et de la ponctuation
   tokens_sans_stopwords = [mot for mot in tokens if mot.lower() not in stop_words and mot not in string.punctuation]

   print("Tokens après suppression des stopwords :", tokens_sans_stopwords)
   ```

4. **Étape 4 :** Lemmatisation  
   La **lemmatisation** ramène chaque mot à sa forme de base (son lemme). Cela aide à normaliser le texte pour que les différentes formes d'un mot soient traitées de manière uniforme. Par exemple, "travailler" et "travaille" seront réduits à la forme "travailler".

   ```python
   from nltk.stem import WordNetLemmatizer

   # Initialisation du lemmatizer
   lemmatizer = WordNetLemmatizer()

   # Lemmatisation des tokens
   tokens_lemmatized = [lemmatizer.lemmatize(mot, pos='v') for mot in tokens_sans_stopwords]
   print("Tokens après lemmatisation :", tokens_lemmatized)
   ```

5. **Étape 5 :** Étiquetage des parties du discours (POS tagging)  
   Nous allons maintenant étiqueter chaque mot avec son type grammatical (nom, verbe, adjectif, etc.). Cette étape est cruciale pour comprendre le rôle de chaque mot dans une phrase.

   ```python
   # Étiquetage des parties du discours
   pos_tags = nltk.pos_tag(tokens_lemmatized, lang='fra')
   print("Étiquetage des parties du discours :", pos_tags)
   ```

6. **Étape 6 :** Reconnaissance des entités nommées (NER - Named Entity Recognition)  
   Les chatbots modernes doivent souvent comprendre les entités importantes dans une phrase comme des noms de personnes, des lieux, des organisations, ou même des dates. NLTK permet de détecter ces entités grâce à **chunking**.

   ```python
   # Reconnaissance des entités nommées (NER)
   entites_nommees = nltk.ne_chunk(pos_tags, binary=False)
   print("Entités nommées :", entites_nommees)
   ```

7. **Étape 7 :** Gestion des entités spécifiques  
   Une fois les entités détectées, nous pouvons extraire les informations importantes comme les noms propres, les lieux, et les heures pour répondre aux demandes des utilisateurs.

   ```python
   for subtree in entites_nommees:
       if isinstance(subtree, nltk.Tree):
           entite = " ".join([mot for mot, tag in subtree.leaves()])
           print(f"Entité détectée : {entite} ({subtree.label()})")
   ```

---

### **Partie 2 : Création d’un pipeline complet pour un chatbot professionnel avec NLTK**

Dans cette section, tu vas créer un pipeline complet qui nettoie, normalise et analyse un texte pour un chatbot. L’objectif est de permettre au chatbot de comprendre et d'extraire les informations nécessaires pour générer une réponse.

### **Étapes à suivre :**

1. **Étape 1 :** Conception du pipeline
   Le pipeline va inclure toutes les étapes précédentes (tokenisation, suppression des *stopwords*, lemmatisation, étiquetage grammatical, et reconnaissance d'entités nommées).

2. **Étape 2 :** Programme Python complet pour le chatbot

   ```python
   import nltk
   from nltk.corpus import stopwords
   from nltk.tokenize import word_tokenize
   from nltk.stem import WordNetLemmatizer
   import string

   # Télécharger les ressources NLTK
   nltk.download('punkt')
   nltk.download('stopwords')
   nltk.download('averaged_perceptron_tagger')
   nltk.download('wordnet')
   nltk.download('maxent_ne_chunker')
   nltk.download('words')

   # Texte à analyser
   texte = "Bonjour, je m'appelle Pierre et je travaille chez OpenAI. Pouvez-vous m'aider à réserver une table dans un restaurant à Paris pour 19h ce soir ?"

   # Tokenisation
   tokens = word_tokenize(texte, language='french')

   # Suppression des stopwords
   stop_words = set(stopwords.words('french'))
   tokens_sans_stopwords = [mot for mot in tokens if mot.lower() not in stop_words and mot not in string.punctuation]

   # Lemmatisation
   lemmatizer = WordNetLemmatizer()
   tokens_lemmatized = [lemmatizer.lemmatize(mot, pos='v') for mot in tokens_sans_stopwords]

   # Étiquetage des parties du discours (POS tagging)
   pos_tags = nltk.pos_tag(tokens_lemmatized, lang='fra')

   # Reconnaissance des entités nommées (NER)
   entites_nommees = nltk.ne_chunk(pos_tags, binary=False)

   # Extraction des entités nommées
   print("Entités nommées détectées :")
   for subtree in entites_nommees:
       if isinstance(subtree, nltk.Tree):
           entite = " ".join([mot for mot, tag in subtree.leaves()])
           print(f"{entite} ({subtree.label()})")

   # Résultat final après traitement
   print("\nTexte nettoyé et lemmatisé :", ' '.join(tokens_lemmatized))
   ```

---

### **Explication du pipeline :**
- **Tokenisation** : Divise le texte en mots pour une analyse individuelle.
- **Stopwords** : Supprime les mots inutiles pour la compréhension globale.
- **Lemmatisation** : Normalise les mots pour réduire la variabilité.
- **POS Tagging** : Étiquette les mots pour comprendre leur fonction grammaticale.
- **NER** : Détecte les noms, lieux, dates, etc., afin que le chatbot puisse extraire et utiliser ces informations.

---

### **Conclusion :**
Ce pipeline exhaustif est conçu pour préparer le texte de manière optimale pour un chatbot. Il intègre toutes les étapes nécessaires au nettoyage, à l'analyse grammaticale, et à l'extraction d'informations importantes. Ce type de préparation est essentiel pour développer un chatbot capable de répondre avec précision à des demandes complexes et variées.
