# 📝 GUÍA COMPLETA DE MARKDOWN PARA GITHUB

## 📋 ÍNDICE
1. [Sintaxis Básica](#sintaxis-básica)
2. [Formato de Texto](#formato-de-texto)
3. [Listas](#listas)
4. [Enlaces e Imágenes](#enlaces-e-imágenes)
5. [Tablas](#tablas)
6. [Código](#código)
7. [**NOVEDADES DE GITHUB - Alertas/Notas**](#novedades-de-github-blockquotes-especiales)
8. [Características Especiales de GitHub](#características-especiales-de-github)
9. [Emojis](#emojis)
10. [Escape de Caracteres](#escape-de-caracteres)
11. [Ejemplos Prácticos](#ejemplos-prácticos)

---

## 📌 SINTaxis BÁSICA

### Encabezados (Headings)
```markdown
# Título nivel 1 (H1)
## Título nivel 2 (H2)
### Título nivel 3 (H3)
#### Título nivel 4 (H4)
##### Título nivel 5 (H5)
###### Título nivel 6 (H6)
```

# Título nivel 1
## Título nivel 2
### Título nivel 3

### Párrafos y saltos de línea
```markdown
Este es un párrafo normal.
Para saltar de línea se dejan dos espacios al final  
o se deja una línea en blanco entre párrafos.

Este es otro párrafo.
```

---

## ✨ FORMATO DE TEXTO

```markdown
**Texto en negrita**
__También negrita__
*Texto en cursiva*
_También cursiva_
***Texto negrita y cursiva***
~~Texto tachado~~
<sub>Texto subíndice</sub>
<sup>Texto superíndice</sup>
<ins>Texto subrayado</ins>
<mark>Texto resaltado</mark>
```

**Resultado:**
**Texto en negrita**  
*Texto en cursiva*  
***Texto negrita y cursiva***  
~~Texto tachado~~  
<sub>Texto subíndice</sub>  
<sup>Texto superíndice</sup>  
<ins>Texto subrayado</ins>  
<mark>Texto resaltado</mark>

---

## 📋 LISTAS

### Listas no ordenadas
```markdown
- Item 1
- Item 2
  - Subitem 2.1 (con 2 espacios)
  - Subitem 2.2
- Item 3

* También con asteriscos
* Otro item
  + Subitem con más
```

### Listas ordenadas
```markdown
1. Primer item
2. Segundo item
   1. Subitem indentado (3 espacios)
   2. Otro subitem
3. Tercer item
```

### Listas de tareas (GitHub)
```markdown
- [x] Tarea completada
- [ ] Tarea pendiente
- [ ] Otra tarea pendiente
- [x] ~~Tarea cancelada~~ (con tachado)
```

**Resultado:**
- [x] Tarea completada
- [ ] Tarea pendiente
- [ ] Otra tarea pendiente
- [x] ~~Tarea cancelada~~

---

## 🔗 ENLACES E IMÁGENES

### Enlaces
```markdown
[Texto del enlace](https://ejemplo.com)

[Texto con título](https://ejemplo.com "Título del enlace")

[Enlace a sección](#sintaxis-básica)

<https://automatico.com>

email@ejemplo.com

[Referencia][1]
[1]: https://ejemplo.com "Título opcional"
```

### Imágenes
```markdown
![Texto alternativo](https://url-de-imagen.com/imagen.jpg)

![Texto alternativo con título](url-imagen.jpg "Título")

[![Imagen como enlace](url-imagen.jpg)](https://enlace-destino.com)
```

---

## 📊 TABLAS

```markdown
| Encabezado 1 | Encabezado 2 | Encabezado 3 |
|--------------|:-------------|-------------:|
| Izquierda    | Centro       | Derecha      |
| Celda 1      | Celda 2      | Celda 3      |
| Texto largo  | **negrita**  | `código`     |

| Alineaciones | Con | :---: | :--- | ---: |
|--------------|-----|-------|------|------|
| Default      | Izquierda | Centro | Derecha |
```

**Resultado:**

| Encabezado 1 | Encabezado 2 | Encabezado 3 |
|--------------|:-------------:|-------------:|
| Izquierda    | Centro       | Derecha      |
| Celda 1      | Celda 2      | Celda 3      |

---

## 💻 CÓDIGO

### Código en línea
```markdown
`console.log("Hola mundo")`
```

### Bloques de código
````markdown
```javascript
function saludar(nombre) {
    console.log(`Hola ${nombre}`);
}
```

```python
def saludar(nombre):
    print(f"Hola {nombre}")
```

```bash
git add .
git commit -m "mensaje"
git push origin main
```

```diff
+ Línea agregada
- Línea eliminada
```
````

### Citas de código
```markdown
> Esto es una cita
> Puede tener múltiples líneas
> 
> > Citas anidadas
```

---

## 🚨 NOVEDADES DE GITHUB - BLOCKQUOTES ESPECIALES

Estos son los **nuevos formatos de alerta** que mencionaste (funcionan en GitHub):

```markdown
> [!NOTE]
> Useful information that users should know, even when skimming content.

> [!TIP]
> Helpful advice for doing things better or more easily.

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.
```

**Resultado:**

> [!NOTE]
> Useful information that users should know, even when skimming content.

> [!TIP]
> Helpful advice for doing things better or more easily.

> [!IMPORTANT]
> Key information users need to know to achieve their goal.

> [!WARNING]
> Urgent info that needs immediate user attention to avoid problems.

> [!CAUTION]
> Advises about risks or negative outcomes of certain actions.

---

## ⭐ CARACTERÍSTICAS ESPECIALES DE GITHUB

### 1. **Menciones a usuarios**
```markdown
@usuario
@organizacion/equipo
```

### 2. **Referencias a issues y PRs**
```markdown
#123
usuario/repo#456
```

### 3. **Referencias a commits**
```markdown
`a2b3c4d5`
usuario/repo@a2b3c4d5
```

### 4. **Emojis**
```markdown
:smile: :rocket: :fire: :bug: :sparkles:
```

**Resultado:** :smile: :rocket: :fire: :bug: :sparkles:

### 5. **Task lists avanzadas**
```markdown
- [ ] Tarea pendiente
- [x] Tarea completada
- [ ] **Tarea con formato** `código`
- [ ] @menciones funcionan
- [ ] #123 referencias también
```

### 6. **Diagramas Mermaid** (GitHub soporta)
````
```mermaid
graph TD;
    A-->B;
    A-->C;
    B-->D;
    C-->D;
```
````

### 7. **Footnotes (Notas al pie)**
```markdown
Texto con nota al pie[^1].

[^1]: Esta es la nota explicativa.
```

### 8. **Highlighting automático**
```markdown
=== Esto se resalta automáticamente ===
```

---

## 😊 EMOJIS ÚTILES PARA GITHUB

### Comunes en commits y READMEs:
```markdown
:sparkles:    # Nuevas características
:bug:         # Corrección de bugs
:books:       # Documentación
:rocket:      # Despliegue
:art:         # Mejoras de estilo
:zap:         # Mejoras de rendimiento
:fire:        # Eliminar código
:white_check_mark: # Tests
:lock:        # Seguridad
:recycle:     # Refactorización
:package:     # Dependencias
:construction: # En progreso
:ok_hand:     # Code review
:twisted_rightwards_arrows: # Merge
:heavy_check_mark: # Completado
:x:           # Error
:warning:     # Advertencia
:question:    # Pregunta
:bulb:        # Idea
:poop:        # Código mejorable
:green_heart: # CI pasando
:red_circle:  # CI fallando
```

---

## 🔧 ESCAPE DE CARACTERES

Para mostrar caracteres especiales, usa `\`:

```markdown
\*Esto no es cursiva\*
\# Esto no es encabezado
\[Esto no es enlace\]
```

---

## 📝 EJEMPLOS PRÁCTICOS

### README Completo Ejemplo:
````markdown
# Mi Proyecto 🚀

> [!NOTE]
> Este proyecto está en desarrollo activo

## 📦 Instalación

```bash
npm install mi-proyecto
```

## 🚀 Uso Rápido

```javascript
import { saludar } from 'mi-proyecto';

saludar('Mundo');
```

## ✨ Características

- [x] Feature 1 completa
- [ ] Feature 2 en progreso
- [ ] Feature 3 planeada

## 🤝 Contribuir

1. Fork el proyecto
2. Crea tu rama (`git checkout -b feature/Amazing`)
3. Commit cambios (`git commit -m 'Add some Amazing'`)
4. Push (`git push origin feature/Amazing`)
5. Abre un Pull Request

## ⚠️ Problemas Comunes

> [!CAUTION]
> No ejecutar en producción sin pruebas

> [!TIP]
> Usa `--save-dev` para dependencias de desarrollo

## 📊 Estado del Proyecto

| Rama | Estado | Tests |
|------|--------|-------|
| main | ✅ | ![Tests](https://img.shields.io/badge/tests-passing-brightgreen) |
| dev  | 🚧 | ![Tests](https://img.shields.io/badge/tests-pending-yellow) |

## 📝 Licencia

MIT © [Tu Nombre](https://github.com/tuusuario)
````

### Plantilla para Pull Request:
````markdown
## 📝 Descripción
<!-- Qué cambios introduces? -->

## ✅ Checklist
- [ ] Tests pasan localmente
- [ ] Documentación actualizada
- [ ] Código sigue estándares

> [!IMPORTANT]  
> Revisar cambios en la base de datos

## 🔍 Screenshots
<!-- Si aplica, agrega capturas -->
````

### Plantilla para Issues:
````markdown
### 🐛 Descripción del Bug
<!-- Qué está pasando? -->

### 🔄 Pasos para Reproducir
1. Ir a '...'
2. Click en '....'
3. Scroll a '....'
4. Ver error

> [!CAUTION]
> Esto afecta a usuarios en producción

### 📋 Comportamiento Esperado
<!-- Qué debería pasar? -->

### 📸 Screenshots
<!-- Si aplica -->
````

---

## 🎯 RESUMEN DE NOVEDADES GITHUB

### Las **ALERTAS** que mencionaste son lo más nuevo:
```markdown
> [!NOTE]     # Información general
> [!TIP]      # Consejos útiles
> [!IMPORTANT] # Información crítica
> [!WARNING]  # Urgente atención
> [!CAUTION]  # Riesgos potenciales
```

### Otras novedades:
- **Mermaid diagrams** nativos
- **Math expressions** con $$ 
- **Mejor soporte para tablas**
- **Auto-completado de @menciones**
- **Preview mejorado en tiempo real**

---

## 📚 CHEATSHEET RÁPIDO

```markdown
# Título
## Subtítulo
**negrita** *cursiva* ***ambas***
~~tachado~~ `código`

[Lista de tareas]
- [x] Hecho
- [ ] Pendiente

[Enlace](url) ![Imagen](url)

| Tabla | Columna |
|-------|---------|
| Dato  | Valor   |

> [!NOTE]
> Nota importante

:emoji: #123 @usuario

```código con sintaxis```
```

---

**¡Con esto ya puedes crear READMEs profesionales y aprovechar todas las características de GitHub Markdown!** 🎉
