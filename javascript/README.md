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

  <a name="objets--access-props"></a><a name="3.7"></a>
  - [3.7](#access-props) 

  <a name="objets--spread-operator"></a><a name="3.8"></a>
  - [3.8](#objets--spread-operator) **ES6**: Utilisez l'opérateur de décomposition à la place de [`Object.assign`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Object/assign) pour cloner des objets.

  ```javascript
  const original = { a: 1, b: 2 };

  // Très mauvais
  const copy = Object.assign(original, { c: 3 }); // Modifie `original` ಠ_ಠ

  // Mauvais

  const copy = Object.assign({}, original, { c: 3 }); // copy => { a: 1, b: 2, c: 3 }

  // Bon
  const copy = { ...original, c: 3 }; // copy => { a: 1, b: 2, c: 3 }
  ```

  <a name="objets--omit-properties"></a><a name="3.9"></a>
  - [3.9](#objets--omit-properties) **ES6**: Utilisez l'opérateur de décomposition pour cloner un objet en omettant certaines propriétés.

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

  :pushpin: _A noter qu'il n'y a pas d'espace entre les `{}` pour les variables extrapolées. Contrairement à la recommendation [X.X](#types)._

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
  - [7.4](#operateurs--switch-blocks) Utilisez les crochets pour créer des blocks dans les clauses `case` et `default`. eslint: [`no-case-declarations`](http://eslint.org/docs/rules/no-case-declarations.html)

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
  const foo = stack1.countSomething() > stack2.countSomething() ? 'bigger' : 'smaller';

  // Bon
  const stack1Count = stack1.countSomething();
  const stack2Count = stack2.countSomething();
  const foo = stack1Count > stack2Count ? 'bigger' : 'smaller';

  // Bon aussi
  let foo = 'smaller';
  if (stack1.countSomething() > stack2.countSomething()) {
    foo = 'bigger';
  }

  // Mauvais
  const foo = 3 > 2 ? stack.countSomething() : null;

  // Bon
  let foo = null;
  if (3 > 2) {
    foo = stack.countSomething();
  }
  ```

  <a name="operateurs--unneed-ternaries"></a><a name="7.7"></a>
  - [7.7](#operateurs--unneeded-ternaries) Évitez les ternaires inutiles. eslint: [`no-unneeded-ternary`](http://eslint.org/docs/rules/no-unneeded-ternary.html)

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

:point_up: **[back to top](#tables-des-matières)**

# };
