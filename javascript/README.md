# NCC Javascript() {

  Les conventions qui suivent s'applique à tous notre Javascript.

  Elles sont majoritairement inspirées des [conventions d'Airbnb](https://github.com/airbnb/javascript).

  Autres conventions
  - [Php](../php/)

## Tables des matières
  1. [Types](#types)
  1. [Références](#références)
  1. [Objets](#objets)

## Types

  <a name="types--primitifs"></a><a name="1.1"></a>
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

  <a name="types--complexes"></a><a name="1.2"></a>
  - [1.2](#types--complexes) **Complexes**: Quand vous accédez à un type complexe, vous travaillez sur une référence de sa valeur.

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

**[⬆ back to top](#tables-des-matières)**

## Références

  <a name="references--prefer-const"></a><a name="2.1"></a>
  - [2.1](#references--prefer-const) Utilisez `const` pour toutes vos références. Évitez d'utiliser `var`. eslint: [`prefer-const`](http://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](http://eslint.org/docs/rules/no-const-assign.html)

  > Pourquoi ? Cela garantit que vous ne pouvez pas réattribuer vos références, ce qui pourrait conduire à des bugs et nuire à la compréhension du code.

  ```javascript
  // Pas bien
  var a = 1;
  var b = 2;
  
  // Bien
  const a = 1;
  const b = 2;
  ```

  <a name="references--no-var"></a><a name="2.2"></a>
  - [2.2](#references--no-var) Si vous devez réassigner vos références, utilisez `let` à la place de `var`. eslint: [`no-var`](http://eslint.org/docs/rules/no-var.html) jscs: [`disallowVar`](http://jscs.info/rule/disallowVar)

  > Pourquoi ? `let` est block-scopée ce qui est mieux que `var` qui est function-scopée.

  ```javascript
  // Pas bien
  var count = 1;
  if (true) {
    count += 1;
  }
  
  // Bien
  let count = 1;
  if (true) {
    count += 1;
  }
  ```

  <a name="references--block-scoped"></a><a name="2.3"></a>
  - [2.3](#references--block-scoped) Notez que `const` et `let` sont toutes les deux block-scopées.

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

**[⬆ back to top](#tables-des-matières)**

## Objets

  <a name="objets--no-new"></a><a name="3.1"></a>
  - [3.1](#objets--no-new) Utilisez la syntaxe littérale à la place du constructeur pour la création d'objet. eslint: [`no-new-object`](http://eslint.org/docs/rules/no-new-object.html)

  > Pourquoi ? Bien que la performance ne varie pas entre les deux méthodes, la syntaxe littérale est plus concise et présente un gain de bit, ce qui en fait la méthode standard de création d'objet.

  ```javascript
  // Pas bien
  const item = new Object();

  // Bien
  const item = {};
  ```

  <a name="es6-computed-properties"></a><a name="3.2"></a>
  - [3.2](#es6-computed-properties) **ES6**: Utilisez les noms de propriété calculés quand vous créez des objets avec des noms de propriété dynamiques.

  > Pourquoi ? Les noms de propriété calculés vous permettent de définir toutes les propriétés d'un objet en un seul endroit.

  ```javascript
  function getKey(k) {
    return '#' + k;
  }

  // Pas bien
  const obj = {
    id: 3,
    name: 'Obi-Wan',
  };
  obj[getKey('jedi')] = true;

  // Bien
  const obj = {
    id: 3,
    name: 'Obi-Wan',
    [getKey('jedi')]: true,
  };
  ```

# };
