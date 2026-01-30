# Módulo Costos

## Módulo Costos – Documentación Técnica (Completa y Actualizada)

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
