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

  :pencil: _A noter qu'il n'y a pas d'espace entre les `{}` pour les variables extrapolées. Contrairement à la recommendation [X.X](#types)._

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

# };
