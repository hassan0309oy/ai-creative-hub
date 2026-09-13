HASSAN FOOD — PLAN DE CONSTRUCTION DÉFINITIF

&nbsp;

Mission

&nbsp;

Construire Hassan Food, une application web/PWA gratuite de planification intelligente des repas.

&nbsp;

Le parcours principal doit être entièrement fonctionnel de bout en bout :

&nbsp;

Questionnaire adaptatif → contraintes utilisateur → génération du plan → recettes illustrées → calcul du budget → liste de courses agrégée → mode cuisine.

&nbsp;

L'objectif n'est pas de créer une simple maquette ou une interface de démonstration.

&nbsp;

Je veux une véritable application fonctionnelle, avec backend, base de données, authentification, logique métier, génération IA, stockage, gestion des erreurs, persistance des données et architecture évolutive.

&nbsp;

Toutes les fonctionnalités essentielles du parcours utilisateur doivent fonctionner réellement.

&nbsp;

---

&nbsp;

1. PRINCIPES NON NÉGOCIABLES

&nbsp;

1.1 Application gratuite

&nbsp;

Le parcours principal de Hassan Food doit être accessible gratuitement.

&nbsp;

Ne pas créer de paywall artificiel pour les fonctionnalités essentielles :

&nbsp;

- questionnaire ;

- génération de plans ;

- recettes ;

- liste de courses ;

- remplacement de repas ;

- favoris ;

- mode cuisine ;

- gestion du placard ;

- personnalisation.

&nbsp;

---

&nbsp;

1.2 Ne jamais exposer les clés API

&nbsp;

Toutes les clés/API keys doivent être stockées exclusivement dans les secrets/variables d'environnement du backend.

&nbsp;

Aucune clé ne doit :

&nbsp;

- apparaître dans le frontend ;

- être présente dans le code client ;

- être envoyée au navigateur ;

- être présente dans Git ;

- être affichée dans les logs ;

- être intégrée dans les prompts publics.

&nbsp;

Les clés actuellement communiquées précédemment sont considérées comme compromises et devront être régénérées avant leur utilisation réelle.

&nbsp;

---

&nbsp;

1.3 Ne jamais faire confiance à l'IA pour les calculs critiques

&nbsp;

L'IA peut :

&nbsp;

- comprendre les préférences ;

- proposer des recettes ;

- sélectionner des recettes existantes ;

- générer du contenu ;

- proposer des substitutions ;

- expliquer une recette.

&nbsp;

Mais l'IA ne doit jamais être la source de vérité pour :

&nbsp;

- les prix ;

- les totaux ;

- les quantités finales ;

- les calculs de portions ;

- les contraintes d'allergie ;

- la validation du budget.

&nbsp;

Ces éléments doivent être contrôlés par du code côté serveur.

&nbsp;

---

&nbsp;

2. ARCHITECTURE GÉNÉRALE

&nbsp;

Architecture cible :

&nbsp;

HASSAN FOOD

    │

    ▼

Frontend Lovable / TanStack Start

    │

    ▼

Backend serveur

    │

    ├── Auth

    ├── Database

    ├── Recipe Engine

    ├── Planning Engine

    ├── Price Engine

    ├── Shopping Engine

    ├── AI Provider Layer

    ├── Image Provider Layer

    ├── TTS Provider Layer

    ├── Search Provider Layer

    └── Memory/Vector Provider Layer

    │

    ▼

Supabase / Lovable Cloud

&nbsp;

Utiliser une architecture modulaire.

&nbsp;

Les services externes doivent être derrière des interfaces communes afin de pouvoir changer de fournisseur sans réécrire toute l'application.

&nbsp;

Exemple :

&nbsp;

providers/

    llm/

    image/

    prices/

    search/

    tts/

    vector/

    memory/

&nbsp;

---

&nbsp;

3. FONDATIONS

&nbsp;

Construire en premier :

&nbsp;

Interface

&nbsp;

Créer un système de design cohérent avec une identité culinaire chaleureuse et moderne.

&nbsp;

Contraintes :

&nbsp;

- pas de violet comme couleur dominante ;

- typographie soignée ;

- interface mobile-first ;

- cartes repas visuelles ;

- grosses images culinaires ;

- navigation simple ;

- animations légères ;

- excellente lisibilité ;

- boutons suffisamment grands pour mobile ;

- accessibilité correcte ;

- pas de mode sombre pour la première version.

&nbsp;

L'interface doit fonctionner parfaitement sur :

&nbsp;

- smartphone ;

- tablette ;

- ordinateur.

&nbsp;

---

&nbsp;

4. AUTHENTIFICATION

&nbsp;

Créer :

&nbsp;

- inscription par email/mot de passe ;

- connexion ;

- déconnexion ;

- récupération de mot de passe ;

- connexion Google ;

- profil utilisateur.

&nbsp;

Les données personnelles doivent être isolées par utilisateur.

&nbsp;

Utiliser les mécanismes de sécurité de Supabase, notamment les politiques RLS lorsque pertinentes.

&nbsp;

---

&nbsp;

5. BASE DE DONNÉES

&nbsp;

Créer une vraie structure relationnelle.

&nbsp;

Tables principales :

&nbsp;

users

profiles

meal_preferences

diet_preferences

allergies

disliked_foods

favorite_foods

cuisines

equipment

pantry_items

stores

store_locations

products

ingredients

ingredient_aliases

allergens

product_prices

recipes

recipe_ingredients

recipe_steps

recipe_images

plans

plan_meals

shopping_lists

shopping_items

favorites

feedback

generation_runs

generation_events

user_history

&nbsp;

Prévoir les relations, index, contraintes et timestamps nécessaires.

&nbsp;

---

&nbsp;

6. QUESTIONNAIRE ADAPTATIF

&nbsp;

Créer un moteur de questionnaire piloté par configuration.

&nbsp;

Chaque question doit pouvoir posséder :

&nbsp;

id

type

title

description

options

required

validation

condition

order

&nbsp;

Le questionnaire doit être réellement adaptatif.

&nbsp;

Exemple :

&nbsp;

Si l'utilisateur ne sélectionne pas le petit-déjeuner, ne pas lui poser de questions spécifiques au petit-déjeuner.

&nbsp;

Si l'utilisateur sélectionne végétalien, masquer les choix incompatibles.

&nbsp;

Si l'utilisateur indique ne pas posséder de four, ne pas proposer ensuite des recettes nécessitant obligatoirement un four.

&nbsp;

---

&nbsp;

Informations à collecter

&nbsp;

Le questionnaire doit couvrir au minimum :

&nbsp;

1. langue ;

2. pays ;

3. devise ;

4. localisation facultative ;

5. magasin préféré ;

6. durée du plan ;

7. petit-déjeuner ;

8. déjeuner ;

9. dîner ;

10. nombre de repas souhaités ;

11. nombre de personnes ;

12. adultes/enfants si nécessaire ;

13. budget ;

14. régime alimentaire ;

15. allergies ;

16. aliments interdits ;

17. aliments aimés ;

18. cuisines préférées ;

19. équipement disponible ;

20. temps maximal de préparation ;

21. placard ;

22. objectifs ;

23. niveau de variété ;

24. batch cooking/repas réutilisables ;

25. récapitulatif final modifiable.

&nbsp;

---

&nbsp;

7. NOMBRE EXACT DE REPAS

&nbsp;

Le moteur doit respecter exactement la demande.

&nbsp;

Exemple :

&nbsp;

Utilisateur :

&nbsp;

7 jours

Petit-déjeuner : 7

Déjeuner : 7

Dîner : 7

&nbsp;

Résultat :

&nbsp;

21 repas

&nbsp;

Si l'utilisateur demande :

&nbsp;

5 petits-déjeuners

3 déjeuners

7 dîners

&nbsp;

le système doit générer exactement :

&nbsp;

15 repas

&nbsp;

Ne jamais générer arbitrairement plus ou moins de repas.

&nbsp;

Permettre également :

&nbsp;

- répétition volontaire d'un repas ;

- petit-déjeuner identique plusieurs jours ;

- repas différents chaque jour ;

- restes ;

- batch cooking ;

- repas verrouillés ;

- repas gratuits/pantry-only si applicable.

&nbsp;

---

&nbsp;

8. CONTRAINTES DURES ET PRÉFÉRENCES SOUPLES

&nbsp;

Séparer strictement deux catégories.

&nbsp;

Contraintes dures

&nbsp;

Elles ne doivent jamais être violées :

&nbsp;

- allergies ;

- régime ;

- aliments interdits ;

- équipement indisponible ;

- nombre de personnes ;

- type de repas ;

- contraintes explicitement obligatoires.

&nbsp;

Préférences souples

&nbsp;

Elles peuvent être optimisées :

&nbsp;

- cuisine préférée ;

- variété ;

- temps idéal ;

- budget idéal ;

- répétition ;

- difficulté ;

- objectif nutritionnel ;

- anti-gaspillage.

&nbsp;

Une allergie doit toujours avoir priorité sur une préférence culinaire.

&nbsp;

---

&nbsp;

9. BASE DE RECETTES

&nbsp;

Créer une vraie base structurée de recettes.

&nbsp;

Chaque recette doit pouvoir contenir :

&nbsp;

title

description

meal_type

servings

prep_time

cook_time

total_time

difficulty

cuisine

tags

diet

allergens

equipment

ingredients

steps

nutrition

storage

substitutions

image

&nbsp;

Les ingrédients doivent être structurés individuellement.

&nbsp;

Exemple :

&nbsp;

ingredient_id

name

quantity

unit

optional

category

allergens

&nbsp;

---

&nbsp;

10. VALIDATEUR DE RECETTES

&nbsp;

Toute recette produite ou modifiée par l'IA doit être validée avant utilisation.

&nbsp;

Pipeline :

&nbsp;

IA

 ↓

JSON structuré

 ↓

Validation du schéma

 ↓

Validation ingrédients

 ↓

Validation allergènes

 ↓

Validation régime

 ↓

Validation équipement

 ↓

Validation temps

 ↓

Validation portions

 ↓

Validation disponibilité/prix

 ↓

Recette acceptée

&nbsp;

Une recette qui échoue à une contrainte bloquante ne doit pas être affichée comme valide.

&nbsp;

---

&nbsp;

11. MOTEUR DE GÉNÉRATION DU PLAN

&nbsp;

C'est le cœur de Hassan Food.

&nbsp;

Ordre obligatoire :

&nbsp;

1. Lire les réponses utilisateur

2. Normaliser les données

3. Construire les contraintes

4. Charger les recettes candidates

5. Filtrer les contraintes dures

6. Vérifier allergies/régime

7. Vérifier équipement

8. Vérifier temps

9. Récupérer les prix disponibles

10. Calculer le coût réel estimé

11. Calculer le coût par portion

12. Construire plusieurs combinaisons candidates

13. Optimiser le budget

14. Optimiser la variété

15. Optimiser l'utilisation du placard

16. Optimiser l'anti-gaspillage

17. Éviter les répétitions inutiles

18. Vérifier le budget final

19. Faire une seconde validation des contraintes

20. Sauvegarder le plan

21. Générer/compléter les textes nécessaires

22. Générer les images manquantes

23. Construire la liste de courses

24. Afficher le résultat

&nbsp;

Le moteur doit être déterministe autant que possible et testable.

&nbsp;

---

&nbsp;

12. OPTIMISATION

&nbsp;

Utiliser une stratégie de recherche gloutonne + amélioration locale ou une stratégie équivalente.

&nbsp;

Objectifs :

&nbsp;

minimiser coût

+

maximiser variété

+

maximiser utilisation du placard

+

minimiser gaspillage

+

respecter contraintes

&nbsp;

Les contraintes dures ont toujours priorité.

&nbsp;

Si le budget est dépassé :

&nbsp;

repérer les repas coûteux

↓

chercher alternatives compatibles

↓

remplacer

↓

recalculer

↓

répéter

&nbsp;

Si aucune solution n'existe :

&nbsp;

ne pas inventer un plan.

&nbsp;

Afficher clairement le problème et proposer :

&nbsp;

- augmenter le budget ;

- diminuer le nombre de repas ;

- augmenter la durée ;

- utiliser davantage le placard ;

- accepter davantage de répétitions ;

- modifier certaines préférences non obligatoires.

&nbsp;

---

&nbsp;

13. SYSTÈME DE PRIX

&nbsp;

Le prix est une partie critique de l'application.

&nbsp;

Créer une architecture de fournisseurs interchangeables.

&nbsp;

Chaque prix doit contenir au minimum :

&nbsp;

product_id

store_id

price

currency

source

fetched_at

confidence

&nbsp;

Le système doit distinguer :

&nbsp;

- prix actuel ;

- dernier prix connu ;

- prix estimé.

&nbsp;

Ne jamais afficher une estimation comme un prix réel.

&nbsp;

---

&nbsp;

IMPORTANT

&nbsp;

Tavily et Firecrawl ne doivent pas être considérés comme une source universelle de vérité pour les prix.

&nbsp;

Ils peuvent être utilisés comme outils complémentaires pour rechercher/extraire des informations publiques lorsque cela est techniquement et légalement approprié.

&nbsp;

La source principale des prix doit être conçue pour accepter :

&nbsp;

- API de catalogue ;

- flux de données ;

- fournisseurs de prix ;

- partenariats ;

- données autorisées.

&nbsp;

Le système doit pouvoir ajouter un nouveau fournisseur sans modifier le moteur de planification.

&nbsp;

---

&nbsp;

14. PRODUITS ET OPEN FOOD FACTS

&nbsp;

Utiliser Open Food Facts comme source complémentaire pour :

&nbsp;

- produits ;

- marques ;

- codes-barres ;

- ingrédients ;

- allergènes ;

- informations nutritionnelles.

&nbsp;

Ne pas considérer Open Food Facts comme la source principale des prix de supermarché.

&nbsp;

---

&nbsp;

15. GÉOLOCALISATION ET MAGASINS

&nbsp;

La localisation doit être facultative.

&nbsp;

Avec autorisation :

&nbsp;

position utilisateur

↓

recherche magasins proches

↓

sélection magasin

&nbsp;

Si l'utilisateur refuse :

&nbsp;

pays

+

ville

+

code postal

&nbsp;

peuvent être utilisés.

&nbsp;

Le système doit pouvoir identifier un magasin précis lorsque possible :

&nbsp;

enseigne

+

localisation

+

adresse

+

identifiant magasin

&nbsp;

---

&nbsp;

16. LISTE DE COURSES

&nbsp;

Créer une liste par repas et une liste globale.

&nbsp;

La liste globale doit :

&nbsp;

- fusionner les ingrédients identiques ;

- convertir les unités lorsque possible ;

- tenir compte des portions ;

- déduire les quantités disponibles dans le placard ;

- distinguer quantité nécessaire et quantité réellement achetée ;

- tenir compte des conditionnements ;

- associer les produits du magasin ;

- afficher le prix ;

- afficher le total ;

- classer les produits par rayon.

&nbsp;

Exemple :

&nbsp;

Besoin recette : 400 g riz

Produit magasin : paquet 1 kg

Quantité achetée : 1 paquet

&nbsp;

Le coût doit correspondre au produit réellement acheté, pas simplement aux grammes utilisés.

&nbsp;

---

&nbsp;

17. PLACARD

&nbsp;

L'utilisateur doit pouvoir enregistrer :

&nbsp;

- riz ;

- pâtes ;

- huile ;

- épices ;

- conserves ;

- farine ;

- produits surgelés ;

- etc.

&nbsp;

Chaque élément peut avoir :

&nbsp;

nom

quantité

unité

date facultative

&nbsp;

Lors de la génération :

&nbsp;

besoin recette

-

quantité placard

=

quantité à acheter

&nbsp;

Le système doit mémoriser les informations du placard lorsque l'utilisateur les confirme.

&nbsp;

---

&nbsp;

18. INTERFACE DU PLAN

&nbsp;

Afficher les repas :

&nbsp;

Jour 1

Petit-déjeuner

Déjeuner

Dîner

&nbsp;

Jour 2

Petit-déjeuner

Déjeuner

Dîner

...

&nbsp;

Chaque carte doit afficher :

&nbsp;

- image ;

- nom ;

- type de repas ;

- coût ;

- coût/personne ;

- durée ;

- portions ;

- tags ;

- indication éventuelle « déjà au placard ».

&nbsp;

Afficher également :

&nbsp;

- budget total ;

- montant estimé ;

- budget restant ;

- nombre de repas.

&nbsp;

---

&nbsp;

19. FICHE RECETTE

&nbsp;

Lorsqu'on clique sur un repas, ouvrir une page dédiée.

&nbsp;

Contenu :

&nbsp;

Image principale

Titre

Type de repas

Coût total

Coût par personne

Portions

Temps

Difficulté

Tags

&nbsp;

Description

&nbsp;

Ingrédients

&nbsp;

Ingrédients déjà au placard

&nbsp;

Substitutions

&nbsp;

Étapes numérotées

&nbsp;

Minuteurs

&nbsp;

Mode cuisine

&nbsp;

Ajouter aux favoris

&nbsp;

Remplacer le repas

Verrouiller le repas

&nbsp;

---

&nbsp;

20. REMPLACEMENT D'UN REPAS

&nbsp;

Le bouton « Remplacer » doit générer des alternatives compatibles.

&nbsp;

Il ne doit pas régénérer inutilement tout le plan.

&nbsp;

Lorsqu'un repas est remplacé :

&nbsp;

ancien repas

↓

nouvelle recette compatible

↓

recalcul du prix

↓

recalcul du budget

↓

recalcul de la liste de courses

&nbsp;

Conserver l'historique du remplacement.

&nbsp;

Éviter de proposer immédiatement la même recette.

&nbsp;

---

&nbsp;

21. FAVORIS ET FEEDBACK

&nbsp;

Permettre :

&nbsp;

- aimer une recette ;

- ne pas aimer ;

- ajouter aux favoris ;

- retirer des favoris ;

- indiquer pourquoi une recette ne convient pas si pertinent.

&nbsp;

Ces données pourront influencer les générations futures.

&nbsp;

---

&nbsp;

22. MODE CUISINE

&nbsp;

Créer un mode cuisine plein écran.

&nbsp;

Une seule étape visible à la fois.

&nbsp;

Fonctions :

&nbsp;

- étape actuelle ;

- étape suivante ;

- étape précédente ;

- compteur ;

- minuteur ;

- portions recalculées ;

- ingrédients accessibles ;

- progression ;

- fin de recette.

&nbsp;

Prévoir l'utilisation sur mobile.

&nbsp;

---

&nbsp;

23. GESTION DES JOBS

&nbsp;

Les générations longues doivent fonctionner avec un système de jobs.

&nbsp;

États :

&nbsp;

QUEUED

PROCESSING

COMPLETED

PARTIAL

FAILED

CANCELLED

&nbsp;

Exemple :

&nbsp;

Utilisateur clique « Générer »

↓

job créé

↓

QUEUED

↓

PROCESSING

↓

recettes sélectionnées

↓

prix calculés

↓

images générées

↓

liste créée

↓

COMPLETED

&nbsp;

L'interface doit afficher une progression.

&nbsp;

Si une étape échoue, ne pas perdre les données déjà produites.

&nbsp;

---

&nbsp;

24. VERSIONNAGE DES PLANS

&nbsp;

Chaque plan doit pouvoir posséder une version.

&nbsp;

Exemple :

&nbsp;

Plan 001

Version 1

Version 2

Version 3

&nbsp;

Un remplacement de repas ou une modification importante ne doit pas détruire silencieusement l'état précédent.

&nbsp;

Permettre à terme :

&nbsp;

- annulation ;

- restauration ;

- historique.

&nbsp;

---

&nbsp;

25. OPENAI — GÉNÉRATION D'IMAGES

&nbsp;

Intégrer l'API officielle OpenAI côté serveur.

&nbsp;

Utiliser :

&nbsp;

gpt-image-2

&nbsp;

Le service doit fonctionner comme un provider indépendant :

&nbsp;

ImageProvider

    └── OpenAIImageProvider

&nbsp;

Lorsqu'une recette ne possède pas d'image :

&nbsp;

Recipe

↓

ImageService

↓

image déjà disponible ?

   ↓ oui → utiliser

   ↓ non

OpenAI

↓

génération

↓

stockage

↓

URL

↓

recipe_images

&nbsp;

Les images doivent être mises en cache et réutilisées.

&nbsp;

Ne pas générer une nouvelle image à chaque affichage.

&nbsp;

Prévoir :

&nbsp;

- gestion des erreurs ;

- quotas ;

- timeout ;

- retry contrôlé ;

- placeholder ;

- possibilité de régénérer.

&nbsp;

L'image doit être culinaire, réaliste et sans texte intégré dans l'image.

&nbsp;

---

&nbsp;

26. HUGGING FACE

&nbsp;

Intégrer Hugging Face Inference Providers comme couche de modèles de langage configurable.

&nbsp;

Prévoir :

&nbsp;

HF_TOKEN

HF_MODEL

HF_PROVIDER

&nbsp;

avec :

&nbsp;

provider = auto

&nbsp;

par défaut.

&nbsp;

Le token doit rester côté serveur.

&nbsp;

Utiliser le SDK officiel :

&nbsp;

@huggingface/inference

&nbsp;

L'architecture doit permettre de changer :

&nbsp;

- modèle ;

- provider ;

- configuration ;

&nbsp;

sans modifier le reste de l'application.

&nbsp;

---

&nbsp;

27. AUTRES MODÈLES IA

&nbsp;

Prévoir une architecture multi-provider permettant également :

&nbsp;

- OpenAI ;

- Google Gemini ;

- Hugging Face.

&nbsp;

Exemple :

&nbsp;

LLMProvider

 ├── HuggingFaceProvider

 ├── OpenAIProvider

 └── GeminiProvider

&nbsp;

Le moteur Hassan Food ne doit pas dépendre directement d'un fournisseur particulier.

&nbsp;

---

&nbsp;

28. AGENT IA

&nbsp;

Créer un agent backend capable d'utiliser des outils contrôlés.

&nbsp;

Exemples de tools :

&nbsp;

search_recipes

get_recipe

find_substitution

calculate_price

regenerate_meal

update_pantry

scale_recipe

explain_recipe

generate_recipe_image

&nbsp;

L'agent ne doit pas avoir accès directement aux secrets.

&nbsp;

Il appelle uniquement les outils autorisés.

&nbsp;

Les outils critiques doivent eux-mêmes effectuer les validations.

&nbsp;

---

&nbsp;

29. RECHERCHE WEB

&nbsp;

Prévoir des providers :

&nbsp;

SearchProvider

 ├── Tavily

 └── Firecrawl

&nbsp;

Utilisation pour :

&nbsp;

- recherche d'informations ;

- enrichissement de données ;

- extraction de contenu public lorsque approprié.

&nbsp;

Ne pas dépendre de la recherche Web pour le fonctionnement normal du planificateur.

&nbsp;

---

&nbsp;

30. MÉMOIRE ET RECHERCHE SÉMANTIQUE

&nbsp;

Pour la première version :

&nbsp;

Supabase PostgreSQL + pgvector doit être privilégié afin de limiter la complexité.

&nbsp;

Prévoir ensuite des adaptateurs permettant éventuellement d'ajouter :

&nbsp;

- Mem0 ;

- Qdrant ;

- Pinecone.

&nbsp;

Ne pas utiliser simultanément Pinecone et Qdrant sans raison technique.

&nbsp;

Architecture :

&nbsp;

VectorProvider

 ├── SupabaseVectorProvider

 ├── QdrantProvider

 └── PineconeProvider

&nbsp;

Mem0 peut être utilisé pour la mémoire personnalisée avancée :

&nbsp;

- préférences ;

- habitudes ;

- goûts ;

- historique ;

- feedback.

&nbsp;

---

&nbsp;

31. VOIX

&nbsp;

Prévoir une interface :

&nbsp;

TTSProvider

&nbsp;

avec :

&nbsp;

- ElevenLabs ;

- Kokoro ;

- Piper.

&nbsp;

Utilisation principale :

&nbsp;

lecture vocale des étapes en mode cuisine.

&nbsp;

Si le service vocal est indisponible, le mode cuisine doit continuer à fonctionner normalement en texte.

&nbsp;

---

&nbsp;

32. RUNWAY

&nbsp;

Runway est optionnel.

&nbsp;

Ne pas le mettre au cœur du parcours principal.

&nbsp;

Il pourra être ajouté ultérieurement pour :

&nbsp;

- vidéos de recettes ;

- contenus promotionnels ;

- vidéos culinaires générées.

&nbsp;

---

&nbsp;

33. E2B

&nbsp;

E2B est optionnel.

&nbsp;

Ne pas l'utiliser pour les fonctionnalités normales de Hassan Food.

&nbsp;

Il pourra être ajouté plus tard pour des agents nécessitant l'exécution contrôlée de code.

&nbsp;

---

&nbsp;

34. POWERPOINT

&nbsp;

PptxGenJS et Microsoft Graph sont optionnels.

&nbsp;

Ils ne font pas partie du parcours principal.

&nbsp;

Ils pourront être ajoutés ultérieurement si Hassan Food doit générer/exporter des présentations.

&nbsp;

---

&nbsp;

35. FALLBACKS

&nbsp;

Aucun service externe ne doit être un point de défaillance unique.

&nbsp;

Exemples :

&nbsp;

IA

&nbsp;

Hugging Face

↓ erreur

OpenAI/Gemini

↓ erreur

message d'erreur propre

&nbsp;

Voix

&nbsp;

ElevenLabs

↓ erreur

Kokoro/Piper

↓ erreur

texte uniquement

&nbsp;

Images

&nbsp;

OpenAI

↓ erreur

image existante

↓

placeholder

&nbsp;

Prix

&nbsp;

prix actuel

↓ indisponible

dernier prix connu

↓ indisponible

prix estimatif clairement marqué

&nbsp;

Le reste de l'application doit continuer à fonctionner.

&nbsp;

---

&nbsp;

36. GESTION DES ERREURS

&nbsp;

Prévoir explicitement :

&nbsp;

- chargement ;

- génération ;

- succès ;

- succès partiel ;

- erreur ;

- hors ligne ;

- prix obsolète ;

- aucun plan possible ;

- API indisponible ;

- timeout ;

- image indisponible ;

- magasin introuvable ;

- localisation refusée.

&nbsp;

Les messages doivent être compréhensibles pour l'utilisateur.

&nbsp;

---

&nbsp;

37. PWA

&nbsp;

Transformer Hassan Food en PWA.

&nbsp;

Prévoir :

&nbsp;

- installation ;

- cache ;

- fonctionnement hors ligne de la liste de courses ;

- persistance des cases cochées ;

- consultation des recettes déjà chargées ;

- synchronisation lorsque la connexion revient.

&nbsp;

---

&nbsp;

38. ADMINISTRATION

&nbsp;

Créer un espace admin sécurisé permettant de gérer :

&nbsp;

- recettes ;

- ingrédients ;

- allergènes ;

- équipements ;

- produits ;

- magasins ;

- prix ;

- images ;

- prompts ;

- modèles IA ;

- providers ;

- générations ;

- erreurs ;

- imports CSV ;

- statistiques.

&nbsp;

---

&nbsp;

39. ANALYTICS

&nbsp;

Prévoir des événements tels que :

&nbsp;

onboarding_started

questionnaire_completed

generation_started

generation_completed

generation_failed

recipe_opened

meal_replaced

recipe_liked

recipe_disliked

shopping_list_opened

shopping_item_checked

plan_regenerated

cooking_mode_started

recipe_completed

&nbsp;

Ne pas envoyer les réponses sensibles brutes du questionnaire dans les analytics.

&nbsp;

---

&nbsp;

40. SÉCURITÉ

&nbsp;

Prévoir :

&nbsp;

- authentification sécurisée ;

- RLS ;

- validation côté serveur ;

- validation des entrées ;

- rate limiting ;

- secrets uniquement côté serveur ;

- protection des endpoints ;

- séparation admin/utilisateur ;

- suppression de compte ;

- export des données lorsque nécessaire ;

- gestion du consentement pour localisation et analytics.

&nbsp;

---

&nbsp;

41. PERFORMANCE

&nbsp;

Ne pas bloquer l'interface pendant les longues générations.

&nbsp;

Utiliser :

&nbsp;

- jobs asynchrones ;

- cache ;

- pagination ;

- lazy loading des images ;

- compression ;

- stockage CDN ;

- index DB ;

- traitement par étapes.

&nbsp;

Les images doivent être mises en cache.

&nbsp;

Les prix doivent également disposer d'un cache par magasin/produit.

&nbsp;

---

&nbsp;

42. TESTS

&nbsp;

Créer des tests pour les parties critiques.

&nbsp;

Tester notamment :

&nbsp;

Allergies

&nbsp;

Une recette contenant un allergène interdit doit être rejetée.

&nbsp;

Régime

&nbsp;

Une recette incompatible avec le régime doit être rejetée.

&nbsp;

Budget

&nbsp;

Le plan final doit respecter la contrainte lorsque cela est mathématiquement possible.

&nbsp;

Nombre de repas

&nbsp;

La quantité finale doit correspondre exactement à la demande.

&nbsp;

Placard

&nbsp;

Les ingrédients disponibles doivent être correctement déduits.

&nbsp;

Liste de courses

&nbsp;

Les doublons doivent être correctement fusionnés.

&nbsp;

Portions

&nbsp;

Les quantités doivent être recalculées correctement.

&nbsp;

Remplacement

&nbsp;

Le budget et la liste doivent être recalculés après remplacement.

&nbsp;

Prix

&nbsp;

Un prix absent ne doit jamais être inventé.

&nbsp;

---

&nbsp;

43. ORDRE DE CONSTRUCTION

&nbsp;

Construire dans cet ordre :

&nbsp;

PHASE 1

&nbsp;

Fondations :

&nbsp;

- design system ;

- frontend ;

- Supabase ;

- authentification ;

- base de données ;

- sécurité ;

- Storage.

&nbsp;

PHASE 2

&nbsp;

Questionnaire adaptatif complet.

&nbsp;

PHASE 3

&nbsp;

Base de recettes + ingrédients + allergènes + équipements + validation.

&nbsp;

PHASE 4

&nbsp;

Moteur de planification et optimisation.

&nbsp;

PHASE 5

&nbsp;

Affichage du plan + fiches recettes + remplacement + favoris.

&nbsp;

PHASE 6

&nbsp;

Liste de courses + placard.

&nbsp;

PHASE 7

&nbsp;

Mode cuisine.

&nbsp;

À ce stade, l'application doit déjà fonctionner de bout en bout sans dépendre de toutes les APIs secondaires.

&nbsp;

PHASE 8

&nbsp;

Intégration IA :

&nbsp;

- Hugging Face ;

- OpenAI ;

- Gemini.

&nbsp;

PHASE 9

&nbsp;

Génération d'images avec OpenAI.

&nbsp;

PHASE 10

&nbsp;

Prix réels + fournisseurs de catalogues/prix.

&nbsp;

PHASE 11

&nbsp;

Mémoire avancée + recherche sémantique.

&nbsp;

PHASE 12

&nbsp;

Voix.

&nbsp;

PHASE 13

&nbsp;

Recherche Web.

&nbsp;

PHASE 14

&nbsp;

Runway, E2B, PowerPoint et autres fonctionnalités secondaires.

&nbsp;

---

&nbsp;

44. RÈGLE FINALE DE CONSTRUCTION

&nbsp;

Ne pas créer une fausse fonctionnalité uniquement pour donner l'impression que l'application est terminée.

&nbsp;

Si une fonctionnalité n'est pas encore connectée à son backend, l'indiquer clairement et la laisser prête à être branchée.

&nbsp;

Chaque fonctionnalité déclarée comme terminée doit être réellement fonctionnelle.

&nbsp;

Le résultat attendu est une application Hassan Food complète, cohérente, maintenable et évolutive, et non une simple démonstration visuelle.

&nbsp;

Construire étape par étape et vérifier chaque phase avant de passer à la suivante.

&nbsp;

Le cœur de l'application doit rester indépendant des fournisseurs externes : si Hugging Face, OpenAI, Gemini, ElevenLabs, Tavily, Firecrawl, Pinecone, Qdrant ou un autre service devient indisponible, Hassan Food doit conserver autant de fonctionnalités que possible.

&nbsp;

Priorité absolue :

&nbsp;

fiabilité du moteur de planification + exactitude des contraintes + exactitude des calculs + liste de courses + expérience utilisateur.