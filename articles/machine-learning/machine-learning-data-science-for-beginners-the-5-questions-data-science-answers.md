<properties
   pageTitle="Las cinco preguntas de la ciencia de datos: Ciencia de datos para principiantes | Microsoft Azure"
   description="Obtenga una introducción rápida a la ciencia de datos en Ciencia de datos para principiantes, cinco vídeos breves que comienzan con Las cinco preguntas a las que responde la ciencia de datos."
   keywords="realizar ciencia de datos, introducción a la ciencia de datos, ciencia de datos para principiantes, tipos de preguntas, preguntas de ciencia de datos, algoritmos de ciencia de datos"
   services="machine-learning"
   documentationCenter="na"
   authors="brohrer-ms"
   manager="paulettm"
   editor="cjgronlund"/>

<tags
   ms.service="machine-learning"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="06/24/2016"
   ms.author="cgronlun;brohrer;garye"/>

# Ciencia de datos para principiantes, vídeo 1: Las cinco preguntas a las que responde la ciencia de datos

Obtenga una rápida introducción a la ciencia de datos en la serie *Ciencia de datos para principiantes*, con cinco vídeos breves. Esta serie de vídeos es útil si está interesado en usar la ciencia de datos o trabaja con personas que usan la ciencia de datos, y desea comenzar por los conceptos más básicos.

Para obtener el máximo partido de la serie, véalos en orden. [Ir a la lista de vídeos](#other-videos-in-this-series)

> [AZURE.VIDEO data-science-for-beginners-series-the-5-questions-data-science-answers]

## Transcripción: Las cinco preguntas a las que responde la ciencia de datos

¡Hola! Bienvenido a la serie de vídeos *Ciencia de datos para principiantes*.

La ciencia de datos puede ser algo intimidante, por lo que he presentado aquí los fundamentos básicos sin ecuaciones ni jerga de programación.

En este primer vídeo, hablaremos sobre "Las cinco preguntas a las que responde la ciencia de datos".

La ciencia de datos utiliza números y nombres (también denominados categorías o etiquetas) y predice respuestas a preguntas.

Puede sorprenderle, pero *solo hay cinco preguntas a las que responde la ciencia de datos*:

  * ¿Esto es A o B?
  * ¿Es extraño?
  * ¿Cuánto? o ¿cuántos?
  * ¿Cómo está organizado?
  * ¿Qué debo hacer a continuación?

  Cada una de estas preguntas se responde mediante una familia de métodos de aprendizaje automático independientes, denominados algoritmos.


Resulta útil pensar en un algoritmo como una receta y los datos como los ingredientes. Un algoritmo indica cómo combinar y mezclar los datos para obtener una respuesta. Los equipos son similares a una batidora. Llevan a cabo una gran parte del trabajo duro del algoritmo y lo hacen bastante rápido.

## Pregunta 1: ¿esto es A o B? utiliza algoritmos de clasificación

Comenzamos con la siguiente pregunta: ¿esto es A o B?

![Algoritmos de clasificación: ¿esto es A o B?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-classification-algorithms.png)

Esta familia de algoritmos se llama clasificación de dos clases.

Es útil para cualquier pregunta que solo tenga dos respuestas posibles.

Por ejemplo:

  *	Se romperá este neumático en las próximas 1000 millas ¿sí o no?
  *	Qué aporta más clientes ¿un cupón de 5 USD o un descuento del 25 %?

Esta pregunta también puede modificarse para que incluya más de dos opciones: ¿esto es A o B o C o D, etc.? Esto se denomina clasificación multiclase y es útil cuando hay varias respuestas, o varios miles de respuestas, posibles. La clasificación multiclase elige la más probable.

## Pregunta 2: ¿es extraño? utiliza algoritmos de detección de anomalías

La siguiente pregunta que la ciencia de datos puede responder es: ¿es extraño? Esta pregunta se responde mediante una familia de algoritmos denominada detección de anomalías.

![Algoritmos de detección de anomalías: ¿es extraño?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-anomaly-detection-algorithms.png)


Si tiene una tarjeta de crédito, ya se habrá beneficiado de la detección de anomalías. Su compañía de tarjetas de crédito analiza sus patrones de compra para poder alertarle de un posible fraude. Cargos "extraños" podrían ser una compra en una tienda donde normalmente no compra o comprar un artículo inusualmente caro.

Esta pregunta puede ser útil de muchas formas. Por ejemplo:

  *	Si tiene un automóvil con indicadores de presión, puede desear saber: ¿la lectura de este medidor de presión es normal?
  *	Si está supervisando Internet puede desear saber: ¿este mensaje de Internet es típico?

La detección de anomalías marca eventos o comportamientos inesperados o poco habituales. Proporciona pistas sobre dónde buscar problemas.



## Pregunta 3: ¿cuánto? o ¿cuántos? utiliza algoritmos de regresión

Aprendizaje automático también puede predecir la respuesta a ¿cuánto? o ¿cuántos? La familia de algoritmos que responde a esta pregunta se denomina regresión.

![Algoritmos de regresión: ¿Cuánto? o ¿Cuántos?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-regression-algorithms.png)


Los algoritmos de regresión realizan predicciones numéricas, como:

  *	¿Qué temperatura hará el martes que viene?
  *	¿Cuáles serán las ventas del cuarto trimestre?

Ayudan a responder cualquier pregunta que pueda solicitar un número.

## Pregunta 4: ¿cómo está organizado? utiliza algoritmos de clústeres

Ahora bien, las dos últimas preguntas son un poco más avanzadas.

A veces deseará comprender la estructura de un conjunto de datos: ¿cómo está organizado? Para esta pregunta, no hay ejemplos para los que ya conozca resultados.

Hay muchas maneras de determinar la estructura de datos. Un enfoque son los clústeres. Separa los datos en "grupos" naturales para interpretarlos más fácilmente. Con los clústeres no hay una respuesta correcta.

![Algoritmos de clústeres ¿Cómo está organizado?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-clustering-algorithms.png)

Algunos ejemplos comunes de preguntas con agrupación en clústeres son:

  *	¿A qué espectadores les gusta el mismo tipo de películas?
  *	¿Qué modelos de impresora generan errores del mismo modo?

Al comprender cómo se organizan los datos, puede comprender y predecir mejor eventos y comportamientos.

## Pregunta 5: ¿qué debo hacer ahora? utiliza algoritmos de aprendizaje reforzado

La última pregunta: ¿qué debo hacer ahora? utiliza una familia de algoritmos llamados aprendizaje reforzado.

El aprendizaje reforzado se inspiró en cómo responde el cerebro de las ratas y los seres humanos los castigos y las recompensas. Estos algoritmos aprenden de los resultados y deciden la acción siguiente.

Normalmente, el aprendizaje reforzado es una buena elección para los sistemas automatizados que tengan que tomar una gran cantidad de pequeñas decisiones sin ayuda humana.

![Algoritmos de aprendizaje reforzado ¿Qué debo hacer a continuación?](./media/machine-learning-data-science-for-beginners-the-5-questions-data-science-answers/machine-learning-data-science-reinforcement-learning-algorithms.png)

Las preguntas a las que responde son siempre sobre la acción que debe llevar a cabo una máquina o un robot (normalmente). Algunos ejemplos son:

  *	Si soy un sistema de control de temperatura de una casa: ¿ajusto la temperatura o la dejo como está?
  *	Si soy un automóvil sin conductor: ante un semáforo en ámbar ¿freno o acelero?
  *	Para un robot aspirador: ¿Sigo aspirando o vuelvo a la estación de carga?

Los algoritmos de aprendizaje reforzado recopilan datos a medida que avanzan, aprendiendo mediante prueba y error.

Eso es todo: las cinco preguntas a las que puede responder la ciencia de datos.



## Otros vídeos de la serie

*Ciencia de datos para principiantes* es una introducción rápida a la ciencia de datos en cinco vídeos de corta duración. Vea los cuatro vídeos:

  * Vídeo 2: [¿Están sus datos preparados para la ciencia de datos?](machine-learning-data-science-for-beginners-is-your-data-ready-for-data-science.md) **Ya disponible.**
  * Vídeo 3: [Realización de preguntas que pueden responderse con datos](machine-learning-data-science-for-beginners-ask-a-question-you-can-answer-with-data.md). **Ya disponible.**
  * Vídeo 4: [Predicción de respuestas con un modelo sencillo](machine-learning-data-science-for-beginners-predict-an-answer-with-a-simple-model.md). **Ya disponible.**
  * Vídeo 5: [Copia del trabajo de otras personas para realizar ciencia de datos](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md). Disponible el 30 de junio.

## Pasos siguientes

  * [Prueba de su primer experimento de ciencia de datos con Aprendizaje automático de Azure](machine-learning-create-experiment.md)
  * [Introducción a Aprendizaje automático en Microsoft Azure](machine-learning-what-is-machine-learning.md)

<!---HONumber=AcomDC_0629_2016-->