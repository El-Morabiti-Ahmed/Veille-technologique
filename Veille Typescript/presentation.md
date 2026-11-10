---
marp: true
theme: gaia
paginate: true
size: 16:9
style: |
  section {
    font-family: "Aptos", "Avenir Next", sans-serif;
    background: #f7f9fc;
    color: #14243a;
    font-size: 27px;
    line-height: 1.2;
    padding: 46px 60px;
  }
  h1, h2 { color: #1266a8; }
  h1 { font-size: 2em; }
  h2 { font-size: 1.3em; border-bottom: 2px solid #54b9d5; padding-bottom: .2em; }
  h3 { font-size: .95em; margin: .45em 0 .2em; }
  p, ul, ol { margin: .45em 0; }
  li { margin: .18em 0; }
  pre { font-size: .9em; line-height: 1.15; }
  table { font-size: .88em; }
  strong { color: #087e8b; }
  code { color: #a43e65; background: #eaf0f7; }
  footer { color: #63758b; }
  section.title { background: linear-gradient(135deg, #102b46, #1266a8); color: #fff; }
  section.title h1, section.title h2 { color: #fff; border: 0; }
  section.title strong { color: #71d4df; }
  section.lead { background: #eaf4fb; }
---

<!-- _class: title -->
<!-- _paginate: false -->

<img src="images/ofppt.png"><img src="images/solicode.png">

# TypeScript
## Du JavaScript plus sûr, à grande échelle

**Présenté par** : Ahmed El Morabiti  
**Encadré par** : ES-SARRAJ FOUAD

<!-- notes: Cette présentation explique comment TypeScript renforce JavaScript et dans quels projets ses garanties sont utiles. -->

---

## Le défi : faire évoluer une application JavaScript

- JavaScript permet de démarrer vite et reste le langage du Web.
- Mais les valeurs et les formes de données peuvent changer sans prévenir.
- Une erreur de type peut alors n’apparaître qu’au moment d’exécuter un parcours précis.
- Dans une grande équipe, comprendre les contrats entre modules devient plus difficile.

```js
function total(price, quantity) {
  return price * quantity;
}
total("12", 3); // résultat inattendu : "121212"
```

<!-- notes: Une fonction JavaScript accepte ici une chaîne de caractères même si le calcul attendu porte sur des nombres. -->

---

## TypeScript, en une phrase

> TypeScript est un langage construit au-dessus de JavaScript qui ajoute un système de types et se compile en JavaScript.

- Le code TypeScript est vérifié avant son exécution.
- Le navigateur et Node.js exécutent le JavaScript produit, pas le code TypeScript directement.
- JavaScript valide reste généralement du TypeScript valide : on peut adopter les types progressivement.

<!-- notes: TypeScript conserve l’écosystème JavaScript tout en ajoutant une vérification statique pendant le développement. -->

---

## Le même calcul, avec un contrat explicite

```ts
function total(price: number, quantity: number): number {
  return price * quantity;
}

total(12, 3);       // accepté
total("12", 3);    // erreur signalée par l’éditeur ou le compilateur
```

- `number` décrit les paramètres et le résultat attendu.
- L’erreur est détectée avant que l’utilisateur ne rencontre ce chemin.
- Les annotations disparaissent lors de la compilation.

<!-- notes: Les annotations rendent le contrat visible et permettent de repérer l’appel incorrect avant l’exécution. -->

---

## Ce que TypeScript apporte au quotidien

### Détection précoce
Repérer de nombreuses erreurs de données et d’appels avant le lancement.

### Compréhension
Lire les paramètres, retours et structures sans deviner leur forme.

### Assistance de l’éditeur
Obtenir l’autocomplétion, la navigation et les renommages avec davantage de contexte.

### Évolution plus sereine
Modifier une API interne et retrouver les usages à corriger.

<!-- notes: Ces bénéfices se renforcent quand plusieurs personnes font évoluer le même code. -->

---

## Décrire les données avec des types utiles

```ts
type Utilisateur = {
  id: number;
  nom: string;
  email?: string;
};

function saluer(personne: Utilisateur): string {
  return `Bonjour ${personne.nom}`;
}
```

- `email?` indique une propriété facultative.
- Les types nommés évitent de répéter les mêmes formes de données.
- TypeScript vérifie les usages connus, sans valider automatiquement les données externes à l’exécution.

<!-- notes: Un type décrit la forme attendue dans le programme mais ne remplace pas la validation des données reçues du réseau. -->

---

## Les unions rendent les états explicites

```ts
type Etat = "chargement" | "succes" | "erreur";

function afficher(etat: Etat) {
  if (etat === "succes") return "Données prêtes";
  if (etat === "erreur") return "Réessayer";
  return "Chargement…";
}
```

- Une union limite une valeur à des choix définis.
- Elle réduit les états impossibles ou mal orthographiés.
- Elle aide l’éditeur à guider la logique selon chaque cas.

<!-- notes: Une union de littéraux transforme les états métier en choix vérifiables plutôt qu’en chaînes arbitraires. -->

---

## Types avancés : réutiliser sans perdre la précision

```ts
type Reponse<T> = {
  donnees: T;
  succes: boolean;
};

type ReponseUtilisateur = Reponse<Utilisateur>;
```

- Les génériques, comme `T`, adaptent un type à plusieurs données.
- Les types utilitaires aident à construire des variantes de types existants.
- On gagne en réutilisation tout en conservant l’information sur les données.

<!-- notes: Les génériques permettent de factoriser des structures tout en gardant le type précis de leur contenu. -->

---

## TypeScript et JavaScript : complémentaires

| JavaScript | TypeScript |
|---|---|
| Langage exécuté par les navigateurs et Node.js | Langage de développement compilé en JavaScript |
| Typage dynamique à l’exécution | Vérifications statiques pendant le développement |
| Syntaxe et démarrage simples | Contrats, outils et refactorisations plus guidés |
| Aucun système de types intégré | Types effacés à la compilation |

**TypeScript ne remplace pas JavaScript : il s’appuie sur lui.**

<!-- notes: TypeScript complète le langage JavaScript en ajoutant des informations utilisées par les outils avant l’exécution. -->

---

## Ce que TypeScript ne garantit pas

- Il ne détecte pas toutes les erreurs : la logique métier peut rester incorrecte.
- Les types sont effacés à la compilation et ne contrôlent pas les données à l’exécution.
- Les données d’API, fichiers ou formulaires doivent être validées à leur frontière.
- Une configuration stricte et des types bien entretenus sont nécessaires pour tirer parti de ses contrôles.

```ts
const utilisateur = JSON.parse(texte) as Utilisateur;
// L’assertion ne vérifie pas le contenu réel de « texte ».
```

<!-- notes: Une assertion de type indique une hypothèse au compilateur mais ne transforme ni ne valide la valeur réelle. -->

---

## Quand le choisir ?

### TypeScript est particulièrement utile si…
- le projet comporte plusieurs modules ou une longue durée de vie ;
- l’équipe partage des interfaces et des modèles métier ;
- les refactorisations et la fiabilité des changements comptent.

### JavaScript seul peut suffire si…
- le script est court, isolé et peu susceptible d’évoluer ;
- le coût initial de configuration dépasserait le bénéfice attendu.

<!-- notes: Le bon choix dépend de la taille, de la durée de vie et du coût d’évolution du projet. -->

---

## Adopter TypeScript progressivement

1. Ajouter TypeScript à l’outillage du projet.
2. Configurer la compilation et activer les vérifications adaptées.
3. Convertir un module utile et documenter ses contrats.
4. Valider les données aux frontières et traiter les erreurs révélées.
5. Étendre progressivement les types aux autres modules.

**Commencer petit permet d’apprendre sans bloquer le développement.**

<!-- notes: Une migration progressive permet à l’équipe d’introduire les types à mesure que les modules évoluent. -->

---

<!-- _class: lead -->

## À retenir

- TypeScript ajoute des types statiques à l’expérience JavaScript.
- Il rend les erreurs plus visibles et le code plus facile à explorer et à faire évoluer.
- Il apporte le plus de valeur quand le projet et l’équipe grandissent.
- Il complète les tests, la validation à l’exécution et les bonnes pratiques de conception.

### Moins de suppositions, des changements plus maîtrisés.

<!-- notes: TypeScript améliore la confiance dans les changements en rendant explicites les contrats du programme. -->
