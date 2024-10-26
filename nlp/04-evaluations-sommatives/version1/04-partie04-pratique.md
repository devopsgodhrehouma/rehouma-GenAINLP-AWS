# Problème Pratique : Extraction et Analyse des Mots-Clés pour un Avis Client

#### Énoncé du Problème

- Vous êtes chargé de créer un petit programme qui extrait les mots-clés significatifs d'un avis client pour un produit donné. 
- Ces mots-clés devraient refléter les aspects positifs ou négatifs de l'avis et être utilisables pour une analyse rapide des sentiments.

**Texte à analyser :**  
Voici un nouvel avis client que vous devrez traiter en utilisant les étapes vues précédemment pour extraire les mots-clés :  
```plaintext
"Ce produit est totalement décevant. Après deux semaines, aucun résultat visible ! J’ai perdu mon argent, et je ne recommande pas."
```

#### Consignes

1. **Étapes de Traitement :**
   - Utilisez la tokenisation pour diviser le texte en mots.
   - Supprimez les stopwords pour ne garder que les mots utiles.
   - Appliquez la normalisation des accents et des caractères spéciaux, ainsi que la suppression de la ponctuation.
   - Passez tous les mots en minuscules.
   - Appliquez ensuite le stemming et la lemmatisation pour simplifier les mots restants.

2. **Questions de Développement :**
   - **1.** Expliquez chaque étape de votre solution : comment le code suit-il les étapes du traitement NLP et pourquoi chaque étape est-elle nécessaire pour ce type d’analyse ?
   - **2.** Affichez et interprétez la liste finale de mots-clés : en quoi ces mots représentent-ils bien l'opinion du client ?
   - **3.** Proposez une méthode pour automatiser ce processus d’analyse pour des dizaines d’avis à la fois.

#### Code de Base pour Débuter

Vous pouvez réutiliser le code de la première partie en l'adaptant au nouvel avis et en expliquant les résultats obtenus.

```python
# Texte brut (nouvel avis client)
avis_client = "Ce produit est totalement décevant. Après deux semaines, aucun résultat visible ! J’ai perdu mon argent, et je ne recommande pas."

# Application des étapes de traitement NLP
# Suivez le même processus que pour le premier exercice pour obtenir la liste finale de mots-clés
```
