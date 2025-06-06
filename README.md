# Acme-School


// 2. Insertar nuevo estudiante
```js
db.students.insertOne({
  code: 3823,
  name: "Chapman Tillman",
  email: "chapmantillman@zaj.com",
  courses: [
    { code: 1, name: "Introducción a la programación", grade: 4.1 },
    { code: 5, name: "Scrum", grade: 3.4 },
    { code: 6, name: "Javascript", grade: 3.5 }
  ],
  place: {
    address: "Cra 170B # 94 - 58",
    city: "Bogotá"
  },
  age: 20
});
```

// 3. Agregar propiedades a todos los estudiantes
```js
db.students.updateMany({}, {
  $set: {
    active: true,
    modalidad: "Virtual"
  }
});
```

// 4. Actualizar estudiantes específicos
```js
  //code: 6503, actualizar edad a 20
db.students.updateOne({ code: 6503 }, { $set: { age: 20 } });
  //code: 6714, actualizar la ciudad de residencia a Barranquilla.
db.students.updateOne({ code: 6714 }, { $set: { "place.city": "Barranquilla" } });
  //code: 3875, agregar un nuevo hobby, Volleyball.
db.students.updateOne({ code: 3875 }, { $push: { hobbies: "Volleyball" } });
  //code: 2354, agregar los siguientes hobbies, Lectura y Música.
db.students.updateOne({ code: 2354 }, { $push: { hobbies: { $each: ["Lectura", "Música"] } } });
  //code: 7066, agregar los siguientes hobbies, Fútbol, Lectura.
db.students.updateOne({ code: 7066 }, { $push: { hobbies: { $each: ["Fútbol", "Lectura"] } } });
  //name: Hilary Lee, actualizar el correo electrónico a hilarylee09@pearlessa.com
db.students.updateOne({ name: "Hilary Lee" }, { $set: { email: "hilarylee09@pearlessa.com" } });
```

// 5. Renombrar modalidad a modality
```js
db.students.updateMany({}, { $rename: { "modalidad": "modality" } });
```

// 6. Cambiar modalidad a "On Site" para estudiantes en Bucaramanga
```js
db.students.updateMany(
  { "place.city": "Bucaramanga" },
  { $set: { modality: "On Site" } }
);
```

// 7. Consultas
```js
  //a.      Consultar cuantos estudiantes viven en Barranquilla.
db.students.countDocuments({ "place.city": "Barranquilla" });
  //b.      Consultar cuantos estudiantes son mayores de edad.
db.students.countDocuments({ age: { $gte: 18 } });
  //c.      Consultar cuantos estudiantes no son de Bogotá.
db.students.countDocuments({ "place.city": { $ne: "Bogotá" } });
  //d.      Consultar el código, nombre y hobbies de los estudiantes que tienen 18 años.
db.students.find({ age: 18 }, { code: 1, name: 1, hobbies: 1, _id: 0 });
  //e.      Consultar el código, nombre y edad de los estudiantes que son menores de edad.
db.students.find({ age: { $lt: 18 } }, { code: 1, name: 1, age: 1, _id: 0 });
```

// 8. Eliminar la propiedad active
```js
db.students.updateMany({}, { $unset: { active: "" } });
```

// 9. Eliminar hobby "Ciclismo" de estudiante con código 2354
```js
db.students.updateOne(
  { code: 2354 },
  { $pull: { hobbies: "Ciclismo" } }
);
```

// 10. Eliminar hobbies "Lectura" y "Senderismo" de estudiante con código 3875
```js
db.students.updateOne(
  { code: 3875 },
  { $pull: { hobbies: { $in: ["Lectura", "Senderismo"] } } }
);
```

// 11. Eliminar estudiante con código 9241
```js
db.students.deleteOne({ code: 9241 });
```

// 12. Eliminar todos los estudiantes de Barranquilla
```js
db.students.deleteMany({ "place.city": "Barranquilla" });
```


| Operador | Acción |
| --- | --- |
| `$set` | Establece o actualiza campos |
| `$unset` | Elimina campos |
| `$push` | Agrega a un array |
| `$each` | Agrega varios elementos al array |
| `$pull` | Elimina de un array |
| `$in` | Condición: valor está en lista |
| `$rename` | Renombra campos |
| `$ne` | No es igual |
| `$lt`/`$gt`/`$lte`/`$gte` | Comparaciones |
| `$regex` | Búsqueda por patrón de texto |

---

| Operador | Significado                              |
| -------- | ---------------------------------------- |
| `$gt`    | Mayor que                                |
| `$lt`    | Menor que                                |
| `$gte`   | Mayor o igual que                        |
| `$lte`   | Menor o igual que                        |
| `$ne`    | No igual                                 |
| `$in`    | Coincide con alguno de una lista         |
| `$nin`   | No coincide con ninguno de una lista     |
| `$regex` | Coincidencia de texto (búsqueda parcial) |




# 📘 Comandos básicos de MongoDB

Este documento resume los comandos más importantes utilizados en MongoDB para realizar operaciones de inserción, actualización, eliminación y consultas.

---

## 📥 Inserción de documentos

| Comando         | Descripción                        |
|-----------------|------------------------------------|
| `insertOne()`   | Inserta un único documento.        |
| `insertMany()`  | Inserta múltiples documentos.      |

```js
db.collection.insertOne({ nombre: "Juan", edad: 20 })
db.collection.insertMany([{ nombre: "Ana" }, { nombre: "Luis" }])
```

---

## 🔄 Actualización de documentos

| Comando         | Descripción                                        |
|-----------------|----------------------------------------------------|
| `updateOne()`   | Actualiza el primer documento que coincida.        |
| `updateMany()`  | Actualiza todos los documentos que coincidan.      |
| `$set`          | Crea o actualiza un campo.                         |
| `$unset`        | Elimina un campo.                                  |
| `$push`         | Agrega un valor a un array.                        |
| `$each`         | Agrega múltiples valores a un array.              |
| `$pull`         | Elimina un valor específico de un array.          |
| `$rename`       | Cambia el nombre de un campo.                     |

```js
db.collection.updateOne({ nombre: "Juan" }, { $set: { edad: 25 } })
db.collection.updateMany({}, { $set: { activo: true } })
db.collection.updateOne({ nombre: "Luis" }, { $unset: { direccion: "" } })
db.collection.updateOne({ nombre: "Ana" }, { $push: { hobbies: "Lectura" } })
db.collection.updateOne({ nombre: "Ana" }, { $push: { hobbies: { $each: ["Lectura", "Música"] } } })
db.collection.updateOne({ nombre: "Ana" }, { $pull: { hobbies: "Ciclismo" } })
db.collection.updateMany({}, { $rename: { "modalidad": "modality" } })
```

---

## ❌ Eliminación de documentos

| Comando         | Descripción                                        |
|-----------------|----------------------------------------------------|
| `deleteOne()`   | Elimina el primer documento que coincida.          |
| `deleteMany()`  | Elimina todos los documentos que coincidan.        |

```js
db.collection.deleteOne({ nombre: "Juan" })
db.collection.deleteMany({ edad: { $lt: 18 } })
```

---

## 🔍 Consultas

| Comando             | Descripción                                    |
|---------------------|------------------------------------------------|
| `find()`            | Encuentra todos los documentos que coincidan.  |
| `findOne()`         | Encuentra un solo documento.                   |
| `countDocuments()`  | Cuenta los documentos que cumplan una condición.|

```js
db.collection.find({ ciudad: "Bogotá" })
db.collection.findOne({ nombre: "Juan" })
db.collection.countDocuments({ edad: { $gte: 18 } })
```

---

## 🧠 Operadores comunes

| Operador   | Uso                                      |
|------------|------------------------------------------|
| `$gt`      | Mayor que                                |
| `$lt`      | Menor que                                |
| `$gte`     | Mayor o igual que                        |
| `$lte`     | Menor o igual que                        |
| `$ne`      | No igual                                 |
| `$in`      | Valor está en una lista                  |
| `$nin`     | Valor no está en una lista               |
| `$regex`   | Búsqueda por coincidencia de texto       |

```js
db.collection.find({ edad: { $gt: 18 } })
db.collection.find({ ciudad: { $ne: "Bogotá" } })
db.collection.find({ hobbies: { $in: ["Lectura", "Fútbol"] } })
db.collection.find({ nombre: /juan/i }) // búsqueda sin distinguir mayúsculas
```

---
