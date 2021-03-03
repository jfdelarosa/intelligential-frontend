# Frontend

El frontend está montado en Netlify: [https://jfdelarosa-prueba-intelligential.netlify.app](https://jfdelarosa-prueba-intelligential.netlify.app/)

### Consideraciones

Debido al tiempo reducido, tuve que hacer algunas elecciones que me ayudaron a ganar un poco de tiempo pero que perjudican un poco en la arquitectura del proyecto:

- Utilicé Nuxt (Vue 2) en vez de Vue 3.
- Utilicé [Vuetify](https://vuetifyjs.com) en vez de crear mis propios componentes y templates con [TailwindCSS](https://tailwindcss.com/).
- No utilicé validaciones adicionales en los formularios (Usualmente utilizo [vee-validate](https://github.com/logaretm/vee-validate)).

## Correr la app de forma local

### Instalación de dependencias

```
npm install
```

### Variables de entorno

Renombrar el archivo `.env.example` a `.env` y configurar las variables necesarias.

### Iniciar aplicación

```
npm run dev
```
