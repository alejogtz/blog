---
layout: post
category: tutorials
slug: Configurar Servidor IIS en Windows
---

# Configurar Servidor IIS
## 1. Habilitar los Servicios en `⊞ > Caracteristicas de Windows`
    
![Caracteristicas de windows](https://i.imgur.com/5tRdE71.png)
    

## 2. Crear un Grupo de Aplicaciones

> Un grupo de aplicaciones están asociados a procesos de trabajo, contienen una o más aplicaciones y **proporcionan aislamiento entre aplicaciones**

Ir a `⊞ > IIS > Conexiones > Grupos aplicaciones > Agregar...aplicaciones` y elegir

|Campo|Valor|Comentario|
|------|-----|----------|
|Nombre|`<<Nombre>>`|*El nombre que desees y que reprensente ese grupo de aplicaciones*|
|Version|.Net CLR versión 4.0.3..|*Version Common Languaje Runtime con el cual se ejecutarán las aplicaciones de este grupo*|
|Modo Canalizacion|Integrada|*X_X*|

## 2.1 Creación de un `SITIO`,`APLICACION` y `DIRECTORIO VIRTUAL`

> `SITIO`: Un sitio es el contenedor de aplicaciones. Establecen el puerto de red por la cual las aplicaciones que contiene atenderán sus solicitudes. ej. http://myserver.com:8181,http://myserver.com:80,http://myserver.com:9090 

> `APLICACION`: Una aplicación es una carpeta la cual será interpretada como un proyecto que necesita un CLR determinado de acuerdo al grupo de aplicaciones al que pertenece.
> Una aplicacion agrega al prefijo de su ruta las carpetas padre [*directorios virtuales*] dentro del cual se encuentre. ej. **SITIO:**`http://myserver.com:8181`, **DIRECTORIO VIRTUAL:**`/ws/huixquilucan`, entonces la ruta para acceder al `index.html` de esta aplicaciones será: `http://myserver.com:8181/ws/huixquilucan/index.html`

> `DIRECTORIO_VIRTUAL`:  Un directorio virtual ofrece una herramienta para tener organizados las aplicaciones. Para garantizar que no existan conflictos o colisiones entre las aplicaciones. el directorio virtual agrega la ruta de sus carpetas como prefijo de la aplicaciones despues de la url del stio. EJ: **DIRECTORIO VIRTUAL:**`/ws/huixquilucan`, entonces la aplicacion comienza a partir de la ruta: `http://myserver.com:8181/ws/huixquilucan/`


## 3. Preparar el entorno para publicar
1. Crear Carpeta en `C:/api/servicios`
2. Habilitarle Permisos a Los usuarios Necesarios

> **Warning:** si no se configuran los permisos a la hora de abrir la aplicacion en el navegador encontrará un problema con que el CLR no tiene permisos de lectura de la carpeta donde se encuentra la aplicacion

## 3.5 Publicar como Aplicacion
1. Desde a `⊞ > IIS > Conexiones > Sitios > <click derecho> > Agregar Sitio Web ` completar con la siguiente configuracion
   - Nombre del Sitio: `apirest`
   - Grupo de Aplicaciones: DefaultAppPool
   - Ruta de Acceso Fisica: `C:/api/servicios`
   - Enlace > Tipo: `http`
   - Enlace > Direccion IP: `Todas las Asignadas`
   - Enlace > Puerto: `80`
   - Enlace > Nombre del Host: `<Vacio>`
 2. Click en `Aceptar`
  

## 4. Publicar desde Visual Studio

1. En `Solucion > Proyecto > <Clic Derecho> > Publicar` elegir la siguiente configuración.
* En `Folder Profile > Editar` Seleccionar
  - Metodo de Publicación : `Sistema de Archivos`
  - Ubicacion de Destino : `C:/api/servicios`
  - Finalmente `Guardar`
* Click en `Publicar`