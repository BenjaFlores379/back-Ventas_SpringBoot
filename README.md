# Backend Ventas - Innovatech Chile (Proyecto Semestral)

Este repositorio contiene la API REST del microservicio de **Ventas**, desarrollado en Spring Boot y Java. Forma parte de la arquitectura de microservicios desplegada en AWS.

## Tecnologías utilizadas

- **Java 17**
- **Spring Boot 3.4.4**
- **Spring Data JPA** (persistencia)
- **MySQL** (Amazon RDS)
- **Lombok**
- **SpringDoc OpenAPI** (documentación Swagger)
- **Maven**

## Arquitectura y Despliegue

El servicio se encuentra dockerizado y alojado en **Amazon ECR**. Su ejecución es orquestada por **Amazon ECS (Fargate)**, permitiendo un despliegue Serverless con alta disponibilidad. Se conecta a una base de datos **MySQL en Amazon RDS** y se comunica con el Frontend a través de un Application Load Balancer (ALB).

El servicio corre en el puerto **8083**.

## Pipeline CI/CD automatizado

Mediante GitHub Actions, cualquier cambio subido a la rama `main` activa un pipeline que:

1. Autentica de forma segura con AWS (vía Secrets).
2. Construye la imagen Docker desde la subcarpeta `Springboot-API-REST`.
3. Sube la imagen actualizada a Amazon ECR (`innovatech/ventas`).
4. Actualiza el servicio en ECS Fargate (`ventas-service`) sin interrupción del servicio.

## Modelo de datos: Venta

| Campo | Tipo | Descripción |
|---|---|---|
| `idVenta` | Long | Identificador único, autogenerado |
| `direccionCompra` | String | Dirección asociada a la compra (obligatorio) |
| `valorCompra` | int | Monto de la venta |
| `fechaCompra` | LocalDate | Fecha de la compra (obligatorio) |
| `despachoGenerado` | Boolean | Indica si ya se generó el despacho asociado |

## Endpoints Principales

Base path: `/api/v1/ventas`

| Método | Endpoint | Descripción |
|---|---|---|
| POST | `/api/v1/ventas` | Crear una nueva venta |
| GET | `/api/v1/ventas` | Obtener todas las ventas |
| GET | `/api/v1/ventas/{idVenta}` | Obtener una venta por ID |
| PUT | `/api/v1/ventas/{idVenta}` | Actualizar una venta existente |
| DELETE | `/api/v1/ventas/{idVenta}` | Eliminar una venta |

Documentación Swagger/OpenAPI disponible en la ruta: `/swagger-ui.html`.

## Manejo de errores

El servicio implementa manejo centralizado de excepciones (`RestResponseEntityExceptionHandler`), incluyendo el caso `VentaNotFoundException` cuando se solicita una venta con un ID inexistente.
