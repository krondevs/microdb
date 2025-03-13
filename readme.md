# Microservicio de Conexión a Bases de Datos

## Descripción General
Este microservicio permite realizar operaciones de consulta y manipulación de datos en diferentes tipos de bases de datos, incluyendo MySQL, MariaDB, SQLite3, BadgerDB y JSON.

## Tipos de Bases de Datos Soportados
- MySQL
- MariaDB
- SQLite3
- BadgerDB
- JSON Cifrados

## Estructura General del JSON de Solicitud

### Parámetros Comunes
- `dbtype`: Tipo de base de datos (obligatorio)
- `dbname`: Nombre de la base de datos (obligatorio)
- `apikey`: Clave de API para autenticación (obligatorio)
- `querytype`: Tipo de consulta (`select`, `exec`, `update`, `delete`)
- `dbquery`: Consulta o comando a ejecutar
- `params`: Parámetros de la consulta

## Ejemplos de Uso por Tipo de Base de Datos

### MySQL / MariaDB / SQLite3

#### Consulta Parametrizada
```json
{
  "dbtype": "mysql",
  "dbname": "database_name",
  "apikey": "apikey",
  "querytype": "select", 
  "dbquery": "SELECT * FROM productos WHERE id BETWEEN ? AND ?",
  "params": [100, 200]
}
```

#### Consulta Sin Parámetros
```json
{
  "dbtype": "mysql",
  "dbname": "database_name",
  "apikey": "apikey",
  "querytype": "select", 
  "dbquery": "SELECT * FROM productos WHERE id BETWEEN valor AND valor",
  "params": []
}
```

#### Inserción Parametrizada
```json
{
  "dbtype": "mysql",
  "dbname": "database_name",
  "apikey": "apikey",
  "querytype": "exec", 
  "dbquery": "INSERT INTO tabla (campo, campo) VALUES (?,?)",
  "params": [100, 200]
}
```

#### Inserción Sin Parámetros
```json
{
  "dbtype": "mysql",
  "dbname": "database_name",
  "apikey": "apikey",
  "querytype": "exec", 
  "dbquery": "INSERT INTO tabla (campo, campo) VALUES (valor,valor)",
  "params": []
}
```

### BadgerDB

#### Consulta por Clave Exacta
- morfologias: map[string]any, []map[string]any, []string, []any, map[string]map[string]any, etc.
```json
{
  "dbtype": "badgerdb",
  "dbname": "database_name",
  "apikey": "apikey",
  "querytype": "select", 
  "dbquery": "clave",
  "params": "map[string]any"
}
```

#### Consulta por Prefijo
- devuelve un json con la morfologia []map[string]any
```json
{
  "dbtype": "badgerdb",
  "dbname": "database_name",
  "apikey": "apikey",
  "querytype": "select", 
  "dbquery": "prefijo",
  "params": "prefix"
}
```

#### Inserción
- El campo params debe ser un json serializado en string dentro del json de la consulta
```json
{
  "dbtype": "badgerdb",
  "dbname": "database_name",
  "apikey": "apikey",
  "querytype": "exec", 
  "dbquery": "clave",
  "params": "{\"edad\": 54}"
}
```

#### Actualización
```json
{
  "dbtype": "badgerdb",
  "dbname": "database_name",
  "apikey": "apikey",
  "querytype": "update", 
  "dbquery": "clave",
  "params": "{\"edad\": 55}"
}
```

#### Eliminación
```json
{
  "dbtype": "badgerdb",
  "dbname": "database_name",
  "apikey": "apikey",
  "querytype": "delete", 
  "dbquery": "clave",
  "params": ""
}
```


## Notas Importantes
- Asegúrese de proporcionar siempre una `apikey` válida
- Los parámetros deben coincidir con el tipo de base de datos seleccionado
- Para BadgerDB, utilice JSON serializado para estructuras de datos complejas
- Maneje siempre los parámetros con precaución para prevenir inyecciones de SQL

### JSON

#### Consulta por Clave Exacta
- morfologias: map[string]any, []map[string]any, []string, []any, map[string]map[string]any, etc.
```json
{
  "dbtype": "json",
  "dbname": "database_name",
  "apikey": "apikey",
  "querytype": "select", 
  "dbquery": "clave",
  "params": "map[string]any"
}
```

#### Consulta por Prefijo
- devuelve un json con la morfologia []map[string]any
```json
{
  "dbtype": "json",
  "dbname": "database_name",
  "apikey": "apikey",
  "querytype": "select", 
  "dbquery": "prefijo",
  "params": "prefix"
}
```

#### Inserción
- El campo params debe ser un json serializado en string dentro del json de la consulta
```json
{
  "dbtype": "json",
  "dbname": "database_name",
  "apikey": "apikey",
  "querytype": "exec", 
  "dbquery": "clave",
  "params": "{\"edad\": 54}"
}
```

#### Actualización
```json
{
  "dbtype": "json",
  "dbname": "database_name",
  "apikey": "apikey",
  "querytype": "update", 
  "dbquery": "clave",
  "params": "{\"edad\": 55}"
}
```

#### Eliminación
```json
{
  "dbtype": "json",
  "dbname": "database_name",
  "apikey": "apikey",
  "querytype": "delete", 
  "dbquery": "clave",
  "params": ""
}
```
#### Respaldo
```json
{
  "dbtype": "json",
  "dbname": "database_name",
  "apikey": "apikey",
  "querytype": "select", 
  "dbquery": "",
  "params": "backup"
}
```
## Notas Importantes JSON
- Los archivos se almacenan en colecciones que son indicadas por el parametro `dbname`
- Para JSON, se utiliza JSON serializado para estructuras de datos complejas
- Los datos se almacenan cifrados en archivos de extension `.dat` detro de la carpeta de la coleccion indicada
- Los backups se almacenan con el nombre de la base de datos indicada en el archivo `dbsettings.json`, alli se respaldan comprimidos en zip con todas las colecciones, nombre por defecto de con hora y fecha en una carpeta `backups`.

## Morfologías de Datos Soportadas
- `map[string]any`
- `[]map[string]any`
- `[]string`
- `[]any`
- `map[string]map[string]any`

## Recomendaciones de Seguridad
1. Utilice siempre consultas parametrizadas
2. Valide y limpie las entradas de usuario
3. Limite los permisos de la apikey
4. Implemente registro de auditoría para todas las operaciones
```
