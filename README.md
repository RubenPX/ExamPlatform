# ExamPlatform - Monolito Modular en C#

Plataforma educativa para evaluar exámenes con una arquitectura de monolito modular implementada en .NET 10.

## 📋 Descripción

ExamPlatform es una plataforma de evaluación de exámenes que implementa una arquitectura de **monolito modular**. El sistema está dividido en módulos independientes que colaboran entre sí pero mantienen límites claros de responsabilidad.

### Roles
- **Profesor**: Crear exámenes, calificar, invitar alumnos
- **Alumno**: Aceptar invitaciones, realizar exámenes

## 🏗️ Arquitectura

### Estructura de Módulos

```
ExamPlatform/
├── src/
│   ├── ExamPlatform.Core/              # Núcleo - Entidades y interfaces compartidas
│   ├── ExamPlatform.TeacherModule/     # Módulo de Profesores
│   ├── ExamPlatform.StudentModule/     # Módulo de Estudiantes
│   ├── ExamPlatform.ExamModule/        # Módulo de Exámenes
│   ├── ExamPlatform.NotificationModule/# Módulo de Notificaciones
│   ├── ExamPlatform.Api/               # API REST
│   └── ExamPlatform.Infrastructure/    # Infraestructura compartida
└── tests/
    └── ExamPlatform.Tests/
```

## 🎯 Características

### Módulo de Exámenes (ExamModule)
- Crear exámenes con preguntas
- Gestionar preguntas (opción múltiple, verdadero/falso, respuesta abierta)
- Publicar/despublicar exámenes
- Versioning de exámenes

### Módulo de Profesores (TeacherModule)
- Crear y editar exámenes
- Invitar estudiantes a exámenes
- Revisar respuestas de estudiantes
- Calificar exámenes
- Ver estadísticas

### Módulo de Estudiantes (StudentModule)
- Aceptar/rechazar invitaciones
- Realizar exámenes
- Ver resultados
- Ver retroalimentación

### Módulo de Notificaciones (NotificationModule)
- Notificar invitaciones a estudiantes
- Notificar calificaciones
- Notificar cambios en exámenes

## 🔄 Patrones de Comunicación

- **MediatR**: Para solicitudes entre módulos
- **Domain Events**: Para eventos de dominio
- **Repository Pattern**: Para acceso a datos

## 🛠️ Tecnologías

- **.NET 10**
- **C#**
- **Entity Framework Core** (ORM)
- **MediatR** (Mediator Pattern)
- **AutoMapper**
- **Fluent Validation**
- **xUnit** (Testing)
- **Moq** (Mocking)

## 📦 Instalación

### Requisitos
- .NET 10 SDK
- Visual Studio 2024 o VS Code

### Clonar y ejecutar

```bash
git clone https://github.com/RubenPX/ExamPlatform.git
cd ExamPlatform
dotnet restore
dotnet build
dotnet run --project src/ExamPlatform.Api
```

## 📝 Estructura de Proyectos

### ExamPlatform.Core
Contiene las abstracciones y entidades base del sistema.

### ExamPlatform.TeacherModule
Lógica de negocio relacionada con profesores.

### ExamPlatform.StudentModule
Lógica de negocio relacionada con estudiantes.

### ExamPlatform.ExamModule
Gestión de exámenes y preguntas.

### ExamPlatform.NotificationModule
Sistema de notificaciones.

### ExamPlatform.Infrastructure
Implementaciones de repositorios, DbContext y servicios compartidos.

### ExamPlatform.Api
API REST que expone los endpoints públicos.

## 🔌 Endpoints API (Ejemplos)

### Profesores
- `POST /api/teachers/exams` - Crear examen
- `GET /api/teachers/exams/{examId}` - Obtener examen
- `POST /api/teachers/exams/{examId}/invite` - Invitar estudiantes
- `GET /api/teachers/exams/{examId}/submissions` - Ver entregas
- `POST /api/teachers/submissions/{submissionId}/grade` - Calificar

### Estudiantes
- `GET /api/students/invitations` - Obtener invitaciones
- `POST /api/students/invitations/{invitationId}/accept` - Aceptar invitación
- `GET /api/students/exams` - Obtener mis exámenes
- `POST /api/students/exams/{examId}/start` - Iniciar examen
- `POST /api/students/submissions/{submissionId}/submit` - Entregar examen
- `GET /api/students/submissions/{submissionId}` - Ver resultado

## 💡 Patrones Implementados

### 1. **Modular Monolith**
Cada módulo puede ser extraído a un microservicio en el futuro.

### 2. **CQRS**
Separación de comandos (escritura) y queries (lectura).

### 3. **Domain-Driven Design (DDD)**
Cada módulo tiene su propio bounded context.

### 4. **Dependency Injection**
Configuración centralizada en el API.

### 5. **Mediatr Handlers**
Desacoplamiento entre módulos.

## 📚 Guía de Desarrollo

### Crear un nuevo comando

```csharp
// En TeacherModule/Commands
public class CreateExamCommand : IRequest<ExamDto>
{
    public string Title { get; set; }
    public string Description { get; set; }
    public Guid TeacherId { get; set; }
}

public class CreateExamCommandHandler : IRequestHandler<CreateExamCommand, ExamDto>
{
    private readonly IExamRepository _repository;

    public CreateExamCommandHandler(IExamRepository repository)
    {
        _repository = repository;
    }

    public async Task<ExamDto> Handle(CreateExamCommand request, CancellationToken cancellationToken)
    {
        var exam = new Exam(request.Title, request.Description, request.TeacherId);
        await _repository.AddAsync(exam, cancellationToken);
        return new ExamDto { /* mapeo */ };
    }
}
```

### Configurar módulo en API

```csharp
// En Program.cs
services.AddTeacherModule();
services.AddStudentModule();
services.AddExamModule();
services.AddNotificationModule();
```

## 🧪 Testing

```bash
dotnet test
```

## 🚀 Próximos Pasos

- [ ] Implementar autenticación y autorización
- [ ] Agregar logging
- [ ] Implementar caché
- [ ] Agregar validaciones adicionales
- [ ] Documentación API con Swagger
- [ ] Integración continua (CI/CD)
- [ ] Migración a microservicios (opcional)

## 📄 Licencia

MIT License

## 👤 Autor

RubenPX