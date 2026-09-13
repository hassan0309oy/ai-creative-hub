# Hassan Food — plan de construction

Application gratuite de planification de repas : questionnaire adaptatif → plan de repas → fiches recettes illustrées → liste de courses agrégée → mode cuisine.

## Avertissement important sur les clés

Les clés que vous avez collées dans le message (OpenAI, Gemini, Runway, ElevenLabs, Pinecone, E2B, Tavily, Firecrawl, Hugging Face) sont maintenant visibles dans l'historique de conversation. **Il faut les régénérer chez chaque fournisseur**, puis les enregistrer via le formulaire sécurisé de Lovable. Elles ne seront jamais écrites dans le code ni accessibles depuis le navigateur.

## Étape 1 — Fondations (ce que je construis d'abord)

- Système de design complet (identité culinaire chaleureuse, pas de violet, typographie soignée), pas de mode sombre.
- Backend Lovable Cloud : comptes (email/mot de passe + Google), base de données, stockage d'images.
- Tables : profils, préférences repas, régime/allergies, placard, magasins, produits/prix, recettes, ingrédients, plans, repas planifiés, listes de courses, favoris, retours, historique de génération. Accès strictement limité à chaque utilisateur.
- Landing page + parcours d'inscription.

## Étape 2 — Questionnaire adaptatif

Moteur de questions piloté par configuration (chaque question a un type, des options, une condition d'affichage, une validation). Localisation optionnelle, pays/devise, magasin, durée, choix indépendant petit-déjeuner/déjeuner/dîner et nombres, foyer, budget, régime, allergies (bloquantes), goûts, équipement, temps, placard, objectifs, puis récapitulatif modifiable.

## Étape 3 — Moteur de génération (côté serveur)

Ordre imposé : contraintes → recettes candidates → exclusions bloquantes → prix → coût par portion → combinaisons → optimisation (coût, variété, anti-gaspillage) → déduction du placard → contrôle du budget → seconde vérification allergies/régime → sauvegarde → textes → images → liste de courses.
Les prix et les calculs sont faits en code, jamais par l'IA. Progression affichée pendant la génération. Message clair et propositions d'ajustement si aucun plan ne tient dans le budget.

## Étape 4 — Résultat, recette, courses, cuisine

- Plan groupé par jour et catégorie, budget restant.
- Fiche repas : image, coût total et par personne, portions, durée, ingrédients (marqués « déjà au placard »), substitutions, étapes numérotées, minuteurs, remplacer le repas, verrouiller, favoris.
- Liste de courses par repas + globale : agrégation, rayons, quantité d'achat, prix unitaire, total, cases cochées persistantes, ajout manuel, partage, utilisable hors-ligne.
- Mode cuisine plein écran : une étape à la fois, minuteurs, portions recalculées.

## Étape 5 — Prix magasins

Système de fournisseurs de prix interchangeables avec cache : source, date, niveau de confiance affichés. Repli sur des prix moyens explicitement marqués « estimatifs ». Enrichissement produits via Open Food Facts.

## Étape 6 — Services externes demandés

Chacun est ajouté derrière une interface unique afin de pouvoir changer de fournisseur sans toucher au reste de l'app :

- **Images de recettes** : OpenAI (gpt-image-2) comme moteur principal, appelé automatiquement par l'assistant quand une image manque ; aperçu et téléchargement. Gestion des erreurs et des quotas.
- **Modèles de langage** : Hugging Face Inference Providers (jeton sécurisé, choix du modèle et du fournisseur, « auto » par défaut) + OpenAI + Gemini, sélectionnables.
- **Recherche web** : Tavily et Firecrawl pour récupérer des informations produits/prix publiques.
- **Mémoire et recherche sémantique** : Pinecone / Qdrant / Mem0 pour l'historique de goûts et la recherche de recettes similaires.
- **Voix** : ElevenLabs pour lire les étapes en mode cuisine (Kokoro/Piper en option auto-hébergée).
- **Vidéo (Runway), PowerPoint (PptxGenJS), E2B** : ajoutés en dernier, une fois le cœur de l'app opérationnel — ce ne sont pas des besoins du parcours principal.

## Étape 7 — Administration, analytics, PWA

Espace admin (recettes, ingrédients, allergènes, prix, enseignes, images, prompts, tableau de bord des générations, import CSV). Événements d'usage sans données sensibles. Installation PWA + hors-ligne pour les courses.

## Détails techniques

- TanStack Start + Lovable Cloud ; toute logique IA/prix dans des fonctions serveur, aucune clé côté navigateur.
- Couche `providers/` : `llm`, `image`, `tts`, `search`, `vector`, `prices` — chaque fournisseur implémente une interface commune, sélection par variable d'environnement ou réglage admin.
- Optimisation du plan : filtrage dur (allergies, régime, équipement, temps) puis recherche gloutonne + amélioration locale sous contrainte de budget, déterministe et testable.
- Prix : table `product_prices` avec `source`, `fetched_at`, `confidence` ; cache lecture-écriture par magasin.
- Images : génération à la demande, stockées dans le stockage Cloud, réutilisées ensuite.

## Livraison

Je livre par étapes fonctionnelles (1→4 d'abord, l'app est utilisable de bout en bout), puis prix réels et services externes. À chaque étape vous pouvez tester dans l'aperçu.
