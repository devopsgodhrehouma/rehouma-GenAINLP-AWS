# **Pratique : Création d'un Pipeline Complet en NLP avec BERT**

### **Contexte :**
BERT est un modèle de langage basé sur l'architecture Transformer qui comprend le contexte des mots dans les deux directions (avant et arrière). Cela le rend extrêmement puissant pour des tâches telles que la classification de texte, l'analyse de sentiment, et la reconnaissance des entités nommées.

Dans cette pratique, nous allons créer un pipeline NLP complet utilisant BERT pour l'analyse de texte, en intégrant les étapes de prétraitement, d'analyse des sentiments, et de reconnaissance d'entités nommées. 

---

## **Partie 1 : Prétraitement du texte pour l'analyse avec un modèle BERT**

### **Étape 1 : Installation des bibliothèques**
Nous utiliserons la bibliothèque **Transformers** de Hugging Face, qui permet de charger et d'utiliser des modèles pré-entraînés comme BERT. Voici les bibliothèques à installer :

```bash
pip install transformers
pip install torch
pip install nltk
```

### **Étape 2 : Chargement du modèle BERT pré-entraîné**
Le modèle **BERT-base-uncased** sera utilisé dans cet exemple pour l'analyse. Nous utiliserons également un tokenizer adapté à BERT pour transformer le texte en une forme compréhensible par le modèle.

```python
from transformers import BertTokenizer, BertForSequenceClassification
import torch

# Charger le tokenizer et le modèle BERT pré-entraîné
tokenizer = BertTokenizer.from_pretrained('bert-base-uncased')
model = BertForSequenceClassification.from_pretrained('bert-base-uncased', num_labels=2)

# Texte à analyser
texte = "I love programming with Python and I hate debugging."
```

### **Étape 3 : Tokenisation et encodage du texte**
Avant d'envoyer le texte à BERT, il doit être transformé en tokens. Le tokenizer BERT divisera le texte en sous-mots, ajoutera des tokens spéciaux (`[CLS]`, `[SEP]`) et convertira ces tokens en identifiants numériques.

```python
# Tokenisation et encodage du texte
inputs = tokenizer(texte, return_tensors='pt', padding=True, truncation=True, max_length=128)

# Vérifier la forme des inputs pour BERT
print(inputs)
```

- **Explication :**
  - Le **tokenizer** découpe le texte en sous-mots (BERT gère bien les mots rares en les fractionnant).
  - Les tokens **[CLS]** et **[SEP]** sont ajoutés pour signifier respectivement le début et la fin de la phrase.

### **Étape 4 : Passage du texte dans le modèle BERT**
Nous allons maintenant faire passer les données tokenisées dans le modèle BERT et obtenir des résultats sous forme de logits (scores bruts avant d'être convertis en probabilités).

```python
# Passage dans BERT pour obtenir les logits
with torch.no_grad():
    outputs = model(**inputs)
    logits = outputs.logits

# Conversion des logits en probabilités
probabilities = torch.softmax(logits, dim=1)

# Classe prédite (0 = négatif, 1 = positif)
predicted_class = torch.argmax(probabilities, dim=1).item()

print(f"Classe prédite : {predicted_class}, Probabilités : {probabilities}")
```

- **Explication :**
  - **Logits** : Scores non normalisés produits par le modèle BERT.
  - **Softmax** : Transforme les logits en probabilités entre 0 et 1.
  - **torch.argmax** : Identifie la classe ayant la probabilité la plus élevée.

---

## **Partie 2 : Utilisation de BERT pour la reconnaissance des entités nommées (NER)**

### **Étape 1 : Chargement d’un modèle BERT pour la NER**
Nous allons utiliser un modèle pré-entraîné pour la reconnaissance d'entités nommées afin d'identifier les personnes, les lieux, et les organisations dans un texte.

```python
from transformers import AutoTokenizer, AutoModelForTokenClassification
from transformers import pipeline

# Charger un modèle BERT pré-entraîné pour la NER
tokenizer = AutoTokenizer.from_pretrained("dbmdz/bert-large-cased-finetuned-conll03-english")
model = AutoModelForTokenClassification.from_pretrained("dbmdz/bert-large-cased-finetuned-conll03-english")

# Créer un pipeline pour la NER
ner_pipeline = pipeline("ner", model=model, tokenizer=tokenizer)
```

### **Étape 2 : Analyse d’un texte pour la NER**
Nous allons soumettre une phrase à BERT pour extraire les entités nommées.

```python
# Texte à analyser pour la NER
texte_ner = "Hugging Face Inc. is a company based in New York City. Its founders include Thomas Wolf and Julien Chaumond."

# Reconnaissance des entités nommées
entities = ner_pipeline(texte_ner)
for entity in entities:
    print(f"Entité : {entity['word']}, Label : {entity['entity']}, Score : {entity['score']}")
```

- **Explication :**
  - Le pipeline BERT pour la NER identifiera automatiquement les entités comme les personnes (PER), les lieux (LOC), et les organisations (ORG).
  - Les résultats incluent les **labels** pour chaque entité et leur **confiance** sous forme de score.

### **Sortie attendue :**
```
Entité : Hugging, Label : B-ORG, Score : 0.999
Entité : Face, Label : I-ORG, Score : 0.999
Entité : Inc., Label : I-ORG, Score : 0.999
Entité : New, Label : B-LOC, Score : 0.999
Entité : York, Label : I-LOC, Score : 0.999
Entité : City, Label : I-LOC, Score : 0.999
Entité : Thomas, Label : B-PER, Score : 0.998
Entité : Wolf, Label : I-PER, Score : 0.999
Entité : Julien, Label : B-PER, Score : 0.999
Entité : Chaumond, Label : I-PER, Score : 0.999
```

---

## **Partie 3 : Création d’un pipeline complet pour un chatbot avec BERT**

### **Étape 1 : Utilisation combinée de BERT pour l’analyse des sentiments et la NER**

Nous allons combiner ces techniques pour créer un pipeline capable d’identifier les sentiments et les entités nommées dans une seule interaction, permettant ainsi de répondre à une demande utilisateur avec un chatbot.

```python
# Texte à analyser (imaginons qu'il soit envoyé par un utilisateur au chatbot)
texte_chatbot = "Can you help me book a table at a restaurant in Paris for 7pm tonight?"

# Analyse des sentiments
inputs = tokenizer(texte_chatbot, return_tensors='pt', padding=True, truncation=True, max_length=128)
with torch.no_grad():
    outputs = model(**inputs)
    logits = outputs.logits
probabilities = torch.softmax(logits, dim=1)
sentiment = torch.argmax(probabilities, dim=1).item()

# Affichage des sentiments
if sentiment == 1:
    print("Le sentiment est positif.")
else:
    print("Le sentiment est négatif.")

# Reconnaissance des entités nommées
entities = ner_pipeline(texte_chatbot)
print("Entités détectées :")
for entity in entities:
    print(f"Entité : {entity['word']}, Label : {entity['entity']}, Score : {entity['score']}")
```

### **Explication du pipeline :**
- Le texte est d'abord analysé pour déterminer le **sentiment** global (positif ou négatif).
- Ensuite, nous utilisons la **NER** pour extraire des entités comme des lieux, des dates ou des organisations à partir du texte.
- Ce pipeline pourrait être utilisé par un chatbot pour répondre à des demandes complexes impliquant la réservation de services ou la gestion de requêtes personnalisées.

### **Conclusion :**
Ce pipeline exhaustif montre comment utiliser BERT pour construire un système robuste capable de comprendre le sentiment d'un texte et d'extraire des informations spécifiques, telles que des noms ou des lieux. Vous pouvez maintenant utiliser cette base pour explorer des applications plus complexes avec vos étudiants, notamment l'automatisation de tâches basées sur les demandes utilisateurs.


--------------------------------------
# Annexe :
--------------------------------------

- Dans cet exemple avec BERT, les **données** (data) utilisées sont principalement les phrases ou textes que nous soumettons au modèle pour analyse. 
- Voici un résumé des différents types de données utilisés dans chaque partie de l'exemple :

# **Partie 1 : Analyse des sentiments avec BERT**
- **Données textuelles** : 
  - Exemple : `"I love programming with Python and I hate debugging."`
  - Ce texte est utilisé pour évaluer le sentiment global de la phrase (positif ou négatif).

# **Partie 2 : Reconnaissance des entités nommées (NER) avec BERT**
- **Données textuelles** :
  - Exemple : `"Hugging Face Inc. is a company based in New York City. Its founders include Thomas Wolf and Julien Chaumond."`
  - Ce texte est utilisé pour identifier les **entités nommées** telles que :
    - `ORG` (Organisation) : Hugging Face Inc.
    - `LOC` (Lieu) : New York City
    - `PER` (Personnes) : Thomas Wolf, Julien Chaumond

# **Partie 3 : Pipeline combiné pour chatbot**
- **Données textuelles** :
  - Exemple : `"Can you help me book a table at a restaurant in Paris for 7pm tonight?"`
  - Ce texte est analysé pour deux tâches :
    1. **Analyse des sentiments** : Le modèle détermine si la phrase exprime une émotion positive ou négative.
    2. **Reconnaissance des entités nommées (NER)** : Le modèle identifie des entités spécifiques telles que :
       - `LOC` (Lieu) : Paris
       - `TIME` (Heure) : 7pm
       - `MISC` (autres entités, comme les événements ou organisations) : restaurant

---

# **Détails techniques des données passées à BERT** :

1. **Tokenisation** : Chaque phrase est d'abord tokenisée, c'est-à-dire divisée en mots ou sous-mots (tokens). Ces tokens sont ensuite transformés en **identifiants numériques** que BERT peut comprendre. Par exemple :
   - La phrase `"I love programming with Python and I hate debugging."` sera convertie en une série d'identifiants numériques représentant chaque mot et sous-mot.

2. **Features** : Les principales données fournies à BERT sont :
   - **Input IDs** : Les identifiants numériques pour chaque token.
   - **Attention Mask** : Masque indiquant quels tokens doivent être pris en compte (1) ou ignorés (0).
   - **Token Type IDs** (si nécessaire) : Pour certains modèles, ces données peuvent indiquer si un token fait partie de la première ou de la seconde phrase dans une paire de phrases (utilisé dans les tâches comme la classification de paires de phrases).

---

# **Les types de données que BERT peut traiter :**
- **Texte brut** : Comme les exemples ci-dessus, qui sont des phrases ou paragraphes que BERT transformera en tokens avant traitement.
- **Tokens** : Les mots et sous-mots qui ont été générés à partir du texte brut.
- **Identifiants numériques** : Chaque token est converti en identifiant numérique pour être utilisé par le modèle.

---

En résumé, dans cet exemple, les **données** sont les textes que le modèle BERT traite pour effectuer des tâches d'analyse des sentiments ou de reconnaissance d'entités nommées, transformés en tokens et en identifiants numériques exploitables par le modèle.
