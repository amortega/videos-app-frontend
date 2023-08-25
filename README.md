# videos-app-frontend

Frontend para interactuar con listas de reproducción de YouTube, desarrollado con Vue 3 utilizando Vite.

## Demo en línea

Puede encontrar una demo de este frontend en línea en el link: http://test-apps.xyz/

Para la correcta visualización debe **permitir la opción "Contenido no seguro"** para este dominio. 

A continuación un pequeño tutorial para lograr activar esta opción:

**Paso 1**
Click en simbolo de candado:

![Print paso 1](/public/paso-1.png)

**Paso 2**
Click en configuración de sitios:

![Print paso 2](/public/paso-2.png)

**Paso 3**
En el apartado "Contenido no seguro" seleccionar la opción "Permitir":

![Print paso 3](/public/paso-3.png)


## Configuración del proyecto

### Crear archivo ".env" con los siguientes datos, ejemplo:
```sh
VITE_API_URL='http://localhost:3000/api/v1'
```

### Instalar dependencias:
```sh
npm install
```

### Compilar con Hot-Reload para desarrollo.

```sh
npm run dev
```

### Compilar versión minificada para producción

```sh
npm run build
```
