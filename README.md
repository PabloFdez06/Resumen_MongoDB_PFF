
# MongoDB: Conceptos Básicos – Resumen 

## 1. ¿Qué es MongoDB?

MongoDB es una base de datos NoSQL. Esto significa que no utiliza el modelo clásico de tablas y filas como las bases de datos relacionales (por ejemplo, MySQL o PostgreSQL). En lugar de eso, almacena los datos en documentos con una estructura similar a JSON, aunque internamente se usa BSON (una versión binaria de JSON más eficiente).

Una de sus principales ventajas es que permite estructuras flexibles. Es decir, no es necesario que todos los datos tengan exactamente la misma forma, lo que resulta muy útil en proyectos donde los datos pueden cambiar con el tiempo o no tener una estructura totalmente definida.

### Características clave de MongoDB

- Admite grandes volúmenes de datos.
- Escala horizontalmente (distribución en múltiples servidores).
- Compatible con múltiples lenguajes de programación (JavaScript, Python, Java, etc.).
- Buen rendimiento tanto en lectura como en escritura.
- Ideal para aplicaciones web modernas, APIs, microservicios y análisis de datos.

---

## 2. Documentos, Colecciones y Diferencias con SQL
---
### 2.1 Documentos

Un documento en MongoDB es una unidad de información parecida a un objeto JSON. Puede tener texto, números, booleanos, fechas, listas, e incluso subdocumentos.

#### Ejemplo de documento

```
{
  "nombre": "Portátil",
  "precio": 899.99,
  "stock": 12,
  "especificaciones": {
    "procesador": "Intel i7",
    "ram": "16GB",
    "almacenamiento": "512GB SSD"
  }
}
```

### 2.2 Colecciones
Una colección es un conjunto de documentos. A diferencia de las tablas en SQL, en MongoDB no es obligatorio que todos los documentos tengan los mismos campos o estructura.

#### Ejemplo de colección

```
[
  {
    "nombre": "Ratón",
    "precio": 25.99,
    "categoria": "Accesorios"
  },
  {
    "nombre": "Teclado",
    "precio": 45.00,
    "categoria": "Accesorios",
    "retroiluminado": true
  }
]
```

### 2.3 Diferencias con bases de datos relacionales

| Característica  | MongoDB                 | Bases de datos relacionales |
| --------------- | ----------------------- | --------------------------- |
| Modelo de datos | Documentos (JSON/BSON)  | Tablas (filas y columnas)   |
| Estructura      | Flexible                | Fija (esquema rígido)       |
| Relaciones      | Referencias o embebido  | JOINs                       |
| Escalabilidad   | Horizontal (sharding)   | Vertical                    |
| Transacciones   | Limitadas pero posibles | Completas                   |

## 3. Operaciones CRUD
---
### 3.1 Crear documentos

- insertOne() añade un único documento.
- insertMany() añade varios documentos a la vez.

```
db.productos.insertOne({
  nombre: "Smartphone",
  precio: 599.99,
  stock: 25
})

```

### 3.2 Leer documentos

` db.productos.find({ precio: { $gt: 500 } }) `

Proyección (mostrar campos específicos)

`db.productos.find({}, { nombre: 1, _id: 0 })`

### 3.3 Actualizar documentos
- updateOne() cambia el primer documento que cumpla con el filtro.
- updateMany() cambia todos los documentos que lo cumplan.

```
db.productos.updateOne(
  { nombre: "Smartphone" },
  { $set: { precio: 649.99 } }
)
```

### 3.4 Borrar documentos
- deleteOne() borra el primero que coincida.
- deleteMany() borra todos los que coincidan.

`db.productos.deleteMany({ categoria: "Accesorios" })`

## 4. Consultas Avanzadas
---
### 4.1 Filtros
`db.productos.find({ precio: { $gte: 100, $lte: 500 } }) `

Operadores comunes
- $eq: igual
- $ne: distinto
- $gt: mayor que
- $lt: menor que
- $in: dentro de un conjunto

### 4.2 Operadores lógicos

```
db.productos.find({
  $and: [
    { categoria: "Electrónica" },
    { stock: { $gt: 10 } }
  ]
})

```

Operadores lógicos

- $and, $or, $not, $nor

### 4.3 Consultas con arrays

`db.recetas.find({ ingredientes: { $all: ["harina", "huevo"] } })`

- $all: todos los valores deben estar
- $size: tamaño exacto del array
- $elemMatch: al menos un elemento cumple condiciones

### 4.4 Ordenar y limitar resultados

`db.productos.find().sort({ precio: -1 }).limit(5)`

## 5. Índices
---
### 5.1 ¿Qué son?
Los índices permiten que las consultas sean más rápidas. MongoDB crea automáticamente uno en _id.

### 5.2 Tipos de índices

- Índices simples
- Índices compuestos
- Índices únicos
- Índices de texto
- Índices geoespaciales

### 5.3 Crear y gestionar índices

```
db.usuarios.createIndex({ email: 1 }, { unique: true })
db.usuarios.getIndexes()
db.usuarios.dropIndex({ email: 1 })
```

### 5.4 Analizar consultas

`db.productos.find({ precio: { $gt: 500 } }).explain("executionStats")`

## 6. Agregaciones
Permiten realizar operaciones más complejas como sumar, agrupar, filtrar, transformar, etc.
---
### 6.1 Ejemplo básico

```
db.productos.aggregate([
  { $match: { precio: { $gt: 100 } } },
  { $group: { _id: "$categoria", media: { $avg: "$precio" } } },
  { $sort: { media: -1 } }
])
```

### 6.2 Etapas comunes en un pipeline

- $match: filtrar
- $group: agrupar
- $project: seleccionar campos
- $sort: ordenar
- $limit: limitar resultados
- $unwind: descomponer arrays

## 7. Replicación y Alta Disponibilidad
---
### 7.1 Replica Sets
MongoDB permite tener varias copias sincronizadas de una base de datos en diferentes servidores.

- Nodo primario: acepta escrituras
- Nodos secundarios: copias de solo lectura
- Árbitro: decide en caso de fallo qué nodo se vuelve primario

### 7.2 Lectura desde secundarios
Se puede habilitar para distribuir la carga de trabajo.

## 8. Seguridad y Sharding
---
### 8.1 Seguridad

- Autenticación (usuarios, contraseñas)
- Roles y permisos
- Cifrado (TLS/SSL y en reposo)
- Auditoría de accesos

### 8.2 Sharding
Sistema para dividir automáticamente los datos en fragmentos en diferentes servidores.
- Componentes del sistema de sharding:
- Shards: particiones de datos
- mongos: enrutador de peticiones
- config servers: configuración del clúster

## Conclusión
MongoDB es una base de datos moderna, potente y flexible, muy adecuada para aplicaciones dinámicas. A diferencia de SQL, permite una estructura de datos adaptable y facilita el trabajo con datos complejos o en constante cambio. Además, gracias a características como la replicación, el sharding y las agregaciones, puede escalar y adaptarse a sistemas distribuidos y con alta demanda.
