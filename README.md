# Guía de Sass

**Autora de la guía** : Claudia Torres Cruz.

**Código por parte de**: AlexCG Design.

## Índice
- [Guía de Sass](#guía-de-sass)
  - [Índice](#índice)
  - [Instalación de Sass](#instalación-de-sass)
  - [Variables](#variables)
  - [Partials](#partials)
  - [Anidamiento](#anidamiento)
  - [BEM (Block Element Modifier)](#bem-block-element-modifier)
  - [Interpolación de Variables](#interpolación-de-variables)
  - [Ciclos y Condicionales](#ciclos-y-condicionales)
  - [Mixins](#mixins)
  - [Funciones](#funciones)

## Instalación de Sass

No se puede referenciar directamente un archivo Sass en HTML; es necesario compilar Sass a CSS para poder referenciarlo. Existen dos métodos de instalación:

1. **Instalación local en el proyecto** (desarrollador):
   ```bash
   npm install sass --save-dev
   ```
2. Instalación global en el sistema:
   ```bash
   npm i -g sass
   ```

Una vez instalado, para compilar los archivos `.sass` o `.scss` a CSS, se especifica la carpeta de entrada y salida en la terminal:

```bash
sass --watch sass:css
```

## Variables
Las variables en Sass permiten tener un código legible y escalable, ya que pueden referenciarse y modificarse fácilmente. Las variables se definen en la parte superior del archivo Sass:

```scss
$nombre: valor;
$color-primario: blue;
```

Para utilizarla:

```scss
p{
    color: $color-primario;
}
```

## Partials
Los partials son archivos de estilo que no se compilan por sí mismos, sino que se importan en un archivo principal. Para crear un partial, el archivo debe comenzar con un guión bajo `(_)`:

```scss
// Partial: _navbar.scss
```
En el archivo principal, se importa de la siguiente forma:

```scss
@use 'navbar';
```
Los partials pueden llamarse entre sí, lo cual es común para archivos de variables:

```scss
@use 'variables';

.nav{
    background-color: variables.$color;
}
```

## Anidamiento
Sass permite crear selectores descendientes dentro del mismo selector, pero ten en cuenta que esto puede generar un CSS difícil de mantener si no se usa con precaución.

En vez de tener esto:
```css
.nav{
    
}

.nav ul{

}

.nav ul li{
    
}
```

Con sass quedaría de la siguiente forma:

```scss
.nav{
    background-color: variables.$color;

    ul{
        color:tomato;

        li{
            color: white;
        }
    }
}
```
![WARNING]
> Advertencia: La práctica de anidamiento excesivo es poco recomendada. Es mejor utilizar el método de BEM.

## BEM (Block Element Modifier)
BEM es una convención de nombres que facilita el anidamiento usando `&`, que referencia al elemento padre. Las clases se estructuran con dobles guiones bajos`(__)` y dobles guiones `(--)` para indicar elementos y modificadores.
```scss
.nav{
    background-color: variables.$color;

    &__container{
        width: 90%;
        margin: auto;
        height: 70px;
        display: flex;

        &--active{
            width: 100%;
        }
    }
}
```
También se pueden anidar `:hover` o `media queries` dentro del selector.

## Interpolación de Variables
La interpolación permite utilizar variables como nombres de selectores o propiedades:

```scss
$display: 'display';
#{$display}: flex;
```

## Ciclos y Condicionales
Los ciclos permiten repetir código. Por ejemplo:

```scss
@for $iterador from 1 through 5{
    .selector-#{iterador}{
        color: black;
    }
}
```

Esto creará selectores `.selector-1` a `.selector-5` en el CSS.

Los condicionales se utilizan de forma similar a otros lenguajes de programación y pueden incluirse en bucles:

```scss
@if $iterador == 5{
    animation-direction: alternate-reverse;
}
```

## Mixins
Los mixins permiten definir bloques de código que pueden reutilizarse en varios lugares. Se pueden incluir en partials y recibir parámetros:

```scss
@mixin crear-flexbox{
    display: flex;
    align-items: center;
    justify-content: center;
}
```

Para utilizarlos:

```scss
@use 'mixinsfunc';
@include mixinsfunc.crear-flexbox;
```

También puedes definir mixins para propiedades complejas como background:

```scss
@mixin crear-flexbox($justify){
    display: flex;
    align-items: center;
    justify-content: $justify;
}
```

## Funciones
Las funciones en Sass son similares a los mixins, pero también pueden incluir cálculos y devolver un valor:

```scss
@function pixeles-rem($pixeles){
    $rem: math.div($pixeles, 16) * 1rem;

    @return $rem;
}
```

Para usar la función:

```scss
font-size: mixinsfunc.pixeles-rem(30);
```