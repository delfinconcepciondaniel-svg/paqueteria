# Modelo de dominio — RutaExpress

El dominio contempla Usuario, Remitente, Destinatario, Paquete, Envío, Sucursal, Ruta, Repartidor, Vehículo, Pago, Incidencia, Entrega y Comprobante.

Las relaciones centrales son: un remitente registra muchos envíos; un envío pertenece a un destinatario, contiene uno o varios paquetes, puede asociarse a una ruta y repartidor, y puede generar pago, entrega, incidencia y comprobante.

Ver el diagrama completo en `RutaExpress_GitDiagram.md`.
