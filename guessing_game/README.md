# Explicación de Variables y Entrada de Usuario en Rust

## Declaración de variables

En Rust, las variables se declaran con ``let``:

```rust
let x = 5;
```

Por **predeterminado**, las variables son **inmutables**, es decir, **no se pueden modificar** después de asignarlas.

Para que una variable pueda ser modificada, se debe usar ``mut``:

```rust
let mut x = 5;
x = 10; // ahora sí se puede modificar
```

## Entrada de usuario

La función ``io::stdin().read_line()`` permite recibir lo que el usuario escribe y almacenarlo en una variable.

Al usarla, la variable debe ser mutable, ya que ``read_line`` no sobreescribe el contenido, sino que lo agrega al final.

```rust
use std::io;

let mut input = String::new();
io::stdin().read_line(&mut input).expect("Error al leer la línea");
```
### Paso por referencia

El símbolo ``&`` indica que algo se pasa **por referencia**.
Por defecto, las referencias son **inmutables**, por eso al usarlo con ``io::stdin()`` se necesita ``mut``:

```rust
&mut input
```

## El tipo ``Result``

El método ``.read_line()`` retorna un valor de tipo ``Result``, que es una **enumeración (enum)** con dos variantes:

- ``Ok(valor)``: la operación se realizó correctamente y contiene el valor generado.
- ``Err(error)``: la operación falló y contiene información sobre el error.

Una instancia de ``Result`` tiene un método ``.expect()``, que se comporta así:

- Si el Result es ``Err``, provoca un fallo del programa y muestra el mensaje proporcionado.
- Si el Result es ``Ok``, devuelve solo el valor contenido en ``Ok`` para poder usarlo.

## Imprimir valores

Para imprimir el valor de una variable, se usa ``println!``:

```rust
println!("x = {x}");
```

Esto es equivalente a usar ``print(f"{x}")`` en Python.
