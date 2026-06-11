# Configuracion de Firebase

La aplicacion usa Firebase Authentication con Google, Cloud Firestore para los
datos y Firebase Hosting para publicarla. Todos los usuarios autenticados
comparten la misma cartera de facturas y fichas de clientes.

## 1. Crear el proyecto

1. Entra en https://console.firebase.google.com/.
2. Pulsa **Crear un proyecto** y asignale un nombre, por ejemplo
   `impagados-playoff`.
3. Analytics es opcional.

## 2. Registrar la aplicacion web

1. En la vista general del proyecto, pulsa el icono **Web** (`</>`).
2. Pon un nombre a la aplicacion y registrala.
3. Firebase mostrara un objeto `firebaseConfig`.
4. Copia sus valores en `firebase-config.js`, respetando los nombres.

La configuracion web no es una clave secreta. La seguridad real esta en las
reglas de Firestore y en la autenticacion.

## 3. Activar el acceso con Google

1. Abre **Authentication** > **Sign-in method**.
2. Activa el proveedor **Google**.
3. Selecciona un correo de asistencia y guarda.

Cuando uses un dominio propio, agregalo tambien en **Authentication** >
**Settings** > **Authorized domains**.

## 4. Crear Firestore

1. Abre **Firestore Database** y pulsa **Create database**.
2. Elige una region europea cercana.
3. Puedes iniciar en modo produccion.

Las reglas incluidas en `firestore.rules` permiten que todos los usuarios
autenticados accedan al espacio compartido de Control de Impagados.

## 5. Instalar Firebase CLI

Necesitas Node.js instalado. Desde una terminal:

```powershell
npm install -g firebase-tools
firebase login
```

## 6. Vincular y publicar

Ejecuta estos comandos dentro de esta carpeta:

```powershell
firebase use --add
firebase deploy
```

En `firebase use --add`, selecciona el proyecto creado y asignale el alias
`default`. El despliegue publica Hosting y las reglas de Firestore.

Firebase devolvera una URL similar a:

```text
https://impagados-playoff.web.app
```

## Desarrollo local

Los modulos de Firebase no funcionan bien abriendo `index.html` con doble
clic. Sirve la carpeta por HTTP:

```powershell
firebase emulators:start --only hosting
```

Tambien puedes usar cualquier servidor estatico local.

## Datos

- Facturas: `workspaces/control-impagados/invoices`
- Fichas: `workspaces/control-impagados/clients`

Cada nueva importacion de Excel reemplaza las facturas compartidas. Las fichas
de clientes se conservan y los cambios se sincronizan en tiempo real.
