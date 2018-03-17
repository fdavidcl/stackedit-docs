# A guide to Ruby in ten lines of code

This is a slightly unorthodox guide to the Ruby language. Through this guide we will examine just ten one-line snippets of code, and discover a lot of features from each of them.

This is the post version of a talk I gave for [LibreIM](https//libreim.github.io).

## Syntax

```ruby
puts "Hello #{gets.strip}!"
```

This snippet accepts a string from standard input, then cleans the leading and trailing spacing characters and embeds it in a greeting which is shown on standard output. Notice that:
- `puts` and `gets` are the standard functions for displaying messages and reading input, respectively.
- You can omit parenthesis around parameters for better readability.
- `#{...}` in strings interprets small pieces of code and transforms the output into a string.

```ruby
puts "boooored".upcase unless Time.now.saturday?
```

This example is so readable you probably don't need a description. Notice that:
- Literals are Ruby objects and can receive methods.
- `if`, `unless`, `while` and `until` can be used as one-line structures.
- There is a convention to end methods which return booleans with `?` and methods which have non-explicit side effects with `!`.

```ruby
real, imag = (1 + 3i).rectangular
```

This snippet assigns the real part of 1 + 3i to `real` and the imaginary part to `imag`. Notice that:
- Ruby has built-in support for complex numbers, big numbers (as in symbolic, bigger than `long long` numbers) and symbolic rationals.
- Assignment allows splatting arrays. The splat operator `*` allows for related operations. For example,
    ```ruby
    x, *xs = [1, 2, 3, 4]
    ```
    performs some Haskell-ish pattern matching: `x` receives the first element and `xs` holds the rest of the array. There is also a similar double-splat `**` for hashes.

## Object oriented programming

```ruby
class DeadPlayerError < StandardError; end
```

Defines a class useful to return errors (exceptions). Notice that:
-   Classes are defined with `class` (which is syntax sugar for `Class.new { ... }`).
-   There is simple inheritance with `<`
-   You can throw an error with `raise DeadPlayerError`. Ruby distinguishes errors, from which a program can recover, and other kinds of exceptions, which leave the program in an invalid state and therefore it must stop.

``` {.ruby}
Point = Struct.new :x, :y, :z
```

Creates a class with getters and setters for the given attributes. Notice that:
-   Member attributes of a class are **always** private (or protected, as they are inherited). Thus, the *dot* only sends methods and doesn't directly access attributes.
-   Methods can be defined in a `do ... end` block. You should consider defining a class if you want to add them.
-   New objects are created with `Point.new`.


## Iteration and data structures

``` {.ruby}
puts %w[such elegant wow].each_with_index.map { |w,i| "#{i}. #{w}" }
```

\normalsize
. . .

Recorre el array, obtiene un array de strings *índice. elemento* y lo
imprime.

-   `%w[]` separa por espacios una tira de palabras
-   `each_with_index` recorre dando cada elemento e índice
-   `map` aplica una función sobre cada elemento y devuelve el resultado
-   `{...}` o `do ... end` denotan *blocks*

``` {.ruby}
dot = ->(v1, v2) { v1.zip(v2).reduce(0) { |p, (n, m)| p + n * m } }
```

\normalsize
. . .

Uso: `dot.([1, 2, 3], [-1, 0, 2])`

-   Funciones lambda con `->`
-   `zip` empareja los elementos de dos o más arrays
-   `reduce` acumula resultados de una función binaria

``` {.ruby}
fib = Hash.new { |h, i| h[i] = h[i - 2] + h[i - 1] }.update(0 => 0, 1 => 1)
```

\normalsize
. . .

Crea un Hash que contiene en cada índice el término correspondiente de
la secuencia de Fibonacci.

-   `Hash` tiene una inicialización por defecto (normalmente `nil`)
-   `Hash#update` asigna varios valores a la vez
-   Equivalente a una función recursiva memoizada

``` {.ruby}
solution.neighborhood.detect { |attempt, fitness| fitness > @current_fitness }
```

\normalsize
. . .

Encuentra la primera solución del vecindario que mejora la función
rendimiento. Uso real: <https://git.io/vPxQ6>

-   `detect` recibe un predicado y devuelve el primer elemento del array
    que lo cumple
-   Máxima pendiente? `max_by`
-   Muchos más métodos de iteración:
    [`Enumerable`](https://ruby-doc.org/core-2.5.0/Enumerable.html)


## I/O

``` {.ruby}
open(DATA.read, "w").write IO.read($0).gsub(/^#' /, "")
```

\normalsize
. . .

Lee el propio programa, descomenta los comentarios marcados con `#'` y
pasa el resultado como entrada al programa abierto por `Kernel#open`.

-   `$0` es el nombre del programa en ejecución
-   `gsub` hace sustitución global

. . .

*La trampa*  La sección de datos de este mismo archivo contiene el
comando de terminal a ejecutar:

``` {.sh}
__END__
|pandoc -t beamer -o slides.pdf --pdf-engine=xelatex
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbNjA3MDM0ODQ2XX0=
-->