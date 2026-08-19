# 📊 Protocolo de Gestión Multi-Proyecto (Ecosistema)

Esta guía define los estándares operativos para gestionar, escalar y mantener múltiples aplicaciones web y PWAs (ej. Barodria, Frecuenciiaa, Juanapp) centralizadas en torno a un sistema de diseño único (UI Kit Maestro).

---

## 1. Estandarización del Backlog (El Ecosistema)

Para evitar silos de información y cuellos de botella, todas las tareas, bugs y nuevas funcionalidades de todos los proyectos deben vivir en un **único tablero centralizado** (GitHub Projects, Linear o Notion).

*   **Iniciativas / Epics:** Cada repositorio o aplicación se trata como una Épica principal. 
    *   `[BAR]` Barodria PWA
    *   `[FRE]` Frecuenciiaa
    *   `[JUA]` Juanapp
    *   `[UI]` Kit Maestro
*   **Etiquetas Transversales (Tags):** Utiliza un sistema de etiquetas global: `Frontend`, `Backend/DB`, `Bug`, `UI-Kit-Update`. 
*   **Gestión de Dependencias:** Si un proyecto requiere un componente nuevo, la tarea se asigna primero al Epic `[UI]` y se marca como bloqueante para la aplicación final.

---

## 2. Versionado Semántico y Release Notes

Dado que el `UI Kit Maestro` se comparte mediante Git Submodules, cualquier cambio impacta el ecosistema completo. Es obligatorio mantener un archivo `CHANGELOG.md` en el repositorio central y aplicar versionado semántico (SemVer):

*   **Mayor (Major - X.0.0):** Cambios estructurales que rompen la compatibilidad en las aplicaciones que lo consumen (ej. cambiar o eliminar props obligatorios en un componente base).
*   **Menor (Minor - 0.X.0):** Funcionalidades nuevas retrocompatibles (ej. añadir un nuevo componente `<Calendar>` o una nueva variante a un `<Button>`).
*   **Parche (Patch - 0.0.X):** Correcciones de bugs o ajustes de diseño menores que no alteran la API del componente (ej. ajustes de padding o colores semánticos).

---

## 3. Flujo de Trabajo (Feature-Driven Development)

Para construir una nueva funcionalidad (Feature) en cualquiera de las aplicaciones, el ciclo de desarrollo estricto es el siguiente:

1.  **Capa de Datos:** Definir el modelo, tipos y relaciones en la base de datos (PostgreSQL/Supabase).
2.  **Capa de Diseño (UI Kit):** Evaluar si los componentes visuales necesarios ya existen en el repositorio central. Si faltan, desarrollarlos y hacer push al `ui-kit-maestro`.
3.  **Capa de Integración:** Ensamblar la vista en la aplicación correspondiente, consumiendo el componente centralizado (mediante Git pull en el submódulo).
4.  **Pruebas y Despliegue:** Validar la interacción completa y enviar a producción.
