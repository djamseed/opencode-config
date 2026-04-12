# LikeC4 Architecture-as-Code Standards

| Component         | Usage                                                         |
| :---------------- | :------------------------------------------------------------ |
| **Specification** | Define global `element` types and `relationship` styles.      |
| **Model**         | The hierarchical source of truth for the system.              |
| **Views**         | Perspective-based diagrams (Landscape, Container, Component). |
| **Deployment**    | Mapping containers to physical nodes/environments.            |

## Syntax Conventions

- **Nesting**: Use curly braces to define child elements (e.g., `system` contains `container`).
- **Tags**: Use tags (e.g., `#frontend`, `#external`) to allow for dynamic filtering in views.
- **Relational Labels**: Every arrow `->` must have a description (e.g., `-> api 'POST /v1/orders'`).

## Modeling Hierarchy

| Level         | Scope                                                              |
| :------------ | :----------------------------------------------------------------- |
| **System**    | High-level business boundary (e.g., "Payment Gateway").            |
| **Container** | Deployable unit (e.g., "Go API", "Postgres DB").                   |
| **Component** | Internal structural logic (e.g., "Order Service", "Auth Handler"). |
