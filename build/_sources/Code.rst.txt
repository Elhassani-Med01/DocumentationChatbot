Code Documentation
==================

Cette section présente une explication détaillée du code source de l'application interactive chatbot ENSAM. Chaque partie est expliquée en profondeur pour aider à comprendre les concepts et les implémentations utilisées dans ce projet académique.

---


Chargement des variables d'environnement
-----------------------------------------

Le fichier `.env` contient des variables sensibles ou spécifiques à l'environnement, comme les clés API ou les chemins de fichiers critiques. La fonction `load_dotenv()` permet de les charger automatiquement dans l'environnement d'exécution, évitant de coder ces valeurs en dur dans le script. Cela améliore la sécurité et facilite la portabilité du code.

.. code-block:: python

    from dotenv import load_dotenv

    # Charger les variables d'environnement
    load_dotenv()

*Explication* :
- Le fichier `.env` peut contenir des variables comme `API_KEY` ou `BASE_URL`.
- La bibliothèque `dotenv` lit ces valeurs et les ajoute à l'environnement du programme.
- Cela permet de modifier les configurations sans toucher au code source.

---

Initialisation des modèles LLM et embeddings
--------------------------------------------

Le projet utilise un modèle de langage (LLM) et un modèle d'embedding pour traiter les textes. Ces modèles sont initialisés via l'API Ollama, qui est hébergée localement. Le modèle LLM est utilisé pour répondre aux questions, tandis que le modèle d'embedding convertit le texte en vecteurs numériques pour faciliter la recherche.

.. code-block:: python

    from langchain_community.llms import Ollama
    from langchain_community.embeddings import OllamaEmbeddings

    # Initialiser les modèles LLM et embeddings
    llm = Ollama(model="mistral", base_url="http://127.0.0.1:11434")
    embed_model = OllamaEmbeddings(model="mistral", base_url="http://127.0.0.1:11434")

*Explication détaillée* :
- **LLM** (Large Language Model) : Le modèle Mistral est capable de comprendre et de générer du texte en langage naturel.
- **Embeddings** : Le modèle transforme les textes en représentations vectorielles, qui sont nécessaires pour comparer ou rechercher des similarités entre les morceaux de texte.
- **base_url** : Spécifie l'URL où l'API Ollama est hébergée.

---

Lecture et traitement des données textuelles
--------------------------------------------

Le contenu d'un fichier texte contenant les données ENSAM est chargé et divisé en morceaux (chunks). Cela est nécessaire pour gérer efficacement de grands ensembles de données et pour s'assurer que chaque portion de texte est utilisable pour l'analyse et la recherche.

.. code-block:: python

    from langchain.text_splitter import RecursiveCharacterTextSplitter

    file_path = 'C:/Users/nelba/Desktop/Chatbot/Chatbot/ProcessedData.txt'

    # Lire le contenu du fichier texte
    with open(file_path, 'r', encoding='utf-8') as file:
        file_content = file.read()

    # Diviser le texte en morceaux
    text_splitter = RecursiveCharacterTextSplitter(chunk_size=1350, chunk_overlap=110)
    chunks = text_splitter.split_text(file_content)

*Explication détaillée* :
- **file_path** : Chemin vers le fichier contenant les données ENSAM.
- **RecursiveCharacterTextSplitter** : Une méthode efficace pour diviser un texte en morceaux de taille fixe, tout en permettant un chevauchement entre les morceaux pour conserver le contexte.
- **chunk_size** : La taille de chaque morceau en caractères.
- **chunk_overlap** : Le nombre de caractères qui se chevauchent entre deux morceaux consécutifs, pour éviter de perdre des informations importantes.

---

Gestion de la base de vecteurs
------------------------------

La base de vecteurs permet de stocker et d'effectuer des recherches dans les représentations vectorielles du texte. Le projet utilise Chroma, une bibliothèque spécialisée dans la gestion des bases de vecteurs.

.. code-block:: python

    from langchain.vectorstores import Chroma
    import os

    vector_store_path = 'C:/Users/nelba/Desktop/Chatbot/Chatbot/VDB'

    if not os.path.exists(vector_store_path):
        os.makedirs(vector_store_path)
        print("Dossier pour la base de vecteurs créé avec succès !")

    try:
        if os.listdir(vector_store_path):
            vector_store = Chroma(persist_directory=vector_store_path, embedding_function=embed_model)
            print("Base de vecteurs chargée avec succès !")
        else:
            vector_store = Chroma.from_texts(chunks, embed_model, persist_directory=vector_store_path)
            vector_store.persist()
            print("Base de vecteurs créée et sauvegardée avec succès !")
    except Exception as e:
        print(f"Une erreur s'est produite : {e}")

*Explication détaillée* :
- **Chroma** : Une bibliothèque pour créer, stocker et rechercher dans une base de vecteurs.
- **vector_store_path** : Chemin vers le dossier où les vecteurs sont sauvegardés.
- **vector_store.persist()** : Sauvegarde la base de vecteurs sur le disque pour une utilisation future.
- Gestion des exceptions : Gère les erreurs liées à l'accès ou à la création des fichiers.

---

Création de la chaîne de questions/réponses
-------------------------------------------

Une chaîne de questions/réponses (RetrievalQA) est créée pour relier le modèle LLM aux données vectorisées, permettant ainsi de répondre efficacement aux requêtes des utilisateurs.

.. code-block:: python

    from langchain.chains import RetrievalQA

    retriever = vector_store.as_retriever()

    # Configurer la chaîne RetrievalQA
    qa_chain = RetrievalQA.from_chain_type(
        llm=llm,
        chain_type="stuff",
        retriever=retriever,
        return_source_documents=True
    )

*Explication détaillée* :
- **RetrievalQA** : Une chaîne qui combine un modèle LLM et une base de vecteurs pour répondre aux questions.
- **retriever** : Permet d'extraire les morceaux de texte pertinents de la base de vecteurs.
- **return_source_documents** : Retourne les documents sources associés à chaque réponse, pour plus de transparence.

---

Interface utilisateur avec Streamlit
-------------------------------------

L'application utilise Streamlit pour offrir une interface interactive où les utilisateurs peuvent poser des questions et recevoir des réponses basées sur les données ENSAM.

.. code-block:: python

    import streamlit as st

    st.title("Application Interactive - Questions sur ENSAM")
    st.write("Posez une question et obtenez une réponse pertinente !")

    user_question = st.text_input("Votre question :", placeholder="Posez une question ici...")

    if st.button("Obtenir la réponse"):
        if user_question.strip():
            with st.spinner("Traitement de la question..."):
                try:
                    # Construire le prompt avec les instructions
                    prompt = f"""Tu es un assistant virtuel spécialisé dans les questions relatives à l'ENSAM. Répond en français uniquement en te basant sur les informations disponibles dans la base de données fournie. Si la réponse à une question n'est pas dans la base de données, indique clairement que tu ne disposes pas de l'information. Reste concis et précis dans tes réponses.

                            Question de l'utilisateur : {user_question}"""

                    # Obtenir la réponse
                    response = qa_chain({"query": prompt.strip()})
                    result = response.get("result", "").strip()
                    source_docs = response.get("source_documents", [])

                    if result:
                        st.success("Voici la réponse :")
                        st.write(result)
                        if source_docs:
                            st.subheader("Documents Source")
                            for i, doc in enumerate(source_docs, 1):
                                st.write(f"Source {i}: {doc.page_content}")
                    else:
                        st.warning("Désolé, je ne peux pas répondre à cette question en fonction des données fournies.")
                except Exception as e:
                    st.error(f"Une erreur s'est produite : {e}")
        else:
            st.error("Veuillez entrer une question valide.")

*Explication détaillée* :
- **Streamlit** : Bibliothèque Python pour créer des applications web interactives.
- **st.text_input** : Permet à l'utilisateur de saisir une question.
- **st.button** : Lance le traitement lorsque l'utilisateur clique sur le bouton.
- **st.spinner** : Affiche une animation pendant que la réponse est en cours de traitement.
- **Prompt personnalisé** : Donne des instructions spécifiques au chatbot pour garantir des réponses précises.

---

Conclusion
----------

Ce projet combine des technologies avancées telles que LangChain, Chroma, et Streamlit pour fournir une solution complète de questions/réponses basée sur des données spécifiques à l'ENSAM. Chaque section du code a été soigneusement conçue pour assurer une performance optimale et une facilité d'utilisation.
