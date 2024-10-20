# **Partie 1 : Manipulation des Stopwords avec NLTK**

### **Paragraphe à analyser :**

```
Le chat dort tranquillement sur le canapé dans le salon. Cependant, lorsqu'il entend un bruit venant de la cuisine, il se réveille et commence à miauler.
```

### **Objectif :**
Dans cet exercice, tu apprendras à utiliser NLTK pour identifier et supprimer les *stopwords* dans un texte de manière automatisée et professionnelle. NLTK dispose d'une bibliothèque complète pour ce type d'opérations.

### **Étapes à suivre :**

1. **Étape 1 :** Installation de NLTK  
   Si tu n'as pas encore installé NLTK, tu peux l'installer avec la commande suivante :
   ```bash
   pip install nltk
   ```

2. **Étape 2 :** Téléchargement des *stopwords* depuis NLTK  
   Dans ce projet, nous allons utiliser la liste intégrée des *stopwords* en français de NLTK. Il faudra donc télécharger ces ressources en utilisant les commandes suivantes dans Python :

   ```python
   import nltk
   nltk.download('stopwords')
   ```

3. **Étape 3 :** Chargement et suppression des *stopwords* avec NLTK  
   Voici un programme Python qui charge le paragraphe, identifie les *stopwords* en français, et les supprime.

   ```python
   from nltk.corpus import stopwords
   from nltk.tokenize import word_tokenize
   import string

   # Télécharger les stopwords et le tokenizer si nécessaire
   nltk.download('punkt')

   # Paragraphe à analyser
   paragraphe = "Le chat dort tranquillement sur le canapé dans le salon. Cependant, lorsqu'il entend un bruit venant de la cuisine, il se réveille et commence à miauler."

   # Tokenisation du paragraphe
   tokens = word_tokenize(paragraphe, language='french')

   # Liste des stopwords en français
   stop_words = set(stopwords.words('french'))

   # Suppression des stopwords et de la ponctuation
   tokens_sans_stopwords = [mot for mot in tokens if mot.lower() not in stop_words and mot not in string.punctuation]

   # Résultat après suppression
   texte_sans_stopwords = ' '.join(tokens_sans_stopwords)
   print("Texte après suppression des stopwords :")
   print(texte_sans_stopwords)
   ```

4. **Étape 4 :** Explication du code
   - **nltk.tokenize.word_tokenize** : Cette fonction est utilisée pour diviser le texte en *tokens* (mots). Nous utilisons ici le tokenizer en français.
   - **nltk.corpus.stopwords.words('french')** : Charge une liste de *stopwords* en français.
   - **string.punctuation** : Nous utilisons cette bibliothèque pour nous assurer que la ponctuation est également supprimée.

5. **Étape 5 :** Exécution du programme  
   Exécute le programme et observe le texte transformé. Les *stopwords* et la ponctuation seront supprimés, ce qui rendra le texte plus compact.

---

# **Partie 2 : Automatisation Avancée avec NLTK pour la suppression des *stopwords* et plus encore**

### **Objectif :**
Dans cette partie, tu vas automatiser le processus de nettoyage du texte et ajouter d'autres fonctions comme la lemmatisation et l'analyse des parties du discours (POS tagging).

### **Étapes à suivre :**

1. **Étape 1 :** Lemmatisation avec NLTK  
   La lemmatisation est une technique pour réduire les mots à leur forme de base. Nous allons utiliser le module **WordNetLemmatizer** de NLTK.

   ```python
   from nltk.stem import WordNetLemmatizer
   nltk.download('wordnet')

   lemmatizer = WordNetLemmatizer()

   # Exemple de lemmatisation
   tokens_lemmatized = [lemmatizer.lemmatize(mot, pos='v') for mot in tokens_sans_stopwords]
   print("Texte après lemmatisation :")
   print(' '.join(tokens_lemmatized))
   ```

2. **Étape 2 :** Étiquetage des Parties du Discours (POS tagging)  
   Le POS tagging consiste à étiqueter chaque mot d'une phrase avec sa catégorie grammaticale. Cela permet une meilleure analyse syntaxique.

   ```python
   # Télécharger les ressources pour le POS tagging
   nltk.download('averaged_perceptron_tagger')

   # Application du POS tagging
   pos_tags = nltk.pos_tag(tokens_sans_stopwords, lang='fra')
   print("Étiquetage des parties du discours :")
   print(pos_tags)
   ```

3. **Étape 3 :** Programme complet d'automatisation  
   Voici un programme complet qui lit un texte, supprime les *stopwords*, effectue la lemmatisation, et étiquette chaque mot avec sa catégorie grammaticale.

   ```python
   from nltk.corpus import stopwords
   from nltk.tokenize import word_tokenize
   from nltk.stem import WordNetLemmatizer
   import string
   import nltk

   # Télécharger les ressources nécessaires
   nltk.download('punkt')
   nltk.download('wordnet')
   nltk.download('averaged_perceptron_tagger')
   nltk.download('stopwords')

   # Paragraphe à analyser
   paragraphe = "Le chat dort tranquillement sur le canapé dans le salon. Cependant, lorsqu'il entend un bruit venant de la cuisine, il se réveille et commence à miauler."

   # Tokenisation
   tokens = word_tokenize(paragraphe, language='french')

   # Suppression des stopwords et de la ponctuation
   stop_words = set(stopwords.words('french'))
   tokens_sans_stopwords = [mot for mot in tokens if mot.lower() not in stop_words and mot not in string.punctuation]

   # Lemmatisation
   lemmatizer = WordNetLemmatizer()
   tokens_lemmatized = [lemmatizer.lemmatize(mot, pos='v') for mot in tokens_sans_stopwords]

   # POS tagging
   pos_tags = nltk.pos_tag(tokens_lemmatized, lang='fra')

   print("Texte après suppression des stopwords et lemmatisation :")
   print(' '.join(tokens_lemmatized))

   print("\nÉtiquetage des parties du discours :")
   print(pos_tags)
   ```

### **Explication :**
- **Lemmatisation** : Cette étape ramène les mots à leur forme racine, ce qui est utile pour normaliser le texte avant analyse.
- **POS Tagging** : Cette méthode étiquette chaque mot avec sa catégorie grammaticale, ce qui peut être utile pour des tâches plus avancées comme l'analyse syntaxique ou la traduction automatique.

---

# **Conclusion :**
Cette approche plus professionnelle avec NLTK permet d'automatiser plusieurs tâches courantes du NLP, telles que la suppression des *stopwords*, la lemmatisation, et l'étiquetage des parties du discours, rendant ainsi l'analyse textuelle plus précise et plus puissante.
