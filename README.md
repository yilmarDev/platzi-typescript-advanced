# Curso de Typescript: Tipos avanzados y funciones

- Tutor: Nicolas Molina
- Plataforma: Platzi
- Segundo curso se la serie de TS
<hr />

# Módulo 1: Introducción

## Lección 3: Enum

Se declaran con la palabra reservada "enum" y sus nombre se escriben con mayúsculas, igual que los types nos permiten establecer un conjunto de valores posibles para una variable, pero tienen la ventaja de que estos valores se muestran directamente en el editor de código al momento de usar el enum.
Esto hace que sea más fácil usar los valores correctos y evita que se comentan errores en el código.

<pre>
  <code>
    enum ROLES {
      ADMIN = "admin",
      SELLER = "seller",
      CUSTOMER = "customer",
    }
  </code>
</pre>

## Leccion 4: Tuplas

Permiten definir Arreglos fuertemente tipados y establecer característacas en las posiciones de los elementos y en la cantidad de los mismos.

Es decir que permiten limitar las posiciones de los arreglos y definir el tipo de dato para cada posición.

### Tipando arreglos sin tuplas

<pre>
  <code>
    // Arreglo con un solo tipo de datos
    const prices: number[] = [1500, 3999, 10000, 300];

    // Arreglo con dos tipos de datos
    const various: (number | string)[] = [1500, 3000, "Hola"];
  </code>
</pre>

### Tipando arreglos con tuplas

<pre>
  <code>
    // Arreglo de 2 elementos; string y number
    const user: [string, number] = ["yilmardev", 30];

    //array de 4 elementos; 2 string 1 number y 1 boolean
    const product: [string, string, number, boolean] = ["Uniforme médico", "rojo", 95000, true];
  </code>
</pre>

### Haciendo desctructuración

Las tuplas permiten hacer desctructuración

<pre>
  <code>
    //array de 4 elementos; 2 string 1 number y 1 boolean
    const product: [string, string, number, boolean] = [
      'Uniforme médico',
      'rojo',
      95000,
      true,
    ];

    const [title, color] = product;
  </code>
</pre>

## Tipo unknow

Es una mejor forma de evitar el tipo "any". Es un tipado que nos permite cambiar el tipo de dato que contiene la variable, es esto es muy similar a any.

Sin embargo, unknow no permite ejecutar funciones sin verfificar el tipo de dato.

<pre>
  <code>
    let unknowVar: unknown;

    // el tipo unknown permite cambiar el tipo de valor de las variables 
    unknownVar = 124;
    unknownVar = null;
    unknownVar = undefined;
    unknownVar = {};
    unknownVar = [];

    // Sin embargo, esta línea da error porque es necesario validar el tipo de valor
    unknownVar.toUpperCase();

    // Este código funciona porque se hace "verificación de tipo" antes de usar una función de los string
    if (typeof unknownVar === 'string') {
      unknownVar.toUpperCase();
    }

    // Haciendo validación de tipo para asignar el valor unknown a otra variable
    if(typeof unknownVar === 'boolean') {
      let unknownVarV2: boolean = unknownVar;
    }
  </code>
</pre>

Aplicándolo en funciones

<pre>
  <code>
    const parse = (str: string): unknown => {
      return JSON.parse(str);
    }
  </code>
</pre>

## Tipo Never

El tipo "never" se usa para funciones que sabemos que nunca van a terminar, como los ciclos infinitos.
TypeScript es capaz de detectar cuando una función nunca se va a detener y la tipa como "never", deteniendo la ejecución de la aplicación.

<pre>
  <code>
    const withoutEnd = () => {
      while (true) {
        console.log('Hola mundo infinito');
      }
    };

    const fail = (message: string) => {
      throw new Error(message);
    };

    const example = (input: unknown) => {
      if (typeof input === 'string') {
        return 'Es un string';
      }
      // Así podemos comprobar si un objeto es un Array
      if (Array.isArray(input)) {
        return 'Es un arreglo';
      }
      return fail('Not match');
    };

    console.log('Hola mundo');
    console.log([1, 5, 7, 8]);
    console.log(4589);

    //Esta línea nunca se va a ejecutar porque TS detiene la ejecución al detectar el never
    console.log('Hola mundo');
  </code>
</pre>
<hr />

# Módulos 2: Funciones

## Parámetros opcionales y Nullish-coalescing

### Parámetros opcionales

Los parámetros opcionales en funciones TS se declaran al añadir un signo de interrogación junsto despues de declarar el parámetro y deben ir al final de la lista de parámetros aceptados por la función.

<pre>
  <code>
    export const createProduct = (
      id: string | number,
      stock?: number,
      isNew?: boolean
    ) => {
      return { id, stock, isNew };
    };
  </code>
</pre>

En caso de que los parámetros opcionales no se envíen, estos quedarán como undefined, lo que puede causar errores o problemas que van a necesitar validaciones a futuro, para evitar eso podemos establecer valores por defecto para estos parámetros usando "||".

<pre>
  <code>
    export const createProduct = (
      id: string | number,
      stock?: number,
      isNew?: boolean
    ) => {
      return { id, stock: stock || 10, isNew: stock || true };
    };
  </code>
</pre>

Así, los parámetros opcionales siempre van a tener un vlor definido. Aunque el operador "||" tiene algunos problemas, ya que JS interpreta los valores "0", "" y false como false, así que si se reciben estos valores en uno de los parámetros opcionales se va a utilizar el valor predeterminado, y esto no es lo que queremos.

### Nullish-coalescing "??"

La solución de JS para este problema es el operador Nullish-coalescing "??", que solo va a usar la parte derecha del operador, si la parte izquierda es "null" o "undefined".

<pre>
  <code>
    export const createProduct = (
      id: string | number,
      stock?: number,
      isNew?: boolean
    ) => {
      return { id, stock: stock ?? 10, isNew: stock ?? true };
    };
  </code>
</pre>

## Parámetros por defecto en TypeScript

TypeScrip también permite manejar parámetros por defecto, esta es la forma en que se usa:

<pre>
  <code>
    export const createProduct = (
      id: string | number,
      stock: number = 10,
      isNew: boolean = true
    ) => {
      return { id, stock, isNew };
    };

    const product1 = createProduct(23, 5, false);
    const product2 = createProduct(45, 5);
    const product3 = createProduct(74);
    
    console.log(product1);
    console.log(product2);
    console.log(product3);
  </code>
</pre>

# Parámetros REST

La sintaxis de los parámetros rest nos permiten representar un número indefinido de argumentos como un array.

<pre>
  <code>
    function sum(...theArgs) {
      let total = 0;
      for (const arg of theArgs) {
        total += arg;
      }
      return total;
    }

    console.log(sum(1, 2, 3));
    // Expected output: 6

    console.log(sum(1, 2, 3, 4));
    // Expected output: 10
  </code>
</pre>

Ejemplo más detallado en el archivo src/07-rest.ts

## Sobrecarga de funciones

Hay situaciones en las que necesitamos enviarle parámetros a una función para que efectúe acciones basándonos en el tipo de esos parámetros, y JS permite hacerlo, aunque no con mucha seguridad, como en este caso:

<pre>
  <code>
    function parseStr(input: string | string[]): string | string[] {
      if (Array.isArray(input)) return input.join('');
      else return input.split('');
    }

    const rtaArray = parseStr('yilmar');
    if (Array.isArray(rtaArray)) rtaArray.reverse();
    console.log('rtaArray', 'yilmar =>', rtaArray);

    const rtaStr = parseStr(['y', 'i', 'l', 'm', 'a', 'r']);
    if (typeof rtaStr === 'string') rtaStr.toLowerCase();
    console.log('rtaStr', "['y', 'i', 'l', 'm', 'a', 'r'] =>", rtaStr);
  </code>
</pre>

La limitante que tiene este código es que al momento de llamar a la función, TS no nos proporciona ayuda sobre las funciones a elegir, ya que TS no puede determinar que tipo de dato está recibiendo exactamente, además, debemos hacer validaciones de tipo para poder usar la función.

Para solucionar este problema existe la sobrecarga de funciones:

<pre>
  <code>
    export function parseStr(input: string): string[];
    export function parseStr(input: string[]): string;

    export function parseStr(input: string | string[]): string | string[] {
      if (Array.isArray(input)) return input.join('');
      else return input.split('');
    }

    const rtaArray = parseStr('yilmar');
    console.log('rtaArray', 'yilmar =>', rtaArray);

    const rtaStr = parseStr(['y', 'i', 'l', 'm', 'a', 'r']);
    console.log('rtaStr', "['y', 'i', 'l', 'm', 'a', 'r'] =>", rtaStr);
  </code>
</pre>

En esta versión del código el editor nos ayuda más y ya no tenemos que hacer validaciones de tipo.

### Buenas prácticas

- Si una función tiene sobrecargas y una de ellas recibe parametros de tipo "any" o "unknown" esta sobrecarga debe ir al final.
- Si una función puede usar una cantidad variable de parámetros, pero en todos los casos retorna algo del mismo tipo, sería mejor usar parámetros opcionales.
- Si una función puede usar parámetros de diferente tipo pero la salida es del mismo tipo, se puede usar "Union Type" y evitar una sobrecarga.

# Interfaces

## Diferencias entre Interfaces y Types

Las interfaces funcionan casi del mismo modo que los typos y en muchos casos solucionan los mismos problemas, pero existen algunas diferencias:

- Type solo permite definir tipos primitivos o directos, como: string, number o boolean.
- Las interfaces necesitan un cuerpo completo, por lo tanto no es posible definir una interfaz con un solo dato.
- Las interfaces pueden usar "extend" es decir que pueden heredar.

## Estructuras complejas con interfaces

Las interfaces nos permiten crear estructuras mucho más complejas de tipado, como modelos de entidades.

Puedes observar un ejemplo dentro de la carpeta "src/app" donde se creó una estructura compleja de entidades para administrar órdenes de compra con varios productos de un cliente.

## Extender interfaces

Extender las interfaces nos permiten crear estructuras mucho más complejas de tipado, como modelos de entidades, esto resulta muy útil, ya que podemos generar estructuras de tipado mucho más complejas y eficieentes.

<pre>
  <code>
   export interface BaseModel {
      id: string | number;
      createdAt: Date;
      updatedAt: Date;
    }

    export interface Order extends BaseModel {
      products: Product[];
      user: User;
    }
  </code>
</pre>

## Propiedades de solo lectura

Naturalmente las propiedades de un objeto pueden ser sobreescritas y está bien porque los elementos deben poder actualizarse y cambiar según la necesidad del negocio, sin emabargo, hay algunos datos o propieadades que podemos querer evitar que sean actualizables, como el id de una entidad o la fecha de creación del mismo, por ejemplo.

Para lograr esto debemos marcar la propiedad con readonly al momento de declararla.

<pre>
  <code>
   export interface BaseModel {
      readonly id: string | number;
      readonly createdAt: Date;
      updatedAt: Date;
    }
  </code>
</pre>

## DTO: Data Transfer Objects

Los DTO son versiones ligeramente modificadas o resumidas de los modelos de datos que se usan para fines específicos como enviar data a la BD o cambiar ligeramente los campos que representan las llaves foraneas en la BD.

Esto nos permite generar las versiones que necesitamos del modelo pero conservando la estructura de datos y previniendo problemas futuros al actualizar el modelo.

## Utility Types

Son una herramienta de TS para facilitar las transformaciones de tipos comunes.Estas utilidades están disponibles a nivel global. y entre estas tenemos a: Omit, Pick y Partial entre otros.

### Omit

Es un "Utility Type" que nos permite omitir propiedades específicas de una interface o de un type, y nos ayuda a crear los DTO.

<pre>
  <code>
    // Interface de producto original y completa
    export interface Product extends BaseModel {
      title: string;
      description: string;
      image: string;
      stock: number;
      size?: Sizes;
      color: string;
      price: number;
      isNew: boolean;
      tags: string[];
      category: Category;
    }

    // Versión modificada del producto para enviar a guardar en BD
    export interface CreateProductDto
    
    interface createProductDto extends Omit< Product, "id" | 'createdAt' | 'updatedAt' | 'category' > {
      categoryId: string;
    }
  </code>
</pre>

### Pick

Es un "Utility Type" que nos permite seleccionar propiedades específicas de una interface o de un type, y nos ayuda a crear los DTO.

<pre>
  <code>
    // Interface de producto original y completa
    export interface Product extends BaseModel {
      title: string;
      description: string;
      image: string;
      stock: number;
      size?: Sizes;
      color: string;
      price: number;
      isNew: boolean;
      tags: string[];
      category: Category;
    }

    // Versión modificada del producto para enviar a guardar en BD
    export interface CreateProductDto    
    type example = Pick< Product, 'title' | 'image' | 'color'>

  </code>
</pre>

### Partial

Es un "Utility Type" que permite cambiar las propiedades de una interface y hacerlas opcionales, esto nos permite hacer actualizaciones de entidades de una forma sencilla y sin tener que generar código duplicado.

<pre>
  <code>
    // Interface de producto original y completa
    export interface Product extends BaseModel {
      title: string;
      description: string;
      image: string;
      stock: number;
      size?: Sizes;
      color: string;
      price: number;
      isNew: boolean;
      tags: string[];
      category: Category;
    }

    // UpdateProductDto será una versión de las interface CreateProductDto que nos permite enviar una, 
    // varias o todas las propiedades del producto
    export interface UpdateProductDto extends Partial< CreateProductDto> {}

  </code>
</pre>

### Required

Es un "Utility Type" que permite cambiar las propiedades de una interface y hacerlas obligatorias, esto significa que para usar esta interface tenemos que envíar todas las propiedades de product.

<pre>
  <code>
    // Interface de producto original y completa
    export interface Product extends BaseModel {
      title: string;
      description: string;
      image: string;
      stock: number;
      size?: Sizes;
      color: string;
      price: number;
      isNew: boolean;
      tags: string[];
      category: Category;
    }

    // RequiredProductDto será una versión de las interface CreateProductDto que nos obliga 
    // a enviar todas las propiedades del producto
    export interface RequiredProductDto extends Required< CreateProductDto> {}

  </code>
</pre>

### Radonly

Es un "Utility Type" que permite modificar las propiedades de la interface para que solo se puedan leer, esto es ideal en casos de buscadores, ya que en esa situación no se quiere que se pueda editar el objeto, solo usar la data para filtrar.

<pre>
  <code>
    // Interface de producto original y completa
    export interface Product extends BaseModel {
      title: string;
      description: string;
      image: string;
      stock: number;
      size?: Sizes;
      color: string;
      price: number;
      isNew: boolean;
      tags: string[];
      category: Category;
    }

    // FindProductDto será una versión de las interface Product que recibe una o varias propiedades
    // del producto para hacer la búsqueda pero no puede modificarlas.
    export interface FindProductDto extends Readonly< Partial< Product>> {}

  </code>
</pre>

## Acceder al tipado por índice

Cuando estamos asignando el tipado a una variable es posible acceder a las propieades de la interface usando los [] de la nomenclatura de los arreglos. Esto nos permite acceder al tipado exacto de la propiedad dentro de la interface y puede ayudar en caso de que ese tipado cambie en algún momento.

<pre>
  <code>
    // Establecemos que el id es de tipo string
    export const updateProduct = ( id: string, changes: UpdateProductDto ) => { };

    // Establecemos que el id es del mismo tipo que la propiedad id de la interface Product
    export const updateProduct = ( id: Product[id], changes: UpdateProductDto ) => { };
  </code>
</pre>

## ReadonlyArray

Existe utility type que permite hacer que un arreglo sea inmutable, esto es muy útil para trabajar con tecnologías como react o Redux que se basan en la inmutabilidad para funcionar correctamente.

<pre>
  <code>
    const mutableNumbers: number[] = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11];

    // Métodos mutables que modifican directamente el array
    mutableNumbers.push(5);
    mutableNumbers.pop();
    mutableNumbers.unshift(2);

    const inmutableNumbers: ReadonlyArray<number> = [1, 2, 3, 4, 5, 5, 6, 7, 8];

    //Métodos inmutables que no modifican el array original, en su lugar crear
    // una copia de este.
    inmutableNumbers.filter(() => {});
    inmutableNumbers.reduce(() => 0);
    inmutableNumbers.map(() => 0);
  </code>
</pre>
