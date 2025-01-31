 ![alt text](img/NombreEscuela.png)

<p align="center"> <b>Práctica 1</b>
</p><p align="center"> <b>Escuela:</b>
Instituto Tecnológico Superior de Irapuato </p>
<p align="center"> <b>Carrera:</b> Ingeniería en Sistemas Computacionales </p>
<p align="center"> <b>Integrantes:</b>
</p> <p align="center"> Arévalo Salinas Karina Janet IS21110440 <br>
 Díaz Zavala Daniel IS21111005 <br>
Hernández Vázquez Karen Daniela IS21110660 <br>
Campos Guevara Fabiola Margarita IS21110197 <br>
Ledesma Medina Jonathan IS21110030 <br>
Montenegro Guerrero Óscar IS21110922 <br>
Muñoz Olvera Francisco Gerardo IS21110228 <br>
Natal Velázquez Kimberly Michelle IS21110668 <br>
Villanueva Barbosa Jesús Ismael IS21110829 <br>
Vazquez García Nahum Josué IS21111282 </p>
<p align="center">
<b>Materia:</b>
Arquitectura y Diseño de Software </p>
<p align="center"> <b>Grupo:</b> ISCC70C </p><p align="center"> 
<b>SR DOCTOR MAESTRO PROFESOR INGENIERO:</b> Manuel Alejandro Guzmán Hernández </p>
<p align="center"> <b>Semestre:</b> Enero-Mayo 2025 </p>
<p align="center"> <b>Tema:</b> Dominancia Cerebral</p>

![alt text](img/Logo.jpg)

# Entendimiento del problema
La dominancia cerebral es muy útil en diversas evaluaciones en las que se necesita identificar la forma de trabajar y el desempeño en distintas habilidades, dependiendo del tipo de dominancia de cada persona. Por ello, conocer esta
información es valioso para considerar a alguien para un puesto específico o para entender las necesidades de los
alumnos y mejorar el desempeño en la relación profesor-alumno.

Sin embargo, obtener esta información de manera clara, precisa y no invasiva es poco común. Una página web que permita
crear un evento único para realizar encuestas a un grupo de personas determinadas, donde solo el creador tenga acceso
a los resultados y estos no puedan ser alterados, sería muy útil para los casos mencionados.

# Descomposición de los elementos del problema
- Interfaces 🫡
- Validación y Autenticación🔒
- Gestión de los datos ⚙️
- Cumplimiento de ISO 27001👩‍🔧
- Feedback🚨
- Interfaz de usuario 💻
- Interfaz para respuesta de cuestionario📃
- Interfaz de administrador📈
- Interfaces de respuesta de dominio cerebral🧠

# Diagrama de clases de la integración

```mermaid
classDiagram
    class Usuario {
        +Nombre
        +IdUsuario
        +Correo
        -Contraseña
        +verResultado()
        +contestarCuestionario()
        +verInformacion()
    }
    
    class Administrador {
        +Nombre
        +IdUsuario
        +Correo
        -Contraseña
        +crearEvento()
        +verResultados()
        +verEvento()
        +editarEvento()
        +verGrafico()
        +eliminarEvento()
    }
    
    class Evento {
        +IdEvento
        +FechaLimite
        -CantidadLimite
        +Link
        +Nombre
    }
    
    class Respuesta {
        +IdRespuesta
        +IdEvento
        +IdUsuario
        +Respuestas
        +TipoFinal
    }
    class Encuesta {
        +Preguntas
        +IdEvento
    }

    Usuario --> Encuesta : Contesta
    Encuesta --> Evento : Tiene
    Encuesta --> Respuesta : Genera
    Evento --> Administrador : Crea
```

# Diagramas UML
#### **1. Diagrama de Casos de Uso**
```mermaid
graph TD
    UsuarioFinal["Usuario Final"] --> |"Iniciar sesión"| Login["Login"]
    UsuarioFinal --> |"Registrarse"| Registro["Registro"]
    UsuarioFinal --> |"Recuperar contraseña"| Recuperar["Recuperar Contraseña"]
    UsuarioFinal --> |"Responder preguntas"| Responder["Responder Encuestas"]

    Administrador["Administrador"] --> |"Gestionar usuarios"| GestionUsuarios["Gestionar Usuarios"]
    Administrador --> |"Gestionar preguntas"| GestionPreguntas["Gestionar Preguntas"]
    Administrador --> |"Gestionar respuestas"| GestionRespuestas["Gestionar Respuestas"]
    Administrador --> |"Crear eventos"| CrearEvento["Crear Eventos"]

    UsuarioFinal --- Sistema["Sistema Web"]
    Administrador --- Sistema
```
#### **2. Diagrama de Base de Datos**

```mermaid
erDiagram
    eventos {
        INT id PK
        INT idEvento
        VARCHAR(50) nombre
        INT cantidadLimite
        VARCHAR(50) fechaLimite
        VARCHAR(100) enlace
    }

    usuarios {
        INT idUsuario PK
        VARCHAR(30) nombreUsuario
        VARCHAR(100) correoElectronico
        VARCHAR(100) contrasena
        VARCHAR(20) rolUsuario
    }

    respuestas {
        INT idRespuesta PK
        INT idEvento FK
        VARCHAR(100) usuarioRespuesta
        VARCHAR(100) respuesta
        INT tipoA
        INT tipoB
        INT tipoC
        INT tipoD
        VARCHAR(20) tipoFinal
    }

    usuarios ||--o{ respuestas : "contesta"
    eventos ||--o{ respuestas : "tiene"

```
#### **3. Diagrama de Despliegue**
```mermaid
graph TD
    subgraph "Cliente"
        navegador["Navegador Web"]
    end

    subgraph "Servidor Web"
        servidorWeb["Servidor Apache/Tomcat"]
    end

    subgraph "Servidor de Aplicaciones"
        backend["Aplicación Backend (Node.js, Java, Python, etc.)"]
    end

    subgraph "Base de Datos"
        database["Servidor MySQL/PostgreSQL"]
    end

    navegador -->|HTTP/HTTPS| servidorWeb
    servidorWeb -->|Comunicación API| backend
    backend -->|Consultas SQL| database
```

#### 4. Diagrama de Actividad

```mermaid
stateDiagram-v2
    [*] --> SolicitarDatos
    SolicitarDatos --> ValidarDatos
    ValidarDatos --> DatosInvalidos: Datos no válidos
    DatosInvalidos --> SolicitarDatos: Reintentar ingreso
    ValidarDatos --> GuardarUsuario: Datos válidos
    GuardarUsuario --> Confirmacion
    Confirmacion --> [*]

```
## 5. Diagrama de secuencia
### 1. login

```mermaid
sequenceDiagram
    participant Usuario
    participant FrontEnd
    participant Backend

    loop While
        Usuario->>FrontEnd: 1. Solicita credenciales
        Usuario->>FrontEnd: 2. Escribe credenciales
        FrontEnd-->>Usuario: 3. Comprueba credenciales no vacías
        FrontEnd-->>Usuario: 4. Feedback_error
    end

    FrontEnd->>Backend: 5. Manda credenciales
    alt Usuario y contraseña válidos
        Backend-->>FrontEnd: 6. Usuario y contraseña válidos
    else Usuario no válido
        Backend-->>FrontEnd: 7. Feedback_Error
    end

    alt Usuario existe?
        Backend-->>FrontEnd: 8. Usuario existe?
    else Usuario no encontrado
        Backend-->>FrontEnd: 9. Feedback_Error
    end

    alt Contraseña válida?
        Backend-->>FrontEnd: 10. Contraseña válida?
    else Contraseña incorrecta
        Backend-->>FrontEnd: 11. Feedback_Error
    end

    Backend-->>FrontEnd: 12. ¿Qué rol tiene el usuario?
    FrontEnd-->>Usuario: 13. Interfaz usuario o admin
```

### 2. Diagrama de Registro
### 3. Diagrama de Contestar cuestionario

```mermaid
flowchart TD
    %% Definición de objetos principales
    subgraph Usuario
        A[Inicio] --> B[Definir propósito del cuestionario]
        B --> C[Identificar público objetivo]
    end

    subgraph Cuestionario
        C --> D[Crear cuestionario]
        D --> E[Agregar instrucciones]
        E --> F[Seleccionar tipo de preguntas]
    end

    subgraph Pregunta
        F --> G{¿Qué tipo de preguntas usar?}
        G -- Abiertas --> H[Redactar preguntas abiertas]
        G -- Cerradas --> I[Redactar preguntas cerradas]
        G -- Mixtas --> J[Combinar preguntas abiertas y cerradas]
        H --> K[Validar preguntas]
        I --> K
        J --> K
    end

    subgraph Respuesta
        K --> L[Publicar y distribuir cuestionario]
        L --> M[Recopilar respuestas]
        M --> N[Analizar respuestas]
    end

    N --> O[Generar informe y conclusiones]
    O --> P[Fin]
```

### 4. Diagrama de Mostrar resultados

```mermaid
sequenceDiagram
    participant Usuario
    participant Frontend
    participant Backend

    Usuario->>Frontend: 1: Solicita resultado
    Frontend->>Backend: 2: Solicita respuestas

alt 3: Respuestas Válidas
    Backend-->>Frontend: 4. Si
    Frontend-->>Usuario: 5. Test Completado

else 
    Backend-->>Frontend: 6. No
    Frontend-->>Usuario: 7. Test Incompleto
end

Frontend->>Usuario: 2: Mostrar resultados

```

### 5. Diagrama de cerrar sesión

```mermaid
sequenceDiagram
    participant Usuario
    participant FrontEnd
    participant Backend

    loop While
        Usuario->>FrontEnd: Solicita credenciales
        Usuario->>FrontEnd: Escribe credenciales
        FrontEnd->>Backend: Comprueba credenciales no vacías
        alt Credenciales no vacías
            Backend->>FrontEnd: Feedback_error
        end
    end

    Usuario->>FrontEnd: Manda credenciales
    alt Usuario y contraseña válidos
        FrontEnd->>Backend: Usuario y contraseña válidos
    else Usuario no válido
        FrontEnd->>Usuario: Feedback_error
    end

    alt Usuario existe?
        Backend->>FrontEnd: Usuario existe
    else Usuario no encontrado
        FrontEnd->>Usuario: Feedback_error
    end

    alt Contraseña válida?
        Backend->>FrontEnd: Contraseña válida
    else Contraseña incorrecta
        FrontEnd->>Usuario: Feedback_error
    end

    Backend->>FrontEnd: ¿Qué rol tiene el usuario?
```

### 6. Diagrama de Modo Nocturno

```mermaid
sequenceDiagram
    participant Usuario
    participant FrontEnd

    Usuario->>FrontEnd: Hace clic en "Modo Nocturno"
    FrontEnd->>Usuario: Cambia a tema oscuro
    Usuario->>FrontEnd: Hace clic en "Modo Nocturno" nuevamente
    FrontEnd->>Usuario: Restaura tema claro
```

### 7. Diagrama de Crear evento
### 8. Diagrama de Ver gráfica

```mermaid
sequenceDiagram
    participant Usuario
    participant Frontend
    participant Backend

    Usuario->>Frontend: 1: Solicita gráfica de resultados
    Frontend->>Backend: 2: Verifica respuestas

    alt 3: Respuestas Válidas
        Backend-->>Frontend: 4. Sí
        Backend->>Backend: 5. Recopila respuestas del usuario
        Backend->>Backend: 6. Calcula porcentajes (A, B, C, D)
        Backend->>Backend: 7. Prepara datos para gráfica
        Backend-->>Frontend: 8. Envía datos procesados
        Frontend->>Frontend: 9. Usa librería de gráficas
        Frontend-->>Usuario: 10. Muestra gráfica con porcentajes (A, B, C, D)
        Frontend-->>Usuario: 11. Muestra lista de respuestas
    else
        Backend-->>Frontend: 12. No
        Frontend-->>Usuario: 13. Respuestas incompletas
    end

    Frontend->>Usuario: 14: Mostrar gráfica y resultados
```

### 9. Diagrama de Ver resultados

# Implementación de ISO 27001 en el proyecto

#### ¿Qué es la norma ISO 27001?
La norma ISO 27001 establece un sistema de gestión de seguridad de la información para proteger los datos.

#### Beneficios de la ISO 27001
- Garantiza la confidencialidad, integridad, disponibilidada y legalidad de los datos.
- Genera confianza entre clientes, proveedores y empleados.
- Ofrece herramientas para proteger la información contra ciberataques.

#### Proceso de implementación
1.	#### Análisis inicial
- Definición del alcance del SGSI de los resultados, datos personales de los usuarios y sistema backend.
- Identificación de partes interesadas (usuarios y administradores).
- Establecimiento de objetivos de seguridad para garantizar la privacidad de los usuarios y la protección de datos sensibles.

2.	#### Identificación de riesgos
- **Activos**: Datos personales de los usuarios y resultados de las encuestas.
- **Riesgos posibles:** Accesos no autorizados o pérdida de datos.
- **Evaluar el impacto:** Determina el daño que causaría cada riesgo.
- **Planificar el tratamiento de riesgos:** En caso de una perdidad de informacion, los datos seran evaluados para determinar si se debe mitigar o mejora.

3.	#### Definición de alcance
- **Política de privacidad de datos:** Los datos de los usuarios son almacenados en una base de datos, y se definen cómo se recopilan, almacenan y utilizan los datos del usuario.
- **Gestión de acceso:** Se definen roles y permisos para acceder al sistema (administradores y usuarios).
los **administradores** tienen acceso a todas las funciones del sistema como por ejmplo.

- **Gestión del Sistema:** Configurar, supervisar y mantener el sistema.
- **Control de Accesos:** Crear y gestionar las cuentas de usuarios, estableciendo los permisos necesarios.
- **Monitoreo de Datos:** Revisar y analizar el rendimiento del sistema para garantizar que funcione sin interrupciones.
- **Resolución de Problemas:** Solucionar errores técnicos que puedan surgir en la plataforma.
- **Actualización y Seguridad:** Implementar actualizaciones y medidas de seguridad para proteger los datos de los usuarios y los resultados del test.

Mientras que los **usuarios** solo tienen acceso a las funciones que les permiten realizar su trabajo por ejmplo.

- **Realización del Test:** Completar el test de dominancia cerebral respondiendo las preguntas de acuerdo con las instrucciones.
- **Consulta de Resultados:** Visualizar e interpretar los resultados obtenidos.
- **Registro y Acceso:** Crear una cuenta o iniciar sesión en la plataforma para acceder al test.
- **Descarga de Información:** Descargar reportes o análisis del test.
- **Control de incidentes:** En caso de que ocurra un incidente, se definen los procedimientos para identificar, responder y resolver incidentes de seguridad como por ejemplo backups de la base de datos, monitoreo de los logs, etc.

4.	#### Desarrollo de controles y políticas

**Controles técnicos:**

- Encriptación de datos, autenticación y autorización.
- Autenticación y autorización de usuarios.
- Capacitación en seguridad de la información.

**Controles organizativos:**

- Capacita a los colaboradores en seguridad de la información.

5.	#### Capacitación
- Integrar herramientas tecnológicas para monitorear y proteger los datos, como firewalls, software de análisis de vulnerabilidades.
6.	#### Auditorias internas
Realizacion de  auditorías internas para asegurarte de que los controles funcionan como se espera.
Realizacion de pruebas para identificar incidentes y realizar mejoras continuas en el SGSI.

# AOO

## Analisis de requerimientos:
### Definición del sistema en general

Sistema web que permite la administración de encuestas y los 4 tipos de dominancia cerebral de una persona mostrando sus resultados de manera grafíca y escrita, limitando el numero de personas con acceso a la encuesta a través de fecha de cierre y limite de respuestas, para esto el sistema generara de manera automatica un link unico el cual dara acceso a la encuesta correspondiente.

En relación al usuario, al momento del inicio de sesión este podra visualizar información correspondiente a cada tipo de dominancia cerebral y al finalizar la encuesta el sistema arrojara los resultados de su dominancia de manera grafíca y escrita. Cada usuario tendrá solo una oportunidad de responder la encuesta y que esta se registre en la base de datos de nuestro sistema web.

### Requisitos y partes del sistema

  
#### 1. **Interfaces** 🫡

- Inicio de sesión
- Registrar
- Recuperación de credenciales
- Administrador: 
    - Ventana Principal
    - Visualización de eventos
    - Creación de eventos
    - Sección de respuestas
    - Visualización de Grafícos
- Usuario: 
    - Ventana de bienvenida
    - Sección de resultados
    - Cuestionario
    - Descripción de loss tipos de dominancia


#### 2. **Validación y Autenticación**🔒
- Autorización al sistema mediante un modulo para registro de usuarios.
- Alertas por correos no validos,no existentes o por credenciales vacias.
-  Protección contra SQL injection.
-  Limite de una respuesta por usuario a un evento.
-  Solo debe haber una opción seleccionada por respuesta
-  La encuesta solo puede ser contestada mientras este disponible
-  Creación de eventos en una fecha valida.
-  Se debe iniciar sesión para acceder al sistema.

#### 3. **Gestión de los datos**⚙️
- Base de datos segura para el almacenamiento de información mediante encriptación de datos.

#### 4. **Retroalimentación al usuario**🚨
- Envio de alertas o notificaciones cada vez que sea necesario.

#### 6. **Funciones de la seccion de usuario** 💻
- Función para contestar un formulario
- Función para cerrar sesión
- Seccion de informacion de los 4 tipos de dominancia cerebral
- Funcion para conocer sus resultados

#### 8. **Funciones de la seccion administrador**📈
- Función para crear,editar,desactivar un evento
- Función para ver las respuestas de un evento
- Funcion para ver las respuestas globales


## Diseño: 
**Arquitectura**
Al aplicar la arquitectura cliente-servidor a nuestro proyecto, podemos estructurarlo de la siguiente manera:

**Cliente (Navegador Web):**
El usuario interactúa con la aplicación a través de una interfaz desarrollada con tecnologías como HTML, CSS
y JavaScript. Las principales funciones que realiza el cliente incluyen:

- Registro de usuario.
- Envío de solicitud de inicio de sesión.
- Respuesta al cuestionario.
- Visualización de resultados en formato gráfico.
- (Para administradores) Creación de eventos.
- (Para administradores) Consulta de resultados generales.

Además, el cliente recibe datos del servidor en formato JSON y los muestra al usuario de manera dinámica.

**Servidor (Backend):**
El servidor maneja la lógica principal de la aplicación utilizando PHP e incluye las siguientes responsabilidades:
- Gestión de usuarios (registro y autenticación).
- Almacenamiento y procesamiento de respuestas del cuestionario.
- Generación y visualización de resultados.
- Creación de gráficos con la librería Chart.js.
- Gestión integral de eventos (creación, edición y acceso).
- Validación y control de datos.

**Base de Datos:** 
El servidor interactúa con una base de datos MySQL para almacenar y gestionar la siguiente información:
- Datos de los usuarios.
- Preguntas del cuestionario.
- Opciones de respuesta.
- Resultados de los tests.
- Información sobre los eventos creados.









 