---
layout: post
title: "Creando imágenes para Marketing con GenAI"
description: "En mi trabajo me tocó generar imágenes con GenAI para campañas de marketing. Quise desafiarme e ir un poco más allá con un desafío distinto e interesante."
image: /imgs/imagen_portada_post3_v6.jpeg
---

<img src="/imgs/imagen_portada_post3_v8.jpeg" style="width: 100%; max-width: none; margin: 0 auto; display: block;" alt="Planner">

## Introducción

En los últimos 3 meses en mi trabajo estuve dedicado a un problema que tenía relación con la creación de imágenes con IA para campañas de marketing. Hoy existen modelos generativos que crean imágenes a partir de prompts como DALL-E (OpenAI), Stable Diffusion (Stability AI), MidJourney e Imagen3 de Google. Este último fue el que utilicé para el problema en mi trabajo y la verdad que los resultados son sorprendentes. Ahora bien, ¿Cómo funcionan estos modelos? Creas un prompt (instrucción) basándote en lo que quieres y el modelo crea la imagen que tú estás solicitando. Veámoslo con un ejemplo que se acerca un poco a lo que tuve que hacer para marketing utilizando GenAI.

<br>
<figure>
<img src="/imgs/imagen_post3c_1.jpeg" style="width: 95%; max-width: none; margin: 0 auto; display: block;" alt="Foto_Intro">
<figcaption style="text-align: center; font-style: italic;">Figura 1: Prompt -> Imagen creada con IA.</figcaption>
</figure>
<br>

Hoy en el tercer post de Ideas en Papel quiero desafiarme y hacer algo interesante. Pensaba, ¿Qué pasa si, como marca, ya tienes definido el perfil de tu cliente y deseas poner a ese mismo cliente en distintos contextos y además de eso, realizando ciertas acciones que tú decidas, pero sin utilizar agencias, imágenes genéricas o diseñadores? 
Es decir, la imagen o representación del perfil de cliente no se mueve porque está definido, pero lo que sí se mueve o cambia son 2 cosas:

1. El entorno, contexto o fondo de tu cliente. Ejemplo: En un restaurant, fiesta, supermercado, etc.
2. Las acciones que realiza tu cliente dentro de una imagen. Ejemplo: Saltando, conversando, mirando su celular, etc.

En un esquema sería algo como esto:

<br>
<figure>
<img src="/imgs/imagen_post_3_2.jpeg" style="width: 95%; max-width: none; margin: 0 auto; display: block;" alt="Foto_Intro">
<figcaption style="text-align: center; font-style: italic;">Figura 2: Explicación problema.</figcaption>
</figure>
<br>

¿En qué ayudaría esto? Si quieres automatizar las imágenes para distintas comunicaciones, awareness o mail de ventas hacia tus clientes y hacerlo 100% con inteligencia artificial, pero resguardando que el perfil de tu cliente aparezca presente en tus imágenes, esto sería una posible solución.

¿Interesante o no? Para explorar este mini proyecto lo haremos con un caso guiado seleccionando una cuenta digital bancaria y marca llamada [MACH](https://www.somosmach.com/). 

## Manos a la Obra

Primero que todo, necesitamos definir el *perfil de cliente* de la cuenta MACH, el *perfil de cliente* en marketing es la descripción del cliente en términos de sus características y descripción ideal dado lo que quiero comunicar como marca o lo que quiero vender. (Esto dice internet jaja)
Pero, ¿Cómo obtenemos el perfil del cliente MACH, si no trabajo ahí con el equipo de marketing? Buena pregunta.
Lo que hice fue ingresar a su página y extraje un par de imágenes de clientes (no sabemos si esos son sus clientes, pero es lo que hay en su página) para luego utilizar Claude (LLM de Anthropic en su versión 3.5 sonnet) para que nos genere una descripción de estos clientes y así tendremos algo muy parecido a la descripción o brief de cliente. Teniendo la descripción se tomará este output y será tomado como input por un modelo de generación de imágenes de Google llamado Imagen3. A continuación, te presento el resultado de este experimento, donde tenemos las imágenes de los "MACHERS", la descripción que el modelo de lenguaje hizo sobre ellos y este output entra como input hacia Imagen3 de Google. 

**Cabe destacar que se tuvo que hacer ciertos ajustes en los prompts para llegar a los resultados que se muestran a continuación*

<br>
<figure>
<img src="/imgs/imagen_personajes_post_3.jpeg" style="width: 95%; max-width: none; margin: 0 auto; display: block;" alt="Foto_Intro">
<figcaption style="text-align: center; font-style: italic;">Figura 3: Clientes en formato IA.</figcaption>
</figure>
<br>

Si te das cuentas o por lo menos lo que me impresionó a mí es que solamente haciendo una buena descripción se llega a clientes bastante similares a los originales. 

En segundo lugar, luego de definir los perfiles de los clientes y sus respectivas descripciones creadas por IA debí crear una función que tomara como input esto y alimentará a otra función que tomara la descripción y el lugar/fondo que el usuario quiere junto con lo que está realizando el cliente. Si tomamos la figura 2 como ejemplo, la función toma como input la descripción de la persona que viene dado por nuestro **perfil de cliente**, el contexto que sería el paradero y además que está mirando su teléfono en dicho lugar. Con estos 3 elementos se crea un prompt que consolida estos 3 inputs y crea otro prompt que va hacia un modelo de generación de imágenes y genera el resultado final. En código, sin mostrar tanto detalle (porque mi trabajo no me lo permite) es algo así:
<br>
<br>

```python 
class Imagina:
    def __init__(self, temperature: int = 0, max_tokens: int = 8_192):
        pass
    
    def __repr__(self):
        pass
    
    @property
    def _create_token(self) -> str:
        pass
    
    def person_description(self, img_path: str) -> str:
        pass
    
    def create_context(
        self,
        prompt: str,
        situation: str,
        action: str
    ) -> str:
        pass
    
    def create_image(
        self,
        situation: str,
        action: str,
        n_images: int = 4,
        hard_prompt: str = None,
        img_path: str = None
    ) -> str:
        pass

if __name__ == "__main__":
    imagina = Imagina()
    img_path = ".fotos_clientes/img_1.jpg"
    situation = "En el paradero"
    action = "Mirando su celular. Se ve desde la cintura para arriba" # Esto ayuda, ya que los zoom in de las imágenes generalmente son muy cerca.
    imagina.create_image(
        img_path=img_path,
        n_images=1,
        hard_prompt=None,
        situation=situation,
        action=action
    )
```

Tomando el código podemos ver que la creación y la ejecución es secuencial. Más allá del detalle, lo que importa es que a partir de una imagen de un cliente se crea una descripción de él, luego de eso se crea una situación y una acción a realizar por parte de la persona y eso se consolida en un gran prompt que crea un nuevo prompt coherente y que adapta cada una de esas instrucciones deseadas y finalmente pasa esa información para crear la imagen con GenAI.

## Experimento

Ahora viene lo más interesante, si seguimos jugando un poco lo que podemos hacer es lo siguiente:

1. Generar un listado de contextos o lugares donde nos gustaría ver a nuestros clientes.
2. Definir un listado de acciones.
3. Hacer una iteración para cada cliente, para cada lugar y acción y mostrar los resultados para todos los clientes en sus distintas combinaciones de cliente-lugar-acción.

**Finalmente, dejé solamente a 2 clientes. Esto fue solamente por espacio y para que se pudiera apreciar el resultado final*

El listado de los lugares y situaciones será:

```python

places = [
    "supermercado", "centro comercial", 
    "restaurant", "nave espacial", 
    "montaña rusa"]

situations = ["con un amigo", "con su pareja", "enojado"]

```

El resultado final de las iteraciones para 2 clientes MACH y tomando cada uno de los lugares es el siguiente: 

**Te recomiendo hacer zoom para mirar los detalles.*

<br>
<figure>
<img src="/imgs/imagen_resultados_post_3.jpeg" style="width: 95%; max-width: none; margin: 0 auto; display: block;" alt="Foto_Intro">
<figcaption style="text-align: center; font-style: italic;">Figura 4: Resultados finales masivos.</figcaption>
</figure>
<br>

Personalmente, creo que son resultados bastante aceptables y que capturan a lo que quería llegar.

## Conclusión

Disfruté mucho trabajando en este problema y aprendí mucho de los modelos generativos aplicados a imágenes. Este post me tomó un poco más tiempo (2 semanas), puesto que la experimentación fue bastante larga hasta obtener resultados que creí que estaban a la altura. Por otro lado, creo que la generación de imágenes en marketing migrará un poco a esto, modelos generativos que son capaces de capturar la esencia de tu cliente y tú, como experto de marketing, mediante palabras, definir qué quieres que haga y dónde basándose en lo que se quiere transmitir. Por último, ojalá haber motivado a alguien del mundo Tech (y a los no tan Tech también) a desafiar el cómo hacemos las cosas y a mover la barra de lo que antes parecía imposible. Qué bonito momento de la tecnología para estar vivo y dar vida a las ideas.

