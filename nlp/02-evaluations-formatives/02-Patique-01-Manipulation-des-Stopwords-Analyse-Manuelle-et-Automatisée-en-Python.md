# Partie 1

**Paragraphe à analyser :**

```
Le chat dort tranquillement sur le canapé dans le salon. Cependant, lorsqu'il entend un bruit venant de la cuisine, il se réveille et commence à miauler.
```

**Exercice :**  
Dans cet exercice, tu vas apprendre à identifier et supprimer les *stopwords* dans un texte. Les *stopwords* sont des mots courants comme "le", "la", "de", qui n'apportent pas beaucoup de sens à l'analyse du texte.

**Étapes à suivre :**

1. **Étape 1 :** Copie le paragraphe ci-dessus dans un éditeur de texte.
2. **Étape 2 :** Lis attentivement le paragraphe et identifie les mots qui sont *stopwords*. Voici une liste de *stopwords* pour t'aider :
   ```
   le, la, les, un, une, de, dans, sur, et, il, elle, en, avec, sans, à, au
   ```
3. **Étape 3 :** Supprime tous les mots que tu trouves dans la liste des stopwords. Par exemple, "Le" et "dans" doivent être supprimés du paragraphe.
4. **Étape 4 :** Réécris le paragraphe avec uniquement les mots qui restent après avoir supprimé les *stopwords*.


# Partie 2 -création d'un programme Python simple pour automatiser la suppression des *stopwords* à partir d'un fichier texte. 

---

**Paragraphe à analyser :**

```
Le chat dort tranquillement sur le canapé dans le salon. Cependant, lorsqu'il entend un bruit venant de la cuisine, il se réveille et commence à miauler.
```

**Exercice :**

Dans cet exercice, tu vas créer un programme Python qui lira un fichier `.txt` contenant un paragraphe et supprimera tous les *stopwords* à partir d'une liste donnée. Voici les étapes que tu vas suivre :

### Étapes à suivre :

1. **Étape 1 :** Crée un fichier texte nommé `paragraphe.txt` et ajoute-y le paragraphe ci-dessus.
2. **Étape 2 :** Crée un autre fichier texte nommé `stopwords.txt` qui contiendra la liste suivante de *stopwords* :
   ```
   le, la, les, un, une, de, dans, sur, et, il, elle, en, avec, sans, à, au, cependant, lorsqu'
   ```

3. **Étape 3 :** Écris un programme Python simple qui lira le fichier `paragraphe.txt` et supprimera tous les mots trouvés dans `stopwords.txt`.

Voici un modèle du programme Python que tu peux utiliser pour t'inspirer :

```python
# Lecture des stopwords depuis le fichier stopwords.txt
with open('stopwords.txt', 'r') as f:
    stopwords = f.read().split(',')

# Lecture du paragraphe depuis le fichier paragraphe.txt
with open('paragraphe.txt', 'r') as f:
    paragraphe = f.read()

# Suppression des stopwords
mots = paragraphe.split()
mots_sans_stopwords = [mot for mot in mots if mot.lower().strip(",.'\"") not in stopwords]

# Réécriture du texte sans les stopwords
texte_sans_stopwords = ' '.join(mots_sans_stopwords)
print("Texte après suppression des stopwords :")
print(texte_sans_stopwords)

# Sauvegarde du texte modifié dans un nouveau fichier
with open('paragraphe_sans_stopwords.txt', 'w') as f:
    f.write(texte_sans_stopwords)
```

### **Ce que fait le programme :**
- Il lit la liste des *stopwords* depuis un fichier texte.
- Il lit le paragraphe depuis un autre fichier texte.
- Il supprime tous les *stopwords* du paragraphe.
- Il affiche le texte sans *stopwords* et le sauvegarde dans un nouveau fichier nommé `paragraphe_sans_stopwords.txt`.

**Étape 4 :** Exécute le programme et observe le texte modifié dans le fichier `paragraphe_sans_stopwords.txt`.
