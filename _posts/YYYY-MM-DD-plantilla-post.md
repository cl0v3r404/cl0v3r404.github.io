---
title: Título del Post
date: YYYY-MM-DD HH:MM:SS +0000
categories: [Categoría Principal, Subcategoría]
tags: [etiqueta1, etiqueta2, etiqueta3]
description: Una breve descripción del contenido del post para SEO y vista previa.
image:
  path: /ruta/a/imagen.jpg
  lqip: data:image/webp;base64,UklGRiYAAABXRUJQ... # opcional, para carga progresiva
  alt: Descripción alternativa de la imagen
---

## Introducción

Aquí comienza tu contenido. Este párrafo puede ser una introducción al tema que vas a tratar.

## Sección 1

Contenido de la primera sección principal.

### Subsección

Detalles adicionales aquí.

## Sección 2

Más contenido organizado en secciones claras.

## Conclusión

Resumen o reflexión final sobre el tema.

---

## Notas útiles para escribir posts

### Formato de la fecha
- Usa formato `YYYY-MM-DD HH:MM:SS +0000`
- Ejemplo: `2026-05-07 14:30:00 +0200`

### Categorías
- Máximo 2 niveles recomendado
- La primera es la principal, la segunda es una subcategoría

### Etiquetas
- Usa etiquetas cortas y descriptivas
- Sepáralas por comas

### Imagen destacada
- `path`: ruta relativa a la imagen
- `lqip`: (opcional) versión borrosa para carga progresiva
- `alt`: siempre incluye texto alternativo para accesibilidad

### Sintaxis Markdown
- `# Título 1`, `## Título 2`, `### Título 3`, etc.
- `**negrita**`, `*cursiva*`, `` `código inline` ``
- ` ``` código ``` ` para bloques de código
- `> cita` para citas
- `- lista` para listas sin orden
- `1. lista` para listas numeradas
- `[enlace](url)` para enlaces

### Front matter opcional
```yaml
author: Nombre del autor # Si tienes múltiples autores
pin: true # Para fijar el post en la portada
math: true # Si usas ecuaciones matemáticas
mermaid: true # Si usas diagramas Mermaid
comments: true # Para habilitar comentarios (por defecto activado)
```

### Ejemplo con más opciones
```yaml
---
title: Mi Primer Post
date: 2026-05-07 10:00:00 +0200
categories: [Desarrollo, Web]
tags: [jekyll, blog, tutorial]
description: Aprende a crear tu primer post en Jekyll con Chirpy.
image:
  path: /assets/img/posts/2026-05-07-primer-post.jpg
  alt: Captura de pantalla del post
author: Tu Nombre
pin: false
math: false
mermaid: false
---
```

## Instrucciones para usar esta plantilla

1. Copia este archivo
2. Renómbralo como `YYYY-MM-DD-tu-titulo.md` (ej: `2026-05-07-mi-primer-post.md`)
3. Reemplaza el contenido del front matter y el cuerpo
4. Guarda en la carpeta `_posts/`
5. Haz push al repositorio
6. El sitio se compilará automáticamente