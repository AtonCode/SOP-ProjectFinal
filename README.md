# Sistema publicador/suscriptor usando hilos y pipes en GNU/LINUX

## El sistema se compone de tres modulos:

Publicador: proceso que envía noticias al sistema central (central.c) a través de un pipe nominal. Solo hay noticias de 2 topicos. Se envían 5 noticias de cada topico. El proceso se invoca asi:

$ ./Publicador pipepub


Suscriptor: Este proceso envía al sistema central un tópico, en el cual está interesado y recibe las noticias asociadas a ese topico. El proceso e invoca de la siguiente manera

$ ./Suscriptor pipesub topico

Donde pipesub, es el nombre del pipe que lo comunica con el proceso central. Por este pipe envía el tópico que es el segundo argumento y puede tomar valores 1 y 2 (no hay validación de argumentos)
Internamente este proceso crea otro pipe para recibir las noticias


Central: Es el proceso que recibe las noticias del publicador y los topicos de los suscriptores, hce el match y envía las noticias a los suscriptores correspondientes. Para realizar su tarea el proceso se compone de tres hilos:
- Un hilo lee las noticias del publicador y la pone en un buffer,
- El hilo principal está pendiente de la conexión de nuevos suscriptores
- Otro hilo se encarga de recoger las noticias del buffer y enviarlas a los suscriptores (se implementa un productor/consumidor)

El programa se invoca de la siguiente forma:
 $central pipesus pipepub

El primer argumento es el pipe de comunicación con los suscriptores y el segundo con el publicador.

Solo se manejan dos tópicos y se guarda únicamente la última noticia de cada tópico. Solo fue probado con un publicador que envía 10 noticias asociadas a dos topicos.  El suscriptor no necesariamente reciben todas las noticias, dependerá de su tiempo de creación y de las noticias que ya se hayan producido (solo se guarda la pultima)
