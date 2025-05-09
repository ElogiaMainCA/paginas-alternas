# 🔎 Auditoría de Etiquetas Canónicas en Páginas Alternas (Home, Landings, etc.) y Gestión vía robots.txt

## 🎯 Objetivo

Garantizar que todas las páginas clave del sitio (**Home**, **Landings**, **PLPs**, **PDPs**) utilicen correctamente una sola etiqueta canónica válida y que **las versiones alternas o duplicadas sean bloqueadas** desde el archivo `robots.txt` para optimizar la indexación.

---

## 🧩 Descripción del Problema

Durante la auditoría técnica de **https://www.cyamoda.com**, se identificó que:

- Algunas páginas (como landings o variaciones con parámetros UTM, `?cid=`, `?utm_source=`, etc.) **no declaran una etiqueta canónica correcta** o **carecen de ella**.
- Existen **variantes accesibles vía diferentes rutas**, como:
  - `/home`
  - `/default`
  - `/landingpage?cid=...`
- Estas variantes **pueden ser indexadas por error** en Google y diluir la autoridad del contenido principal.

---

## 📌 Ejemplos Detectados

| Página | Estado de Canonical | Observaciones |
|--------|---------------------|---------------|
| `/` (Home) | ✅ Correcta | Canonical bien configurada |
| `/home` | ⚠️ Incorrecta o ausente | Debe redirigir o usar misma canonical que `/` |
| `/landingpage?cid=x` | ❌ No canonical | Puede ser indexada por Google |
| `/mujer.html?utm_source=...` | ⚠️ Duplicado con canonical inconsistente | Necesita consolidación |

---

## ⚠️ Impacto SEO

- **Dispersión de autoridad** entre URLs alternas.
- **Indexación innecesaria** de URLs con parámetros.
- **Contenido duplicado** interno.
- **Confusión de señales** para los motores de búsqueda.
- **Reducción del crawl budget**, especialmente en sitios con alta paginación.

---

## ✅ Recomendaciones

### 1. Unificación de Canonicals

Asegurar que cada página tenga **una sola etiqueta canónica declarada en `<head>`**, y que esta:

- Apunte a la **versión limpia y amigable** de la URL.
- **No incluya parámetros** (`cid`, `utm_`, `gclid`, etc.).
- Sea generada **por el backend** (SFCC/Demandware), no por scripts.

**Ejemplo correcto para Home:**

html
<link rel="canonical" href="https://www.cyamoda.com/" />



-------------


2. Revisión de Canonicals en Plantillas de Landings
Revisar templates .isml utilizados para landings y banners temporales.
Asegurar que el canonical:
Apunte a la URL sin parámetros.
Se genere desde el controller si es dinámica.
3. Redirecciones 301 (Opcional)
Redirigir variantes como /home → /
Redirigir landings caducas → categoría principal o /
🔒 Gestión con Robots.txt

Objetivo:
Evitar que variantes de URLs no canónicas sean rastreadas por Google.

Propuesta de líneas en robots.txt:
# Bloqueo de URLs con parámetros innecesarios
Disallow: /*?cid=
Disallow: /*?utm_
Disallow: /*?gclid
Disallow: /*?fbclid
Disallow: /home
Disallow: /default
Disallow: /landingpage
Notas:

Se puede utilizar el Disallow: /*? para bloquear todos los parámetros si es seguro hacerlo.
Es importante mantener en producción un robots.txt bien probado para no bloquear rutas necesarias.
🧪 Verificación Técnica

Herramientas sugeridas:
Google Search Console → Inspección de URLs indexadas
Screaming Frog SEO Spider (modo JS render) → Identificación de canonicals
Ahrefs / SEMrush → Auditoría de contenido duplicado y canonicals
DevTools + JavaScript habilitado → Validar DOM final (document.querySelectorAll('link[rel="canonical"]'))
Comandos de Google para verificación:
site:cyamoda.com inurl:cid=
site:cyamoda.com inurl:utm_
site:cyamoda.com inurl:landingpage
📅 Plan de Acción Recomendado

Fase	Descripción	Duración estimada
Mapeo de URLs duplicadas	Usar GSC + Ahrefs para detectar páginas alternas indexadas	1 día
Auditoría de canonicals	Validar configuración de etiquetas en Home, PLP, PDP, Landings	2 días
Configuración de robots.txt	Incluir reglas y validarlas con robots.txt tester	1 día
QA + Redirecciones	Verificar variantes activas y si es mejor redirigir	1-2 días
Implementación	Push a producción	1 día
Monitoreo post-lanzamiento	Validar cambios en indexación y cobertura de GSC	7 días
🧠 Conclusión

Establecer una estrategia clara para canonicals en páginas alternas y complementar con reglas en robots.txt es esencial para:

Consolidar autoridad.
Mejorar el posicionamiento de URLs prioritarias.
Evitar duplicidad de contenido.
Optimizar la eficiencia del rastreo (crawl budget).
Recomendamos realizar este ajuste antes del siguiente cambio de temporada o campaña de alto tráfico.
