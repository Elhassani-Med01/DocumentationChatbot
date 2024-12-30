Overview
========

Introduction
------------

Cette application interactive est conçue pour fournir des réponses pertinentes aux questions liées à l'ENSAM (École Nationale Supérieure d'Arts et Métiers). Le projet repose sur l'utilisation de modèles de langage avancés et une base de vecteurs pour une recherche efficace dans les données textuelles.

Le chatbot utilise Streamlit pour l'interface utilisateur et intègre des modèles de langage comme **Mistral** via l'API Ollama. Le système est capable de répondre en français et d'identifier clairement si une information n'est pas disponible dans la base de données.

---

Fonctionnalités principales
---------------------------

1. **Réponses personnalisées** : Le chatbot utilise des modèles de langage pour répondre aux questions spécifiques à l'ENSAM.
2. **Base de vecteurs** : Gestion des données à l'aide de Chroma pour une recherche rapide et efficace.
3. **Interface intuitive** : Application utilisateur construite avec Streamlit pour une expérience interactive.
4. **Support multilingue** : Bien que le chatbot réponde en français, l'architecture pourrait être adaptée à d'autres langues.
5. **Gestion des réponses inconnues** : Si une réponse ne peut être trouvée dans les données disponibles, l'utilisateur est informé clairement.

---

Architecture technique
-----------------------

1. **Langchain** :
   - Gestion des chaînes (RetrievalQA) pour relier le modèle LLM aux données vectorisées.
   - Utilisation des fonctionnalités d'intégration de texte pour diviser et traiter les données textuelles.

2. **Modèle LLM** : 
   - **Mistral**, utilisé via l'API Ollama, pour une compréhension et une génération avancées de texte.

3. **Chroma** :
   - Base de données vectorielle utilisée pour stocker et récupérer les textes pertinents en fonction des embeddings générés.

4. **Streamlit** :
   - Fournit une interface utilisateur simple et intuitive pour poser des questions et afficher les résultats.

---

Processus de fonctionnement
----------------------------

1. **Chargement des données** :
   - Les données textuelles sont lues à partir d'un fichier (ex. : `ProcessedData.txt`).
   - Le texte est découpé en morceaux via un algorithme de text-splitting (RecursiveCharacterTextSplitter).

2. **Création de la base de vecteurs** :
   - Si une base de vecteurs existe dans le dossier spécifié, elle est chargée.
   - Sinon, une nouvelle base de vecteurs est créée et persistée à l'aide de **Chroma**.

3. **Question/Réponse** :
   - Les questions sont saisies via l'interface utilisateur Streamlit.
   - Une chaîne RetrievalQA relie le modèle LLM à la base de vecteurs pour générer une réponse pertinente.
   - Les réponses incluent, si nécessaire, des informations sur les documents sources.

4. **Gestion des réponses inconnues** :
   - Si une question ne peut être répondue à partir des données, le chatbot informe clairement l'utilisateur.

---

Public cible
------------

- Étudiants et enseignants de l'ENSAM.
- Personnel administratif.
- Candidats et visiteurs externes intéressés par l'ENSAM.

---

Installation et exécution
--------------------------

1. **Prérequis** :
   - Python 3.8 ou supérieur.
   - Bibliothèques nécessaires : Langchain, Streamlit, dotenv, Chroma.

2. **Étapes d'installation** :
   - Clonez le dépôt du projet.
   - Installez les dépendances :
     ```
     pip install -r requirements.txt
     ```

3. **Exécution de l'application** :
   - Démarrez l'API Ollama en local.
   - Lancez l'application Streamlit :
     ```
     streamlit run app.py
     ```

---

Conclusion
----------

Ce projet constitue une solution innovante pour améliorer l'accès aux informations spécifiques à l'ENSAM. En combinant des technologies de pointe comme les modèles de langage, les embeddings vectoriels et une interface interactive, il offre une expérience utilisateur fluide et efficace.


