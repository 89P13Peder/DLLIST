```c++
    #include "DLList.h"
```
Se incluye el header.
```c++
    template<typename Object>
    DLList<Object>::Node::Node(const Object &d, Node *p, Node *n)
        : data{d}, prev{p}, next{n} {}
```
Se implementa el constructor de la estructura `Node`, el cual inicializa cada miembro del nodo
con los valores pasados por parámetro (`const Object &d, Node *p, Node *n`)
- data va a recibir una copia del objeto &d.
- prev va a recibir un puntero hacia el nodo anterior.
- next va a recibir un puntero hacia el siguiente nodo.
```c++
template<typename Object>
DLList<Object>::Node::Node(Object &&d, Node *p, Node *n)
    : data{std::move(d)}, prev{p}, next{n} {}
```
Esta es la implementación del constructor por movimiento los valores desde los argumentos recibidos.

```c++
template<typename Object>
DLList<Object>::const_iterator::const_iterator() : current{nullptr} {}
```
Implementación del constructor por defecto del iterador, que inicializa un iterador que apunta a un nodo nulo.

```c++
template<typename Object>
const Object &DLList<Object>::const_iterator::operator*() const {
    return retrieve();
}
```
Implementación de la sobrecarga del operador de apuntador `*`, esta implementación regresará una referencia
constante al objeto almacenado en el nodo al que apunta el iterador.
La función `retrieve()` es para saber el valor del nodo al que apunta el iterador, y se devuelve ese valor.

```c++
template<typename Object>
typename DLList<Object>::const_iterator &DLList<Object>::const_iterator::operator++() {
    current = current->next;
    return *this;
}
```
Esta es la implementación de la sobrecarga del operador pre-incremento, esta lo que hace
es que el iterador actual ahora apuntará al siguiente nodo, y posteriormente se retorna una referencia a ese iterador.

```c++
template<typename Object>
typename DLList<Object>::const_iterator DLList<Object>::const_iterator::operator++(int) {
    const_iterator old = *this;
    ++(*this);
    return old;
}
```
Esta es la implementación de la sobrecarga del operador de post-incremento.
Se crea una copia del iterador actual, almacenando su estado en `old`. Luego el iterador se incrementa.
Finalmente retorna la copia al iterador almacenada en `old`, que contiene el estado de este antes de que se incrementara.

```c++
template<typename Object>
bool DLList<Object>::const_iterator::operator==(const const_iterator &rhs) const {
    return current == rhs.current;
}
```
Implementación de la sobrecarga del operador de igualdad. Se retorna la respuesta que de
la comparación entre el iterador actual con el que es pasado como copia por parámetro, para saber si estos son iguales o no.

```c++
template<typename Object>
bool DLList<Object>::const_iterator::operator!=(const const_iterator &rhs) const {
    return !(*this == rhs);
}
```
Implementación de la sobrecarga del operador de desigualdad, este recibe una copia a `const_iterator`,
y retorna si el iterador actual no es igual al pasado por parámetro.
```c++
template<typename Object>
Object &DLList<Object>::const_iterator::retrieve() const {
    return current->data;
}
```
La implementación del método `retrieve()` devuelve el valor del nodo al que apunta el iterador.

```c++
template<typename Object>
DLList<Object>::const_iterator::const_iterator(Node *p) : current{p} {}
```
Esta implementación del método `const_iterator(Node *p)` asigna un puntero de un nodo al iterador actual.

```c++
template<typename Object>
DLList<Object>::iterator::iterator() {}
```
Implementación del constructor de un iterador por defecto.

```c++
template<typename Object>
Object &DLList<Object>::iterator::operator*() {
    return const_iterator::retrieve();
}
```
Implementación del operador `*`, devuelve el valor que contiene el nodo del iterador.
```c++
template<typename Object>
const Object &DLList<Object>::iterator::operator*() const {
    return const_iterator::operator*();
}
```
Implementación del operador `*`, devuelve una referencia del valor que contiene el nodo del iterador.
```c++
```c++
template<typename Object>
typename DLList<Object>::iterator &DLList<Object>::iterator::operator++() {
this->current = this->current->next;
return *this;
}
```
Implementación del operador de pre-incremento.
Este lo que hace es que se accede al iterador actual y se iguala al siguiente nodo.
El iterador actual, va a avanzar de apuntar de apuntar a un nodo a apuntar al siguiente.

```c++
template<typename Object>
typename DLList<Object>::iterator &DLList<Object>::iterator::operator--() {
this->current = this->current->prev;
return *this;
}
```
Implementación de la sobrecarga del operador de pre-decremento del iterador de DLList.
Se accede al puntero del nodo al que apunta el iterador actual, luego este se iguala
al puntero del nodo previo a el actual.

```c++
template<typename Object>
typename DLList<Object>::iterator DLList<Object>::iterator::operator++(int) {
iterator old = *this;
++(*this);
return old;
}
```
Implementación de la sobrecarga del operador de post-incremento.
Primero se almacena una referencia al iterador actual en `old`, luego se aumenta el iterador actual,
y finalmente se retorna el estado anterior del iterador.
```c++
template<typename Object>
typename DLList<Object>::iterator DLList<Object>::iterator::operator--(int) {
    iterator old = *this;
    --(*this);
    return old;
}
```
Esta implementación es similar a la de arriba, pero esta vez es el operador de post-decremento.
Se realiza el mismo procedimiento de almacenar la referencia del iterador actual,
luego decrementar una vez este iterador y retornar el estado anterior al decremento.

```c++
template<typename Object>
typename DLList<Object>::iterator DLList<Object>::iterator::operator+(int steps) const {
    iterator new_itr = *this;
    for(int i = 0; i < steps; ++i) {
        ++new_itr;
    }
    return new_itr;
}
```
Esta implementación del operador de suma, avanza el iterador las veces que sean indicadas por parámetro.
Se almacena  una referencia al iterador nuevo `new_itr`.
Se itera las veces indicadas para aumentar el iterador almacenado en `new_itr`.
Se devuelve la posición a la que apunta este iterador al final del ciclo `for`.

```c++
template<typename Object>
DLList<Object>::iterator::iterator(Node *p) : const_iterator{p} {}
```
Implementación del constructor por defecto (protegido) del iterador, que recibe un puntero a un nodo.
Este puntero asignará `p` a `const_iterator`.
```c++
template<typename Object>
DLList<Object>::DLList() : theSize{0}, head{new Node}, tail{new Node} {
    head->next = tail;
    tail->prev = head;
}
```
Implementación del constructor por defecto de una DLList.
Inicializa la lista con un valor de 0 elementos, una Nodo inicial y un Nodo final.
Con 0 elementos se refiere a que no habrá elementos entre head y tail.
Al puntero `next` de `head`, se le asigna un una referencia `tail` de la lista.
Al puntero `prev` de `tail` se le asigna una referencia a `head` de la lista.
Básicamente head tendrá un puntero hacia `tail` y tail uno hacia `head`.
```c++
template<typename Object>
DLList<Object>::DLList(std::initializer_list<Object> initList) : DLList() {
    for(const auto &item : initList) {
        push_back(item);
    }
}
```
Implementación de la lista de inicialización de una DLList.
En un ciclo for se irán colocando los elementos indicados por parámetro mediante
el uso del método push_back.

```c++
template<typename Object>
DLList<Object>::~DLList() {
    clear();
    delete head;
    delete tail;
}
```
Implementación del destructor
Usa el método clear(), para eliminar cada elemento de la DLList.
Finalmente elimina head y tail, para deshacerse de todo ese almacenamiento ocupado.

```c++
template<typename Object>
DLList<Object>::DLList(const DLList &rhs) : DLList() {
    for(auto &item : rhs) {
        push_back(item);
    }
}
```
Implementación de constructor de una lista con copias de valores proporcionados.
El método `push_back` va a introducir cada valor de la lista cada iteración del ciclo.

```c++
template<typename Object>
DLList<Object> &DLList<Object>::operator=(const DLList &rhs) {
    DLList copy = rhs;
    std::swap(*this, copy);
    return *this;
}
```
Implementación del operador de asignación, se iguala a una DLList constante por copia.
Se almacena esta DLList en `copy`. Con el método `swap()` lo que hace es intercambiar o 
re asignar un valor, en este caso `*this` es la referencia a la lista actual y copy la lista del lado derecho de la 
asignación. `Swap()` va a intercambiar los contenidos de `*this` y `copy`.
Después se retorna la referencia de la instancia lista de DLList actual `*this`. 
```c++
template<typename Object>
DLList<Object>::DLList(DLList &&rhs) : theSize{rhs.theSize}, head{rhs.head}, tail{rhs.tail} {
    rhs.theSize = 0;
    rhs.head = nullptr;
    rhs.tail = nullptr;
}
```
Esta es la implementación del constructor de movimiento implícito de la clase DLList.
Este nos permite transferir recursos desde un objeto origen(rhs) a un objeto destino (la instancia de la DLList que se va a construir).

Lo que hará es inicializar los miembros de datos de esta lista  `theSize{rhs.theSize}, head{rhs.head}, tail{rhs.tail}`
con los miembros de datos de la instancia `rhs`. La nueva instancia de DLList se "roba" los datos
de la instancia `rhs`. 
 Despues de transferir estos recursos, a `rhs` se le iguala el tamaño a 0, y sus dos propiedades head y tail se vuelven nulas para evitar los recursos 
duplicados ni datos en un estado inconsistente.
```c++
template<typename Object>
DLList<Object> &DLList<Object>::operator=(DLList &&rhs) {
    std::swap(theSize, rhs.theSize);
    std::swap(head, rhs.head);
    std::swap(tail, rhs.tail);
    return *this;
}

```
Esta implementación del operador de asignación, mueve los valores de los miembros de datos de una lista a otra.
En este caso rhs será un rvalue (estará del lado derecho de la asignación) y cuando se realice esto
los miembros de datos (theSize, head y tail)de la DLList izquierda se pasarán a la nueva instancia de la DLList.
```c++
template<typename Object>
typename DLList<Object>::iterator DLList<Object>::begin() {
    return {head->next};
}
```
Este método solo retorna el iterador que apunta al primer nodo de la lista.
...
```c++

template<typename Object>
typename DLList<Object>::const_iterator DLList<Object>::begin() const {
return {head->next};
}
```
Este método devuelve un iterador constante que apunta al nodo principal de la lista.(No realizará cambios en el nodo)

```c++
template<typename Object>
typename DLList<Object>::iterator DLList<Object>::end() {
    return {tail};
}
```
Este método devuelve el iterador que apunta al nodo final de la DLList.
```c++

template<typename Object>
typename DLList<Object>::const_iterator DLList<Object>::end() const {
return {tail};
}
```
Este método devuelve el iterador constante que apunta al nodo final de la DLList.
```c++
template<typename Object>
int DLList<Object>::size() const {
    return theSize;
}
```
Este método devuelve el tamaño de la DLList.
```c++
template<typename Object>
bool DLList<Object>::empty() const {
    return size() == 0;
}
```
Este método devuelve true si la lista está vacía y devuelve false en caso contrario.
```c++
template<typename Object>
void DLList<Object>::clear() {
    while(!empty()) {
        pop_front();
    }
}
```
Este método elimina los elementos de la DLList.
```c++
template<typename Object>
Object &DLList<Object>::front() {
    return *begin();
}
```
Este método devuelve una referencia al valor almacenado en el primer nodo de la DLList.
```c++
template<typename Object>
const Object &DLList<Object>::front() const {
    return *begin();
}
```
Este método devuelve una referencia constante del valor almacenado en el nodo.
```c++
template<typename Object>
Object &DLList<Object>::back() {
    return *(--end());
}
```
Este método devuelve una referencia al valor almacenado en el último nodo de la lista.
- `return *(--end());` obtener un iterador que apunte al nodo detrás del último elemento de la lista. Luego, se utiliza el operador de decremento `(--)` para obtener un iterador que apunte al último elemento de la lista.
Se usa el operador de desreferencia para obtener el valor al que apunta ese iterador.
Devuelve una referencia al valor almacenado en el úlitmo nodo de la DLList.
```c++
template<typename Object>
const Object &DLList<Object>::back() const {
    return *(--end());
}
```
Realiza un procedimiento igual al anterior método, esta vez de manera constante para que no 
se realicen modificaciones en los valores del nodo.
```c++
template<typename Object>
void DLList<Object>::push_front(const Object &x) {
    insert(begin(), x);
}
```
Este método coloca un nuevo elemento al principio de la lista haciendo uso del método anteriormente
implementado `begin()`.
```c++
template<typename Object>
void DLList<Object>::push_front(Object &&x) {
    insert(begin(), std::move(x));
}
```
Este método realiza un movimiento de un nuevo elemento `x` en una lista.
```c++
template<typename Object>
void DLList<Object>::push_back(const Object &x) {
    insert(end(), x);
}
```
Este método inserta una copia de un objeto constante al final de la DLList.
```c++
template<typename Object>
void DLList<Object>::push_back(Object &&x) {
    insert(end(), std::move(x));
}
```
Este método realiza un movimiento de un objeto al final de la DLList.
```c++
template<typename Object>
void DLList<Object>::pop_front() {
erase(begin());
}
```
Este método elimina el primer elemento de una lista.
```c++
template<typename Object>
void DLList<Object>::pop_back() {
    erase(--end());
}
```
Este método elimina el último elemento de una lista.
```c++
template<typename Object>
typename DLList<Object>::iterator DLList<Object>::insert(iterator itr, const Object &x) {
    Node *p = itr.current;
    theSize++;
    return {p->prev = p->prev->next = new Node{x, p->prev, p}};
}
```
Este método inserta una copia de un objeto constante en la posición indicada por el iterador pasado por parámetro.
1. Se obtiene el punter del nodo apuntado por el iterador actual `itr`.
2. Se aumenta el tamaño de la lista en uno.
3. Se crea un nuevo nodo con el valor de `x` y se inserta en la lista antes de la posición
indicada por el iterador `itr`. El nuevo nodo se enlaza con el anterior y con el nodo apuntado por `itr` (p).
```c++
template<typename Object>
typename DLList<Object>::iterator DLList<Object>::insert(iterator itr, Object &&x) {
    Node *p = itr.current;
    theSize++;
    return {p->prev = p->prev->next = new Node{std::move(x), p->prev, p}};
}
```
Este método inserta un nuevo nodo mediante el movimiento de recursos de la misma manera que el método anterior,
pero haciendo uso del metodo `move()` que se encarga de hacer el movimiento del objeto al nodo isertado en la posición indicada por el iterador.
```c++
template<typename Object>
void DLList<Object>::insert(int pos, const Object &x) {
    insert(get_iterator(pos), x);
}
```
Este método inserta una copia de un nodo nuevo en la posición indicada.
- `insert(get_iterator(pos), x);` para obtener el iterador correspondiente a la posición pos, se utiliza el método privado get_iterator(pos).
```c++
template<typename Object>
void DLList<Object>::insert(int pos, Object &&x) {
    insert(get_iterator(pos), std::move(x));
}
```
Este método realiza la misma acción que el anterior pero mediante el movimiento de recursos.

```c++
template<typename Object>
typename DLList<Object>::iterator DLList<Object>::erase(iterator itr) {
    Node *p = itr.current;
    iterator retVal(p->next);
    p->prev->next = p->next;
    p->next->prev = p->prev;
    delete p;
    theSize--;
    return retVal;
}
```
Este método elimina el elemento apuntado por el iterador.
-` Node *p = itr.current;`: Se obtiene un puntero al nodo actual apuntado por el iterador itr.
-` iterator retVal(p->next);`: Se crea un iterador retVal que apunta al siguiente elemento después del elemento que se eliminará. Esto se hace almacenando el puntero al siguiente nodo en retVal antes de eliminar el nodo actual, ya que una vez eliminado, el iterador actual apuntará a un nodo que ya no existe.
- `p->prev->next = p->next;`: Se ajustan los punteros del nodo anterior al nodo actual y del nodo siguiente al nodo actual para "saltar" el nodo actual y eliminarlo de la lista.

-` p->next->prev = p->prev;`: Se ajusta el puntero del nodo siguiente al nodo actual para que apunte al nodo anterior al nodo actual, completando así la eliminación del nodo actual de la lista.

-` delete p;`: Se elimina el nodo actual de la memoria.

-` theSize--;`: Se decrementa el tamaño de la lista en uno para reflejar la eliminación del elemento.

-` return retVal;`: Se devuelve el iterador retVal que apunta al siguiente elemento después del elemento eliminado.
```c++
template<typename Object>
void DLList<Object>::erase(int pos) {
erase(get_iterator(pos));
}
```
Elimina el nodo apuntado por el iterador de la posición indicada.
```c++
template<typename Object>
typename DLList<Object>::iterator DLList<Object>::erase(iterator from, iterator to) {
    for(iterator itr = from; itr != to;) {
        itr = erase(itr);
    }
    return to;
}
```
Elimina los elementos desde una posición indicada hasta otra.
Retorna solo el iterador del elemnto final del rango que queríamos eliminar.
```c++
template<typename Object>
void DLList<Object>::print() const {
    for(const auto &item : *this) {
        std::cout << item << " ";
    }
    std::cout << std::endl;
}
```
Este método imprime todos los elementos de la lista.
```c++
template<typename Object>
void DLList<Object>::init() {
    theSize = 0;
    head = new Node;
    tail = new Node;
    head->next = tail;
    tail->prev = head;
}
```
Inicializa una DLList con un head y tail, sin valores entre ellos y señalando que el 
puntero al siguiente nodo de head es tail y el puntero al nodo anterior a tail es head.

```c++
template<typename Object>
typename DLList<Object>::iterator DLList<Object>::get_iterator(int pos) {
    iterator itr = begin();
    for(int i = 0; i < pos; ++i) {
        ++itr;
    }
    return itr;
}
```
Este método devuelve un iterador que apunta al elemento en la posición indicada.



