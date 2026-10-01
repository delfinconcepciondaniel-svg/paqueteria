# Diagrama de estados de un envío

```mermaid
stateDiagram-v2
[*] --> Registrado
Registrado --> Clasificado
Clasificado --> EnDespacho
EnDespacho --> EnTransito
EnTransito --> EnReparto
EnReparto --> Entregado
EnReparto --> Incidencia
Incidencia --> EnReparto
Incidencia --> Devuelto
Registrado --> Cancelado
Entregado --> [*]
Devuelto --> [*]
Cancelado --> [*]
```
