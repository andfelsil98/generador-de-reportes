# Informe de carteras Teclogi

Aplicación estática para GitHub Pages. Lee archivos JSON localmente, procesa los datos con DuckDB-WASM dentro del navegador y descarga un libro Excel con estas hojas:

- Resumen
- Datos
- PorCliente
- PorEstado
- PorAnio
- Antiguedad
- PorCausa
- TopDeudores
- PorGestor

## Datos esperados

Cada archivo debe ser un arreglo JSON de objetos de cartera. Los campos que usa el informe son `consecutive`, `licensePlate`, `companyName`, `creationDate`, `amountInitial`, `amountReceived`, `reason`, `state.description`, `whoCreates.name`, `whoReports.name`, `ownerDocument` y `modifications`.

`creationDate` se interpreta tomando la fecha ISO inicial (`AAAA-MM-DD`) para no alterar el día por la zona horaria de origen.

Los estados se interpretan así: `paid` es Pagada, `deleted` es Anulada y cualquier otro valor es Pendiente.

## Publicación en GitHub Pages

1. Crea un repositorio vacío en GitHub y sube este proyecto a la rama `main`.
2. En GitHub, abre **Settings > Pages**.
3. En **Build and deployment**, selecciona **GitHub Actions** como fuente.
4. Cada `push` a `main` ejecutará `.github/workflows/deploy-pages.yml` y publicará la página.

La página descarga DuckDB-WASM 1.29.0 y SheetJS desde jsDelivr cuando se abre por primera vez. Los JSON seleccionados no se envían a GitHub ni a ningún backend.

GitHub Pages no permite añadir los encabezados COOP y COEP necesarios para el modo multihilo de DuckDB-WASM. Por eso la aplicación usa automáticamente el modo `eh` o `mvp`, que sigue ejecutando SQL en WebAssembly y conserva los datos en el navegador. Para habilitar hilos en el futuro, habría que alojar la misma página detrás de un servicio que permita esos encabezados.
