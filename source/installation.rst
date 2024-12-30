Installation
============

Cette section explique comment installer et configurer le projet chatbot ENSAM sur votre machine locale.

---

Prérequis
---------

Avant de commencer, assurez-vous que votre environnement respecte les exigences suivantes :

- **Python** : Version 3.8 ou supérieure.
- **Pip** : Gestionnaire de paquets Python installé.
- **Virtualenv** (optionnel) : Pour isoler l'environnement du projet.
- **Sphinx** : Pour générer la documentation (facultatif si vous ne travaillez pas sur la documentation).

---

Étapes d'installation
----------------------

1. **Cloner le dépôt du projet**

   Clonez le dépôt Git du projet sur votre machine locale :

   .. code-block:: bash

      git clone https://github.com/Elhassani-Med01/Ensam-AI-chatbot-by-Elhassani-Mohamed-et-Romaissae-Belhadj.git

   

2. **Naviguer dans le répertoire du projet**

   Accédez au répertoire cloné :

   .. code-block:: bash

      cd chatbot-ensam

3. **Créer un environnement virtuel (recommandé)**

   Créez un environnement virtuel pour isoler les dépendances :

   .. code-block:: bash

      python -m venv venv

   Activez l'environnement virtuel :

   - Sous Windows :

     .. code-block:: bash

        venv\Scripts\activate

   - Sous macOS/Linux :

     .. code-block:: bash

        source venv/bin/activate

4. **Installer les dépendances**

   Installez les bibliothèques requises en utilisant le fichier `requirements.txt` :

   .. code-block:: bash

      pip install -r requirements.txt


6. **Lancer l'application**

   Démarrez l'application Streamlit :

   .. code-block:: bash

      streamlit run app.py

   Cela ouvrira l'application dans votre navigateur par défaut.

---

Problèmes courants
-------------------

1. **Dépendances non installées :**
   Si une bibliothèque est manquante, vérifiez que vous avez bien exécuté la commande :
   
   .. code-block:: bash

      pip install -r requirements.txt

2. **Clé API manquante ou invalide :**
   Vérifiez le contenu du fichier `.env` et assurez-vous que toutes les variables nécessaires sont définies correctement.

3. **Port déjà utilisé :**
   Si le port utilisé par Streamlit est déjà occupé, lancez l'application sur un autre port :

   .. code-block:: bash

      streamlit run app.py --server.port=8502

---

Conclusion
----------

Une fois ces étapes terminées, votre chatbot ENSAM sera opérationnel. Si vous rencontrez des difficultés,  ouvrez une issue sur le dépôt GitHub.
