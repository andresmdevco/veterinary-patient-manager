# 🐾 Control de Pacientes Veterinarios
Aplicación web construida con **React**, **TypeScript** y **Tailwind CSS** para el seguimiento de pacientes de una veterinaria. Permite registrar, editar y eliminar pacientes con sus datos y síntomas, con estado global gestionado mediante **Zustand** y persistencia automática entre sesiones.

## 🌐 Demo
🔗 [https://veterinary-patient-manager.vercel.app/](https://veterinary-patient-manager.vercel.app/)

## 🛠️ Tecnologías Utilizadas
![React](https://img.shields.io/badge/React-61DAFB?style=for-the-badge&logo=react&logoColor=black)
![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=for-the-badge&logo=typescript&logoColor=white)
![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-38B2AC?style=for-the-badge&logo=tailwind-css&logoColor=white)
![Vite](https://img.shields.io/badge/Vite-646CFF?style=for-the-badge&logo=vite&logoColor=white)
- React 19
- Tailwind CSS 4
Además:
- **Zustand** — manejo del estado global, con los middlewares `devtools` y `persist`
- **react-hook-form** — manejo y validación del formulario de pacientes
- **react-toastify** — notificaciones de éxito/error al registrar, actualizar o eliminar
- **uuid** — generación de identificadores únicos para cada paciente

## ✨ Características
- ➕ Registro de pacientes con nombre, propietario, email, fecha de alta y síntomas.
- ✅ Validación de campos obligatorios y formato de email mediante expresión regular.
- ✏️ Edición de pacientes existentes, precargando sus datos en el formulario.
- 🗑️ Eliminación de pacientes desde el listado.
- 🔔 Notificaciones toast al registrar, actualizar o eliminar un paciente.
- 📋 Listado de pacientes con mensaje de estado vacío cuando no hay registros.
- 💾 Persistencia automática de los pacientes entre sesiones.
- 📱 Diseño responsive con Tailwind CSS.

## 📂 Archivos principales
| Archivo | Descripción |
|---|---|
| `store.ts` | Store de Zustand (`usePatientStore`) envuelto en los middlewares `devtools` y `persist`. Define `patients`, `activeId` y las acciones `addPatient`, `deletePatient`, `getPatientById` y `updatePatient` |
| `App.tsx` | Componente raíz. Renderiza `PatientForm`, `PatientsList` y el `ToastContainer` de las notificaciones |
| `components/PatientForm.tsx` | Formulario de registro/edición construido con `react-hook-form`. Precarga los datos del paciente activo (`activeId`) para edición y dispara `addPatient` o `updatePatient` según corresponda |
| `components/PatientsList.tsx` | Lee `patients` del store y renderiza un `PatientDetails` por cada uno, o un mensaje de estado vacío |
| `components/PatientDetails.tsx` | Tarjeta con los datos de un paciente. Botón "Editar" dispara `getPatientById` y botón "Eliminar" dispara `deletePatient` con notificación toast |
| `components/PatientDetailItem.tsx` | Componente reutilizable que muestra una etiqueta (`label`) junto a su valor (`data`) |
| `components/Error.tsx` | Componente reutilizable para mostrar mensajes de error de validación del formulario |
| `types/index.ts` | Tipos `Patient` y `DraftPatient` (`Omit<Patient, 'id'>`) compartidos por la app |

## 🧠 Cómo funciona
1. `usePatientStore` inicializa el estado (`patients: []`, `activeId: ''`) y lo persiste en `localStorage` bajo la clave `patient-storage` gracias al middleware `persist`.
2. `PatientForm` usa `react-hook-form` (`register`, `handleSubmit`, `formState.errors`) para capturar y validar los datos del paciente antes de enviarlos.
3. Si `activeId` está vacío, al enviar el formulario se dispara `addPatient`, que crea el paciente con un `id` generado por `uuid` y lo agrega al store.
4. `PatientsList` lee `patients` del store y renderiza un `PatientDetails` por cada paciente registrado; si no hay ninguno, muestra el mensaje de estado vacío.
5. Al presionar "Editar" en `PatientDetails`, se dispara `getPatientById`, que guarda el `id` del paciente en `activeId`.
6. Un `useEffect` en `PatientForm` detecta el cambio de `activeId` y precarga sus datos en el formulario con `setValue`, cambiando el texto del botón a "Actualizar Paciente".
7. Al reenviar el formulario en modo edición, se dispara `updatePatient`, que reemplaza al paciente con `id === activeId` y limpia `activeId`.
8. Al presionar "Eliminar", se dispara `deletePatient`, que filtra al paciente del array `patients`.
9. Cada acción exitosa (registrar, actualizar, eliminar) dispara una notificación con `react-toastify`, mostrada por el `ToastContainer` en `App.tsx`.

## 📚 Conceptos aplicados
- Manejo de estado global con **Zustand**, usando el patrón `create<Type>()(...)` con tipado explícito del store.
- Composición de middlewares de Zustand: `devtools` (inspección en DevTools) y `persist` (persistencia automática en `localStorage`).
- Formularios con **react-hook-form**: `register`, `handleSubmit`, `setValue`, `reset` y manejo de `formState.errors`.
- Validaciones declarativas de formulario, incluyendo validación de email con expresión regular.
- Flujo de edición reutilizando un mismo formulario para crear y actualizar, controlado mediante `activeId`.
- Notificaciones de usuario con `react-toastify`.
- Organización del proyecto por responsabilidades (`components`, `types`, `store`).

## 🚀 Cómo ejecutar el proyecto
1. Clonar el repositorio:
```bash
   git clone https://github.com/andresmdevco/veterinary-patient-manager.git
   cd veterinary-patient-manager
```
2. Instalar las dependencias:
```bash
   npm install
```
3. Ejecutar el proyecto en modo desarrollo:
```bash
   npm run dev
```
4. Abrir [http://localhost:5173](http://localhost:5173) en el navegador.
