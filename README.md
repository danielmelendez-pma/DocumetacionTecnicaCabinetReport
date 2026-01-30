# FrmCostos.vb

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>

El módulo de <mark style="color:$primary;">**Costos**</mark> es para gestionar los cálculos económicos de cada ambiente y generar los cuadros de costos y cotizaciones.

## <mark style="color:$primary;">**Funcionalidad de Costos**</mark>

{% columns %}
{% column width="33.33333333333333%" %}
Inicia por el menú <mark style="color:$primary;">**Opciones**</mark> el cual tiene las siguientes funciones:
{% endcolumn %}

{% column width="66.66666666666667%" %}
<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>
{% endcolumn %}
{% endcolumns %}

* <mark style="color:$primary;">**Cargar reportes de compra XLSX:**</mark> Es para seleccionar y cargar los Reportes de Compras generados por Cabinet Report
* <mark style="color:$primary;">**Cargar archivos de ostos XML:**</mark> Es para seleccionar y cargar los archivos de Costos Generados por Cabinet Report desde el módulo de Costos
* <mark style="color:$primary;">**Agregar archivo Promob PDF:**</mark> Es para selecionar los PDF de Promob con el contenido de materiales generados a partir del diseño
* <mark style="color:$primary;">**Márgenes:**</mark>
  * Tiene las opciones de Guardar Márgenes o Limpiar Márgenes ![](<.gitbook/assets/image (2).png>)
    * <mark style="color:$primary;">Guardar Márgenes:</mark> Es para guardar los archivos (MargenOperativo.xml y MargenProduccion.xml) en la carpeta "ArchivosCliente" ubicada en la misma ruta de la aplicación, los márgenes establecidos para que sean cargados cada vez que se abre la ventana de Costos
    * <mark style="color:$primary;">Limpiar Márgenes:</mark> Es para eliminar los archivos (MargenOperativo.xml y MargenProduccion.xml) de la carpeta "ArchivosCliente" ubicada en la misma ruta de la aplicación
* <mark style="color:$primary;">**Notas Globales:**</mark>
  * Tiene las opciones de Guardar Márgenes o Limpiar Márgenes <img src=".gitbook/assets/image (3).png" alt="" data-size="original">
    * <mark style="color:$primary;">Guardar Notas Globales:</mark> Es para guardar los archivos (NotasGlobales.xml) en la carpeta "ArchivosCliente" ubicada en la misma ruta de la aplicación, las Notas Globales establecidas para que sean cargadas cada vez que se abre la ventana de Costos
    * <mark style="color:$primary;">Limpiar Notas Globales:</mark> Es para eliminar los archivos (NotasGlobales.xml) de la carpeta "ArchivosCliente" ubicada en la misma ruta de la aplicación
* <mark style="color:$primary;">**Cargar Precios Unitarios:**</mark> Al activar esta opción, la aplicacion debe buscar en la base de datos la Tabla de Aglomerados "TblAglomerados" y la Tabla de Herrajes "TblAccHerrajes" cada item de la lista, tal cual están escritas sin curar texto, para buscar los valores de:&#x20;
  * Para la tabla de Herrajes  "IdAccesorio", "Tipo", "Costo", "Unidad", donde "Descripcion" sea igual a la columna DESCRIPCION del DataGridView de Materia Prima y cargar las columnas de UND, ID, TIPO, COST-UNIT con los valores encontratos.
  * Para la tabla de Aglomerados "IdAglomerado", "PrecioProveedor" donde "Aglomerado" sea igual a la columna DESCRIPCION del DataGridView de Materia Prima y cargar las columnas de ID, COST-UNIT con los valores encontrados, y en las columnas UND, colocar "UND", en la columna TIPO, colorar "Tablero".
* <mark style="color:$primary;">**Limpiar:**</mark> Al hacer clic, debe volver a cargar el formulario en limpio, como si lo volviese a abrir desde el menú de Costos

## <mark style="color:$primary;">**Mostrar Materia Prima**</mark>

<mark style="color:$primary;">**Campos de Materia Prima Publica y Privada:**</mark>

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

* <mark style="color:$primary;">**Pública:**</mark> al activar este CheckBox, debe filtrar los ComboBox del DataGridView de Materia Prima con solo los Herrajes y Aglomerados que están identificados (Publica) en el campo "Visibilidad" de la tabla de Aglomerados y (Publica) en el campo "Acceso" de la tabla de Herrajes en la base de datos
* <mark style="color:$primary;">**Privada:**</mark> al activar este CheckBox, debe filtrar los ComboBox del DataGridView de Materia Prima con solo los Herrajes y Aglomerados que están identificados (Privada) en el campo "Visibilidad" de la tabla de Aglomerados y (Privada) en el campo "Acceso" de la tabla de Herrajes en la base de datos, pero solo donde el campo "Cliente" de ambas tablas sea igual al cliente indicado en el campo de Cliente de la ventana principal de Cabinet Report (FrmPrincipalCR). Si la opcion privada esta activa, en primera instancia debe cargar los valores de "Costo" y "PrecioProveedor" para el cliente activo en caso de que exista para este cliente.

Siempre debe estar una selección activa, pueden estar ambas activas pero nunca ambas desactivas, y no debe afectar la lista de materia prima establecida actualmente

<mark style="color:$primary;">**Listado de Materia Prima:**</mark>

<figure><img src=".gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

Después de cargado un archivo de Reporte de Compras debe activar el algoritmo de revisar Descripciones en las tablas de la base de datos.

Pero tambien puede ser llenado manualmente por el usuario:

1. Seleccionar una opción de la lista de la columna DESCRIPCION, tambien puede escribir un valor que no esté en la lista.
2. Al seleccionar una opción de la lista, se debe cargar los valores de UND, ID, TIPO, COST-UNIT después de haber identificado los valores de DESCIPCION en la base de datos
