# Azure App Service y Java
Hola, bienvenido al taller de Java + Azure `:)`. En este documento se describen los prerequisitos para el taller y los pasos a seguir para completar la actividad.

## Objetivo

Crear una aplicaci&oacute;n web Java que corra en Azure App Services y se actualice de forma autom&aacute;tica cada que despleguemos un cambio a GitHub.

## Prerequisitos
Para poder llevar a cabo la actividad, necesitar&aacute;s estos elementos:

* Crear cuenta en [GitHub](https://github.com/).
  * Debes crear un fork de [este repositorio](https://github.com/it-influencers-mx/azure_java_basic_example), pues el c&oacute;digo que usaremos ser&aacute; el mismo.
* Crear cuenta en [Azure](https://azure.microsoft.com/es-mx/free/).
  * Debes tener al menos una suscripci&oacute;n activa. Si ya no tienes la suscripci&oacute;n de prueba, puedes crear una suscripci&oacute;n del tipo *Pay as you go*. No te preocupes, no se har&aacute;n cargos a tu cuenta, pues los servicios que usaremos **&iexcl;son gratis&excl;**
* Tener navegador Google Chrome, Microsoft Edge o Mozilla Firefox.
  * Todo se har&aacute; desde el navegador, as&iacute; que no te preocupes por instalar Git o alg&uacute;n editor de texto.
* Muchas ganas de aprender `:)`

# Infraestructura en Azure

Lo primero que debemos tener en consideraci&oacute;n es la infraestructura en Azure. En este taller, se trabajar&aacute; con Azure App Service, por lo que se describen abajo los pasos para crear tu propio App Service.

Para comenzar, accede al [portal de Azure](https://portal.azure.com/#home) (inicia sesi&oacute;n si a&uacute;n no lo has hecho).

## Resource Group

Un *Resource group* (o grupo de recursos) es una agrupaci&oacute;n l&oacute;gica de recursos de Azure dentro de una suscripci&oacute;n. Para crear el resource group, accede a la secci&oacute;n de [Resource groups](https://portal.azure.com/#blade/HubsExtension/BrowseResourceGroups) y haz clic en el bot&oacute;n **Create**.

![Crear Resource group](https://raw.githubusercontent.com/it-influencers-mx/azure_java_basic_example/main/docs/imgs/rg01.png)

Una lista de campos se desplegar&aacute;. Rellena los campos como se indica a continuaci&oacute;n.

| Campo          | Valor                                                                      |
| -------------- | -------------------------------------------------------------------------- |
| Subscription   | *La suscripci&oacute;n que hayas creado - se llena autom&aacute;ticamente* |
| Resource group | Un nombre que quieras darle a tu Resource group, por ejemplo `rg-default`  |
| Region         | `South Central US`                                                         |

Despu&eacute;s haz clic en **Review + create**. Un mensaje de validaci&oacute;n se mostrar&aacute;. Haz clic en **Create**.

![Resource group creado](https://raw.githubusercontent.com/it-influencers-mx/azure_java_basic_example/main/docs/imgs/rg02.png)

Listo, ya tienes tu resource group creado.

## App Service Plan

Un *App Service Plan*, de manera general, es el hardware sobre el cual tus App Services se ejecutar&aacute;n. Hay distintos tipos, puedes encontrar una descripci&oacute;n detallada [aqu&iacute;](https://docs.microsoft.com/en-us/azure/app-service/overview-hosting-plans).

En este taller usaremos el de tipo gratuito. Accede a la secci&oacute;n de [App Service plans](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Web%2FserverFarms) y haz clic en **Create**.

![Crear App Service plan](https://raw.githubusercontent.com/it-influencers-mx/azure_java_basic_example/main/docs/imgs/asp01.png)

Una lista de campos se desplegar&aacute;. Rellena los campos como se indica a continuaci&oacute;n.

| Campo            | Valor                                                                        |
| ---------------- | ---------------------------------------------------------------------------- |
| Subscription     | *La suscripci&oacute;n que hayas creado - se llena autom&aacute;ticamente*   |
| Resource group   | *Selecciona el resource group que creaste en el paso anterior*               |
| Name             | Un nombre que quieras darle a tu App Service Plan, por ejemplo `asp-default` |
| Operating System | `Linux`                                                                      |
| Region           | `South Central US`                                                           |
| SKU              | Haz clic en *change size* y selecciona `Dev/test > Free F1`                  |

Despu&eacute;s haz clic en **Review + create**. Un mensaje de validaci&oacute;n se mostrar&aacute;. Haz clic en **Create**.

![App Service plan creado](https://raw.githubusercontent.com/it-influencers-mx/azure_java_basic_example/main/docs/imgs/asp02.png)

Listo, ya tienes tu App Service plan creado.

## App Service

Excelente, ya casi terminamos de crear tu aplicaci&oacute;n en Azure. Lo que toca ahora es crear el *App Service* como tal. Un App Service es una aplicaci&oacute;n que corre sobre tu App Service plan, y est&aacute; ligado a las particularidades de ese plan, como por ejemplo la memoria RAM, el CPU, el Sistema Operativo, entre otras cosas.

Para crear tu App Service, ingresa a la secci&oacute;n de [App Service](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Web%2Fsites) y haz clic en el bot&oacute;n **Create**.

![Crear App Service](https://raw.githubusercontent.com/it-influencers-mx/azure_java_basic_example/main/docs/imgs/as01.png)

Una lista de campos se desplegar&aacute;. Rellena los campos como se indica a continuaci&oacute;n.

| Campo                           | Valor                                                                                                   |
| ------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Subscription                    | *La suscripci&oacute;n que hayas creado - se llena autom&aacute;ticamente*                              |
| Resource group                  | *Selecciona el resource group que creaste en pasos anteriores*                                          |
| Name                            | Nombre de tu App Service, por ejemplo `my-app123`. **Este nombre debe ser &uacute;nico en todo Azure**. |
| Publish                         | `Code`                                                                                                  |
| Runtime stack                   | `Java 8`                                                                                                |
| Java web server stack           | `Tomcat 9.0`                                                                                            |
| Operating System                | `Linux`                                                                                                 |
| Region                          | `South Central US`                                                                                      |
| App Service Plan `>` Linux Plan | *El App Service Plan que creaste en el paso anterior.*                                                  |

Despu&eacute;s haz clic en **Review + create**. Un mensaje de validaci&oacute;n se mostrar&aacute;. Haz clic en **Create**.

![App Service creado](https://raw.githubusercontent.com/it-influencers-mx/azure_java_basic_example/main/docs/imgs/as02.png)

Listo, ya tienes tu App Service creado.

## Configuraci&oacute;n del App Service

Antes de seguir con la configuraci&oacute;n de tu App Service, aseg&uacute;rate de que hayas creado el fork en GitHub. Est&aacute; especificado en los [prerequisitos](#prerequisitos).

Si ya tienes el repositorio forkeado en tu cuenta, podemos seguir.

Desde la lista de [App Services](https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Web%2Fsites) accede al que acabas de crear. Se abrir&aacute; el overview de tu App Service. En el men&uacute; de la izquierda, ve a **Deployment** `>` **Deployment Center**.

Dentro del Deployment Center, en el campo Source, selecciona **GitHub**.

![Deployment center de tu App Service](https://raw.githubusercontent.com/it-influencers-mx/azure_java_basic_example/main/docs/imgs/dc01.png)

Se te pedir&aacute; que inicies sesi&oacute;n con tus credenciales de GitHub. Sigue el proceso para iniciar sesi&oacute;n.

Una vez tu sesi&oacute;n est&eacute; activa en Azure, se desplegar&aacute;n los siguientes campos:

| Campo        | Valor                             |
| ------------ | --------------------------------- |
| Organization | *Selecciona tu nombre de usuario* |
| Repository   | `azure_java_basic_example`        |
| Branch       | `main`                            |

Despu&eacute;s de llenar los campos, en la parte de arriba haz clic en **Save**. Esto crear&aacute; una GitHub Action en tu repositorio, la cual ser&aacute; usada para el despliegue de la aplicaci&oacute;n. Espera un par de minutos antes de acceder a tu aplicaci&oacute;n.

Para acceder a tu aplicaci&oacute;n reemplaza en la siguiente liga el nombre de tu App Service: [https://**REPLACE_ME**.azurewebsites.net/](https://REPLACE_ME.azurewebsites.net/).

**&iexcl;Listo&excl;** Tu aplicaci&oacute;n est&aacute; al aire.

![Hello World&excl;](https://raw.githubusercontent.com/it-influencers-mx/azure_java_basic_example/main/docs/imgs/app01.png)

# CI/CD con GitHub

La aplicaci&oacute;n que acabas de desplegar existe como un repositorio en tu cuenta de GitHub. Para acceder a ese repositorio, reemplaza en esta liga tu nombre de usuario: [https://github.com/**REPLACE_ME**/azure_java_basic_example](https://github.com/REPLACE_ME/azure_java_basic_example).

Una de las ventajas que ofrece Azure con GitHub Actions es que puedes desplegar tu c&oacute;digo inmediatamente cada que hay cambios. Para demostrar esto, accede al siguiente archivo `/src/main/webapp/index.jsp`. Una vez en el archivo, haz clic en el &iacute;cono de editar para modificar el archivo.

![Editar archivo](https://raw.githubusercontent.com/it-influencers-mx/azure_java_basic_example/main/docs/imgs/gh01.png)

En la l&iacute;nea 24 del archivo, reemplaza la leyenda `AQU&Iacute;` con tu nombre. Por ejemplo:

``` html
<p>Mi nombre aparecer&aacute; <strong><code>Angel Alonso</code></strong></p>
```

En la parte inferior, selecciona **Commit changes**. Esto desencadenar&aacute; la GitHub Action, que desplegar&aacute; tu nueva aplicaci&oacute;n a tu App Service.

![Aplicaci&oacute;n actualizada](https://raw.githubusercontent.com/it-influencers-mx/azure_java_basic_example/main/docs/imgs/app02.png)

