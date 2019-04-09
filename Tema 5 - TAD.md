# Tema 5 - TADs

## Tipo de Abstracción de Datos

### 5.1 Definición

Un TAD es un tipo organizado de forma que separamos:

* La **descripción** de las operaciones (qué hacen)
* La **implementación** (cómo lo hacen)

Las operaciones visibles del TAD se conocen como *operaciones de la interfície*. En C, serían las **cabeceras** de las funciones escritas en el header (.h).

Un TAD puede tener también **operaciones privadas** que no son visibles desde el exterior (están implementadas en el .c).

### Ventajas

* Permiten trabajar con **conceptos**, independientemente de la forma que estos se implementen y cómo se guarden los tipos usados.
* Facilita el **mantenimiento** de código.
* **Reutilización** de código: un TAD se puede usar en varios programas.
* Mejor **organizacón** del software.
* Fácil de **comprobar/detectar** errores. Un TAD se puede *testear* por separado.

---

### 5.2 Estructuras de datos lineales

Principales estructuras de datos lineales:

1. Stack
2. Queue
3. List

> Cada estructura (TAD) tendrá diferentes operaciones de interfície (añadir, consultar, actualizar y eliminar información).

#### 5.2.1 Stack

Organiza los elementos usando una política LIFO (Last In First Out).

<details>

<summary>Image</summary>

![Stack](./Stack.png)

</details>

##### Operaciones

| Name    |   Parameters   |  Return |
| :------ | :------------: | ------: |
| Create  |       ∅        |   Stack |
| Push    | Stack, element |    Pila |
| Pop     |     Stack      |   Stack |
| Top     |     Stack      | element |
| Void?   |     Stack      | boolean |
| Destroy |     Stack      |       ∅ |

##### Representaciones

* Estática

    Una pila de elementos ```int```.

    ### Header

    ```c
    #ifndef _STACK_H_
    #define _STACK_H_

    #define MAXE    10

    typedef struct {
        int elements[MAXE];
        int last;
    } Stack;

    // Operaciones

    Stack STACK_Create();

    Stack STACK_Push(Stack, int);

    Stack STACK_Pop(Stack);

    int STACK_Top(Stack);

    int STACK_Empty(Stack);

    void STACK_Delete(Stack *);

    #endif
    ```

    ### Source

    ```c
    #include "stack.h"

    Stack STACK_Create() {
        Stack stack;

        stack.last = -1;

        return stack;
    }

    // Pre-condition: The stack is not full
    // (Hay que añadir una operación STACK_Full
    Stack STACK_Push1(Stack stack, int e) {

        stack.last++;
        stack.elements[stack.last] = e;

        return stack;
    }

    // version 2 (checking full stack)

    // Post-condition: return 1 if it could be added
    // return 0 if it couldn't
    int STACK_Push2(Stack *stack, int e) {

        if (stack.last + 1 = MAXE)
            return 0;

        (stack->last)++;
        stack->elements[stack->last] = e;

        return 1;
    }

    // Pre-condition: stack is not empty
    Stack STACK_Pop1(Stack stack) {

        stack.last--;
        return stack;
    }

    // version 2 (good one)
    int STACK_Pop2(Stack *stack) {

        if (stack->last - 1 < 0)
            return 0;

        (stack->last)--;

        return 1;
    }

    int STACK_Top(Stack stack) {

        if (stack.last < 0)
            return UNDEFINED; // ej: -1

        return stack.elements[stack.last];
    }

    // Post-condiction: return 1 if it's empty
    // return 0 if it's not empty
    int STACK_Empty(Stack stack) {

        /*
        if (stack.last < 0) {
            return 1;
        } else {
            return 0
        }
        */
        return stack.last < 0;
    }

    void STACK_Delete(Stack *stack) {
        stack->last = -1
    }
    ```

* Dinámica

    Una pila de elementos ```int```.

    ### Header

    ```c
    #ifndef _STACK_H_
    #define _STACK_H_

    #define MAXE    10

    typedef struct _node {
        int element;
        struct _node *next;
    } Node;

    typedef struct {
        int size; // total elements
        // .. metadatos (max, min, etc.)
        Node *last;
    } Stack;

    // Operaciones

    Stack STACK_Create();

    int STACK_Push(Stack *, int);

    int STACK_Pop(Stack);

    int STACK_Top(Stack);

    int STACK_Empty(Stack);

    void STACK_Delete(Stack *);

    #endif
    ```

    ### Source

    ```c
    #include "stack.h"

    Stack STACK_Create() {
        Stack stack; // = { 0, NULL }

        stack.size = 0;
        stack.last = NULL;

        return stack;
    }

    int STACK_Push(Stack *stack, int e) {
        Node *node = (Node *) malloc(sizeof(Node));

        if (node == NULL)
            return 0;

        node->element = e;
        node->next = stack->last;

        stack->last = node;
        (stack->size)++;

        return 1;
    }

    int STACK_Pop(Stack *stack) {

        if (stack->last == NULL)
            return 0;

        Node *tmp = stack->last;
        stack->last = stack->last->next;
        free(tmp);

        return 1;
    }

    int STACK_Top(Stack *stack, int *e) {

        if (stack->last == NULL)
            return UNDEFINED;

        *e = stack->last->elemet;

        return 1;
    }

    int STACK_Empty(Stack stack) {

        return stack.last == NULL;
    }

    void STACK_Delete(Stack *stack) {

        /*
        while (stack->last != NULL)
            STACK_Pop(stack);
        */

        while (STACK_Pop(stack));
    }
    ```

#### 5.2.2 Queue

Organiza los elementos usando una política FIFO (First In First Out).

##### Operaciones

| Name    |   Parameters   |  Return |
| :------ | :------------: | ------: |
| Create  |       ∅        |   Queue |
| Enqueue | Queue, element |   Queue |
| Dequeue |     Queue      |   Queue |
| Top     |     Queue      | element |
| Void?   |     Queue      | boolean |
| Destroy |     Queue      |       ∅ |

##### Representaciones

* Estática

    Una fila de elementos ```int```.

    ### Header

    ```c
    #ifndef _QUEUE_H_
    #define _QUEUE_H_

    #define MAXE    10

    typedef struct {
        int elements[MAXE];
        int first;
        int last;
        int count;
    } Queue;

    // Operaciones

    Queue QUEUE_Create();

    int QUEUE_Enqueue(Queue *, int);

    int QUEUE_Dequeue(Queue);

    int QUEUE_Top(Queue);

    int QUEUE_Empty(Queue);

    void QUEUE_Delete(Queue *);

    #endif
    ```

    ### Source

    ```c
    #include "queue.h"

    Queue QUEUE_Create() {
        Queue queue = { NULL, 0, 0, 0 };

        return queue;
    }

    int QUEUE_Enqueue(Queue *queue, int e) {

        if (queue->count + 1 == MAXE)
            return 0;

        queue->elements[queue->last] = e;
    //  queue->last = (queue->last + 1) % MAXE
        if (queue->last + 1 == MAXE)
            queue->last = 0;
        else (queue->last)++;

        (queue->count)++;

        return 1;
    }

    int QUEUE_Dequeue(Queue *queue) {

        if (queue->count == NULL)
            return 0;

        if (queue->first + 1 == MAXE)
            queue->first = 0;
        else (queue->first)++;

        (queue->count)--;

        return 1;
    }

    int QUEUE_Top(Queue queue, int *e) {

        if (queue.count < 0)
            return UNDEFINED; // ej: INT_MAX

		*e = queue.elements[queue.first];

        return 1;
    }

    int QUEUE_Empty(Queue queue) {

        /*
        if (queue.last < 0) {
            return 1;
        } else {
            return 0
        }
        */
        return queue.last < 0;
    }

    void QUEUE_Delete(Queue *queue) {
        queue->count = queue->first = queue->last = 0;
    }
    ```

* Dinámica

    ### Header

    ```c
    #ifndef _QUEUE_H_
    #define _QUEUE_H_

    typedef struct _node {
        int e;
        Node _node *next;
    } Node;

    typedef struct {
        Node *first;
        Node *last;
        int size;
    } Queue;

    // Operaciones

    Queue QUEUE_Create();

    int QUEUE_Enqueue(Queue *, int);

    int QUEUE_Dequeue(Queue);

    int QUEUE_Top(Queue);

    int QUEUE_Empty(Queue);

    void QUEUE_Delete(Queue *);

    ```

	### Source

    ```c
    #include "queue.h"

    Queue QUEUE_Create() {
        Queue queue = { NULL, NULL, 0 }

        return queue;
    }

    int QUEUE_Enqueue(Queue *queue, int e) {

        Node *node = (Node *) malloc(sizeof(Node));

        if (*node == NULL)
            return 0;

        *node = { e, NULL };

        if (queue->size == 0)
            queue->first = node;
        else queue->last->next = node;

        queue->last = node;
        (queue->size)++;

        return 1;
    }

    int QUEUE_Dequeue(Queue queue) {

        if (queue->size == 0)
            return 0;

        Node *tmp = queue->first;
        queue->first = tmp->next;
        free(tmp);

        (queue->size)--;

        if (queue->size == 0)
            queue->first = queue->last = NULL;

        return 1;
    }

    int QUEUE_Top(Queue *queue, int *e) {

        if (queue->size == 0)
            return 0;

        *e = queue->first->e;

        return 1;
    }

    int QUEUE_Empty(Queue *queue) {
        return queue->size == 0;
    }

    void QUEUE_destroy(Queue *queue) {
        int has_next = 1;

        while (has_next) {
          has_next = QUEUE_Dequeue(queue);
        }

    }

    ```
  ### 5.2.3 Listas
  Permiten organizar, recorrer y acceder a una sequencia de elementos sequencialmente

  ![Lists](/assets/img/lists.png)
  ## Tipos de Listas
  * PDI (Punto de Interés)

  ![PDI](/assets/img/pdi.png)
  * Bidireccional

  ![PDI](/assets/img/bidirectional.png)
