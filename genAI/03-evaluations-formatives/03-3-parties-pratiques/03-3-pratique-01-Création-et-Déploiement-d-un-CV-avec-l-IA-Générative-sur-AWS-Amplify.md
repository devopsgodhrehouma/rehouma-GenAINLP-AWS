# **Exercice : Créer une page web de CV avec l'IA générative et déploiement sur AWS Amplify**

---

# **Étape 1 : Préparer vos informations personnelles**
---

1. **Préparez les informations suivantes pour alimenter votre CV :**
   - **Nom complet**
   - **Poste actuel** ou celui que vous visez
   - **Courte description personnelle** (2 à 3 phrases résumant votre parcours ou vos objectifs)
   - **Votre programme d'études** (ex. : Informatique, Génie logiciel, Marketing, etc.)
   - **Expériences professionnelles** (nom de l'entreprise, poste occupé, période, et brève description)
   - **Photo de profil** (optionnelle, sinon utilisez un placeholder)

---

# **Étape 2 : Utiliser l'IA pour générer votre CV en HTML**
---

2. **Utilisez un générateur d'IA comme ChatGPT** pour créer automatiquement votre CV en HTML avec le modèle suivant.

   - Utilisez ce prompt pour générer votre CV, y compris les compétences en fonction de votre programme d'études :

   ```plaintext
   Génère un CV en format HTML avec le modèle suivant. Utilise les informations suivantes pour remplir les champs :
   - Nom : [Votre nom]
   - Poste : [Votre poste]
   - Description : [Votre description personnelle]
   - Programme d'études : [Votre programme d'études]
   - Expériences professionnelles : [Votre liste d'expériences]
   - Image : [Lien vers votre photo ou utiliser un placeholder]
   
   Génère également une liste de 3 à 5 compétences principales basées sur le programme d'études.

   Modèle HTML :
   
   <!DOCTYPE html>
   <html lang="fr">

   <head>
       <meta charset="UTF-8">
       <meta name="viewport" content="width=device-width, initial-scale=1.0">
       <title>Notre Équipe</title>
       <link rel="stylesheet" target=_blank href="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css">
       <link rel="stylesheet" target=_blank href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.15.4/css/all.min.css">
   </head>

   <body>

       <!-- Jumbotron pour l'en-tête -->
       <div class="jumbotron text-center">
           <h1>Notre Équipe</h1>
           <p>Rencontrez les esprits brillants derrière notre succès!</p>
       </div>

       <!-- Grille pour les profils de l'équipe -->
       <div class="container">

           <!-- [Nom complet] -->
           <div class="row mb-4">
               <div class="col-md-4">
                   <img src="[Lien vers la photo]" alt="[Nom complet]" class="img-fluid rounded-circle">
               </div>
               <div class="col-md-4">
                   <h3>[Nom complet] <i class="fas fa-user-tie"></i></h3>
                   <p>[Poste]</p>
                   <p>[Description personnelle]</p>
               </div>
               <div class="col-md-4">
                   <h4>Compétences</h4>
                   <span class="badge badge-primary"><i class="fab fa-python"></i> [Compétence 1]</span>
                   <span class="badge badge-success"><i class="fab fa-django"></i> [Compétence 2]</span>
                   <span class="badge badge-warning"><i class="fab fa-js-square"></i> [Compétence 3]</span>
               </div>
           </div>

           <!-- Expériences professionnelles -->
           <div class="container">
               <h2>Expériences professionnelles</h2>
               <div class="row mb-4">
                   <div class="col-md-12">
                       <h4>[Nom de l'entreprise]</h4>
                       <p><strong>[Poste occupé]</strong> - [Période]</p>
                       <p>[Brève description de l'expérience]</p>
                   </div>
               </div>
           </div>

       </div>

       <!-- Bootstrap JS -->
       <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
       <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.3/dist/umd/popper.min.js"></script>
       <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>

   </body>
   </html>
   ```

3. **Téléchargez le fichier généré** et nommez-le `index.html`.

4. **Créez un fichier zip** contenant ce fichier `index.html`.

---

# **Étape 3 : Déployer votre CV sur AWS Amplify**
---

1. **Accédez à AWS Amplify** :
   - Connectez-vous à votre compte AWS.
   - Accédez à la section **AWS Amplify** via la console AWS.

2. **Créer une nouvelle application hébergée** :
   - Cliquez sur **Get Started** dans la section "Host your web app".
   - Choisissez l'option **Déploiement manuel** ("Deploy without Git").
   - Sélectionnez **Déployer via ZIP**.

3. **Téléversez votre fichier zip** :
   - Cliquez sur **Télécharger** et sélectionnez le fichier zip que vous avez créé contenant votre fichier `index.html`.

4. **Déployez votre CV** :
   - AWS Amplify générera une URL que vous pourrez partager.

---

# **Étape 4 : Soumettre votre travail**
---

1. **Envoyez l'URL générée** par AWS Amplify une fois votre CV déployé.

---

# **Critères de réussite** :

- Le fichier HTML est généré avec le modèle fourni et toutes les informations complètes, y compris les compétences générées par l'IA.
- Le fichier est zippé et téléversé correctement sur AWS Amplify.
- Le CV est accessible via une URL fonctionnelle.
