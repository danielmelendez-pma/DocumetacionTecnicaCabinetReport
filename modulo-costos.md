---
description: Especificación normativa histórica del módulo Costos; alcance, cliente, proyecto, archivos, notas y ambientes.
---

# Módulo Costos

**Cómo consultar esta documentación:** [Guía de interfaz y capturas](README.md) · **Reglas normativas** (esta página).

{% hint style="warning" %}
**Alcance de la verificación:** el texto histórico menciona enero de 2026. La fecha no demuestra por sí sola que las reglas estén implementadas en la versión actual. Los apartados 6 y 8 al 13 contienen el marcador «se mantiene íntegro», en lugar de su desarrollo: requieren cotejo con el código y pruebas funcionales antes de considerarse completos.
{% endhint %}

## Especificación normativa del Módulo Costos

> **Aplicación:** Cabinet Report\
> **Módulo:** Costos\
> **Formulario principal:** `FrmCostos`\
> **Estado:** Documentación normativa alineada al código actualizado (enero 2026)

***

### 0. Principio normativo

Si existe conflicto entre el código y esta documentación,\
**LA DOCUMENTACIÓN MANDA.**

Esta guía describe el comportamiento esperado del módulo Costos y debe cumplirse estrictamente.

***

### 1. Alcance del módulo

El módulo Costos permite:

* Importar costos desde archivos (Excel / XML / PDF procesado).
* Gestionar Materia Prima (tableros, herrajes, accesorios, cantos, etc.).
* Calcular costos directos y totales.
* Gestionar Notas Globales (condiciones comerciales, no incluye).
* Gestionar Notas del Proyecto (referencia del proyecto).
* Gestionar Márgenes (producción y operativo).
* Generar archivos de salida (Excel / Word) para cotización.

❌ Este módulo NO genera planos técnicos\
❌ Este módulo NO modifica catálogos

***

### 2. Conceptos fundamentales

#### 2.1 Cliente activo

El cliente activo es el que se muestra en:

```
FrmPrincipalCR.LblCliente
```

Este valor:

* Se usa para filtrar catálogos privados.
* Se usa para identificar al cliente en cotizaciones.
* Es global por instalación.
* Nunca se reemplaza ni se altera por archivos cargados.
* Nunca se obtiene desde XML, Excel o PDF.

***

#### 2.2 Proyecto

Todo valor leído desde archivos (XML, Excel, PDF) identificado como:

* Cliente
* Proyecto
* Referencia

👉 **Debe interpretarse como PROYECTO, no como cliente.**

El proyecto se muestra y edita en:

```
TxtProyecto
```

***

### 3. Estructura de almacenamiento

#### 3.1 Carpeta ArchivosCliente (obligatoria)

Todos los archivos globales del cliente se guardan en:

```
[Carpeta de la aplicación]\ArchivosCliente\
```

* Se crea automáticamente si no existe.
* Es independiente de los proyectos.
* Nunca contiene información específica de un proyecto.

***

#### 3.2 Archivos contenidos (Cliente)

| Archivo              | Descripción                     |
| -------------------- | ------------------------------- |
| NotasGlobales.xml    | Condiciones comerciales y notas |
| MargenProduccion.xml | Margen de producción            |
| MargenOperativo.xml  | Margen operativo                |

***

### 4. Notas Globales (CLIENTE)

#### 4.1 Definición

Las Notas Globales del Cliente contienen exclusivamente:

* Forma de pago
* Porcentajes (despacho, instalación, entrega)
* Opciones de suministro e instalación
* Texto libre de notas
* Lista de No Incluye

🚫 **NO contienen referencia de proyecto**

***

#### 4.2 Ubicación

Siempre se guardan en:

```
ArchivosCliente\NotasGlobales.xml
```

***

#### 4.3 Estructura XML (Cliente)

```xml
<NotasGlobales>
  <FormaDePago tipo="contado" diasCredito="" />
  <Porcentajes tipo="40" despacho="50" instalacion="40" entrega="10" />
  <Suministro>true</Suministro>
  <Instalacion>true</Instalacion>
  <NotasGlobalesTexto />
  <NoIncluye>
    <Item check="true">Instalación de Plomería</Item>
  </NoIncluye>
</NotasGlobales>
```

***

#### 4.4 Carga al abrir FrmCostos

* Las Notas Globales del Cliente se cargan automáticamente desde ArchivosCliente.
* No se guarda nada automáticamente.
* Nunca se carga referencia de proyecto en este paso.

***

### 5. Notas del Proyecto

#### 5.1 Definición

Las Notas del Proyecto contienen información exclusiva del proyecto, incluyendo:

* Referencia del proyecto

***

#### 5.2 Campo de referencia

El campo:

```
TxtReferenciaProyecto
```

* Es exclusivo del proyecto.
* Nunca se carga al abrir FrmCostos.
* Solo se llena cuando:
  * Se carga un XML de proyecto.
  * El usuario la ingresa manualmente.
* Se limpia al usar **Limpiar formulario**.

***

#### 5.3 Ubicación

La referencia del proyecto se guarda únicamente en el XML del proyecto:

```xml
<NotasProyecto>
  <ReferenciaProyecto>FORUM 22A</ReferenciaProyecto>
</NotasProyecto>
```

🚫 Nunca se guarda en ArchivosCliente.

***

#### 5.4 Carga de un proyecto XML

* Las Notas del Proyecto reemplazan las visibles.
* No se mezclan.
* No sobrescriben ArchivosCliente.
* Cada proyecto mantiene su referencia intacta.

***

### 6. Márgenes

(se mantiene íntegro)

***

### 7. Ambientes (NUEVO – NORMATIVO)

#### 7.1 Definición

Un **Ambiente** representa una unidad lógica independiente dentro del proyecto\
(ej.: COCINA, CLOSET, COCINETA).

Cada ambiente:

* Tiene su propio nombre.
* Puede tener su propia referencia.
* Contiene su propia lista de materiales.

***

#### 7.2 Reglas de nombre

* El nombre del ambiente:
  * Puede escribirse libremente por el usuario.
  * **Siempre se guarda y muestra en MAYÚSCULAS**.
* El sistema normaliza el texto automáticamente.
* El orden alfabético de los ambientes debe respetarse.

***

#### 7.3 Independencia de datos

📌 **REGLA CRÍTICA**

Cada ambiente mantiene:

* Su **lista de Materia Prima totalmente independiente**.
* Sus cantidades, costos y totales.

🚫 **Nunca se mezclan materiales entre ambientes**.

***

#### 7.4 Selección de ambiente

* El DGV de Materia Prima funciona como una **vista del ambiente seleccionado**.
* Al hacer clic en un ambiente:
  * Se cargan **solo** los materiales de ese ambiente.
* Al cambiar de ambiente:
  * Los cambios realizados permanecen guardados en su ambiente original.

***

#### 7.5 Carga de múltiples XML

* El sistema permite cargar:
  * Uno o varios XML simultáneamente.
* Cada XML:
  * Crea o actualiza un ambiente independiente.
  * Mantiene íntegros sus materiales.
* Al finalizar la carga:
  * Se selecciona automáticamente un ambiente válido.
  * El DGV de Materia Prima refleja ese ambiente.

***

### 8. DataGridView – Materia Prima

(se mantiene íntegro, pero **siempre ligado al ambiente actual**)

***

### 9. Resolución de ítems

(se mantiene íntegro)

***

### 10. Pantalla de espera

(se mantiene íntegro)

***

### 11. Ventana de selección de ítems

(se mantiene íntegro)

***

### 12. Regla de limpieza al agregar ítem

(se mantiene íntegro)

***

### 13. Filtro en FrmCostos

(se mantiene íntegro)

***

### 14. Regla final

Esta documentación define el comportamiento esperado del módulo Costos.\
**Cualquier cambio funcional debe documentarse aquí antes de tocar el código.**


***

## Guía de consulta y trazabilidad

| Tema | Dónde consultarlo | Precaución |
| --- | --- | --- |
| Controles y capturas de pantalla | [Guía de interfaz](README.md) | Descripción histórica; debe comprobarse con la versión instalada. |
| Cliente activo y referencia de proyecto | Apartados 2 y 5 | Son conceptos distintos en esta especificación. |
| Archivos de notas y márgenes | Apartados 3 y 4 | Incluye nombres, rutas y estructura XML. |
| Independencia de ambientes | Apartado 7 | Conserva las reglas normativas sin cambios. |
| Márgenes y operaciones mencionadas sin detalle | Apartados 6 y 8 al 13 | Los marcadores «se mantiene íntegro» no constituyen procedimientos completos. |

### Contrastes pendientes

- Verificar contra el código fuente y en Windows las reglas atribuidas a enero de 2026; no se han ejecutado estas pruebas en la presente edición.
- Completar con evidencia los apartados 6 y 8 al 13, sin inventar procedimientos ni modificarlos antes de la validación.
- Contrastar los nombres de controles y campos usados históricamente con el formulario real antes de corregir identificadores técnicos.

Esta guía de consulta **no reemplaza ni modifica** las reglas normativas anteriores. Para localizar los controles, vuelve a la [guía visual](README.md).

---

## Ensayo reproducible: aislamiento de ambientes y referencias en Costos

Este procedimiento convierte en **casos verificables**, sin modificarlas, las reglas históricas de los apartados **2 (cliente y proyecto), 3–5 (archivos y notas) y 7 (ambientes)**. Busca detectar mezcla de materia prima entre ambientes, sustitución accidental del cliente activo por datos importados y alteraciones no solicitadas de las notas globales.

{% hint style="warning" %}
**Prueba funcional pendiente:** lo siguiente es un protocolo para una ejecución supervisada en Windows, **no** un resultado observado ni una garantía de implementación. No se ha ejecutado en esta intervención. Las expectativas proceden de la especificación normativa histórica; cualquier discrepancia requiere evidencia del código y del formulario real.
{% endhint %}

### Preparación controlada

1. Usa una **instalación o entorno de prueba autorizado**, con un cliente activo de prueba. No utilices presupuestos vigentes, datos privados de terceros ni una carpeta compartida de producción.
2. Respalda íntegramente los archivos existentes de `ArchivosCliente` —en particular `NotasGlobales.xml`, `MargenProduccion.xml` y `MargenOperativo.xml`, si existen— y conserva copias de los XML de Costos originales. No edites manualmente sus estructuras para forzar el ensayo.
3. Prepara **dos XML de Costos válidos y compatibles con la versión en prueba**, del mismo proyecto de ensayo y asociados a dos ambientes identificables (por ejemplo, `COCINA-ENSAYO` y `CLOSET-ENSAYO`). Cada ambiente debe tener un listado de materia prima distinguible; de ser posible, incluye una descripción común con **cantidades diferentes**.
4. Registra antes de comenzar: versión de Cabinet Report, usuario de prueba, cliente activo visible, identificación del proyecto, nombres de ambos archivos y copia o captura del estado inicial de `NotasGlobales.xml`. **No se necesitan precios reales** para esta verificación.

### Secuencia de prueba

1. Abre **Costos** y registra por separado el **cliente activo de la instalación** y el **proyecto** que aparece al cargar el primer XML. Comprueba que la referencia leída del archivo se interprete como proyecto, sin reemplazar la identidad del cliente activo descrita en el apartado 2.
2. Carga el XML del ambiente A; selecciona ese ambiente y registra sus descripciones y cantidades visibles en **Materia Prima**.
3. Carga el XML del ambiente B mediante la opción documentada de **Cargar archivos de Costos XML**. Selecciona B y registra su listado. Si tu versión admite selección múltiple, también puedes repetir la prueba cargando ambos XML juntos en una sesión limpia.
4. Alterna **A → B → A** y compara los registros con los de los pasos anteriores. Los materiales propios de B no deben aparecer en A ni viceversa; un artículo con igual descripción no justifica mezclar sus cantidades.
5. **Solo en el entorno de prueba y sobre datos desechables**, modifica una cantidad identificable en A, cambia a B y vuelve a A. Según el apartado 7, B debe permanecer independiente y A debe conservar la modificación al alternar entre ambientes; esto **no** demuestra persistencia tras cerrar la aplicación.
6. Comprueba que la carga de los XML de proyecto no haya sustituido las **Notas Globales** guardadas para el cliente ni cambiado los archivos de `ArchivosCliente` sin una acción explícita. Si no había notas iniciales, registra esa condición y no declares el control conforme por ausencia de datos.
7. Finaliza **sin guardar encima de los XML originales**, conserva las capturas y restaura el entorno de prueba desde el respaldo cuando corresponda.

### Registro de resultados

| Control | Evidencia mínima | Criterio para marcar CONFORME |
| --- | --- | --- |
| **C-01 · Cliente y proyecto** | Cliente activo y campo de proyecto antes/después de cargar | La referencia importada no sustituye al cliente activo. |
| **C-02 · Separación A/B** | Captura de materia prima de A y de B después de importar ambos | Cada ambiente muestra sus propias partidas y cantidades, sin mezcla. |
| **C-03 · Alternancia y edición** | Capturas A → B → A con modificación controlada | El cambio en A no altera B y se conserva en A durante la alternancia. |
| **C-04 · Custodia global** | Copia/registro previo y posterior de `NotasGlobales.xml` | La importación no sustituye ni sobrescribe las notas globales sin acción solicitada. |

Para **cada control**, consigna **CONFORME**, **NO CONFORME** o **NO EJECUTADO**. Una condición sin datos, una opción ausente o un paso omitido **no** equivale a conformidad. Ante un resultado no conforme, registra el paso exacto, entradas usadas, resultado esperado, resultado observado, capturas y versión de la aplicación; **no corrijas la discrepancia reescribiendo las reglas históricas**.

{% hint style="info" %}
**Límites de la evidencia:** este ensayo no certifica cálculos económicos, precios, fórmulas, acceso entre clientes, integridad de XML externos ni persistencia tras reiniciar. La validación documental de GitHub tampoco acredita la ejecución en Windows.
{% endhint %}
