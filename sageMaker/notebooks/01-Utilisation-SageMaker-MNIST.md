# Tutoriel Interactif : Utilisation de **Amazon SageMaker** pour Entraîner un Modèle sur les Images MNIST

### Contexte
**Objectif :** Nous allons entraîner un modèle de machine learning avec SageMaker pour reconnaître des chiffres manuscrits (dataset MNIST). Ce tutoriel va non seulement vous guider pas à pas, mais aussi vous poser des questions pour tester votre compréhension, vous donner des indices et même des défis !

**Étapes à suivre :**
1. **Visualisation des données**
2. **Préparation des données pour SageMaker**
3. **Téléchargement sur Amazon S3**
4. **Entraînement du modèle avec SageMaker**
5. **Déploiement du modèle**
6. **Validation des prédictions**
7. **Évaluation des résultats**
8. **Nettoyage des ressources**

Chaque étape est conçue pour être claire, amusante et engageante !

---

### Étape 1 : Visualisation des Données

Avant de plonger dans l’apprentissage automatique, prenons un moment pour visualiser les données. Cela aide à **comprendre ce que le modèle va apprendre**.

#### Code à exécuter

```python
%matplotlib inline
import matplotlib.pyplot as plt

plt.rcParams["figure.figsize"] = (2, 10)

def show_digit(img, caption="", subplot=None):
    if subplot is None:
        _, (subplot) = plt.subplots(1, 1)
    imgr = img.reshape((28, 28))
    subplot.axis("off")
    subplot.imshow(imgr, cmap="gray")
    plt.title(caption)

# Affiche une image du dataset et son label
show_digit(train_set[0][30], f"This is a {train_set[1][30]}")
```

#### Explication 
- **%matplotlib inline** : Permet d'afficher des graphiques directement dans le notebook.
- **`show_digit(img, caption, subplot)`** :
  - Transforme l’image en une grille de pixels 28x28 (taille des images MNIST).
  - Affiche une image en niveaux de gris (noir et blanc).
  
#### 🚀 Défi : Qu’est-ce que chaque pixel représente selon vous ?
- Indice : Imaginez chaque pixel comme une cellule qui peut être plus ou moins "lumineuse".
- **Réponse** : Chaque pixel représente un niveau de gris entre le noir (0) et le blanc (255).

---

### Étape 2 : Préparation des Données pour SageMaker

Maintenant que nous avons visualisé les données, nous devons les préparer pour **SageMaker**. SageMaker utilise un format spécial de données pour l'entraînement, appelé **RecordIO-wrapped protobuf**. Mais pourquoi donc ? 🤔

#### Code à exécuter

```python
import io
import numpy as np
import sagemaker.amazon.common as smac

train_set_vectors = np.array([t.tolist() for t in train_set[0]]).astype("float32")
train_set_labels = np.where(np.array([t.tolist() for t in train_set[1]]) == 0, 1, 0).astype("float32")

validation_set_vectors = np.array([t.tolist() for t in valid_set[0]]).astype("float32")
validation_set_labels = np.where(np.array([t.tolist() for t in valid_set[1]]) == 0, 1, 0).astype("float32")

train_set_buf = io.BytesIO()
validation_set_buf = io.BytesIO()
smac.write_numpy_to_dense_tensor(train_set_buf, train_set_vectors, train_set_labels)
smac.write_numpy_to_dense_tensor(validation_set_buf, validation_set_vectors, validation_set_labels)
train_set_buf.seek(0)
validation_set_buf.seek(0)
```

#### Explication 

**Questions pour les étudiants** :
1. **Pourquoi convertir les données en `float32` ?**
   - Indice : Est-ce que SageMaker aime d’autres types de données ?
   - **Réponse** : SageMaker nécessite que les données soient en `float32` pour une compatibilité avec ses algorithmes et pour gagner de la mémoire.
   
2. **Pourquoi utilisons-nous `np.where(... == 0, 1, 0)` ?**
   - Indice : Réfléchissez à la différence entre "binaire" et "multi-classes".
   - **Réponse** : Ce code transforme la classification en un problème binaire : soit l’image représente un "0" (`1`), soit non (`0`).

3. **Pourquoi utiliser `io.BytesIO()` ?**
   - Indice : Pensez aux déplacements de données entre votre ordinateur et un serveur distant.
   - **Réponse** : `io.BytesIO` crée un tampon en mémoire pour stocker les données temporairement avant de les envoyer à S3.

---

### Étape 3 : Téléchargement des Données sur Amazon S3

Nous avons formaté les données, maintenant il est temps de les **envoyer dans le cloud**. SageMaker va récupérer ces données sur S3 pour entraîner le modèle.

#### Code à exécuter

```python
import boto3
import os

key = "recordio-pb-data"
boto3.resource("s3").Bucket(bucket).Object(os.path.join(prefix, "train", key)).upload_fileobj(train_set_buf)
boto3.resource("s3").Bucket(bucket).Object(os.path.join(prefix, "validation", key)).upload_fileobj(validation_set_buf)
s3_train_data = f"s3://{bucket}/{prefix}/train/{key}"
print(f"uploaded training data location: {s3_train_data}")
s3_validation_data = f"s3://{bucket}/{prefix}/validation/{key}"
print(f"uploaded validation data location: {s3_validation_data}")
```

#### Explication 

**Questions pour les étudiants :**
1. **Que représente `s3://` ?**
   - Indice : Réfléchissez à ce qu’Amazon S3 stocke (c’est un stockage dans le...).
   - **Réponse** : `s3://` est un protocole pour indiquer un chemin sur Amazon S3, un service de stockage cloud d’Amazon.
   
2. **Pourquoi avons-nous besoin de mettre les données sur S3 ?**
   - Indice : SageMaker fonctionne dans le cloud.
   - **Réponse** : SageMaker doit accéder aux données depuis S3 pour entraîner le modèle, car il ne peut pas accéder aux données locales.

---

### Étape 4 : Entraînement du Modèle avec SageMaker

Maintenant que tout est en place, passons à la **partie amusante : l’entraînement du modèle** ! Nous allons configurer les paramètres pour qu’il puisse apprendre à identifier les images de chiffres.

#### Code à exécuter

```python
from sagemaker import image_uris

container = image_uris.retrieve(region=boto3.Session().region_name, framework="linear-learner")
deploy_amt_model = True
```

#### Configuration du modèle

```python
linear = sagemaker.estimator.Estimator(
    container,
    role,
    instance_count=1,
    instance_type="ml.c4.xlarge",
    output_path=output_location,
    sagemaker_session=sess,
)
linear.set_hyperparameters(feature_dim=784, predictor_type="binary_classifier", mini_batch_size=200)
linear.fit({"train": s3_train_data})
```

#### Explication détaillée

**Questions pour les étudiants :**
1. **Pourquoi `instance_count=1` ?**
   - Indice : Cela impacte le coût.
   - **Réponse** : Un seul ordinateur est suffisant pour ce petit dataset et cela réduit les coûts.
   
2. **Que signifie `predictor_type="binary_classifier"` ?**
   - Indice : Un modèle pour prédire oui/non.
   - **Réponse** : Indique que le modèle va faire des prédictions binaires : 0 ou non-0.
   
3. **Qu’est-ce qu’un `mini_batch_size` ?**
   - Indice : Le modèle traite-t-il toutes les images en une seule fois ?
   - **Réponse** : C’est le nombre d’images que le modèle traite en même temps ; cela optimise la mémoire et accélère l’apprentissage.

---

### Étape 5 : Déploiement du Modèle

Une fois que le modèle est prêt, nous allons le **déployer pour pouvoir tester des prédictions** en temps réel. 

#### Code à exécuter

```python
if deploy_amt_model:
    linear_predictor = hp_tuner.deploy(initial_instance_count=1, instance_type="ml.m4.xlarge")
else:
    linear_predictor = linear.deploy(initial_instance_count=1, instance_type="ml.m4.xlarge")
```

#### Explication détaillée

**Questions pour les étudiants :**
1. **Pourquoi déployer le modèle ?**
   - Indice : Comment le modèle peut-il répondre aux requêtes de prédiction ?
   - **Réponse** : Pour utiliser le modèle en temps réel, il doit être déployé sur un serveur où il peut traiter les demandes.

2. **Qu’est-ce qu’un `endpoint` ?**
   - Indice : Pensez à une adresse sur Internet.
   - **Réponse** : Un point d’accès que l’on peut appeler pour demander

 des prédictions au modèle.

---

### Étape 6 : Validation du Modèle

Pour tester le modèle, nous allons lui donner des images et voir s’il peut deviner si elles montrent un "0".

#### Code à exécuter

```python
result = linear_predictor.predict(train_set[0][30:31], initial_args={"ContentType": "text/csv"})
print(result)
```

#### Explication 

**Questions pour les étudiants :**
1. **Que signifie `ContentType="text/csv"` ?**
   - Indice : Le modèle comprend-il le format d’autres types de données ?
   - **Réponse** : Le modèle attend les données sous format CSV.

2. **Qu’est-ce que `result["predicted_label"]` ?**
   - Indice : C’est une valeur prédite par le modèle.
   - **Réponse** : `predicted_label` indique si l’image est un "0" (`1`) ou non (`0`).

---

### Étape 7 : Évaluation des Résultats

Nous vérifions la précision du modèle en créant une **matrice de confusion**, qui montre combien de fois il a bien prédit ou s’est trompé.

#### Code à exécuter

```python
import pandas as pd

pd.crosstab(
    np.where(test_set[1] == 0, 1, 0), predictions, rownames=["actuals"], colnames=["predictions"]
)
```

#### Explication 

**Questions pour les étudiants :**
1. **Que représente une matrice de confusion ?**
   - Indice : Compare les prédictions avec les vrais labels.
   - **Réponse** : Elle montre combien de fois le modèle a bien prédit ou s’est trompé.

2. **Pourquoi cette évaluation est-elle importante ?**
   - Indice : Pensez à la fiabilité du modèle.
   - **Réponse** : Elle permet de vérifier si le modèle est assez précis pour être utilisé dans des applications réelles.

---

### Étape 8 : Nettoyage des Ressources

Enfin, nous supprimons le modèle et le point de terminaison pour **éviter des coûts supplémentaires**.

#### Code à exécuter

```python
linear_predictor.delete_model()
linear_predictor.delete_endpoint()
```

**Questions pour les étudiants :**
1. **Pourquoi supprimer le point de terminaison ?**
   - Indice : Les ressources cloud sont-elles gratuites ?
   - **Réponse** : Pour éviter des frais d’hébergement inutiles après la fin du projet.
