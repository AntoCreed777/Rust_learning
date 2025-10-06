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

## Sombreado de variables (Shadowing)

Rust permite volver a declarar una variable con el mismo nombre, incluso cambiando su tipo o volviéndola inmutable.
Este comportamiento se conoce como **shadowing**, y es muy útil para transformar valores sin tener que crear nuevas variables.

Por ejemplo:

```rust
let x = "42";     // x es una cadena
let x: i32 = x.parse().expect("No es un número"); // ahora x es un entero
println!("El valor de x es {x}");
```

Cada vez que se usa ``let``, se **crea una nueva variable** que **somete** (sombrea) la anterior.
La variable anterior deja de estar disponible desde ese punto en adelante.

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

## Dependencias y `Cargo.toml`

Al agregar una nueva dependencia en el archivo ``Cargo.toml``, se indica el nombre y la versión que se utilizará.
Por ejemplo, en nuestro caso se usa la versión `0.8.5`, que es una abreviación de `^0.8.5`.
Esto significa que se puede usar **cualquier versión dentro del rango** `[0.8, 0.9[` (es decir, desde la 0.8.0 hasta justo antes de la 0.9.0).

Después de agregar las dependencias y ejecutar ``cargo build``, el archivo ``Cargo.lock`` se modifica automáticamente.
Este archivo contiene información que permite a Cargo **reproducir las compilaciones** de manera consistente.

* Si se desea **actualizar** las dependencias, se puede eliminar ``Cargo.lock`` para que Cargo lo regenere.
* También se puede usar el comando ``cargo update``, que **ignora el archivo ``.lock``**, descarga las versiones más recientes permitidas por el rango y actualiza ``Cargo.lock``.
* Si se desea actualizar a una **versión mayor** (fuera del rango actual), se debe modificar manualmente el archivo ``Cargo.toml``, por ejemplo cambiando `0.8.5` a `0.9.0`.

## Generación de números aleatorios

El método ``rand::thread_rng()`` genera un **generador de números aleatorios**.

El método ``.gen_range()`` toma un **rango** como argumento y genera un número aleatorio dentro de él.
El tipo de rango que se usa tiene la forma ``start..=end``, e incluye **ambos extremos**.

> Es un poco inusual al principio, pero esta notación de rangos (``..`` y ``..=``) se usa mucho en Rust.
> Más adelante se profundiza en su uso.

## Comparaciones y ``Ordering``

En el encabezado del programa se suele importar lo siguiente:

```rust
use std::cmp::Ordering;
```

El tipo ``Ordering`` es una **enumeración** con las variantes:

* ``Less``
* ``Greater``
* ``Equal``

Estas representan los tres posibles resultados al comparar dos valores.

El método ``.cmp()`` compara dos valores y se puede usar con cualquier tipo que implemente el rasgo ``Ord`` (es decir, que permita comparaciones de orden).

### Uso de ``match``

La expresión ``match`` se compone de **brazos**.
Cada brazo contiene un **patrón** a comparar y el **código a ejecutar** si el valor coincide con ese patrón.

Para incluir múltiples instrucciones dentro de un brazo, se agrupan con ``{}``:

```rust
Ordering::Equal => {
    // Código
}
```

> [!NOTE]
> No es necesario que ``match`` termine con ``;`` para indicar el fin de la expresión.

## Conversión de tipos

El método ``.trim()`` aplicado a un ``String`` elimina los espacios en blanco al inicio y al final, lo cual es necesario antes de convertirla a un número (``u32``), ya que solo puede contener dígitos.

El método ``.parse()`` convierte una cadena (``String``) a otro tipo de dato.
Por ejemplo:

```rust
let guess: u32 = input.trim().parse().expect("Por favor ingresa un número válido");
```

Aquí, ``let guess: u32`` indica explícitamente el tipo de la variable.
El método ``.parse()`` también devuelve un ``Result``, por lo que se usa ``.expect()`` para manejar errores de conversión.


## Bucles infinitos con ``loop``

La estructura:

```rust
loop {
    // Código
}
```

es equivalente a ``while (true)`` en Python.
El bucle se ejecutará de forma indefinida hasta que se use ``break`` para salir de él.

También se puede usar ``continue`` para saltar directamente a la siguiente iteración del bucle.

> [!NOTE]
> ``loop`` tampoco necesita terminar con ``;``.


## Manejo personalizado de ``Result`` con ``match``

En lugar de usar ``.expect()``, se puede manejar el tipo ``Result`` de forma personalizada utilizando ``match``.
Esto evita que el programa se interrumpa si ocurre un error.

Ejemplo:

```rust
let variable = match variable.metodo() {
    Ok(valor) => {
        // Acción si la operación fue exitosa
        valor
    }
    Err(error) => {
        // Acción si ocurrió un error
        println!("Ocurrió un error: {error}");
        0 // valor por defecto o de recuperación
    }
};
```

> [!IMPORTANT]
> En este caso, como el ``match`` forma parte de una **asignación**, sí debe terminar con ``;``.
