```js
// Usar una base de datos llamada 'tineda'
use tineda

// Crear una colección llamada 'ejemplo'
db.createCollection("ejemplo")

// Mostrar las colecciones existentes
show collections

// Insertar un documento en la colección 'ejemplo'
db.ejemplo.insertOne({"nombre": "Casemiro", "edad": 23})

// Consultar documentos en la colección 'libros' con editorial igual a "Biblio"
db.libros.find({editorial:{$eq:"Biblio"}})

// Consultar documentos en la colección 'libros' con precio igual a 25
db.libros.find({precio:25})

// Consultar documentos en la colección 'libros' con precio igual a 25 usando el operador $eq
db.libros.find({precio:{$eq:25}})

// Consultar documentos en la colección 'libros' con precio mayor o igual a 20
db.libros.find({precio:{$gte:20}})

// Consultar documentos en la colección 'libros' con precio menor a 5
db.libros.find({precio:{$lt:5}})

// Consultar documentos en la colección 'libros' con editorial "Bilio" o "Planeta"
db.libros.find({editorial:{$in:['Bilio', 'Planeta']}})

// Encontrar el primer documento en la colección 'libros' con precio 20 o 25
db.libros.findOne({precio:{$in:[20, 25]}})

// Consultar documentos en la colección 'libros' con precio mayor a 25 y cantidad menor a 15
db.libros.find({precio:{$gt:25},cantidad:{$lt:15}})

// Consultar documentos en la colección 'libros' con precio mayor a 25 o cantidad menor a 15
db.libros.find({$or:[{precio:{$gt:25}}, {cantidad:{$lt:15}}]})

// Seleccionar libros de editorial con precio mayor a 40 o libros de la editorial Planeta con precio mayor a 30
db.libros.find({$or:[{editorial:'Biblio', precio:{$gt:40}}, {editorial:'Planeta', precio:{$gt:30}}]})

// Consultar todos los documentos de la colección 'libros' mostrando solo el campo 'titulo'
db.libros.find({},{titulo:1})

// Consultar documentos en la colección 'libros' con título "Don quijote" mostrando solo el campo 'titulo'
db.libros.find({titulo:'Don quijote'},{titulo:1})

// Consultar documentos en la colección 'libros' con título "Don quijote" mostrando solo el campo 'titulo' y sin incluir '_id'
db.libros.find({titulo:'Don quijote'},{_id:0,titulo:1})

// Consultar documentos en la colección 'libros' donde exista el campo 'titulo'
db.libros.find({titulo:{$exists:true}})

// Consultar documentos en la colección 'libros' con precio mayor a 5 y donde exista el campo 'autor'
db.libros.find({$and:[{precio:{$gt:5}} ,{autor:{$exists:true}} ]})

// Insertar un nuevo documento en la colección 'libros'
db.libros.insertOne({_id:12, titulo: "Estupro", finaza:true, anios_carcel:2,precio:13.35 })

// Consultar documentos en la colección 'libros' con precio de tipo NumberInt
db.libros.find({precio:{$type:1}})

// Actualizar el título del documento con título "JSON para todos" a "Java el rey"
db.libros.updateOne({titulo:'JSON para todos'}, {$set:{titulo:'Java el rey'}})

// Actualizar el precio de documentos con precio mayor a 9 a 150
db.libros.updateMany({precio:{$gt:9}}, {$set:{precio:150}} )

// Multiplicar el precio por 2 para documentos con cantidad mayor a 20
db.libros.updateMany({cantidad:{$gt:20}}, {$mul:{precio:2} })

// Reemplazar un documento con _id 13 por un nuevo documento con _id 14 y título especificado
db.libros.replaceOne(
... {"_id":13},{ "_id":14,"titulo":"Las pato aventuras de Ezequiel Alias Mateo"})

// Borrar el documento con _id 13
db.libros.deleteOne({"_id":13})

// Borrar el primer documento que cumple la condición
db.libros.deleteOne({"titulo":"Java el rey"})

// Borrar todos los documentos que cumplan con la condición
db.libros.deleteMany({"cantidad":{$gt:20}})

// Expresiones regulares

// Buscar todos los documentos que contengan la letra 't' en el título
db.libros.find({"titulo":/t/})

// Buscar todos los documentos que contengan la palabra 'Java' en el título
db.libros.find({"titulo":/Java/})

// Buscar todos los documentos cuyo título termine con 'tos'
db.libros.find({"titulo":/tos$/})

// Buscar todos los documentos cuyo título comience con la letra 'J'
db.libros.find({"titulo":/^J/})

// Buscar todos los documentos que contengan la palabra 'para' en el título
db.libros.find({"titulo":{$regex:"para"}})

// Buscar documentos con el nombre especificado, sin importar mayúsculas o minúsculas
db.libros.find({"titulo":{$regex:"JAVA" , $options:'i'}})

// Ordenar documentos por título de forma ascendente
db.libros.find({},{titulo:1,precio:1}).sort({titulo:1})

// Ordenar documentos por título de forma descendente
db.libros.find({},{titulo:1,precio:1}).sort({titulo:-1})

// Ordenar documentos primero por título y luego por editorial
db.libros.find({},{titulo:1,precio:1,editorial:1,_id:0}).sort({titulo:1,editorial:1})

// Saltar 2 documentos y mostrar los siguientes
db.libros.find({},{"titulo":1,"precio":1,_id:0, "editorial":1}).skip(2)

// Limitar la cantidad de documentos mostrados
db.libros.find({},{"titulo":1,"precio":1,_id:0, "editorial":1}).limit(2)

// Contar el número de documentos que cumplen la condición
db.libros.find({},{"titulo":1,"precio":1,_id:0, "editorial":1}).size()

// Mostrar los primeros 3 documentos ordenados por título de forma ascendente
db.libros.find({},{"titulo":1,"precio":1,_id:0, "editorial":1}).
sort({titulo:1}).limit(3)

// Eliminar la colección 'ejemplo'
db.ejemplo.drop()

// Eliminar la base de datos actual
db.dropDatabase()

// Agregaciones

// Consultar documentos con editorial "Biblio", proyectando algunos campos y calculando el total de ganancias
db.libros.aggregate([
    { $match: {editorial:'Biblio'}}, 
    { $project:{id_:0, titulo:1, precio:1,'Nombre Editorial': '$editorial' }},
    { $sort: { precio: 1 } }
])

// Pipeline para contar el número de documentos por editorial y ordenarlos
[
    {
        $group: {
            _id: "$editorial",
            "Numero documentos": {
                $count: {},
            },
        },
    },
    {
        $sort: {
            "Numero documentos": 1,
        },
    },
]

// Pipeline para calcular el número de documentos y la media de precios por editorial
[
    {
        $group: {
            _id: "$editorial",
            "Numero de docuemtos ": {
                $count: {},
            },
            "media:": {
                $avg: "$precio",
            },
            "Precio Max:": {
                $max: "$precio",
            },
        },
    },
    {
        $sort: {
            "Precio Max:": 1,
        },
    },
]

// Pipeline para calcular la media total de precios por editorial y almacenar el resultado en una nueva colección
[
    {
        $group: {
            _id: "$editorial",
            Numero: {
                $count: {},
            },
            media: {
                $avg: "precio",
            },
        },
    },
    {
        $set: {
            "Media Total": {
                $trunc: ["$media", 2],
            },
        },
    },
    {
        $out: "Media_Editoriales",
    },
]

// Pipeline para calcular la media total de precios por editorial y eliminar el campo 'media'
[
    {
        $group: {
            _id: "$editorial",
            Numero: {
                $count: {},
            },
            media: {
                $avg: "precio",
            },
        },
    },
    {
        $set: {
            "Media Total": {
                $trunc: ["$media", 2],
            },
        },
    },
    {
        $unset: "media",
    },
]
