<properties
	pageTitle="Sustitución de claves de firma en Azure AD | Microsoft Azure"
	description="En este artículo se analizan las prácticas recomendadas de sustitución de claves de firma de Azure Active Directory."
	services="active-directory"
	documentationCenter=".net"
	authors="gsacavdm"
	manager="krassk"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/23/2016"
	ms.author="gsacavdm"/>

# Sustitución de claves de firma de Azure Active Directory

En este tema se describe lo que necesita saber de las claves públicas que se usan en Azure Active Directory (Azure AD) para firmar los tokens de seguridad. Es importante tener en cuenta que estas claves se sustituyen de forma periódica y, en caso de emergencia, podrían ser sustituidas inmediatamente. Todas las aplicaciones que usan Azure AD deben poder controlar mediante programación el proceso de sustitución de claves. Siga leyendo para comprender cómo funcionan las claves, cómo evaluar el impacto de la sustitución en la aplicación y cómo actualizar la aplicación para controlar la sustitución de claves si fuera necesario.

> [AZURE.IMPORTANT] La siguiente sustitución de claves de firma se producirá el 15 de agosto de 2016 y *no* afectará a las aplicaciones de la galería o una aplicación de inquilinos B2C.

## Información general sobre las claves de firma de Azure AD

Azure AD emplea una criptografía de clave pública basada en estándares del sector con el fin de establecer una relación de confianza entre ella y las aplicaciones que la utilizan. En términos prácticos, funciona de la siguiente manera: Azure AD usa una clave de firma que consta de un par de claves pública y privada. Cuando un usuario inicia sesión en una aplicación que utiliza Azure AD para realizar la autenticación, Azure AD crea un token de seguridad que contiene información sobre el usuario. Azure AD firma este token con su clave privada antes de enviarlo a la aplicación. Para comprobar que el token es válido y que realmente se originó en Azure AD, la aplicación debe validar la firma del token usando la clave pública expuesta por Azure AD que se encuentra en el [documento de detección de OpenID Connect](http://openid.net/specs/openid-connect-discovery-1_0.html) del inquilino o en el [documento de metadatos de federación](active-directory-federation-metadata.md) de SAML/WS-Fed.

Por motivos de seguridad, Azure AD firma la sustitución de claves de forma periódica y, en caso de emergencia, podrían sustituirse inmediatamente. Cualquier aplicación que se integra con Azure AD debe estar preparada para controlar un evento de sustitución de claves, con independencia de la frecuencia con que se produzca. Si no es así y la aplicación trata de utilizar una clave expirada para comprobar la firma de un token, se producirá un error en la solicitud de inicio de sesión.

Siempre hay más de una clave válida disponible en el documento de detección de OpenID Connect y en el de metadatos de federación. La aplicación debe estar preparada para utilizar cualquiera de las claves especificadas del documento, ya que una de ellas puede sustituirse pronto, otra puede ser su reemplazo, etc.

## Cómo evaluar si su aplicación se verá afectada y qué hacer al respecto

La forma que tiene la aplicación de controlar la sustitución de claves depende de ciertas variables, como el tipo de aplicación o qué protocolo de identidad y biblioteca se han usado. Las secciones siguientes evalúan si los tipos más comunes de aplicaciones se ven afectados por la sustitución de claves y ofrecen orientación sobre cómo actualizar la aplicación para admitir la sustitución automática o actualizar manualmente la clave.

* [Aplicaciones web y API que usan middleware .NET OWIN OpenID Connect, WS-Fed o WindowsAzureActiveDirectoryBearerAuthentication](#owin)
* [Aplicaciones web y API que usan middleware .NET Core OpenID Connect o JwtBearerAuthentication](#owincore)
* [Aplicaciones Web y API que usan el módulo Node.js passport-azure-ad](#passport)
* [Aplicaciones web y API creadas con Visual Studio 2015](#vs2015)
* [Aplicaciones web creadas con Visual Studio 2013](#vs2013)
* [API web creadas con Visual Studio 2013](#vs2013_webapi)
* [Aplicaciones web creadas con Visual Studio 2012](#vs2012)
* [Aplicaciones web creadas con Visual Studio 2010 o 2008 que usan Windows Identity Foundation](#vs2010)
* [Aplicaciones Web y API que usan cualquier otra biblioteca o que implementan manualmente cualquiera de los protocolos admitidos](#other)

### <a name="owin"></a> Aplicaciones web y API que usan middleware .NET OWIN OpenID Connect, WS-Fed o WindowsAzureActiveDirectoryBearerAuthentication

Si la aplicación está utilizando el middleware .NET OWIN OpenID Connect, WS-Fed o WindowsAzureActiveDirectoryBearerAuthentication, ya tiene la lógica necesaria para controlar automáticamente la sustitución de clave.

Puede confirmar que la aplicación utiliza cualquiera de estos, busque cualquiera de los siguientes fragmentos de código en la aplicación Startup.cs o Startup.Auth.cs

```
app.UseOpenIdConnectAuthentication(
	 new OpenIdConnectAuthenticationOptions
	 {
		 // ...
	 });
```
```
app.UseWsFederationAuthentication(
    new WsFederationAuthenticationOptions
    {
	 // ...
 	});
```
```
 app.UseWindowsAzureActiveDirectoryBearerAuthentication(
	 new WindowsAzureActiveDirectoryBearerAuthenticationOptions
	 {
	 // ...
	 });
```

### <a name="owincore"></a> Aplicaciones web y API que usan middleware .NET Core OpenID Connect o JwtBearerAuthentication

Si la aplicación está utilizando el middleware .NET Core OWIN OpenID Connect o JwtBearerAuthentication, ya tiene la lógica necesaria para controlar automáticamente la sustitución de clave.

Puede confirmar que la aplicación utiliza cualquiera de estos, busque cualquiera de los siguientes fragmentos de código en la aplicación Startup.cs o Startup.Auth.cs

```
app.UseOpenIdConnectAuthentication(
	 new OpenIdConnectAuthenticationOptions
	 {
		 // ...
	 });
```
```
app.UseJwtBearerAuthentication(
    new JwtBearerAuthenticationOptions
    {
	 // ...
 	});
```

### <a name="passport"></a> Aplicaciones Web y API que usan el módulo Node.js passport-azure-ad

Si la aplicación está utilizando el módulo Node.js passport-ad, ya tiene la lógica necesaria para controlar automáticamente la sustitución de clave.

Puede confirmar su aplicación passport-ad si busca el siguiente fragmento en el archivo app.js de la aplicación

```
var OIDCStrategy = require('passport-azure-ad').OIDCStrategy;

passport.use(new OIDCStrategy({
	//...
));
```

### <a name="vs2015"></a> Aplicaciones web y API creadas con Visual Studio 2015

Si la aplicación se compiló utilizando una plantilla de aplicación web en Visual Studio 2015 y seleccionó **Cuentas profesionales y educativas** en el menú **Cambiar autenticación**, ya tendrá la lógica necesaria para controlar automáticamente la sustitución de claves. Esta lógica, inserta en el middleware OWIN OpenID Connect, recupera y almacena en caché las claves del documento de detección OpenID Connect y las actualiza periódicamente.

Si ha agregado manualmente la autenticación a la solución, la aplicación no tendrá la lógica necesaria para la sustitución de claves. Tendrá que escribirla usted mismo o seguir los pasos de [Aplicaciones Web y API que usan cualquier otra biblioteca o que implementan manualmente cualquiera de los protocolos admitidos](#other).

### <a name="vs2013"></a> Aplicaciones web creadas con Visual Studio 2013

Si la aplicación se compiló utilizando una plantilla de aplicación web en Visual Studio 2013 y seleccionó **Cuentas profesionales** en el menú **Cambiar autenticación**, ya tendrá la lógica necesaria para controlar automáticamente la sustitución de claves. Esta lógica almacena la información de la clave de firma y el identificador único de la organización en dos tablas de base de datos asociadas al proyecto. Puede encontrar la cadena de conexión de la base de datos en el archivo Web.config del proyecto.

Si ha agregado manualmente la autenticación a la solución, la aplicación no tendrá la lógica necesaria para la sustitución de claves. Tendrá que escribirla usted mismo o seguir los pasos de [Aplicaciones Web y API que usan cualquier otra biblioteca o que implementan manualmente cualquiera de los protocolos admitidos](#other).

Los siguientes pasos lo ayudarán a comprobar que la lógica funcione correctamente en la aplicación.

1. En Visual Studio 2013, abra la solución y, después, haga clic en la pestaña **Explorador de servidores** de la ventana derecha.
2. Expanda **Conexiones de datos**, **DefaultConnection** y, luego, **Tablas**. Busque la tabla **IssuingAuthorityKeys**, haga clic con el botón derecho en ella y, después, con el botón izquierdo, en **Mostrar datos de tabla**.
3. En la tabla **IssuingAuthorityKeys** habrá al menos una fila, que se corresponde con el valor de huella digital de la clave. Elimine las filas de la tabla.
4. Haga clic con el botón derecho en la tabla **Inquilinos** y, después, con el botón izquierdo, en **Mostrar datos de tabla**.
5. En la tabla **Inquilinos**, habrá al menos una fila, que se corresponde con un identificador del inquilino de directorio único. Elimine las filas de la tabla. Si no elimina las filas de las tablas **Inquilinos** y **IssuingAuthorityKeys**, se producirá un error en tiempo de ejecución.
6. Compile y ejecute la aplicación. Una vez que haya iniciado sesión en la cuenta, podrá detener la aplicación.
7. Vuelva a la pestaña **Explorador de servidores** y examine los valores de las tablas **IssuingAuthorityKeys** e **Inquilinos**. Observará que se han vuelto a rellenar automáticamente con la información correspondiente del documento de metadatos de federación.

### <a name="vs2013"></a> API web creadas con Visual Studio 2013

Si creó una aplicación API web en Visual Studio 2013 con la plantilla de API web y, a después, seleccionó **Cuentas profesionales** en el menú **Cambiar autenticación**, la aplicación ya tendrá la lógica necesaria. Si configura manualmente la autenticación, siga estas instrucciones para aprender a configurar la API web con el fin de actualizar automáticamente la información de claves.

El fragmento de código siguiente muestra cómo obtener las claves más recientes del documento de metadatos de federación y, luego, utilizar el [controlador de token JWT](https://msdn.microsoft.com/library/dn205065.aspx) para validar el token. En el fragmento de código se da por hecho que va a utilizar su propio mecanismo de almacenamiento en caché para conservar la clave con el fin de validar los tokens futuros de Azure AD, ya sea en una base de datos, un archivo de configuración o en otro lugar.

```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IdentityModel.Tokens;
using System.Configuration;
using System.Security.Cryptography.X509Certificates;
using System.Xml;
using System.IdentityModel.Metadata;
using System.ServiceModel.Security;
using System.Threading;

namespace JWTValidation
{
    public class JWTValidator
    {
        private string MetadataAddress = "[Your Federation Metadata document address goes here]";

        // Validates the JWT Token that's part of the Authorization header in an HTTP request.
        public void ValidateJwtToken(string token)
        {
            JwtSecurityTokenHandler tokenHandler = new JwtSecurityTokenHandler()
            {
                // Do not disable for production code
                CertificateValidator = X509CertificateValidator.None
            };

            TokenValidationParameters validationParams = new TokenValidationParameters()
            {
                AllowedAudience = "[Your App ID URI goes here, as registered in the Azure Classic Portal]",
                ValidIssuer = "[The issuer for the token goes here, such as https://sts.windows.net/68b98905-130e-4d7c-b6e1-a158a9ed8449/]",
                SigningTokens = GetSigningCertificates(MetadataAddress)

                // Cache the signing tokens by your desired mechanism
            };

            Thread.CurrentPrincipal = tokenHandler.ValidateToken(token, validationParams);
        }

        // Returns a list of certificates from the specified metadata document.
        public List<X509SecurityToken> GetSigningCertificates(string metadataAddress)
        {
            List<X509SecurityToken> tokens = new List<X509SecurityToken>();

            if (metadataAddress == null)
            {
                throw new ArgumentNullException(metadataAddress);
            }

            using (XmlReader metadataReader = XmlReader.Create(metadataAddress))
            {
                MetadataSerializer serializer = new MetadataSerializer()
                {
                    // Do not disable for production code
                    CertificateValidationMode = X509CertificateValidationMode.None
                };

                EntityDescriptor metadata = serializer.ReadMetadata(metadataReader) as EntityDescriptor;

                if (metadata != null)
                {
                    SecurityTokenServiceDescriptor stsd = metadata.RoleDescriptors.OfType<SecurityTokenServiceDescriptor>().First();

                    if (stsd != null)
                    {
                        IEnumerable<X509RawDataKeyIdentifierClause> x509DataClauses = stsd.Keys.Where(key => key.KeyInfo != null && (key.Use == KeyType.Signing || key.Use == KeyType.Unspecified)).
                                                             Select(key => key.KeyInfo.OfType<X509RawDataKeyIdentifierClause>().First());

                        tokens.AddRange(x509DataClauses.Select(token => new X509SecurityToken(new X509Certificate2(token.GetX509RawData()))));
                    }
                    else
                    {
                        throw new InvalidOperationException("There is no RoleDescriptor of type SecurityTokenServiceType in the metadata");
                    }
                }
                else
                {
                    throw new Exception("Invalid Federation Metadata document");
                }
            }
            return tokens;
        }
    }
}
```

### <a name="vs2012"></a> Aplicaciones web creadas con Visual Studio 2012

Si la aplicación se compiló en Visual Studio 2012, probablemente ha utilizado la herramienta de identidad y acceso para configurar la aplicación. También es probable que esté utilizando el [registro de nombres de emisor de validación (VINR)](https://msdn.microsoft.com/library/dn205067.aspx). El VINR se encarga de mantener la información sobre los proveedores de identidad de confianza (Azure AD) y las claves utilizadas para validar los tokens que emiten. El VINR también facilita la tarea de actualizar automáticamente la información de claves almacenada en un archivo Web.config descargando el documento de metadatos de federación más reciente asociado a su directorio, comprobando si la configuración está actualizada con respecto al último documento y actualizando la aplicación para usar la nueva clave según sea necesario.

Si ha creado la aplicación utilizando cualquiera de los ejemplos de código o la documentación de tutorial que proporciona Microsoft, la lógica de sustitución de claves ya estará incluida en el proyecto. Observará que el código siguiente ya existe en el proyecto. Si la aplicación aún no tiene esta lógica, siga estos pasos para agregarla y comprobar que funciona correctamente.

1. En **Explorador de soluciones**, agregue una referencia al ensamblado **System.IdentityModel** del proyecto correspondiente.
2. Abra el archivo **Global.asax.cs** y agregue lo siguiente utilizando directivas:
```
using System.Configuration;
using System.IdentityModel.Tokens;
```
3. Agregue el método siguiente al archivo **Global.asax.cs**:
```
protected void RefreshValidationSettings()
{
    string configPath = AppDomain.CurrentDomain.BaseDirectory + "\" + "Web.config";
    string metadataAddress =
                  ConfigurationManager.AppSettings["ida:FederationMetadataLocation"];
    ValidatingIssuerNameRegistry.WriteToConfig(metadataAddress, configPath);
}
```
4. Invoque el método **RefreshValidationSettings()** en el método **Application\_Start ()** de **Global.asax.cs**, tal y como se muestra:
```
protected void Application_Start()
{
    AreaRegistration.RegisterAllAreas();
    ...
    RefreshValidationSettings();
}
```

Una vez que haya seguido estos pasos, el archivo Web.config de la aplicación se actualizará con la información más reciente del documento de metadatos de federación, incluidas las últimas claves. Esta actualización se producirá cada vez que recicle el grupo de aplicaciones de IIS; de forma predeterminada, esta acción se realizará cada 29 horas.

Siga los pasos que figuran a continuación para comprobar que la lógica de sustitución de claves funciona correctamente.

1. Una vez que haya comprobado que la aplicación está utilizando el código anterior, abra el archivo **Web.config** y desplácese al bloque **<issuerNameRegistry>**; busque expresamente las siguientes líneas:
```
<issuerNameRegistry type="System.IdentityModel.Tokens.ValidatingIssuerNameRegistry, System.IdentityModel.Tokens.ValidatingIssuerNameRegistry">
        <authority name="https://sts.windows.net/ec4187af-07da-4f01-b18f-64c2f5abecea/">
          <keys>
            <add thumbprint="3A38FA984E8560F19AADC9F86FE9594BB6AD049B" />
          </keys>
```
2. En la configuración de **<add thumbprint=””>**, cambie el valor de huella digital reemplazando cualquier carácter por otro diferente. Guarde el archivo **Web.config**.

3. Compile la aplicación y, después, ejecútela. Si puede completar el proceso de inicio de sesión, la aplicación actualizará correctamente la clave descargando la información necesaria del documento de metadatos de federación de su directorio. Si tiene problemas para iniciar sesión, asegúrese de que los cambios en la aplicación sean correctos consultando el tema [Adding Sign-On to Your Web Application Using Azure AD](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) (Incorporación del inicio de sesión único en aplicaciones web mediante Azure AD), o bien descargarlos e inspeccionando el siguiente código de ejemplo: [Multi-Tenant Cloud Application for Azure Active Directory](https://code.msdn.microsoft.com/multi-tenant-cloud-8015b84b) (Aplicación multiinquilino en la nube para Azure Active Directory).


### <a name="vs2010"></a> Las aplicaciones web creadas con Visual Studio 2008 o 2010 y Windows Identity Foundation (WIF) 1.0 para .NET 3.5

Si ha compilado una aplicación en la versión 1.0 de WIF, no habrá ningún mecanismo para actualizar automáticamente la configuración de la aplicación con el fin de usar una nueva clave. La manera más sencilla de actualizar la clave es usar las herramientas de FedUtil incluidas en el SDK de WIF, que pueden recuperar el documento de metadatos más recientes y actualizar la configuración. Estas instrucciones se incluyen a continuación. Como alternativa, puede realizar uno de los siguientes pasos:

- Siga las instrucciones de la sección Recuperación manual de la clave más reciente y actualización de aplicaciones y cree la lógica para realizar los pasos mediante programación.
- Actualice la aplicación a .NET 4.5, que incluye la versión más reciente de WIF ubicada en el espacio de nombres del sistema. Después, podrá utilizar el [registro de nombres de emisor de validación (VINR)](https://msdn.microsoft.com/library/dn205067.aspx) para realizar las actualizaciones automáticas de la configuración de la aplicación.


1. Compruebe que tiene instalado el SDK de la versión 1.0 de WIF en la máquina de desarrollo de Visual Studio 2008 o 2010. También puede [descargarlo desde aquí](https://www.microsoft.com/es-ES/download/details.aspx?id=4451) si aún no lo ha instalado.
2. En Visual Studio, abra la solución y, luego, haga clic con el botón derecho en el proyecto correspondiente y seleccione **Actualizar metadatos de federación**. Si esta opción no está disponible, significa que no se ha instalado FedUtil o el SDK de la versión 1.0 de WIF.
3. En el símbolo del sistema, seleccione **Actualizar** para iniciar la actualización de los metadatos de federación. Si tiene acceso al entorno de servidor donde está hospedada la aplicación, puede utilizar, si lo desea, el [Programador de actualización automática de metadatos](https://msdn.microsoft.com/library/ee517272.aspx) de FedUtil.
4. Haga clic en **Finalizar** para completar el proceso de actualización.

### <a name="other"></a> Aplicaciones Web y API que usan cualquier otra biblioteca o que implementan manualmente cualquiera de los protocolos admitidos,

Si está utilizando otra biblioteca o implementa manualmente cualquiera de los protocolos admitidos, debe revisar la biblioteca o la implementación para asegurarse de que se está recuperando la clave desde el documento de detección OpenID Connect o el documento de metadatos de federación. Una forma de comprobarlo es realizar una búsqueda en el código o en el de la biblioteca de las llamadas al documento de detección OpenID o al documento de metadatos de federación.

Si la clave se almacena en algún lugar o está codificada de forma rígida en la aplicación, puede recuperar manualmente la clave y actualizarla en consecuencia. **Se recomienda encarecidamente que mejore su aplicación para que admita la conversión automática**, mediante cualquiera de los enfoques descritos en este artículo, para evitar futuras interrupciones y sobrecargas si Azure AD aumenta su cadencia de sustitución o tiene una sustitución de fuera de banda de emergencia.

Para recuperar manualmente la clave más reciente del documento de detección OpenID:

1. En el explorador web, acceda a `https://login.microsoftonline.com/your_directory_name/.well-known/openid-configuration`. Verá el contenido del documento de detección OpenID Connect. Para más información acerca de este documento, consulte la [especificación del documento de detección OpenID](http://openid.net/specs/openid-connect-discovery-1_0.html).
2. Copie el vínculo en el valor de jwks\_uri.
3. Abra una nueva pestaña en el explorador y vaya a la dirección URL que acaba de copiar. Verá el contenido del documento del conjunto de claves web de JSON.
4. Para actualizar una aplicación y que utilice una nueva clave, localice cada elemento **x5c** y, luego, copie el valor de cada uno de ellos. Por ejemplo:
```
keys: [
	{
		kty: "RSA",
		use: "sig",
		kid: "MnC_VZcATfM5pOYiJHMba9goEKY",
		x5t: "MnC_VZcATfM5pOYiJHMba9goEKY",
		n: "vIqz-4-ER_vNW...ixLUQ",
		e: "AQAB",
		x5c: [
			"MIIC4jCCAcqgAw...dhXsIIKvJQ=="
		]
	},
```
5. Después de copiar el valor del elemento **<X509Certificate>**, abra un editor de texto sin formato y pegue dicho valor. No se olvide de quitar los espacios en blanco finales y de guardar el archivo con una extensión **.cer**.

Para recuperar manualmente la clave más reciente desde el documento de metadatos de federación:

1. En el explorador web, acceda a `https://login.microsoftonline.com/your_directory_name/federationmetadata/2007-06/federationmetadata.xml`. Verá el contenido del documento XML de metadatos de federación. Para más información sobre este documento, consulte el tema de [metadatos de federación](active-directory-federation-metadata.md).
2. Para actualizar una aplicación que utilice una nueva clave, localice cada bloque **<RoleDescriptor>** y, luego, copie el valor de cada uno de ellos en el elemento **<X509Certificate>** del bloque. Por ejemplo:
```
<RoleDescriptor xmlns:fed="http://docs.oasis-open.org/wsfed/federation/200706" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" protocolSupportEnumeration="http://docs.oasis-open.org/wsfed/federation/200706" xsi:type="fed:SecurityTokenServiceType">
      <KeyDescriptor use="signing">
            <KeyInfo xmlns="http://www.w3.org/2000/09/xmldsig#">
                <X509Data>
                    <X509Certificate>MIIDPjC…BcXWLAIarZ</X509Certificate>
```
3. Después de copiar el valor del elemento **<X509Certificate>**, abra un editor de texto sin formato y pegue dicho valor. No se olvide de quitar los espacios en blanco finales y de guardar el archivo con una extensión **.cer**.

Acaba de crear el certificado X509 que se utiliza como la clave pública de Azure AD. Con los detalles del certificado, como su huella digital y fecha de expiración, puede comprobar manualmente o mediante programación que la huella digital y el certificado que utiliza actualmente la aplicación son válidos.

<!---HONumber=AcomDC_0629_2016-->