#### Étapes et commandes nécessaires pour installer **Amazon CodeWhisperer** dans notre environnement SageMaker Studio :

1. Activez l'environnement **studio** :
   ```bash
   conda activate studio
   ```

2. Installez l'extension **CodeWhisperer** pour JupyterLab :
   ```bash
   pip install amazon-codewhisperer-jupyterlab-ext
   ```

3. Activez l'extension dans Jupyter :
   ```bash
   jupyter server extension enable amazon_codewhisperer_jupyterlab_ext
   ```

4. Redémarrez le serveur Jupyter pour appliquer les modifications :
   ```bash
   restart-jupyter-server
   ```

Ces étapes devraient installer et activer **Amazon CodeWhisperer** dans votre environnement SageMaker Studio.


----------------
# Annexe : 
----------------


- Je vous propose quelques suggestions supplémentaires pour optimiser l'installation et l'utilisation d'Amazon CodeWhisperer dans SageMaker Studio :

1. **Vérifiez l'état de l'extension** :
   Avant de redémarrer le serveur Jupyter, vous pouvez vérifier si l'extension a bien été activée avec la commande suivante :
   ```bash
   jupyter server extension list
   ```
   Cela vous permettra de voir si **amazon_codewhisperer_jupyterlab_ext** est correctement installé et activé.

2. **Mettre à jour SageMaker Studio** :
   Assurez-vous que votre environnement SageMaker Studio est à jour. Vous pouvez utiliser cette commande pour mettre à jour tous les paquets conda dans votre environnement Studio :
   ```bash
   conda update --all
   ```

3. **Dépannage potentiel** :
   Si vous rencontrez des problèmes avec l'installation ou l'activation de l'extension, essayez de désactiver puis réactiver l'extension avec les commandes suivantes :
   ```bash
   jupyter server extension disable amazon_codewhisperer_jupyterlab_ext
   jupyter server extension enable amazon_codewhisperer_jupyterlab_ext
   ```

4. **Documentation et exemples** :
   Familiarisez-vous avec la documentation officielle de **CodeWhisperer** pour exploiter pleinement ses fonctionnalités, notamment l'autocomplétion et les suggestions intelligentes dans le cadre du développement de code en Python, JavaScript, ou d'autres langages pris en charge.

5. **Utiliser en combinaison avec d'autres outils AWS** :
   Intégrer **CodeWhisperer** avec d'autres services SageMaker, comme **SageMaker Experiments** ou **SageMaker Pipelines**, pourrait améliorer la productivité dans des environnements plus complexes, surtout lorsque vous travaillez avec des workflows de machine learning.

6. **Configurer les autorisations** :
   Assurez-vous que vous disposez des autorisations nécessaires dans AWS IAM pour utiliser **Amazon CodeWhisperer** dans SageMaker Studio. Si ce n'est pas le cas, vous devrez peut-être ajuster vos rôles IAM ou demander à un administrateur de les configurer.

En suivant ces recommandations, vous devriez pouvoir tirer le meilleur parti d'Amazon CodeWhisperer dans votre environnement SageMaker Studio !
