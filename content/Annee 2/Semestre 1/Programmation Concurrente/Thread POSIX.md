---
title: Thread POSIX
draft: false
tags:
  - PCO
---
# Libraire POSIX Threads

Libraire C normalisée POSIX pour la gestion et synchronisation des threads : `pthread.h`
## Compilation et édition des liens

```c
gcc -Wall -Wextra -std=gnull prg.c -o prg -lpthread
```

> [!warning] Attention
> `-lpthread` **DOIT** être le dernier argument !
## Déclaration d'un thread

```c
pthread_t thread;
```

# Création d'un thread

```c
int pthread_create(pthread_t *thread, const pthread_attr_t *attr, void * (*start_routine) (void *), void *arg);
```

- `thread` est **l'identifiant** (argument de sortie).
- `attr` permet de définir les attributs du thread (NULL = attributs par défaut)
- `start_routine` : **fonction exécutée** par le thread. **DOIT** avoir le prototype `void *fonction(void *data)`
- `arg` : argument passé à la fonction (NULL = pas d'argument)
- Renvoie 0 en cas de succès -> (**toujours** tester le succès de l'appel).
## Examples trivial

```c
#include <pthread.h>
#include <stdio.h>
#include <stdlib.h>
void *func(void __attribute__((unused)) *arg) {
	printf("Hello from our first thread!\n");
	return NULL;
}

int main(int argc, char *argv[]) {
	pthread_t thread;
	if (pthread_create(&thread, NULL, func, NULL) != 0) {
		perror("thread creation error");
		return EXIT_FAILURE;
	}
	return EXIT_SUCCESS;
}
```

- `thread` = thread
- `attr` = NULL
- `start_routine` = func
- `arg` = NULL

> [!warning] Attention
> Le `main()` se termine avant que le thread se finissent car un `return` est plus rapide qu'un `print`.
> Donc le print ne s'affichera jamais.
# Jointure

- **La jointure** est la forme de communication entre threads la plus simple.
- Joindre un thread signifie **bloquer jusqu'à la terminaison de celui-ci.**
- Le thread terminé **peut retourner une valeur** au thread ayant fait la jointure.

![[Pasted image 20241009143254.png]]

```c
int pthread_join(pthread_t thread, void **value_ptr);
```

- `*value_ptr` contient le pointeur de retour du thread. (Peut être un pointeur sur une valeur à retourner) 
- renvoie 0 en cas de succès.
## Exemple trivial

```c
void *func(void *arg) {
	char *msg = (char *) arg;
	printf("Message = %s\n", msg);
	return NULL;
}

int main(int argc, char *argv[]) {
	pthread_t t;
	char *msg = "Threads are awesome!";
	if (pthread_create(&t, NULL, func, msg) != 0) {
		perror("pthread_create");
		return EXIT_FAILURE;
	}
	if (pthread_join(t, NULL) != 0) {
		perror("pthread_join");
		return EXIT_FAILURE;
	}
return EXIT_SUCCESS;
}
```

Sur le terminal il y'aura :

```shell
Message = Threads are awesome!
```
# Terminaison d'un thread
## Deux manières de se terminer

1. Le thread se termine **lui-même** (auto-terminaison)
	- `return`
	- `pthread_exit` (*return est préférable*)
2. Un autre thread le termine. (mécanisme d'annulation)
	- `pthread_cancel` (pas vu dans ce cours)
## Auto terminaison

```c
void pthread_exit(void *value);
```

Termine le thread et **retourne value au thread** effectuant la jointure.
La fonction `exit` permet de terminer le processus, **termine instantanément tous les threads du processus**, peut importe quel thread a appelé `exit`.


> [!NOTE] Note
> Si `pthread_exit` est appelé dans le `main`, le programme bloque et attend que tous les threads se terminent avant de terminer le processus.
> Peut être utile si le `main` n'a plus rien à faire après avoir créé les threads.

# Attributs de thread
**A COMPLETER**
