<properties
	pageTitle="Red CDN: solución de problemas de compresión de archivos"
	description="Solucione los problemas con la compresión de archivos de red CDN."
	services="cdn"
	documentationCenter=".NET"
	authors="camsoper"
	manager="erikre"
	editor=""/>

<tags
	ms.service="cdn"
	ms.workload="tbd"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/21/2016" 
	ms.author="casoper"/>
    
# Solución de problemas de compresión de archivos de red CDN

Este artículo le ayudará a solucionar los problemas con la [compresión de archivos de red CDN](cdn-improve-performance.md).

Si necesita más ayuda en cualquier punto de este artículo, puede ponerse en contacto con los expertos de Azure en [los foros de MSDN Azure o de desbordamiento de pila](https://azure.microsoft.com/support/forums/). Como alternativa, también puede registrar un incidente de soporte técnico de Azure. Vaya al [sitio de soporte técnico de Azure](https://azure.microsoft.com/support/options/) y haga clic en **Obtener soporte técnico**.

## Síntoma

Está habilitada la compresión para el punto de conexión, pero se devuelven archivos sin comprimir.

## Causa

Hay varias causas posibles, por nombrar algunas:

- El contenido solicitado no es apto para la compresión.
- La compresión no está habilitada para el tipo de archivo solicitado.
- La solicitud HTTP no incluía un encabezado que solicitara un tipo de compresión válido.

## Pasos para solucionar problemas

### Comprobar la solicitud

En primer lugar, se debe hacer una comprobación rápida en la solicitud. Puede usar las [herramientas para desarrolladores](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/) del explorador para ver las solicitudes que se realizan.

- Compruebe que la solicitud se envía a la dirección URL del punto de conexión, `<endpointname>.azureedge.net`, y no a su origen.
- Compruebe que la solicitud contenga un encabezado **Accept-Encoding** y que el valor para el encabezado contenga **gzip**, **defalte** o **bzip2**.

![Encabezados de solicitud de red CDN](./media/cdn-troubleshoot-compression/cdn-request-headers.png)

### Comprobar la configuración de compresión (perfil de red CDN estándar)

> [AZURE.NOTE] Este paso solo se aplica si el perfil de red CDN está en el plan de tarifa **Estándar**.

Desplácese hasta el punto de conexión en el [Portal de Azure](https://portal.azure.com) y haga clic en el botón **Configurar**.

- Compruebe que la compresión está habilitada.
- Compruebe que el tipo MIME del contenido que se va a comprimir se incluya en la lista de formatos comprimidos.

![Configuración de compresión de red CDN](./media/cdn-troubleshoot-compression/cdn-compression-settings.png)

### Comprobar la configuración de compresión (perfil de red CDN Premium)

> [AZURE.NOTE] Este paso solo se aplica si el perfil de red CDN está en el plan de tarifa **Premium**.

Desplácese hasta el punto de conexión en el [Portal de Azure](https://portal.azure.com) y haga clic en el botón **Administrar**. Se abrirá el portal complementario. Desplace el mouse sobre la pestaña **HTTP grande** y luego mantenga el mouse sobre el control flotante **Configuración de caché**. Haga clic en **Compresión**.

- Compruebe que la compresión está habilitada.
- Compruebe que la lista de **Tipos de archivo** contiene una lista de tipos MIME separados por coma.
- Compruebe que el tipo MIME del contenido que se va a comprimir se incluya en la lista de formatos comprimidos.

![Configuración de compresión de red CDN Premium](./media/cdn-troubleshoot-compression/cdn-compression-settings-premium.png)


### Comprobar que el contenido se almacena en caché

Con las herramientas para desarrolladores de su explorador, compruebe los encabezados de respuesta para asegurarse de que el archivo se almacena en caché en la región donde se solicita.

- Compruebe el encabezado de respuesta **Server**. El encabezado debe tener el formato **Plataforma (POP/id. de servidor)**, tal como se muestra en el ejemplo siguiente.
- Compruebe el encabezado de respuesta **X Cache**. Debe poner **HIT**.  

![Encabezados de respuesta de red CDN](./media/cdn-troubleshoot-compression/cdn-response-headers.png)

### Comprobar que el archivo cumple los requisitos de tamaño

Para que un archivo sea apto para la compresión, debe cumplir los siguientes requisitos de tamaño:

- Mayor que 128 bytes.
- Menor que 1 MB.

<!---HONumber=AcomDC_0427_2016-->