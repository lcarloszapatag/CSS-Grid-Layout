# Curso CSS Grid Layout
Para controlar nuestros layout mediante "CSS Grid Layout" es muy sencillo. Primero un contenedor que va a contener nuestro layout. Le damos la propiedad `display: grid`. A partir de aquí controlamos las filas y columnas medinte las propiedades: `grid-template-columns` y `grid-template-rows`.

```Stylus
.container
  display: grid;
  grid-template-columns: 25% 200px 25%;
  grid-template-rows: 300px 150px;

```

En forma condensada:

```Stylus
.container
  display: grid;
  // grid-template: rows / columns;
  grid-template: 300px 150px / 25% 200px 25%;

```

## Displays
Grid Layout va a traer nuevos `displays`, como repetir el grid del padre en una celda hijo, `display: subgrid` repetirá todo el grid del padre. Vendrán mas tipos de _displays_ pero eso será en el futuro.

## Espaciado
Para conseguir el espaciado de los elementos del layout podemos usar la propiedad `margin`. Pero *grid Layout* nos trae un mejor modo de espaciar los diferentes elementos. La forma de hacerlo es con `grid-column-gap` y `grid-row-gap` o en modo compacto, `grid-gap`.

```Stylus
.container
  display: grid;
  grid-template: 300px 150px / 50% 200px 25%;
  grid-column-gap: 20px;
  grid-row-gap: 20px;

```

```Stylus
.container
  display: grid;
  grid-template: 300px 150px / 50% 200px 25%;
  // grid-gap: row / column;
  grid-gap: 20px / 20px;

```

## Fracciones y Funciones
Asi como tenemos pixels, porcentajes, etc.; *CSS Grid* nos proporciona una nueva medida, las fracciones, designadas por `fr`.

```stylus
.container
  display: grid;
  // grid-template: 300px 150px / 50% 200px 25%;
  grid-template: 300px 150px / 1fr 1fr 1fr;
  grid-gap: 20px / 20px;

```
O también, en automático.

```stylus
.container
  display: grid;
  // grid-template: 300px 150px / 1fr 1fr 1fr
  grid-template: 300px 150px / auto auto auto;
  grid-gap: 20px / 20px;

```

*Grid Layout* nos proporciona un par de funciones para facilitarnos la tarea de crear layout. Cuando tenemos que repetir columnas o filas podemos usar la función `repeat(num, size)`. También podemos usar la función `minmax(min, max)` para determinar los valores máximo y mínimo de los elementos. Grid tratara de mantenerse en esos extremos siendo responsivo.

```stylus
.container
  display: grid;
  grid-template: 300px 150px / repeat(4, minmax(200px, 1fr));
  grid-gap: 20px / 20px;

```

## Áreas de contenido
Definimos el layout de un área en el contenedor padre mediante `grid-template-areas` y luego se relaciona con el elemento que ocupará una área.

```Stylus
.container
  display: grid;
  grid-template: 100px 1fr 150px / 200px 1fr;
  grid-template-areas: "header header"
                       "left content"
                       "footer footer";
  grid-gap: 10px / 10px;


.header
  grid-area: header;

.left
  grid-area: left;

.content
  grid-area: content;

.footer
  grid-area: footer;

```

## Controlando el tamaño de columnas y filas
CSS Grid permite el control total sobre el tamaño de columnas y filas para poder encajar cualquier layout de diseño pensado.
Ademas de las áreas podemos controlar individualmente cada elemento del grid, especificando donde comienza y donde termina o cual es su ancho o alto.

Vamos a jugar con un layout de 5 columnas.

```pug
section
  .container
    .item Contenido #1
    .item Contenido #2
    .item Contenido #3
    .item Contenido #4
    .item Contenido #5

```
Y el CSS para controlar la posición y tamaño de los elementos será:

```css
.container {
  display: grid;
  grid-gap: 2px;
  grid-template: repeat(2, 200px) / repeat(7, 1fr);
  height: 100vh;
}
```
Cada uno de los `item`'s los controlamos mediante **`grid-column-start`**, **`grid-column-end`** o en forma abreviada **`grid-column start / end`**. El final también se puede expresar como un **`span`**. En formato _*stylus*_ sería algo así:

```Stylus

.item
  background lightblue
  border 1px solid red
  padding 10px

  &:nth-of-type(1)
    background lightcoral
    grid-column 1 /span 3
    grid-row 1 / span 2

  &:nth-of-type(2)
    // background lightcoral
    grid-column 4 / span 2
    grid-row 1 / span 2

  &:nth-of-type(3)
    background lightcoral
    grid-column 6 / span 2

```

Lo anterior lo podemos realizar poniendo nombres a las líneas de que separan las columnas y/o filas:
  > **Nota**: _Stylus_ no reconoce la asignación de nombres en los grid's. Tampoco reconoce las funciones `repeat()`,`minmax()`. Por lo que usando _Stylus_, es necesario marcarlo como css literal mediante `@css {}`.

  ```css
  .container {
    display: grid;
    grid-gap: 2px;
    grid-template-columns: [inicio] 1fr [lc2] 1fr [lc3] 1fr [fin-dest] 1fr [lc5] 1fr [lc6] 1fr [lc7] 1fr [fin-c];
    grid-template-rows: [lf1] 200px [lf2] 200px [fin-f];
    height: 100vh;
  }

  .item {
    background: lightblue;
    border: 1px solid red;
    padding: 10px;
  }

  .item:nth-of-type(1) {
    background: lightcoral;
    grid-column: inicio / fin-dest;
    grid-row: lf1 / span 2;}

  .item:nth-of-type(2) {
    grid-column: fin-dest / lc6;
    grid-row: lf1 / fin-f;}

  .item:nth-of-type(3) {
    background: lightcoral;
    grid-column: lc6 / fin-c;}
  ```
## Manejando el grid implícito
Como ya se ha podido observar **_CSS Grid_** va colocando todos los elementos según se especifíca y todo aquello que sobra o desborda lo pasa por defecto a una nueva fila. A esto se le conoce como grid ímplicito y es la forma en que va ordenando todo aquello que sobra o no tiene definición explícita.

El grid implícito lo podemos controlar mediante un par de propiedades. Con **`grid-auto-flow`** definimos como se van a tratar los elementos "_sobrantes"_. Por defecto se tratan pasandolos a una nueva fila pero podríamos apilarlos en columnas estableciendo `grid-auto-flow: column;`.

Vamos a añadir mas columnas para ver el efecto:
```pug
section
  .container
    .item Contenido #1
    .item Contenido #2
    .item Contenido #3
    .item Contenido #4
    .item Contenido #5
    .item Contenido #6
    .item Contenido #7
    .item Contenido #8
    .item Contenido #9
    .item Contenido #10
    .item Contenido #11
    .item Contenido #12
```

Ahora vamos a cambiar el tratamiento por defecto para que coloque los elementos por columnas:

```css
.container {
  display: grid;
  grid-gap: 2px;
  grid-template-columns: [inicio] 1fr [lc2] 1fr [lc3] 1fr [fin-dest] 1fr [lc5] 1fr [lc6] 1fr [lc7] 1fr [fin-c];
  grid-template-rows: [lf1] 200px [lf2] 200px [fin-f];
  height: 100vh;

  grid-auto-flow: column;
}
```

Las columnas a partir de la sexta no tienen una definición explícita, así que las va colocando a la derecha y en dos filas, asignándole un espacio mínimo para albergar el contenido.
Si cambiamos `grid-auto-flow: column;` por `grid-auto-flow: row;`, veremos como vuelve al comportamiento por defecto, colocando los elementos por filas y pasando a una nueva fila los elementos a partir del sexto.

También podemos definir un espacio para las nuevas columnas o filas donde se colocaran los elementos que no tienen una definición explicita. Probemos con `grid-auto-colums: 100px 50px;` para definir dos anchos de columnas si tenemos `grid-auto-flow: column;` o `grid-auto-rows: 100px 50px;` si estamos manejando el grid implícito por filas.
