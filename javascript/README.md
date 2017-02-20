# NCC Javascript() {

  Les conventions qui suivent s'applique à tout le Javascript de Neo-nomade.

  Elles sont majoritairement inspirées des [conventions d'Airbnb](https://github.com/airbnb/javascript).

  Autres conventions
  - :point_right: [Php](../php/)

## Tables des matières
  1. [Types](#types)
  1. [Références](#références)
  1. [Objets](#objets)
  1. [Tableaux](#tableaux)
  1. [Strings](#strings)
  1. [Variables](#variables)
  1. [Opérateurs de comparaison et d'égalité](#opérateurs-de-comparaison-et-dégalité)
  1. [Blocks](#blocks)
  1. [Commentaires](#commentaires)
  1. [Espacements](#espacements)
  1. [Virgules](#virgules)
  1. [Fonctions](#fonctions)
  1. [Propriétés](#propriétés)
  1. [Type Cast](#type-cast)
  1. [Nommage](#nommage)
  1. [jQuery](#jquery)
  1. [Ajax](#ajax)

## Types

  <a name="types--primitives"></a><a name="1.1"></a>
  - [1.1](#types--primitives) **Primitifs**: Quand vous accédez à un type primitif, vous travaillez directement sur sa valeur.

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    const foo = 1;
    let bar = foo;
    
    bar = 9;
    
    // => 1, 9
    console.log(foo, bar); 
    ```

  <a name="types--complex"></a><a name="1.2"></a>
  - [1.2](#types--complex) **Complexes**: Quand vous accédez à un type complexe, vous travaillez sur une référence de sa valeur.

    + `object`
    + `array`
    + `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;
    
    bar[0] = 9;
    
    // => 9, 9
    console.log(foo[0], bar[0]);
    ```

:point_up: **[back to top](#tables-des-matières)**

## Références

  <a name="references--prefer-const"></a><a name="2.1"></a>
  - [2.1](#references--prefer-const) **ES6**: Utilisez `const` pour toutes vos références. Évitez d'utiliser `var`. eslint: [`prefer-const`](http://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](http://eslint.org/docs/rules/no-const-assign.html)

  > Pourquoi ? Cela garantit que vous ne pouvez pas réattribuer vos références, ce qui pourrait conduire à des bugs et nuire à la compréhension du code.

  ```javascript
  // Mauvais
  var a = 1;
  var b = 2;
  
  // Bon
  const a = 1;
  const b = 2;
  ```

  <a name="references--no-var"></a><a name="2.2"></a>
  - [2.2](#references--no-var) **ES6**: Si vous devez réassigner vos références, utilisez `let` à la place de `var`. eslint: [`no-var`](http://eslint.org/docs/rules/no-var.html) jscs: [`disallowVar`](http://jscs.info/rule/disallowVar)

  > Pourquoi ? `let` est block-scopée ce qui est mieux que `var` qui est function-scopée.

  ```javascript
  // Mauvais
  var count = 1;
  if (true) {
    count += 1;
  }
  
  // Bon
  let count = 1;
  if (true) {
    count += 1;
  }
  ```

  <a name="references--block-scoped"></a><a name="2.3"></a>
  - [2.3](#references--block-scoped) **ES6**: Notez que `const` et `let` sont toutes les deux block-scopées.

  ```javascript
  // const et let existent seulement dans les blocs dans lesquelles elles sont définies
  {
    const a = 1;
    let b = 1;
  }

  // ReferenceError
  console.log(a);

  //ReferenceError
  console.log(b);
  ```

:point_up: **[back to top](#tables-des-matières)**

## Objets

  <a name="objets--no-new"></a><a name="3.1"></a>
  - [3.1](#objets--no-new) Utilisez la syntaxe littérale à la place du constructeur pour la création d'objet. eslint: [`no-new-object`](http://eslint.org/docs/rules/no-new-object.html)

  > Pourquoi ? Bien que la performance ne varie pas entre les deux méthodes, la syntaxe littérale est plus concise et présente un gain de bit, ce qui en fait la méthode standard de création d'objet.

  ```javascript
  // Mauvais
  const item = new Object();

  // Bon
  const item = {};
  ```

  <a name="objets--computed-properties"></a><a name="3.2"></a>
  - [3.2](#objets--computed-properties) **ES6**: Utilisez les noms de propriété calculés quand vous créez des objets avec des noms de propriété dynamiques.

  > Pourquoi ? Les noms de propriété calculés vous permettent de définir toutes les propriétés d'un objet en un seul endroit.

  ```javascript
  function getKey(k) {
    return '#' + k;
  }

  // Mauvais
  const obj = {
    id: 3,
    name: 'Obi-Wan',
  };
  obj[getKey('jedi')] = true;

  // Bon
  const obj = {
    id: 3,
    name: 'Obi-Wan',
    [getKey('jedi')]: true,
  };
  ```

  <a name="objets--property-shorthand"></a><a name="3.3"></a>
  - [3.3](#objets--property-shorthand) **ES6**: Utilisez l'attribution de propriété shorthand. eslint: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html) jscs: [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)

  > Pourquoi ? C'est plus court à écrire et plus simple à décrire.

  ```javascript
  const lukeSkywalker = 'You are my son !';

  // Mauvais
  const obj = {
    lukeSkywalker: lukeSkywalker,
  };

  // Bon
  const obj = {
    lukeSkywalker,
  };
  ```

  <a name="objets--function-shorthand"></a><a name="3.4"></a>
  - [3.4](#objets--function-shorthand) **ES6**: Utilisez l'attribution de fonction shorthand. eslint: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html) jscs: [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)

  ```javascript
  // Mauvais
  const stormtrooper = {
    jediKilled: 1,

    order66: function (kills) {
      return stormtrooper.jediKilled + kills;
    },
  };

  // Bon
  const stormtrooper = {
    jediKilled: 1,

    order66(kills) {
      return stormtrooper.jediKilled + kills;
    },
  };
  ```

  <a name="objets--grouped-shorthand"></a><a name="3.5"></a>
  - [3.5](#objets--grouped-shorthand) **ES6**: Groupez les propriétés shorthand au début de votre déclaration d'objet.

  > Pourquoi ? C'est plus facile de visualiser quelles propriétés utilisent le shorthand

  ```javascript
  const anakinSkywalker = 'Anakin Skywalker';
  const lukeSkywalker = 'Luke Skywalker';

  // Mauvais
  const obj = {
    episodeOne: 1,
    twoJediWalkIntoACantina: 2,
    lukeSkywalker,
    episodeThree: 3,
    mayTheFourth: 4,
    anakinSkywalker,
  };

  // Bon
  const obj = {
    lukeSkywalker,
    anakinSkywalker,
    episdodeOne: 1,
    twoJediWalkIntoACantina: 2,
    episodeThree: 3,
    mayTheFourth: 4,
  };
  ```

  <a name="objets--quoted-props"></a><a name="3.6"></a>
  - [3.6](#objets--quoted-props) Vous ne devez quoter que les propriétés qui ont un nom invalide. eslint: [`quote-props`](http://eslint.org/docs/rules/quote-props.html) jscs: [`disallowQuotedKeysInObjects`](http://jscs.info/rule/disallowQuotedKeysInObjects)

  > Pourquoi ? C'est plus facile à lire, ça améliore le highlighting de la syntaxe et c'est également mieux optimisé pour de nombreux moteurs JS.

  ```javascript
  // Mauvais
  const obj = {
    'jean': 3,
    'pierre': 4,
    'jean-pierre': 5,
  };

  // Bon
  const obj = {
    jean: 3,
    pierre: 4,
    'jean-pierre': 5,
  };
  ```

  <a name="objets--spread-operator"></a><a name="3.7"></a>
  - [3.7](#objets--spread-operator) **ES6**: Utilisez l'opérateur de décomposition à la place de [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) pour cloner des objets.

  ```javascript
  const original = { a: 1, b: 2 };

  // Très mauvais
  const copy = Object.assign(original, { c: 3 }); // Modifie `original` ಠ_ಠ

  // Mauvais

  const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

  // Bon
  const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }
  ```

  <a name="objets--omit-properties"></a><a name="3.8"></a>
  - [3.8](#objets--omit-properties) **ES6**: Utilisez l'opérateur de décomposition pour cloner un objet en omettant certaines propriétés.

  ```javascript
  const original = { a: 1, b: 2, c: 3 };

  // Très mauvais
  const noA = Object.assign(original);
  delete noA.a; // Modifie `original`

  // Mauvais
  const noA = Object.assign({}, original);
  delete noA.a; // noA => { b: 2, c: 3 }

  // Bon
  const { a, ...noA } = copy; // noA => { b: 2, c: 3 }
  ```

:point_up: **[back to top](#tables-des-matières)**

## Tableaux

  <a name="tableaux--litterals"></a><a name="4.1"></a>
  - [4.1](#tableaux--litterals) Utilisez la syntaxe littérale pour la création de tableaux. eslint: [`no-array-constructor`](http://eslint.org/docs/rules/no-array-constructor.html)

  ```javascript
  // Mauvais
  const items = new Array();

  // Bon
  const items = [];
  ```

  <a name="tableaux--push"></a><a name="4.2"></a>
  - [4.2](#tableaux--push) Utilisez [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) à la place d'un assignement direct pour ajouter des éléments à un tableau.

  ```javascript
  const stack = [];

  // Mauvais
  stack[stack.length] = 'something';

  // Bon
  stack.push('something');
  ```

  <a name="tableaux--spread"></a><a name="4.3"></a>
  - [4.3](#tableaux--spread) **ES6**: Utilisez l'opérateur de décomposition `...` pour cloner des tableaux.

  ```javascript
  // Mauvais
  const len = item.length;
  const itemsCopy = [];
  let i;

  for (i = 0; i < len; i += 1) {
    itemsCopy[i] = items[i];
  }

  // Bon
  const itemsCopy = [...items];
  ```

  <a name="tableaux--from"></a><a name="4.4"></a>
  - [4.4](#tableaux-from) Pour convertir un objet array-like en un véritable tableau, utilisez [`Array.from`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from).

  ```javascript
  const foo = document.querySelectorAll('.foo');
  const nodes = Array.from(foo);
  ```

  <a name="tableaux--from"></a><a name="4.4"></a>

:point_up: **[back to top](#tables-des-matières)**

## Strings

  <a name="strings--single-quotes"></a><a name="5.1"></a>
  - [5.1](#strings--single-quotes) Utilisez les simples quotes `''` pour les chaines de caractères. eslint: [`quotes`](http://eslint.org/docs/rules/quotes.html) jscs: [`validateQuoteMarks`](http://jscs.info/rule/validateQuoteMarks)

  ```javascript
  // Mauvais
  const name = "Jack Sparrow";

  // Mauvais - Les quotes de template devraient contenir des variables extrapolées ou plusieurs lignes
  const name = `Jack Sparrow`;

  // Bon
  const name = 'Jack Sparrow';
  ```

  <a name="strings--line-length"></a><a name="5.2"></a>
  - [5.2](#strings--line-length) Les longues chaines de caractères ne doivent pas être écritent sur plusieurs lignes en utilisant la concatenation.

  > Pourquoi ? Les chaines de caractères cassées par la concaténation sont difficiles à travailler et rendent le code moins recherchable.

  ```javascript
  // Mauvais
  const loseYourself = 'You better lose yourself in the music, the moment \
  You own it, you better never let it go \
  You only get one shot, do not miss your chance to blow \
  This opportunity comes once in a lifetime';

  // Mauvais
  const loseYourself = 'You better lose yourself in the music, the moment ' +
  'You own it, you better never let it go ' +
  'You only get one shot, do not miss your chance to blow ' +
  'This opportunity comes once in a lifetime';

  // Bon
  const loseYourself = 'You better lose yourself in the music, the moment You own it, you better never let it go You only get one shot, do not miss your chance to blow This opportunity comes once in a lifetime';
  ```

  <a name="strings--template"></a><a name="5.3"></a>
  - [5.3](#strings--template) **ES6**: Quand vous construisez des chaines dynamiquement, utilisez les templates de chaine de caractères à la place d'une concaténation. eslint: [`prefer-template`](http://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](http://eslint.org/docs/rules/template-curly-spacing) jscs: [`requireTemplateStrings`](http://jscs.info/rule/requireTemplateStrings)

  > Pourquoi ? Les templates de chaine de caractères nous permettent d'avoir une syntaxe facilement lisible et compréhensible avec des retours à la ligne propres et des interpolations de variables.

  :pushpin: _A noter qu'il n'y a pas d'espace entre les `{}` pour les variables extrapolées. Contrairement à la recommendation [10.10](#espacements--in-braces)._

  ```javascript
  const name = 'Jackson';
  
  // Mauvais
  const heFuckedUp = 'It was at this moment ' + name + ' knew ...';

  // Bon
  const heFuckedUp = `It was at this moment ${name} knew ...`;
  ```

  <a name="strings--no-eval"></a><a name="5.4"></a>
  - [5.4](#strings--no-eval) N'utilisez jamais `eval()` sur une chaine, ça ouvre trop de vulnérabilités.

  <a name="strings--escaping"></a><a name="5.5"></a>
  - [5.5](#strings--escaping) Échappez uniquement les caractères si nécéssaires. eslint: [`no-useless-escape`](http://eslint.org/docs/rules/no-useless-escape)

  > Pourquoi ? Les backslashs diminuent la lisibilité et ils n'ont pas d'intérêt s'ils ne sont pas nécessaires.

  ```javascript
  // Mauvais
  const foo = '\'this\' is \"quoted\"';

  // Bon
  const foo = '\'this\' is "quoted"';
  const foo = `my nickname is '${name}'`;
  ```

:point_up: **[back to top](#tables-des-matières)**

## Variables

  <a name="variables--const"></a><a name="6.1"></a>
  - [6.1](#variables--const) **ES6**: Toujours utiliser `const` pour déclarer ses variables. Ne pas le faire conduirait a déclarer ne variable globale, et nous ne voulons pas déclarer de variables globales par erreur. eslint: [`no-undef`](http://eslint.org/docs/rules/no-undef) [`prefer-const`](http://eslint.org/docs/rules/prefer-const)

  :pushpin: _Si la valeur de votre variable est amenée à être modifiée, alors il faut utiliser `let` comme indiqué dans la recommendation [2.2](#references--no-var)._

  ```javascript
  // Mauvais
  superPower = true;

  // Bon
  const superPower = true;
  ```

  <a name="variables--one-declaration"></a><a name="6.2"></a>
  - [6.2](#variables--one-declaration) Utilisez une déclaration par variable. eslint: [`one-var`](http://eslint.org/docs/rules/one-var.html) jscs: [`disallowMultipleVarDecl`](http://jscs.info/rule/disallowMultipleVarDecl)

  > Pourquoi ? C'est plus facile de rajouter de nouvelles déclarations de variable par la suite, et on n'a pas à se soucier d'oublier un `;` ou une `,`. On peut également débugger les déclarations unes par unes plutôt que de les faire toutes d'un coup.

  ```javascript
  // Mauvais
  const superMan, batMan = true;
  const superMan = true,
        batMan = false,
        spiderMan = 'Tobey Maguire';

  // Bon
  const superMan = true;
  const batMan = false;
  const spiderMan = 'Tobey Maguire';
  ```

  <a name="variables--no-chained-declaration"></a><a name="6.3"></a>
  - [6.3](#variables--no-chained-declaration) Pas de déclaration de variable en chaine.

  > Pourquoi ? Les déclarations en chaine créées implicitement des variables globales.

  ```javascript
  // Mauvais
  // Ici a est `let` et b et c sont des variables globales
  let a = b = c = 1;

  // Bon
  let a = 1;
  let b = a;
  let c = a;

  // Ceci est vrai également avec `const`
  ```

  <a name="variables--grouped-declaration"></a><a name="6.4"></a>
  - [6.4](#variables--grouped-declaration) Groupez vos déclarations du plus fort au plus faible (`const` puis `let`) et dans l'ordre alphabétique.

  ```javascript
  // Mauvais
  let i;
  const items = getItems();
  let dragonball;
  const goSportsTeam = true;
  let len;

  // Bon
  const goSportsTeam = true;
  const items = getItems();
  let dragonball;
  let i;
  let len;
  ```

  <a name="variables--define-where-used"></a><a name="6.5"></a>
  - [6.5](#variables--define-where-used) Assignez vos variables où vous en avez besoin, mais placez les à un endroit raisonnable.

  > Pourquoi ? `const` et `let` sont block-scopées et non fonction-scopées.

  ```javascript
  // Mauvais - appel d'une fonction non nécessaire
  function checkName(hasName) {
    const name = getName();

    if (hasName === 'test') {
      return false;
    }

    if (name === 'test') {
      this.setName('');
      return false;
    }

    return name;
  }

  // Bon
  function checkName(hasName) {
    if (hasName === 'test') {
      return false;
    }

    const name = getName();

    if (name === 'test') {
      this.setName('');
      return false;
    }

    return name;
  }
  ```

  <a name="variables--no-plusplus"></a><a name="6.6"></a>
  - [6.6](#variables--no-plusplus) N'utilisez pas les incrémentations et décrémentations unitaires (`++`, `--`). eslint [`no-plusplus`](http://eslint.org/docs/rules/no-plusplus)

  > Pourquoi ? D'après eslint, l'incrémentation et décrémentation unitaire sont sujets à l'insertion automatique de point virgule, ce qui peut causer des erreurs invisibles en incrémentant et décrémentant des valeurs sans le vouloir. De plus, il est intuitivement plus facile de comprendre l'incrémentation en utilisant la syntaxe `num += 1` plutôt que `num++` ou `num ++`.

  ```javascript
  // Mauvais
  let i = 0;
  i++;
  i++;
  i--;

  // Bon
  let i = 0;
  i += 2;
  i -= 1;
  ```

:point_up: **[back to top](#tables-des-matières)**

## Opérateurs de comparaison et d'égalité

  <a name="operateurs--eqeqeq"></a><a name="7.1"></a>
  - [7.1](#operateurs--eqeqeq) Utilisez `===` and `!==` à la place de `==` et `!=`. eslint: [`eqeqeq`](http://eslint.org/docs/rules/eqeqeq.html)

  <a name="operateurs--if"></a><a name="7.2"></a>
  - [7.2](#operateurs--if) Les déclarations conditionnelles telles que la déclaration `if` evaluent leur expression en utilisant la coertition de la méthode abstraite `ToBoolean` et suit toujours ces règles simples:
    + **Objets** sont évalués comme **vrai**
    + **Tableaux** sont des objets, et donc évalués comme **true**
    + **Undefined** est évalué comme **false**
    + **Null** est évalué comme **false**
    + **Booleans** sont évalués de **la valeur du booléen**
    + **Numbers** sont évalués comme **false** si **+0, -0, NaN**, sinon **true**
    + **Strings** sont évalués comme **false** si la chaine est vide `''`, sinon **true**

  ```javascript
  if ([0] && []) {
    // true
  }
  ```

  <a name="operateurs--shortcuts"></a><a name="7.3"></a>
  - [7.3](#operateurs--shortcuts) Utilisez les raccourcis pour les booleans, mais des comparaisons explicites pour les strings et les numbers

  ```javascript
  // Mauvais 
  if (isValid === true) {
    // ...
  } 

  // Bon
  if (isValid) {
    // ...
  }

  // Mauvais
  if (name) {
    // ...
  }

  // Bon
  if (name !== '') {
    // ...
  }

  // Mauvais
  if (stack.length) {
    // ...
  }

  // Bon
  if (stack.length > 0) {
    // ...
  }
  ```

  <a name="operateurs--switch-blocks"></a><a name="7.4"></a>
  - [7.4](#operateurs--switch-blocks) Utilisez les accolades pour créer des blocks dans les clauses `case` et `default`. eslint: [`no-case-declarations`](http://eslint.org/docs/rules/no-case-declarations.html)

  ```javascript
  // Mauvais
  switch (foo) {
    case 1:
      let x = 1;
      break;
    case 2:
      const y = 2;
      break;
    case 3:
    case 4:
      const z = 3;
      break;
    default:
      const d = true;
  }

  // Bon
  switch (foo) {
    case 1: {
      let x = 1;
      break;
    }

    case 2: {
      const y = 2;
      break;
    }

    case 3:
    case 4: {
      const z = 3;
      break;
    }

    default: {
      const d = true;
    }
  }
  ```

  <a name="operateurs--nested-ternaries"></a><a name="7.5"></a>
  - [7.5](#operateurs--nested-ternaries) Les ternaires ne doivent pas être imbriquées et doivent être des conditions simples sur 1 ligne. eslint: [`no-nested-ternary`](http://eslint.org/docs/rules/no-nested-ternary.html)

  ```javascript
  // Mauvais 
  const foo = maybe1 > maybe2
    ? 'bar'
    : value1 > value2 ? 'baz' : null;

  // Bon
  const maybeNull = value1 > value2 ? 'baz' : null;
  const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
  ```

  <a name="operateurs--simple-ternaries"></a><a name="7.6"></a>
  - [7.6](#operateurs--simple-ternaries) N'utilisez pas de fonction dans les ternaires. Elles ne doivent être utilisées que s'il s'agit d'une comparaison et assignation simple.

  ```javascript
  // Mauvais
  const theSaddest = anakin.countMissingHands() > luke.countMissingHands() ? 'anakin' : 'luke';

  // Bon
  const anakinHands = anakin.countMissingHands();
  const lukeHands = luke.countMissingHands();
  const theSaddest = anakinHands > lukeHands ? 'anakin' : 'luke';

  // Bon aussi
  let theSaddest = 'luke';
  if (anakin.countMissingHands() > luke.countMissingHands()) {
    theSaddest = 'anakin';
  }

  // Mauvais
  const nbOfRemainingHands = !anakin ? jedi.countMissingHands() : 0;

  // Bon
  let nbOfRemainingHands = 0;
  if (!anakin) {
    nbOfRemainingHands = jedi.countMissingHands();
  }
  ```

  <a name="operateurs--unneed-ternaries"></a><a name="7.7"></a>
  - [7.7](#operateurs--unneed-ternaries) Évitez les ternaires inutiles. eslint: [`no-unneeded-ternary`](http://eslint.org/docs/rules/no-unneeded-ternary.html)

  ```javascript
  // Mauvais
  const foo = a ? a : b;
  const bar = c ? true : false;
  const baz = c ? false : true;
  const bax = a > b ? true : false;

  // Bon
  const foo = a || b;
  const bar = !!c;
  const baz = !c;
  const bax = a > b;
  ```

  <a name="operateurs--one-line"></a><a name="7.8"></a>
  - [7.8](#operateurs--one-line) Écrivez une condition par ligne.

  > Pourquoi ? Les grandes conditions sont illisibles si elles sont sur une ligne.

  ```javascript
  // Mauvais
  if ((jsonData && jsonData.results && jsonData.results.geometry && (jsonData.results.geometry.latitude || jsonData.results.geometry.longitude)) || jsonData.coords) {
    // ...
  }

  // Bon
  if ((jsonData 
        && jsonData.results 
        && jsonData.results.geometry 
        && (jsonData.results.geometry.latitude 
            || jsonData.results.geometry.longitude)) 
      || jsonData.coords
  ) {
    // ...
  }
  ```

:point_up: **[back to top](#tables-des-matières)**

## Blocks

  <a name="blocks--braces"></a><a name="8.1"></a>
  - [8.1](#blocks--braces) Utilisez les accolades pour les blocs multi-lignes.

  ```javascript
  // Mauvais
  if (test)
    return false;

  // Bon
  if (test) return false;

  // Bon
  if (test) {
    return false;
  }

  // Mauvais
  function () { return false; }

  // Bon
  function () {
    return false;
  }
  ```

  <a name="blocks--cuddled-elses"></a><a name="8.2"></a>
  - [8.2](#blocks--cuddled-elses) Si vous utilisez les blocs multi-lignes `if` et `else`, placez `else` sur la même ligne que l'accolade de fermeture du `if`. eslint: [`brace-style`](http://eslint.org/docs/rules/brace-style.html) jscs:  [`disallowNewlineBeforeBlockStatements`](http://jscs.info/rule/disallowNewlineBeforeBlockStatements)

  ```javascript
  // Mauvais
  if (test) {
    thing1();
    thing2();
  }
  else {
    thing3();
  }

  // Bon
  if (test) {
    thing1();
    thing2();
  } else {
    thing3();
  }
  ```

## Commentaires

  <a name="comments--empty-line-before"></a><a name="9.1"></a>
  - [9.1](#comments--empty-line-before) Laissez 1 ligne vide au dessus du commentaire, sauf s'il s'agit de la première ligne du bloc.

  > Pourquoi ? Pour une meilleure lisibilité du commentaire et d'aérer le code.

  ```javascript
  // Mauvais
  let year = 1970;
  // A nouvel an on gagne une année
  year += 1;

  // Bon
  let year = 1984;

  // A nouvel an on gagne une année
  year += 1;

  // Bon
  let year = 1970;
  if (georgesOrwell) {
    // L'année du célèbre livre
    year = 1984;
  }
  ```

  <a name="comments--caps"></a><a name="9.2"></a>
  - [9.2](#comments--caps) Commencez toujours votre commentaire par 1 espace puis 1 majuscule. eslint: [`spaced-comment`](http://eslint.org/docs/rules/spaced-comment)

  > Pourquoi ? Augmente la lisibilité

  ```javascript
  // Mauvais
  //pour Frodon
  const aragorn = true;

  // Bon
  // Pour Frodon
  const aragorn = true;
  ```

  <a name="comments--singleline"></a><a name="9.3"></a>
  - [9.3](#comments--singleline) Utilisez `// ...` pour les commentaires sur 1 ligne. Placez le commentaire au dessus du sujet du commentaire. 

  ```javascript
  // Mauvais
  const pillChosenByNeo = 'red'; // La pillule choisie par Neo

  // Bon
  // La pillule choisie par Neo
  const pillChosenByNeo = 'red';
  ```

  <a name="comments--multiline"></a><a name="9.4"></a>
  - [9.4](#comments--multiline) Utilisez `/** ... */` pour commenter sur plusieurs lignes.

  ```javascript
  // Mauvais
  // countMissingHands(jedi) retourne le nombre
  // de mains manquantes du jedi passé en paramètre
  // 
  // @param Jedi jedi
  // @return Number
  function countMissingHands(jedi) {
    // ...
  }

  // Bon
  /**
   * countMissingHands(jedi) retourne le nombre
   * de mains manquantes du jedi passé en paramètre
   *
   * @param Jedi jedi
   * @return Number
   */
  function countMissingHands() {
    // ...
  }
  ```

  <a name="comments--fixme"></a><a name="9.5"></a>
  - [9.5](#comments--fixme) Utilisez le prefix `FIXME:` pour annoter des problèmes constatés.

  ```javascript
  // FIXME: Ne devrait pas utiliser de global ici
  total = 0;
  ```

  <a name="comments--todo"></a><a name="9.6"></a>
  - [9.6](#comments--todo) Utilisez le prefix `TODO: ` pour annoter des futurs développements nécessaires. Si une mantis y est assignée, veuillez préciser le numéro tel quel `TODO: #M802`.

  ```javascript
  // TODO: #M802 Le total devrait-être configurable
  total = 0;
  ```

  <a name="comments--over-comment"></a><a name="9.7"></a>
  - [9.7](#comments--over-comment) Expliquez textuellement en commentaire toutes vos déclarations de fonction, conditions et autres boucles. Commentez, voir sur-commentez, également au maximum votre code.

  ```javascript
  // On instancie notre compteur d'item
  let nbItems = 0;

  // On ajoute le nombre d'items au total
  nbItems += arrItems.length;

  // On retourne le nombre d'items
  return nbItems;
  ```

:point_up: **[back to top](#tables-des-matières)**

## Espacements

  <a name="espacements--spaces"></a><a name="10.1"></a>
  - [10.1](#espacements--spaces) Utilisez la tabulation soft (caractère espace) définie à 4 espaces. eslint: [`indent`](http://eslint.org/docs/rules/indent.html) jscs: [`validateIndentation`](http://jscs.info/rule/validateIndentation)

  ```javascript
  // Mauvais
  function foo() {
  ∙∙let name;
  }

  // Mauvais
  function foo() {
  →   let name;
  }

  // Bon
  function foo() {
  ∙∙∙∙let name;
  }
  ```

  <a name="espacements--before-blocks"></a><a name="10.2"></a>
  - [10.2](#espacements--before-blocks) Mettez toujours 1 espace devant une accolade d'ouverture `{`. eslint: [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks.html) jscs: [`requireSpaceBeforeBlockStatements`](http://jscs.info/rule/requireSpaceBeforeBlockStatements)

  > Pourquoi ? Permet d'aérer le code et rendre les blocs plus visibles

  ```javascript
  // Mauvais
  function test(){
    console.log('test');
  }

  // Bon
  function test() {
    console.log('test');
  }

  // Mauvais
  if (test){
    console.log('test');
  }

  // Bon
  if (test) {
    console.log('test');
  }

  // Mauvais
  cat.set('attr',{
    age: 7,
    color: black,
  });

  // Bon
  cat.set('attr', {
    age: 7,
    color: black,
  });
  ```

  <a name="espacements--around-keywords"></a><a name="10.3"></a>
  - [10.3](#espacements--around-keywords) Placez 1 espace devant une paranthèse ouvrante `(`. Ne placez pas d'espace entre la liste des arguments et le nom de la fonction dans les appels et déclarations. eslint: [`keyword-spacing`](http://eslint.org/docs/rules/keyword-spacing.html) jscs: [`requireSpaceAfterKeywords`](http://jscs.info/rule/requireSpaceAfterKeywords)

  :pushpin: _A noter que ça revient à dire que les mots clés doivent être isolés d'espaces._

  ```javascript
  // Mauvais
  if(isJedi) {
    fight ();
  }else {
    runAway ();
  }

  // Bon
  if (isJedi) {
    fight();
  } else {
    runAway();
  }

  // Mauvais
  function fight () {
    console.log ('Swooosh!');
  }

  // Bon
  function fight() {
    console.log('Swooosh!');
  }
  ```

  <a name="espacements--infix-ops"></a><a name="10.4"></a>
  - [10.4](#espacements--infix-ops) Isolez les opérateurs avec des espaces. eslint: [`space-infix-ops`](http://eslint.org/docs/rules/space-infix-ops.html) jscs: [`requireSpaceBeforeBinaryOperators`](http://jscs.info/rule/requireSpaceBeforeBinaryOperators), [`requireSpaceAfterBinaryOperators`](http://jscs.info/rule/requireSpaceAfterBinaryOperators)

  > Pourquoi ? Permet une bien meilleure lisibilité des opérations.

  ```javascript
  // Mauvais
  const x=y+5;

  // Bon
  const x = y + 5;
  ```

  <a name="espacements--newline-at-end"></a><a name="10.5"></a>
  - [10.5](#espacements--newline-at-end) Terminez vos fichiers par 1 seul retour à la ligne. eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

  ```javascript
  // Mauvais
  return json;

  // Mauvais
  return json;↵
  ↵

  // Bon
  return json;↵
  ```

  <a name="espacements--after-blocks"></a><a name="10.6"></a>
  - [10.6](#espacements--after-blocks) Laissez 1 ligne vide entre la fin d'un bloc et la déclaration suivante. jscs: [`requirePaddingNewLinesAfterBlocks`](http://jscs.info/rule/requirePaddingNewLinesAfterBlocks)

  ```javascript
  // Mauvais
  if (darthVader) {
    return 'sith';
  }
  return 'jedi';

  // Bon
  if (darthVader) {
    return 'sith';
  }

  return 'jedi';

  // Mauvais
  const jedi = {
    fight() {
    },
    useForce() {
    },
  };

  // Bon
  const jedi = {
    fight() {
    },

    useForce() {
    },
  };
  ```

  <a name="espacements--padded-blocks"></a><a name="10.7"></a>
  - [10.7](#espacements--padded-blocks) N'ajoutez pas de lignes vides inutiles dans vos blocs. eslint: [`padded-blocks`](http://eslint.org/docs/rules/padded-blocks.html) jscs:  [`disallowPaddingNewlinesInBlocks`](http://jscs.info/rule/disallowPaddingNewlinesInBlocks)

  ```javascript
  // Mauvais
  function foo() {

    console.log(bar);

  }

  // Bon
  function foo() {
    console.log(bar);
  }

  // Mauvais
  if (baz) {

    console.log(bar);
  } else {
    console.log(foo);

  }

  // Bon
  if (baz) {
    console.log(bar);
  } else {
    console.log(foo);
  }
  ```

  <a name="espacements--in-parens"></a><a name="10.8"></a>
  - [10.8](#espacements--in-parens) N'ajoutez pas d'espaces à l'intérieur de parenthèses. eslint: [`space-in-parens`](http://eslint.org/docs/rules/space-in-parens.html) jscs: [`disallowSpacesInsideParentheses`](http://jscs.info/rule/disallowSpacesInsideParentheses)

  ```javascript
  // Mauvais
  function bar( foo ) {
  }

  // Bon
  function bar(foo) {
  }

  // Mauvais
  if ( foo ) {
  }

  // Bon
  if (foo) {
  }
  ```

  <a name="espacements--in-brackets"></a><a name="10.9"></a>
  - [10.9](#espacements--in-brackets) N'ajoutez pas d'espaces à l'intérieur de crochets. eslint: [`array-bracket-spacing`](http://eslint.org/docs/rules/array-bracket-spacing.html) jscs: [`disallowSpacesInsideArrayBrackets`](http://jscs.info/rule/disallowSpacesInsideArrayBrackets)

  ```javascript
  // Mauvais
  const foo = [ 1, 2, 3 ];
  console.log(foo[ 0 ]);

  // Bon
  const foo = [1, 2, 3];
  console.log(foo[0]);
  ```

  <a name="espacements--in-braces"></a><a name="10.10"></a>
  - [10.10](#espacements--in-braces) Ajoutez des espaces à l'intérieur d'accolades. eslint: [`object-curly-spacing`](http://eslint.org/docs/rules/object-curly-spacing.html) jscs: [`requireSpacesInsideObjectBrackets`](http://jscs.info/rule/requireSpacesInsideObjectBrackets)

  ```javascript
  // Mauvais
  const superMan = {realName: 'Clark Kent'};

  // Bon
  const superMan = { realName: 'Clark Kent' };
  ```

  <a name="espacements--chains"></a><a name="10.11"></a>
  - [10.11](#espacements--chains) Indentez lorsque vous utilisez une longue chaine de méthodes (plus de 2 méthodes). Utilisez un point devant chaque fonction, qui souligne que la ligne est une fonction et non une propriété. eslint: [`newline-per-chained-call`](http://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](http://eslint.org/docs/rules/no-whitespace-before-property)

  ```javascript
  // Mauvais
  $('#items').find('.selected').highlight().end().find('.open').updateCount();

  // Mauvais
  $('#items').
    find('.selected').
      highlight().
      end().
    find('.open').
      updateCount();

  // Bon
  $('#items')
    .find('.selected')
      .highlight()
      .end()
    .find('.open')
      .updateCount();

  // Bon
  const leds = stage.selectAll('.led').data(data);
  ```

  <a name="espacements--max-len"></a><a name="10.11"></a>
  - [10.12](#espacements--max-len) Évitez les lignes de code de plus de 80 caractères (espaces compris). eslint: [`max-len`](http://eslint.org/docs/rules/max-len.html) jscs: [`maximumLineLength`](http://jscs.info/rule/maximumLineLength)

  > Pourquoi ? Améliore la lisibilité et la maintenability

  :pushpin: _A noter que cette règle ne s'applique pas aux Strings, comme défini dans la recommendation [5.2](#strings--line-length)_

  ```javascript
  // Mauvais 
  if (jsonData && jsonData.results && jsonData.results.geometry && jsonData.results.geometry.latitude && jsonData.results.geometry.longitude) {
    // ...
  }

  // Bon
  if (jsonData
      && jsonData.results
      && jsonData.results.geometry
      && jsonData.results.geometry.latitude
      && jsonData.results.geometry.longitude) {
    // ...
  }

  // Mauvais
  $.ajax({ method: 'POST', url: 'http://www.cwtv.com/', data: { arrow: true, }, success(returnedData) { console.log(returnedData); }, error(returnedError) { console.log('You have failed this city !'); }, });

  // Bon
  $.ajax({
    method: 'POST',
    url: 'http://www.cwtv.com/',
    data: {
      arrow: true,
    },
    success(returnedData) {
      console.log(returnedData);
    },
    error(returnedError) {
      console.log('You have failed this city !');
    },
  });
  ```

:point_up: **[back to top](#tables-des-matières)**

## Virgules

  <a name="virgules--leading-trailing"></a><a name="11.1"></a>
  - [11.1](#virgules--leading-trailing) Les virgules en tête. **Nope.** eslint: [`comma-style`](http://eslint.org/docs/rules/comma-style.html) jscs: [`requireCommaBeforeLineBreak`](http://jscs.info/rule/requireCommaBeforeLineBreak)

  ```javascript
    // Mauvais
    const story = [
      once
      , upon
      , aTime
    ];

    // Bon
    const story = [
      once,
      upon,
      aTime,
    ];

    // Mauvais
    const hero = {
      firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'algorithms'
    };

    // Bon
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'algorithms',
    };
  ```

  <a name="virgules--dangling"></a><a name="11.2"></a>
  - [11.2](#virgules--dangling) Virgule additionnelle en queue. **Yup.** eslint: [`comma-dangle`](http://eslint.org/docs/rules/comma-dangle.html) jscs: [`requireTrailingComma`](http://jscs.info/rule/requireTrailingComma)

  > Pourquoi ? Permet de rajouter des déclarations sans se soucier des virgules précédentes. Améliore également la lisibilité des git diffs. Aussi, les transpillers tels que Babel retirent automatiquement la dernière virgule, donc pas besoin de se soucier non plus des [anciens navigateurs](#https://github.com/airbnb/javascript/blob/es5-deprecated/es5/README.md#commas).

  ```diff
  // Mauvais - 2 lignes dans le git diff pour 1 ligne ajoutée
  const hero = {
    firstName: 'Alan',
  - lastName: 'Turing'
  + lastName: 'Turing',
  + superPower: 'computers'
  };

  // Bon - 1 ligne dans le git diff pour 1 ligne ajoutée
  const hero = {
    firstName: 'Alan',
    lastName: 'Turing',
  + superPower: 'computers',
  };
  ```

  ```javascript
  // Mauvais
  const hero = {
    firstName: 'Dana',
    lastName: 'Scully'
  };

  const heroes = [
    'Batman',
    'Superman'
  ];

  // Bon
  const hero = {
    firstName: 'Dana',
    lastname: 'Scully',
  };

  const heroes = [
    'Batman',
    'Superman',
  ];

  // Mauvais
  function createHero(
    firstName,
    lastName,
    superPower
  ) {
    // ...
  }

  // Bon
  function createHero(
    firstName,
    lastName,
    superPower,
  ) {
    // ...
  }

  // Mauvais
  createHero(
    firstName,
    lastName,
    superPower
  );

  // Bon
  createHero(
    firstName,
    lastName,
    superPower,
  );
  ```

:point_up: **[back to top](#tables-des-matières)**

## Fonctions

  <a name="fonctions--declarations"></a><a name="12.1"></a>
  - [12.1](#fonctions--declarations) Utilisez des expressions de fonction nommées plutôt que des déclarations de fonction. Évitez les fonctions anonymes. eslint: [`func-style`](http://eslint.org/docs/rules/func-style) jscs: [`disallowFunctionDeclarations`](http://jscs.info/rule/disallowFunctionDeclarations)

  > Pourquoi ? Les déclarations de fonction sont hissées (« hoisted »). Cela signifie qu'il est facile d'utiliser une fonction avant qu'elle soit déclarée. Ça peut réduire la maintenabilité et la compréhension du code. Si votre fonction doit-être utilisée de manière globale, il faut envisager de l'exporter dans un fichier séparé.

  ```javascript
  // Mauvais
  function foo() {
    // ...
  }

  // Bon
  const foo = function () {
    // ...
  };
  ```

  <a name="fonctions--better-in-function"></a><a name="12.2"></a>
  - [12.2](#fonctions--iife) Évitez au maximum le code en dehors de fonction. Si possible utilisez les IIFE (« [Immediately Invoked Function Expressions](#https://developer.mozilla.org/fr/docs/Glossaire/IIFE) »).

  ```javascript
  // Mauvais
  const user = null;
  const reserved = true;
  let total = 0;

  // Bon
  const init = function () {
    const user = null;
    const reserved = true;
    let total = 0;
  };
  init();

  // Mieux
  (function () {
    const user = null;
    const reserved = true;
    let total = 0;
  }());

  // Encore mieux - Fonction nommée
  (function init() {
    const user = null;
    const reserved = true;
    let total = 0;
  }());
  ```

  <a name="fonctions--iife"></a><a name="12.3"></a>
  - [12.3](#fonctions--iife) Entourez toujours vos IIFE de paranthtèses. eslint: [`wrap-iife`](http://eslint.org/docs/rules/wrap-iife.html) jscs: [`requireParenthesesAroundIIFE`](http://jscs.info/rule/requireParenthesesAroundIIFE)

  > Pourquoi ? Une IIFE est une entité unique et solitaire. Entourer sa déclaration et son appel l'exprime clairement.

  ```javascript
  // Mauvais
  function init() {
    // ...
  }();

  // Bon
  (function init() {
    // ...
  }());
  ```

  <a name="fonctions--arguments-shadow"></a><a name="12.4"></a>
  - [12.4](#fonctions--arguments-shadow) Ne nommez jamais un paramètre `arguments`, cela va modifier l'objet `arguments` qui est utilisé par toutes les fonctions.

  ```javascript
  // Mauvais
  function foo(name, options, arguments) {
    // ...
  }

  // Bon
  function foo(name, options, args) {
    // ...
  }
  ```

  <a name="fonctions--rest"></a><a name="12.5"></a>
  - [12.5](#fonctions--rest) **ES6**: N'utilisez jamais `arguments`, privilégiez l'utilisation de la syntaxe rest `...`. eslint: [`prefer-rest-params`](http://eslint.org/docs/rules/prefer-rest-params)

  > Pourquoi ? `...` est explicite sur quels arguments vous voulez récupérer. En plus, les arguments rest sont de véritables Array, et non des moyennement Array-like comme `arguments`.

  ```javascript
  // Mauvais
  function concatenateAllArguments() {
    const args = Array.slice(arguments);
    return args.join('');
  }

  // Bon
  function concatenateAllArguments(...args) {
    return args.join('');
  }
  ```

  <a name="fonctions--default-parameters"></a><a name="12.6"></a>
  - [12.6](#fonctions--default-parameters) **ES6**: Utilisez les paramètres par défaut, plutôt que de modifier la valeur des paramètres.

  ```javascript
  // Très mauvais
  const handleThings = function (opts) {
    /**
     * Nous ne devons pas modifier la valeur d'un paramètre.
     * Doublement mauvais : Si opts est faux, il sera remplacé par un objet vide,
     * ce qui peut être ce que l'on souhaite, mais peut également créer des bugs.
     */
    opts = opts || {};
    // ...
  };

  // Mauvais
  const handleThings = function (opts) {
    if (opts === void 0) {
      opts = {};
    }
    // ...
  };

  // Bon
  const handleThings = function (opts = {}) {
    // ...
  };
  ```

  <a name="fonctions--default-side-effects"></a><a name="12.7"></a>
  - [12.7](#fonctions--default-side-effects) Les valeurs des paramètres par défaut doivent uniquement être des valeurs constantes. N'appliquez aucune opération sur la valeur de ces paramètres.

  > Pourquoi ? Ils sont déroutants.

  ```javascript
  var b = 1;
  
  // Mauvais
  function count(a = b++) {
    console.log(a);
  }

  count(); // 1
  count(); // 2
  count(3); // 3
  count(); // 3
  ```

  <a name="fonctions--defaults-last"></a><a name="12.8"></a>
  - [12.8](#fonctions--defaults-last) Placez les paramètres par défaut en dernier.

  ```javascript
  // Mauvais
  const handleThings = function (opts = {}, name) {
    // ...
  };

  // Bon
  const handleThings = function (name, opts = {}) {
    // ...
  };
  ```

  <a name="fonctions--constructor"></a><a name="12.9"></a>
  - [12.9](#fonctions--constructor) N'utilisez jamais le constructeur de fonction pour créer une nouvelle fonction. eslint: [`no-new-func`](http://eslint.org/docs/rules/no-new-func)

  > Pourquoi ? Créer une fonction de cette façon traite une String de la même manière que `eval()`, ce qui ouvre des vulérabilités, comme précisé dans la recommendation [5.4](#strings--no-eval).

  ```javascript
  // Mauvais
  const add = new Function('a', 'b', 'return a + b');

  // Encore mauvais
  const substract = Function('a', 'b', 'return a - b');

  // Bon
  const multiply = function (a, b) {
    return a * b;
  };
  ```

  <a name="fonctions--spacing"></a><a name="12.10"></a>
  - [12.10](#fonctions--spacing) Espacez votre fonction toujours de la même manière. eslint: [`space-before-function-paren`](http://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks)

  > Pourquoi ? La constance est une bonne chose.

  :pushpin: _A noter que vous devez pouvoir ajouter et retirer le nom de la fonction sans ajouter ou retirer d'espaces. A noter également que c'est une combinaison des recommendations [10.2](#espacements--before-blocks) et [10.3](#espacements--around-keywords)._

  ```javascript
  // Mauvais
  const a = function(){};
  const b = function (){};
  const c = function() {};

  // Bon
  const x = function () {};
  const y = function z() {};
  ```

  <a name="fonctions--mutate-params"></a><a name="12.11"></a>
  - [12.11](#fonctions--mutate-params) Ne modifiez jamais la valeur des paramètres. eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

  ```javascript
  // Mauvais
  function countSomething(nb) {
    nb += 1;
  }

  // Bon
  function countSomething(nb) {
    const nbTmp = nb;
    nbTmp += 1;
  }

  // Mauvais
  function doSomething(ojb) {
    // Modifie l'objet d'origine, ce qui peut avoir des effets indésirables
    obj.key = 1;
  }

  // Bon
  function doSomething(obj) {
    const key = obj.key;
    key = 1
  }
  ```

  <a name="fonctions--reassign-params"></a><a name="12.12"></a>
  - [12.12](#fonctions--reassign-params) Ne réassignez jamais les paramètres. eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

  > Pourquoi ? Le réassignement de paramètre peut amener à des comportements inattendus, spécialement si on accède à l'objet `arguments`. Ça peut aussi créer des problèmes d'optimisation.

  ```javascript
  // Mauvais
  function doSomething(a) {
    a = 1;
  }

  function doSomething(a) {
    if (!a) { a = 1; }
  }

  // Bon
  function doSomething(a) {
    const b = a || 1;
  }

  function doSomething(a = 1) {

  }
  ```

  <a name="fonctions--signature-invocation-indentation"></a><a name="12.13"></a>
  - [12.13](#fonctions--signature-invocation-indentation) La déclaration et l'appel de fonction avec plusieurs paramètres doivent se comporter comme toutes les autres listes multilignes de cette convention : 1 ligne par paramètre avec 1 virgule en fin.

  ```javascript
  // Mauvais
  function episodeThree(anakin,
    obiWan,
    yoda) {
    // ...
  }

  // Bon
  function episodeThree(
    anakin,
    obiWan, 
    yoda,
  ) {
    // ...
  }

  // Mauvais
  console.log('the', 'walking', 'dead');

  // Bon
  console.log(
    'the',
    'walking',
    'dead',
  );
  ```

:point_up: **[back to top](#tables-des-matières)**

## Propriétés

  <a name="proprietes--dot"></a><a name="13.1"></a>
  - [13.1](#proprietes--dot) Utilisez la notation point `.` pour accéder aux propriétés. Utilisez la notation crochet `[]` pour accéder aux propriétés avec des noms non valide. eslint: [`dot-notation`](http://eslint.org/docs/rules/dot-notation.html) jscs: [`requireDotNotation`](http://jscs.info/rule/requireDotNotation)

  ```javascript
  const luke = {
    age: 28,
    jedi: true,
    'obi-wan': 'mentor',
  };

  // Mauvais
  const isJedi = luke['jedi'];

  // Bon
  const isJedi = luke.jedi;
  const obiWanRole = luke['obi-wan']; 
  ```

  <a name="proprietes--bracket"></a><a name="13.2"></a>
  - [13.2](#proprietes--bracket) Utilisez la notation crochet `[]` pour accéder aux propriétés via une variable.

  ```javascript
  const luke = {
    age: 28,
    jedi: true,
  };

  const prop = 'jedi';
  const isJedi = luke[prop];
  ```

:point_up: **[back to top](#tables-des-matières)**

## Type Cast

  <a name="type-cast--explicit"></a><a name="14.1"></a>
  - [14.1](#type-cast--explicit) Effectuez le cast au début de la déclaration.

  <a name="type-cast--strings"></a><a name="14.2"></a>
  - [14.2](#type-cast--strings) Strings:

  ```javascript
  const score = 9;

  // Mauvais
  // Invoke score.valueOf()
  const totalScore = score + '';

  // Mauvais
  // Ne garantie pas une String en retour
  const totalScore = score.toString();

  // Bon
  const totalScore = String(score);
  ```

  <a name="type-cast--numbers"></a><a name="14.3"></a>
  - [14.3](#type-cast--numbers) Numbers: Utilisez `Number` pour faire un cast, et utilisez `parseInt` toujours avec une base précisée pour parser des Strings. eslint: [`radix`](http://eslint.org/docs/rules/radix)

  ```javascript
  const inputValue = '4';

  // Mauvais
  const val = new Number(inputValue);

  // Mauvais
  const val = +inputValue;

  // Mauvais
  const val = inputValue >> 0;

  // Mauvais
  const val = parseInt(inputValue);

  // Bon
  const val = Number(inputValue);

  // Bon
  const val = paseInt(inputValue, 10);
  ```

  <a name="type-cast--booleans"></a><a name="14.4"></a>
  - [14.4](#type-cast--booleans) Booleans:

  ```javascript
  const age = 0;

  // Mauvais
  const hasAge = new Boolean(age);

  // Bon
  const hasAge = Boolean(age);

  // Mieux
  const hasAge = !!age;
  ```

:point_up: **[back to top](#tables-des-matières)**

## Nommage

  <a name="nommage--descriptive"></a><a name="15.1"></a>
  - [15.1](#nommage--descriptive) Évitez les noms d'une lettre. Utilisez des noms descriptifs. eslint: [`id-length`](http://eslint.org/docs/rules/id-length)

  ```javascript
  // Mauvais
  function q() {
    // ...
  }

  // Bon
  function query() {
    // ...
  }
  ```

  <a name="nommage--camel-case"></a><a name="15.2"></a>
  - [15.2](#nommage--camel-case) Nommez toujours en camelCase (variables, propriétés, fonctions, ...). eslint: [`camelcase`](http://eslint.org/docs/rules/camelcase.html) jscs: [`requireCamelCaseOrUpperCaseIdentifiers`](http://jscs.info/rule/requireCamelCaseOrUpperCaseIdentifiers)

  ```javascript
  // Mauvais
  const snoop_dogg = {
    'death-row-records': true,
  };

  function smokeweed() {
    return 'everyday';
  }

  // Bon
  const snoopDogg = {
    deathRowRecords: true,
  };

  function smokeWeed() {
    return 'everyday';
  }
  ```

  <a name="nommage--pascal-case"></a><a name="15.3"></a>
  - [15.3](#nommage--pascal-case) Utilisez le PascalCase uniquement pour déclarer les constructeurs et les classes. eslint: [`new-cap`](http://eslint.org/docs/rules/new-cap.html) jscs: [`requireCapitalizedConstructors`](http://jscs.info/rule/requireCapitalizedConstructors)

  <a name="nommage--underscore"></a><a name="15.4"></a>
  - [15.4](#nommage--underscore) N'utilisez pas d'underscore devant ou après un nom. eslint: [`no-underscore-dangle`](http://eslint.org/docs/rules/no-underscore-dangle.html) jscs: [`disallowDanglingUnderscores`](http://jscs.info/rule/disallowDanglingUnderscores)

  <a name="nommage--filename"></a><a name="15.5"></a>
  - [15.5](#nommage--filename) Nommez vos fichiers et dossiers en camelCase.

  <a name="nommage--acronyms"></a><a name="15.6"></a>
  - [15.6](#nommage--acronyms) Les acronymes et initiales devraient toujours être en lettres capitales.

  > Pourquoi ? Les noms sont fait pour être lu, et non pour appaiser un algorithme.

  ```javascript
  // Mauvais
  const HttpRequests = [];

  // Bon
  const HTTPRequest = [];
  ```

  <a name="nommage--booleans"></a><a name="15.7"></a>
  - [15.7](#nommage--booleans) Nommez les booleans ou les noms de fonctions qui retournent un boolean tel que `isVal()` ou `hasVal()`.

  > Pourquoi ? Permet de savoir rapidement et visuellement que la valeur est un boolean.

  ```javascript
  // Mauvais
  input.attrExist();
  const checked = true;

  // Bon
  input.hasAttr();
  const isChecked = true;
  ```

  <a name="nommage--english"></a><a name="15.8"></a>
  - [15.8](#nommage--english) Nommez vos éléments en anglais. On ne traduit pas les noms de classe ou constructeur.

  ```javascript
  // Mauvais
  const tableauElements = [];

  // Bon
  const arrayItems = [];

  // Mauvais
  function chargerUtilisateur() {
    return objetRetour;
  }

  // Bon
  function loadUtilisateur() {
    return returnedObject;
  }
  ```

:point_up: **[back to top](#tables-des-matières)**

## jQuery

  <a name="jquery--dollar-prefix"></a><a name="16.1"></a>
  - [16.1](#jquery--dollar-prefix) Préfixez vos variables d'objets jQuery d'un `$`. jscs: [`requireDollarBeforejQueryAssignment`](http://jscs.info/rule/requireDollarBeforejQueryAssignment)

  > Pourquoi ? Permet directement de savoir que l'on peut appliquer les fonctions jQuery sur cette variable.

  ```javascript
  // Mauvais
  const sidebar = $('.sidebar');

  // Bon
  const $sidebar = $('.sidebar');
  const $sidebarBtn = $('sidebar-btn');
  ```

  <a name="jquery--cache"></a><a name="16.2"></a>
  - [16.2](#jquery--cache) Stockez vos éléments jQuery si vous utilisez plus d'1 fois.

  > Pourquoi ? Évite à jQuery de parcourir plusieurs fois le DOM pour récupérer le même élément.

  ```javascript
  // Mauvais
  function setSidebar() {
    $('.sidebar').hide();

    // ...

    $('.sidebar').css({
      'background-color': 'pink',
    });
  }

  // Bon
  function setSidebar() {
    $sidebar = $('.sidebar');
    $sidebar.hide();

    // ...

    $sidebar.css({
      'background-color': 'pink',
    });
  }
  ```

  <a name="jquery--queries"></a><a name="16.3"></a>
  - [16.3](#jquery--queries) Pour les requêtes sur le DOM, utilisez les sélecteurs en cascade `$('.sidebar ul')` ou parent > enfant `$('.sidebar > ul')`. jsPerf: [`find`](http://jsperf.com/jquery-find-vs-context-sel/16)

  <a name="jquery--find"></a><a name="16.4"></a>
  - [16.4](#jquery--find) N'utilisez `find` qu'avec des variables contenant des objets jQuery, sinon utilisez les requêtes comme précisé en [16.3](#jquery--queries).

  ```javascript
  // Mauvais
  $('ul', '.sidebar').hide();

  // Mauvais
  $('.sidebar').find('ul').hide();

  // Bon
  $('.sidebar ul').hide();

  // Bon
  $('.sidebar > ul').hide();

  // Bon
  $sidebar.find('ul').hide();
  ```

:point_up: **[back to top](#tables-des-matières)**

## Ajax

  <a name="ajax--queries"></a><a name="17.1"></a>
  - [17.1](#ajax--queries) **ES6**: Utilisez la méthode de jQuery `$.ajax` pour toutes vos requêtes ajax. Pour chaque requête, veuillez préciser ces 5 paramètres :

    + `method` String définissant le type de requête
    + `url` String définissant à l'url requêtée
    + `data` Objet définissant les paramètres à envoyer - A définir même si aucun paramètre n'est à envoyer
    + `success` Fonction shorthand appelée en cas de succès de la requête
    + `error` Fonction shorthand appelée en cas d'échec de la requête

  ```javascript
  $.ajax({
    method: 'POST',
    url: 'https://www.neo-nomade.com/ajax',
    data: {
      deadpool: 'Francis',
    },
    success(returnedJsonData) {

    },
    error(returnedError) {

    },
  });
  ```

:point_up: **[back to top](#tables-des-matières)**

# };
