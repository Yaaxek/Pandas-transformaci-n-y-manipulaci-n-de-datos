# Resumen de Proyectos de Análisis de Datos

## Descripción General
Estos proyectos tienen como objetivo analizar datos de propiedades para servicios de hosting, con el fin de desarrollar un algoritmo capaz de sugerir precios diarios dinámicos para maximizar las ganancias. A lo largo de estos proyectos, se han abordado diversas etapas del procesamiento de datos, desde la limpieza y preparación hasta el análisis textual y temporal.

## Temas Principales Cubiertos
* Carga de datos y normalización de estructuras JSON anidadas.
* Conversión de tipos de datos para columnas numéricas y de fecha.
* Manipulación de cadenas de texto: conversión a minúsculas, eliminación de caracteres especiales, limpieza de símbolos monetarios.
* Tokenización de texto para análisis descriptivo.
* Manejo de valores nulos (missing values).
* Agregación de datos temporales para análisis por períodos (e.g., mensual).

## Proyecto 1: Análisis de Datos de Propiedades para Hosting

### Problema de Negocio/Objetivo
Desarrollar un algoritmo que analice las características de una propiedad (comodidades, tamaño, ocupación en un período determinado) y sugiera al anfitrión un precio a cobrar por tarifas diarias que garantice ganancias en momentos de alta demanda. Esto implica entender cómo diferentes atributos de la propiedad y su disponibilidad impactan el precio.

### Fuente de Datos
* `/content/datos_hosting.json`: Contiene información detallada sobre las propiedades, incluyendo características, descripciones y precios base.
* `/content/inmuebles_disponibles.json`: Contiene datos de disponibilidad y precios para diferentes fechas.

### Pasos de Procesamiento de Datos
*   Cargar 'datos_hosting.json' y normalizar la estructura JSON anidada usando `pd.json_normalize()`.
*   Expandir columnas con listas (`descripcion_local`, `descripcion_vecindad`, `cantidad_baños`, `cantidad_cuartos`, `cantidad_camas`, `modelo_cama`, `comodidades`, `cuota_deposito`, `cuota_limpieza`, `precio`) para crear registros individuales, utilizando `df.explode()`.
*   Reiniciar el índice del DataFrame resultante (`datos.reset_index(inplace=True, drop=True)`).
*   Convertir las columnas numéricas (`max_hospedes`, `cantidad_baños`, `cantidad_cuartos`, `cantidad_camas`) a tipo entero (`np.int64`).
*   Convertir la columna `evaluacion_general` a tipo flotante (`np.float64`).
*   Limpiar y convertir las columnas monetarias (`precio`, `cuota_deposito`, `cuota_limpieza`) a tipo flotante (`np.float64`) eliminando '$' y comas, y luego convirtiendo a numérico.
*   Procesar la columna de texto `descripcion_local`: convertir a minúsculas, eliminar caracteres especiales (manteniendo guiones y apóstrofes) y tokenizar la cadena en una lista de palabras.
*   Limpiar la columna `comodidades` eliminando caracteres '{', '}', '"' y luego dividir la cadena por comas para obtener una lista de comodidades.
*   Procesar la columna de texto `descripcion_vecindad`: convertir a minúsculas, eliminar caracteres especiales (manteniendo guiones y apóstrofes) y tokenizar la cadena en una lista de palabras.
*   Cargar 'inmuebles_disponibles.json' en un DataFrame separado (`dt_data`).
*   Convertir la columna `fecha` en `dt_data` a tipo datetime usando `pd.to_datetime()`.
*   Rellenar los valores faltantes en la columna `precio` de `dt_data` con '0.0', limpiar la cadena (eliminando '$' y comas) y convertirla a tipo flotante (`np.float64`).
*   Calcular la suma de 'lugar_disponible' agrupada por mes a partir de `dt_data` (`dt_data.groupby(dt_data['fecha'].dt.strftime('%Y-%m'))['lugar_disponible'].sum()`).

