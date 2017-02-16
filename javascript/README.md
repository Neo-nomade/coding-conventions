# NCC Javascript() {

  Les conventions qui suivent s'applique à tous notre Javascript.

  Elles sont majoritairement inspirées des [conventions d'Airbnb](https://github.com/airbnb/javascript).

  Autres conventions
  - [Php](../php/)

## Tables des matières
  1. [Types](#Types)

## Types

  <a name="types--primitifs"></a><a name="1.1"></a>
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
    
    // => 1, 9
    console.log(foo, bar); 
    ```

  <a name="types--complexes"></a><a name="1.2"></a>
  - [1.2](#types--complexes) **Complexes**: Quand on accède à un type complexe, on travaille sur une référence de sa valeur.
  
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
    
# };
