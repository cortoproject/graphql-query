# graphql-query
Run GraphQL queries on corto object store

## Mapping to object store:
The following GraphQL queries use the following corto model:
```
enum Color:/ Red, Blue

table Car:/
    speed: float32
    color: /Color
    leaf Engine:/
        rpm: float32
```
And corto dataset:
```
tableinstance data/Car: type=Car :/
    Car "04-C-1672": speed=55.2 color="Red" :/
        Car/Engine Engine: rpm = 4000
    Car "88-B-3244": speed=23.2 color="Blue" :/
        Car/Engine Engine: rpm = 2000
```

### Request single object
Example GraphQL query:
```graphql
{
  root(id: "data/Car/04-C-1672) {
    _id
    _parent
    speed
    color
  }
}
```

Result:
```json
{
    "data": {
        "root": {
            "_id": "04-C-1672",
            "_parent": ".",
            "speed": 55.2,
            "color": "Red"
        }  
    }
}
```

### Request multiple objects with nested object query
Example GraphQL query:
```graphql
{
  root(select: "*", from: "data/Car") {
    _id
    _parent
    speed
    color
    Engine {
      rpm
    }
  }
}
```
Result:
```json
{
    "data": {
        "root": [
            {
                "_id": "04-C-1672",
                "_parent": ".",
                "speed": 55.2,
                "color": "Red",
                "Engine": {
                    "rpm": 4000
                }
            },
            {
                "_id": "88-B-3244",
                "_parent": ".",
                "speed": 23.2,
                "color": "Blue",
                "Engine": {
                    "rpm": 2000
                }
            }
        ]  
    }
}
```

