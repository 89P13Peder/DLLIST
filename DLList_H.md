```c++
    #include <utility>
    #include <iostream>
```
Se incluyen las librerías a utilizar.
```c++
    template <typename Object>
    class DLList {
```
... 

```c++
    private:
        struct Node {
            Object data;
            Node *prev;
            Node *next;
    
            Node(const Object &d = Object{}, Node *p = nullptr, Node *n = nullptr);
            Node(Object &&d, Node *p = nullptr, Node *n = nullptr);
        };
```
Como atributo privado se crea Node, el cual representará cada nodo en la lista.
Este contiene un `Object data` el cual contiene el dato que contendrá el nodo,
`Node *prev` es el apuntador hacia el nodo anterior y `Node *next` es el apuntador al siguiente nodo.
Se declaran los constructores, el primero inicializa los valores `data`, `prev` y `next` con los
valores proporcionados o con valores predeterminados.

El siguiente constructor inicializa los campos del nodo utilizando valores movidos desde los argumentos proporcionados, o inicializando con valores predeterminados.


```c++
    public:
        class const_iterator{
        public:
            const_iterator();
            const Object &operator*() const;
            const_iterator &operator++();
            const_iterator operator++ (int);
            bool operator== (const const_iterator& rhs) const;
            bool operator!= (const const_iterator& rhs) const;
```

Definición de una clase pública const_iterator, la cual tiene atributos públicos:
- `const_iterator()` Es el constructor predeterminado.
- `const Object &operator*() const` Es la sobrecarga del operador * para acceder a la información a la que apunta el iterador de forma constante(sin cambiar nada de este).
- `const_iterator &operator++()` Sobrecarga el operador de pre-incremento para aumentar el iterador.
- `const_iterator operator++ (int)` Sobrecarga el operador de post-incremento para avanzar el iterador.
- `bool operator== (const const_iterator& rhs) const;` Sobrecarga el operador de igualación para devolver true o false si se comparan dos iteradores que apuntan al mismo nodo o no.
- `bool operator!= (const const_iterator& rhs) const;` Sobrecarga el operador de desigualdad para saber si un iterador no apunta al mismo nodo.

```c++
        protected:
            Node* current;
            Object& retrieve() const;
            const_iterator(Node *p);
    
            friend class DLList<Object>;
        };
```
Estos son los atributos protegidos de la clase `const_iterator`:
- `Node* current`: puntero al nodo actual al que apunta al iterador.
- `Object& retrieve() const` Método protegido para recuperar el valor al que apunta el iterador.
- `const_iterator(Node *p);` Constructor que inicializa el iterador con un nodo dado.
```c++
    class iterator : public const_iterator {
    public:
        iterator();
        Object& operator*();
        const Object& operator*() const;
        iterator & operator++ ();
        iterator &operator--();
        iterator operator++ (int);
        iterator operator-- (int);
        iterator operator+ (int steps) const;
    
    protected:
            iterator(Node *p);
    
            friend class DLList<Object>;
    };
```
Se define otra clase dentro de la clase DLList, `iterator`, esta hereda comportamientos de la clase const_iterator para reutilizar su funcionalidad constante,x se declaran sus atributos públicos:
- `iterator();` Es el constructor por defecto.
- `Object& operator*();` Sobrecarga el operador `*`, para acceder y modificar el valor al que apunta el iterador.
- `const Object& operator*() const;` Sobrecarga el operador `*`, para acceder de forma constante al valor que apunta el iterador.
- `iterator & operator++ ();` Sobrecarga el operador de pre-incremento para aumentar el iterador.
- `iterator &operator--();` Sobrecarga el operador de pre-decremento para retroceder el iterador.
- `iterator operator++ (int);`Sobrecarga el operador de post-incremento para aumentar el iterador.
- `iterator operator-- (int);`Sobrecarga el operador de post-decremento para retroceder el iterador.
- `iterator operator+ (int steps) const;` Sobrecarga el operador de suma para avanzar el iterador la cantidad de veces que se indique.
```c++
    public:
        DLList();
        DLList(std::initializer_list<Object> initList);
        ~DLList();
        DLList(const DLList &rhs);
        DLList &operator=(const DLList &rhs);
        DLList(DLList &&rhs);
        DLList &operator=(DLList &&rhs);
```
Atributos públicos de la clase `DLList`:
-  `DLList()` Este es el constructor por defecto de la lista doblemente enlazada.
- `DLList(std::initializer_list<Object> initList)` Constructor que inicializa la lista doble con una lista de inicialización con los elementos indicados. 
- `~DLList()` Destructor por defecto para liberar memoria a la hora de dejar de usar el objeto `DLList`.
- `DLList(const DLList &rhs)` constructor de una DLList por copia constante.
- `DLList &operator=(const DLList &rhs)` Sobrecarga al operador de igualdad, para igualar una lista a otra.
```c++
    iterator begin();
    const_iterator begin() const;
    iterator end();
    const_iterator end() const;
```
- ` iterator begin();` Iterador que apunta al primer elemento de la lista.
- ` const_iterator begin() const;` Iterador constante que apunta al primer elemento de la lista de forma constante.
- `iterator end();` Iterador que apunta al nodo final de la lista.
- `const_iterator end() const` Iterador constante que apunta al nodo final de la lista de forma constante.

```c++
    int size() const;
    bool empty() const;
    void clear();
```
Se inicializan algunos métodos:
- `int size() const;` El cual devuelve el tamaño de la lista (int). Este método no realiza cambios en la lista doblemente enlazada.
- `bool empty() const;`El cual devuelve `true` o `false` dependiendo si la DLList está vacía o no.
- `void clear();` Este método modifica la lista eliminando todos sus nodos.
```c++
    Object &front();
    const Object &front() const;
    Object &back();
    const Object &back() const;
```
Más métodos:
- ` Object &front();` Devuelve el objeto que esté al principio de la lista por copia.
- `const Object &front() const;` Devuelve el objeto del principio por copia constante, sin hacer cambios en este.
- `Object &back();` Devuelve el objeto al final de la lista por copia.
- `const Object &back() const;`Devuelve el objeto del final por copia constante, sin hacer cambios en este.
```c++
    void push_front(const Object &x);
    void push_front(Object &&x);
    void push_back(const Object &x);
    void push_back(Object &&x);
```
Métodos vacíos:
- `void push_front(const Object &x);` Coloca el principio de la lista el objeto que se indique por copia.
- `void push_front(Object &&x);` Coloca el principio de la lista el objeto que se indique por movimiento de recursos.
- Los siguientes dos métodos colocan un nodo nuevo al final de la lista, uno pasa el valor por copia y el siguiente lo hace por movimiento de recursos.
```c++
    void pop_front();
    void pop_back();
```
- El primer método quita el primer valor de la lista, el segundo quita el último elemento de la lista.
```c++
    iterator insert(iterator itr, const Object &x);
    iterator insert(iterator itr, Object &&x);
```
Estos son métodos para el iterador.
- El primer método para insertar un objeto pasado por argumento (por copia) en la posición que se indique por el iterador pasado como argumento.
- El segundo método inserta un onjeto mediante movimiento, en el espacio al que apunta el iterador.
```c++
    void insert(int pos, const Object &x);
    void insert(int pos, Object &x);
```
Métodos para insertar elementos en la posición indicada por el parámetro, el primero pasa un objeto constante pasado mediante copia,
el segundo igual pero el objeto ya no será constante.
```c++
    iterator erase(iterator itr);
    void erase(int pos);
    iterator erase(iterator from, iterator to);
```
- `iterator erase(iterator itr); ` Método que devuelve un iterador a ser borrado/eliminado.
- `void erase(int pos);` Borra la posición indicada.
- `iterator erase(iterator from, iterator to);` Borra todos los iteradores desde una posición(`from`) hasta otra(to).
```c++
    void print() const;
```
Método para imprimir cada elemento de la DLList.

```c++
private:
int theSize;
Node *head;
Node *tail;

    void init();
    iterator get_iterator(int pos);
};
```
Atributos privados de la clase DLList:
- `int theSize;` para indicar el tamaño de la lista.
- `Node *head;` el puntero a la head de la lista.
- `Node *tail;` el puntero a la tail de la lista.
```c++
#include "DLList.cpp"
```
...