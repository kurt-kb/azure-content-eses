
1. En el Explorador de soluciones de Visual Studio, haga clic con el botón derecho en el proyecto de la aplicación de la Tienda Windows y luego haga clic en **Tienda** > **Asociar aplicación con la Tienda...**.

    ![Asociar aplicación con la Tienda Windows](./media/app-service-mobile-register-wns/notification-hub-associate-win8-app.png)
    
2. En el asistente, haga clic en **Siguiente**, inicie sesión con su cuenta Microsoft, escriba un nombre para la aplicación en **Reserve un nuevo nombre de aplicación** y luego haga clic en **Reservar**.

3. Después de crear correctamente el registro de la aplicación, seleccione el nuevo nombre de la aplicación, haga clic en **Siguiente** y después en **Asociar**. Se agrega la información de registro necesaria de la Tienda Windows al manifiesto de aplicación.

7. Repita los pasos 1 y 3 para el proyecto de aplicación de la Tienda de Windows Phone con el mismo registro que creó anteriormente para la aplicación de la Tienda Windows.

7. Vaya al [Centro de desarrollo de Windows](https://dev.windows.com/es-ES/overview), inicie sesión con su cuenta Microsoft, haga clic en el nuevo registro de aplicación en **Mis aplicaciones** y, por último, expanda **Servicios** > **Notificaciones push**.

8. En la página **Notificaciones push** haga clic en la opción **sitio de Live Services** de **Estado de los Servicios de notificaciones push de Windows (WNS) y Aplicaciones móviles de Microsoft Azure**. Después, anote los valores de **SID del paquete** y el valor *actual* de **Secretos de aplicación**.

    ![Configuración de la aplicación en el Centro para desarrolladores](./media/app-service-mobile-register-wns/mobile-services-win8-app-push-auth.png)

    > [AZURE.IMPORTANT] El secreto de aplicación y el SID del paquete son credenciales de seguridad importantes. No los comparta con nadie ni los distribuya con su aplicación.

<!---HONumber=AcomDC_0629_2016-->