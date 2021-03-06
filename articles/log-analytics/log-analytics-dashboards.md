<properties
	pageTitle="Creación de un panel personalizado en Log Analytics | Microsoft Azure"
	description="Con esta guía le resultará más fácil comprender cómo los paneles de Log Analytics pueden mostrar todas las búsquedas de registros guardadas, lo que le proporciona una sola imagen de visualización de su entorno."
	services="log-analytics"
	documentationCenter=""
	authors="bandersmsft"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="log-analytics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/28/2016"
	ms.author="banders"/>

# Creación de un panel personalizado en Log Analytics

Con esta guía le resultará más fácil comprender cómo los paneles de Log Analytics pueden mostrar todas las búsquedas de registros guardadas, lo que le proporciona una sola imagen de visualización de su entorno.

![Panel de ejemplo](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

## ¿Cómo creo mi panel?

Para empezar, haga clic en el botón Información general en el panel de navegación de la izquierda para ir a Información general de OMS. Verá el mosaico "Mi panel" a la izquierda. Haga clic en él para explorar en profundidad el panel.

![Información general](./media/log-analytics-dashboards/oms-dashboards-overview.png)



## Adición de un mosaico

En los paneles, los mosaicos se generan a partir de las búsquedas de registros guardadas. OMS incluye muchas búsquedas de registros guardadas de creación previa, para que pueda empezar inmediatamente. Verá la siguiente ilustración que describe cómo comenzar.

![Gráfica](./media/log-analytics-dashboards/oms-dashboards-pictorial.png)

En la vista Mi panel, simplemente haga clic en el engranaje "personalizar" en la parte inferior de la página para entrar en el modo personalizar. En el panel que se abre a la derecha de la página se muestran todas las búsquedas de registros guardadas del área de trabajo.

![Agregar mosaicos 1](./media/log-analytics-dashboards/oms-dashboards-add-tile1.png)

Para visualizar una búsqueda de registros guardada como mosaico, basta con arrastrarla hasta el espacio vacío a la izquierda. Al arrastrarla se convertirá en un mosaico.

![Agregar mosaicos 2](./media/log-analytics-dashboards/oms-dashboards-add-tile2.png)

![Agregar mosaicos 3](./media/log-analytics-dashboards/oms-dashboards-add-tile3.png)


## Editar un mosaico

En la vista Mi panel, simplemente haga clic en el engranaje "personalizar" en la parte inferior de la página para entrar en el modo personalizar. Haga clic en el mosaico que quiere editar. El panel derecho cambia a editar y ofrece diferentes opciones:

![Editar mosaico](./media/log-analytics-dashboards/oms-dashboards-edit-tile.png)

### Visualizaciones de mosaico#
Existen dos tipos de visualizaciones de mosaico:

|tipo de gráfico|qué hace|
|---|---|
|![Gráfico de barras](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png)|Muestra una escala de tiempo de los resultados de búsquedas de registros guardadas o una lista de resultados por un campo en función de que la búsqueda de registros agregue resultados por un campo o no.
|![métrica](./media/log-analytics-dashboards/oms-dashboards-metric.png)|Muestra el número total de resultados de la búsqueda de registros como número en un icono. Los mosaicos de métrica permiten establecer un umbral que hará resaltar el mosaico si se alcanza el umbral.|

### Umbral
Puede crear un umbral en un mosaico mediante la visualización Métrica. Seleccione esta opción para crear un valor de umbral en el mosaico. Elija si quiere resaltar el mosaico cuando el valor está por encima o por debajo del umbral seleccionado y, luego, establezca el valor del umbral.

## Organización del panel
Para organizar el panel, navegue a la vista Mi panel y haga clic en el engranaje "personalizar" en la parte inferior de la página para entrar en el modo personalizar. Haga clic y arrastre el mosaico que quiere mover y muévalo hasta donde quiere que esté.

![Organizar el panel](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## Quitar un mosaico
Para quitar un mosaico, navegue a la vista Mi panel y haga clic en el engranaje **personalizar** en la parte inferior de la página para entrar en el modo personalizar. Seleccione el icono que quiere quitar y, luego, en el panel de la derecha, seleccione **Quitar icono**.

![Quitar un mosaico](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## Pasos siguientes

- Cree alertas en Log Analytics para generar notificaciones y solucionar problemas.

<!---HONumber=AcomDC_0504_2016-->