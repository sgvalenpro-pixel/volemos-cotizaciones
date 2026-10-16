# Cotizador Volemos (PWA + Firebase)

App web instalable para generar cotizaciones de Volemos LTDA.
Instalable, funciona offline y sincroniza el consecutivo y el historial
entre equipos mediante Firebase Realtime Database.

## Contenido
- index.html            App (config de Firebase al inicio del <script>)
- jspdf.umd.min.js       Generación de PDF (local, sin internet)
- manifest.webmanifest   Datos de la app instalable
- sw.js                  Service worker (caché offline)
- icons/                 Íconos de la app

## 1) Crear el proyecto Firebase (SEPARADO)
Debe ser un proyecto NUEVO, distinto de `visor-soya-alenpro` y `alenpro-inventario`.
1. console.firebase.google.com → "Agregar proyecto" → nombre sugerido: **cotizador-volemos**.
2. Compilación → Realtime Database → "Crear base de datos" (ubicación: us-central o la más cercana).
3. Compilación → (Project Overview) → ícono Web (</>) → registra una app web → copia el objeto `firebaseConfig`.
4. En `index.html`, reemplaza el bloque `FIREBASE_CONFIG = { ... }` con esa config.
   Mientras diga "PEGA_AQUI", la app trabaja en modo local.

## 2) Reglas de la base (Realtime Database → Reglas)
Aisladas a esta app. Para uso interno básico (lectura/escritura abierta):

    {
      "rules": {
        "config":        { ".read": true, ".write": true },
        "cotizaciones":  { ".read": true, ".write": true }
      }
    }

> Recomendado más adelante: proteger con Firebase Auth (correo/contraseña),
> igual que en Evaluación de Desempeño, cambiando ".read"/".write" a "auth != null".

## 3) Publicar en GitHub Pages
1. Sube TODA la carpeta al repositorio (raíz o subcarpeta).
2. Settings → Pages → Deploy from branch → main / (root).
3. Abre la URL (HTTPS). Aparecerá "Instalar".
> Al cambiar archivos, sube la versión en sw.js (CACHE: v1 → v2 …) para forzar actualización.

## Cómo funciona el consecutivo
- El número se reserva de forma ATÓMICA con una transacción sobre `config/nextConsecutivo`,
  así dos equipos nunca obtienen el mismo número.
- El badge del banner muestra el estado: En línea / Sin conexión / Modo local.
- Sin internet la app sigue funcionando y sincroniza al reconectar.

## Fotos en la cotización
- Botón "📷 Agregar fotos": en celular/tablet ofrece Cámara o Galería (iOS y Android); en PC abre el explorador.
- Se comprimen en el dispositivo antes de guardarse (lado máx. 1400 px), se guardan en Firebase junto a la cotización y salen en el PDF bajo "Anexo fotográfico".
- Tope de 12 fotos por cotización. Cada foto trae una ✕ para quitarla.

## Instalar
- Escritorio (Chrome/Edge): "Instalar" en la barra de direcciones o botón "Instalar app".
- Android (Chrome): menú ⋮ → "Agregar a pantalla de inicio".
- iPhone/iPad (Safari): Compartir → "Agregar a pantalla de inicio".
