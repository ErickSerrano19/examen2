# IF0009 · Examen II · Préstamos Móvil

App móvil **Ionic 9 + Capacitor 8 + Vue 3** que consume la API de *Préstamo de Equipos* del Examen I.

```
backend/   API Laravel 13 ya terminada (la solución del Examen I). No se modifica.
app/       App móvil. Aquí están los TODO M1–M8.
```

## Puesta en marcha (≈ 15 min)

Requisitos: PHP 8.4+, Composer, Node.js 22+, Android Studio con un emulador (API 24+), `npm i -g @ionic/cli`.

Notas de entorno en esta máquina (Windows):

- Use la terminal de **Laragon** para la API: `php` no está en el PATH de WSL. En PowerShell, agregue
  `C:\laragon\bin\php\php-8.4.12-nts-Win32-vs17-x64` al PATH.
- Gradle 8.14.3 no acepta el JBR 25 de Android Studio (`Unsupported class file major version 69`).
  Seleccione JDK 24 en la misma terminal antes de los comandos Capacitor/Gradle:

  ```powershell
  $env:JAVA_HOME = "C:\Program Files\Java\jdk-24"
  $env:PATH = "$env:JAVA_HOME\bin;$env:PATH"
  ```

  En Android Studio: `Settings → Build, Execution, Deployment → Build Tools → Gradle → Gradle JDK = JDK 24`.
- Inicie el AVD (`Medium_Phone_API_37.0`) en Device Manager antes de `npx cap run android`.
- `app/.env` ya existe: no vuelva a ejecutar `cp .env.example .env` después de cambiar `VITE_API_URL`.

```bash
# 1. API  →  http://localhost:8000/api
cd backend
composer run setup
php artisan serve

# 2. App en el navegador  →  http://localhost:8100   (otra terminal)
cd app
npm install
cp .env.example .env
ionic serve                 # o: npm run dev

# 3. App en el emulador de Android
#    en app/.env:  VITE_API_URL=http://10.0.2.2:8000/api
#    en PowerShell, usar JDK 24 (ver notas arriba) y tener el AVD iniciado
$env:JAVA_HOME = "C:\Program Files\Java\jdk-24"
$env:PATH = "$env:JAVA_HOME\bin;$env:PATH"
ionic build
npx cap sync android
npx cap run android         # o: npx cap open android  y Run en Android Studio

# Si la API no levanta: API simulada (mismo contrato)  →  http://localhost:3001/api
npm run mock                # en .env: VITE_API_URL=http://localhost:3001/api  (emulador: http://10.0.2.2:3001/api)
```

Credenciales: `docente@ucr.ac.cr` · `Examen2026!`

> Cada vez que cambie algo en `src/` y quiera verlo en el emulador: `ionic build` → `npx cap sync android` → `npx cap run android`.
> Verificación rápida sin emulador: `npm run build && npm run typecheck` (no hay scripts `lint` ni `test`).

## Qué hay que hacer

| TODO | Archivo | Pts |
|---|---|---|
| M1 | `src/services/api.ts` | 8 |
| M2 | `src/composables/useAuth.ts` | 10 |
| M3 | `src/router/index.ts` | 4 |
| M4 | `src/views/EquiposPage.vue` (listado) | 10 |
| M5 | `src/views/EquipoFormPage.vue` | 10 |
| M6 | `src/views/EquiposPage.vue` (eliminación) | 8 |
| M7 | `src/composables/useConexion.ts`, `src/views/PerfilPage.vue`, `android/app/src/main/AndroidManifest.xml` | 6 |
| M8 | `capacitor.config.ts` y lo que corresponda en `android/` + evidencia | 4 |

Cada comentario `TODO` describe exactamente lo que se espera. Los archivos marcados **YA IMPLEMENTADO** no se evalúan.

## Evidencia (M8)

Guarde en `app/docs/evidencia-android.png` una captura de **su** app corriendo en el emulador,
en la pestaña **Perfil** (debe verse *Plataforma: android*), y complete:

- Carné:
- appId:
- Versión de Node / Ionic CLI / Capacitor (`node -v`, `ionic -v`, `npx cap -V`):
- ¿Se generó el APK de depuración? (sí/no):

## Reglas

- IA permitida en la Parte A. **Usted responde por cada línea de su repositorio**, la haya escrito usted o una herramienta: la Parte B pregunta por *su* código.
- Se califica el último *commit* enviado antes de las **11:00**.
- No modifique `backend/` ni los archivos marcados YA IMPLEMENTADO.
