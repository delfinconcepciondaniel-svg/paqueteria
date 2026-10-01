# RutaExpress — Diagramas para GitDiagram / Mermaid

## 1. Diagrama entidad-relación

```mermaid
erDiagram
    USUARIO {
        int idUsuario PK
        string nombreUsuario
        string contrasena
        string rol
        string estado
    }
    REMITENTE {
        int idRemitente PK
        string documento
        string nombres
        string telefono
        string direccion
        string correo
    }
    DESTINATARIO {
        int idDestinatario PK
        string nombres
        string telefono
        string direccion
        string referencia
    }
    PAQUETE {
        int idPaquete PK
        string codigo
        decimal peso
        string dimensiones
        string tipo
        string contenido
    }
    ENVIO {
        int idEnvio PK
        date fechaRegistro
        string modalidad
        decimal costo
        string estado
        int idRemitente FK
        int idDestinatario FK
        int idSucursal FK
        int idRuta FK
        int idRepartidor FK
    }
    SUCURSAL {
        int idSucursal PK
        string nombre
        string direccion
        string telefono
        string horario
    }
    RUTA {
        int idRuta PK
        string origen
        string destino
        date fechaSalida
        date fechaLlegada
    }
    REPARTIDOR {
        int idRepartidor PK
        string nombres
        string DNI
        string telefono
        string licencia
        int idVehiculo FK
    }
    VEHICULO {
        int idVehiculo PK
        string placa
        string tipo
        decimal capacidad
        string estado
    }
    PAGO {
        int idPago PK
        date fecha
        decimal monto
        string metodoPago
        string estado
        int idEnvio FK
    }
    INCIDENCIA {
        int idIncidencia PK
        string tipo
        string descripcion
        date fecha
        string estado
        int idEnvio FK
    }
    ENTREGA {
        int idEntrega PK
        date fechaProgramada
        date fechaEntrega
        string evidencia
        string estado
        int idEnvio FK
    }
    COMPROBANTE {
        int idComprobante PK
        string numero
        date fecha
        decimal importe
        int idEnvio FK
    }

    REMITENTE ||--o{ ENVIO : registra
    DESTINATARIO ||--o{ ENVIO : recibe
    SUCURSAL ||--o{ ENVIO : gestiona
    RUTA ||--o{ ENVIO : agrupa
    REPARTIDOR ||--o{ ENVIO : entrega
    VEHICULO ||--o{ REPARTIDOR : utiliza
    ENVIO ||--|{ PAQUETE : contiene
    ENVIO ||--o| PAGO : genera
    ENVIO ||--o{ INCIDENCIA : presenta
    ENVIO ||--o| ENTREGA : registra
    ENVIO ||--o| COMPROBANTE : genera
```

## 2. Diagrama de clases

```mermaid
classDiagram
class Usuario {
  +int idUsuario
  +string nombreUsuario
  +string contrasena
  +string rol
  +string estado
}
class Remitente {
  +int idRemitente
  +string documento
  +string nombres
  +string telefono
  +string direccion
  +string correo
}
class Destinatario {
  +int idDestinatario
  +string nombres
  +string telefono
  +string direccion
  +string referencia
}
class Paquete {
  +int idPaquete
  +string codigo
  +decimal peso
  +string dimensiones
  +string tipo
  +string contenido
}
class Envio {
  +int idEnvio
  +date fechaRegistro
  +string modalidad
  +decimal costo
  +string estado
}
class Sucursal {
  +int idSucursal
  +string nombre
  +string direccion
  +string telefono
  +string horario
}
class Ruta {
  +int idRuta
  +string origen
  +string destino
  +date fechaSalida
  +date fechaLlegada
}
class Repartidor {
  +int idRepartidor
  +string nombres
  +string DNI
  +string telefono
  +string licencia
}
class Vehiculo {
  +int idVehiculo
  +string placa
  +string tipo
  +decimal capacidad
  +string estado
}
class Pago {
  +int idPago
  +date fecha
  +decimal monto
  +string metodoPago
  +string estado
}
class Incidencia {
  +int idIncidencia
  +string tipo
  +string descripcion
  +date fecha
  +string estado
}
class Entrega {
  +int idEntrega
  +date fechaProgramada
  +date fechaEntrega
  +string evidencia
  +string estado
}
class Comprobante {
  +int idComprobante
  +string numero
  +date fecha
  +decimal importe
}

Remitente "1" --> "0..*" Envio : registra
Destinatario "1" --> "0..*" Envio : recibe
Sucursal "1" --> "0..*" Envio : gestiona
Ruta "1" --> "0..*" Envio : agrupa
Repartidor "1" --> "0..*" Envio : entrega
Vehiculo "1" --> "0..*" Repartidor : utiliza
Envio "1" *-- "1..*" Paquete : contiene
Envio "1" --> "0..1" Pago : genera
Envio "1" --> "0..*" Incidencia : presenta
Envio "1" --> "0..1" Entrega : registra
Envio "1" --> "0..1" Comprobante : genera
```

## 3. Casos de uso

```mermaid
flowchart LR
R[Remitente]
O[Operador de Agencia]
C[Clasificador]
D[Repartidor]
A[Administrador]

R --- U1((Registrar envío))
R --- U2((Consultar tarifa))
R --- U3((Realizar pago))
R --- U4((Consultar seguimiento))

O --- U5((Registrar remitente y destinatario))
O --- U1
O --- U6((Registrar paquete))
O --- U3
O --- U7((Generar comprobante))

C --- U6
C --- U8((Clasificar paquete))
C --- U9((Asignar ruta))
C --- U10((Preparar despacho))

D --- U11((Consultar ruta))
D --- U12((Actualizar estado))
D --- U13((Registrar entrega))
D --- U14((Registrar incidencia))

A --- U15((Gestionar usuarios y roles))
A --- U16((Gestionar sucursales y rutas))
A --- U17((Generar reportes))
A --- U18((Gestionar parámetros))
```

## 4. Estados de un envío

```mermaid
stateDiagram-v2
[*] --> Registrado
Registrado --> Clasificado : clasificar
Clasificado --> EnDespacho : despachar
EnDespacho --> EnTransito : iniciar transporte
EnTransito --> EnReparto : asignar reparto
EnReparto --> Entregado : confirmar entrega
EnReparto --> Incidencia : registrar incidencia
Incidencia --> EnReparto : resolver incidencia
Incidencia --> Devuelto : devolución
Registrado --> Cancelado : cancelar
EnTransito --> Cancelado : cancelar
Entregado --> [*]
Devuelto --> [*]
Cancelado --> [*]
```

## 5. Flujo de recepción y entrega

```mermaid
flowchart TD
I([Inicio]) --> R1[Registrar remitente y destinatario]
R1 --> R2[Registrar peso, dimensiones y tipo de paquete]
R2 --> V{¿Datos válidos?}
V -->|No| C[Corregir datos]
C --> R2
V -->|Sí| T[Calcular tarifa]
T --> P[Registrar pago]
P --> B[Generar comprobante y código de seguimiento]
B --> CL[Clasificar paquete]
CL --> RT[Asignar ruta]
RT --> RP[Asignar repartidor]
RP --> TR[Transportar y actualizar estado]
TR --> E{¿Entrega posible?}
E -->|Sí| EN[Registrar entrega y evidencia]
E -->|No| IN[Registrar incidencia]
IN --> REV[Revisión / nueva programación]
REV --> TR
EN --> F([Fin])
```

## 6. Diagrama de secuencia — Registrar envío y coordinar entrega

```mermaid
sequenceDiagram
actor Remitente
participant Operador
participant Sistema
participant Clasificador
participant Repartidor

Remitente->>Operador: Solicita envío
Operador->>Sistema: Registra remitente y destinatario
Operador->>Sistema: Registra datos del paquete
Sistema->>Sistema: Valida datos y calcula tarifa
Sistema-->>Operador: Muestra tarifa
Remitente->>Operador: Selecciona método de pago
Operador->>Sistema: Registra pago
Sistema-->>Remitente: Genera comprobante y código de seguimiento
Sistema->>Clasificador: Envía orden de clasificación
Clasificador->>Sistema: Asigna ruta
Sistema->>Repartidor: Asigna ruta y envío
Repartidor->>Sistema: Actualiza estado
Repartidor->>Sistema: Registra entrega o incidencia
Sistema-->>Remitente: Actualiza seguimiento
```

## 7. Arquitectura lógica

```mermaid
flowchart TD
subgraph PRESENTACION[Capa de presentación]
  WEB[Portal / interfaz web]
  APP[Interfaz móvil del repartidor]
end
subgraph NEGOCIO[Capa de negocio]
  US[Usuarios y roles]
  CL[Clientes y envíos]
  LG[Logística y rutas]
  PG[Pagos y comprobantes]
  SE[Seguimiento e incidencias]
  RP[Reportes]
end
subgraph DATOS[Capa de datos]
  DAO[Repositorios / acceso a datos]
  DB[(Base de datos relacional)]
end
WEB --> US
WEB --> CL
WEB --> LG
WEB --> PG
APP --> SE
APP --> LG
CL --> DAO
LG --> DAO
PG --> DAO
SE --> DAO
RP --> DAO
US --> DAO
DAO --> DB
```
