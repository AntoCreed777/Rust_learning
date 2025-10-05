# Rust con Cargo

## Crear un nuevo proyecto

```bash
cargo new hello_cargo
```

> [!Important]
> Crea el proyecto como *nuevo repositorio* (genera un ``.git\`` y un ``.gitignore``).
> Si ya estás dentro de otro repositorio, no generará estos archivos.

## Compilar el proyecto
Para compilación en *modo debug* (por defecto):
```bash
cargo build
```

El ejecutable se genera en:

```bash
target/debug/hello_cargo
```

## Ejecutar el proyecto

Para ejecutar el código compilado:

```bash
./target/debug/hello_cargo
```

Para compilar y ejecutar el código de inmediato:

```bash
cargo run
```

## Verificar compilación sin generar binario

```bash
cargo check
```
Este comando solo valida que el código compile correctamente, *sin crear un ejecutable*.

## Compilar con optimizaciones (modo release)

```bash
cargo build --release
```

> [!Note]
> El ejecutable optimizado se genera en:
> ```bash
> target/release/hello_cargo
> ```

## Estructura básica del proyecto

- ``src/`` → Carpeta donde se encuentra el código fuente (main.rs, etc.).

- ``Cargo.toml`` → Archivo donde se definen las dependencias y la configuración del proyecto.

- ``Cargo.lock`` → Archivo que asegura que todos usen exactamente las mismas versiones de dependencias.

- ``target/`` → Carpeta donde Cargo coloca los binarios compilados (debug/ o release/).
