# NCC Javascript() {

  Les conventions qui suivent s'applique à tous notre Javascript.

  Elles sont majoritairement inspirées des [conventions d'Airbnb](https://github.com/airbnb/javascript).

  Autres conventions
  - [Php](../php/)

## Tables des matières
  1. [Types](#Types)

## Types

  <a name="types--primitives"></a><a name="1.1"></a>
  - [1.1](#types--primitives) **Primitifs**: Quand on accède à un type primitif, on travaille directement sur sa valeur.
  
    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`
    
    ```javascript
    const foo = 1;
    let bar = foo;
    
    bar = 9;
    
    console.log(foo, bar); // => 1, 9
    ```
    
# };
