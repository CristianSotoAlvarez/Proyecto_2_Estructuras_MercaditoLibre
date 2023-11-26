#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define CAPACIDADMAX 100

struct MercaditoLibre
{
    int cantidadBodegas;
    struct NodoBodega *abbBodegas; //Arbol Binario de Busqueda
    struct NodoCompraVenta *transacciones; //Lista Doblemente Enlazada
};

/*Arbol de Busqueda Binario (Bodega)*/
struct NodoBodega
{
    struct Bodega *datosBodega;
    struct NodoBodega *izq, *der;
};

struct Producto
{
    int idProducto;
    char *nombre;
    int precio;
    int stock;
};

struct Bodega
{
    int idBodega;
    struct Producto almacenamiento[CAPACIDADMAX]; //Arreglo Estatico
    char *ubicacion;
};

/*Lista Doblemente Enlazada para Compras/Ventas (Transacciones)*/
struct NodoCompraVenta
{
    struct CompraVenta *detalle;
    struct NodoCompraVenta *ant, *sig;
};

struct CompraVenta
{
    int idTransaccion;
    char *tipo; //"ClienteBodega" o "ProveedorBodega"
    int cantidad;
    struct Producto *productoComprado;
};

struct Rotacion
{
    struct Producto *producto;
    int cantRotacion;
};

/**********************************************************************************************************************/

//FUNCIONES GENERALES

/*Esta función crea un nuevo nodo de compra/venta para almacenar un usuario y lo inicializa. Luego, devuelve
el nuevo nodo creado.*/
struct NodoCompraVenta *crearNodoCompraVenta(struct CompraVenta *usuario)
{
    struct NodoCompraVenta *nodoNuevo;
    nodoNuevo = (struct NodoCompraVenta *)malloc(sizeof(struct NodoCompraVenta));

    nodoNuevo->detalle = usuario;
    nodoNuevo->sig = NULL;
    nodoNuevo->ant = NULL;

    return nodoNuevo;
}

/*Esta función agrega una nueva transacción de compra/venta a una lista enlazada de transacciones. Si el usuario pasado
como argumento es nulo, la función no realiza ninguna acción. Crea un nuevo nodo de compra/venta utilizando la función
crearNodoCompraVenta y agrega este nodo al final de la lista enlazada. Si la lista está vacía, el nuevo nodo se
convierte en el primer elemento.*/
void addTransacciones(struct NodoCompraVenta **transacciones, struct CompraVenta *usuario)
{
    if (usuario == NULL) return;

    struct NodoCompraVenta *nodoNuevo = NULL;
    nodoNuevo = crearNodoCompraVenta(usuario);

    if ((*transacciones) == NULL)
    {
        (*transacciones) = nodoNuevo;
    }
    else
    {
        struct NodoCompraVenta *rec;
        rec = (*transacciones);

        while (rec->sig != NULL)
        {
            rec= rec->sig;
        }
        rec->sig = nodoNuevo;
        nodoNuevo->ant = rec;
    }
}

/*Esta función permite al usuario ingresar información sobre un producto, como el ID, nombre y precio. Luego, crea un
nuevo objeto Producto y lo inicializa con los valores ingresados por el usuario. El producto se devuelve como resultado.*/
struct Producto *ingresarProductoUsuario()
{
    struct Producto *producto;
    char buffer[200];
    int dato;

    producto= (struct Producto *)malloc(sizeof(struct Producto));
    do{
        printf("\nIngrese el ID del producto:");
        scanf("%d", &dato);
        if (dato <= 0)
            printf ("Ingrese un ID de producto mayor que 0");
        producto->idProducto = dato;
    }while(dato<= 0);

    printf("Ingrese nombre de producto:");
    scanf(" %[^\n]s", buffer);
    producto->nombre = (char *)malloc((strlen(buffer) + 1) * sizeof(char));
    strcpy(producto->nombre, buffer);

    printf("\nIngrese el precio del producto:");
    scanf("%d", &dato);
    producto->precio = dato;
    return producto;
}

/*Esta función permite al usuario ingresar información sobre una transacción de compra/venta. El usuario proporciona
un ID de transacción y especifica si se trata de un proveedor o un cliente. Dependiendo de esta elección, el usuario
también debe ingresar la cantidad de productos a ingresar o quitar. Luego, se llama a la
función 'ingresarProductoUsuario' para obtener información sobre el producto comprado y se almacena en la estructura
de CompraVenta. Finalmente, la función devuelve la estructura de CompraVenta inicializada con los datos ingresados por el usuario.*/
struct CompraVenta *ingresarDatosUsuario()
{
    struct CompraVenta *usuario = NULL;
    char buffer[20];
    int dato, check = 0;

    usuario= (struct CompraVenta *)malloc(sizeof(struct CompraVenta));

    do{
        printf("\nIngrese datos IdTransaccion:");
        scanf("%d", &dato);
        if (dato <= 0)
        {
            printf ("Ingrese un ID de transaccion mayor que 0!\n");
            scanf ("%d", &dato);
        }
    }while(dato <= 0);
    usuario->idTransaccion = dato;

    do
    {
        printf("\nSi son datos de un proveedor escriba 'ProveedorBodega'\n");
        printf("Si son datos de un cliente escriba 'ClienteBodega'\n");
        scanf(" %[^\n]s", buffer);

        if ((strcmp(buffer, "ProveedorBodega") == 0) || (strcmp(buffer, "ClienteBodega") == 0))
        {
            usuario->tipo = (char *)malloc((strlen(buffer) + 1) * sizeof(char));
            strcpy(usuario->tipo, buffer);
            check++;
        }
    } while (check == 0);

    if((strcmp(buffer, "ProveedorBodega") == 0))
    {
        do {
            printf("\nIngrese la cantidad del producto a ingresar:");
            scanf("%d", &dato);
            if (dato <= 0)
                printf ("Ingrese una cantidad de productos a ingresar mayor que 0!");
            usuario->cantidad = dato;
        }while(dato <= 0);

    }
    else if ((strcmp(buffer, "ClienteBodega") == 0))
    {
        do{
            printf("\nIngrese la cantidad del producto a quitar:");
            scanf("%d", &dato);
            if (dato <= 0)
                printf ("Ingrese una cantidad de productos a quitar mayor que 0!");
            usuario->cantidad = dato;
        }while(dato<= 0);
    }

    usuario->productoComprado = ingresarProductoUsuario();
    usuario->productoComprado->stock = dato;

    return usuario;
}

/*Esta funcion permite realizar el intercambio necesario por puntero*/
void intercambiar(struct Producto *a, struct Producto *b) {
    struct Producto temp = *a;
    *a = *b;
    *b = temp;
}


/*Esta funcion utiliza el algoritmo de ordenamiento "Burbuja" para ordenar el arreglo de almacenamiento de menor a mayor
 basado en la ID de cada producto*/
void bubbleSort(struct Producto almacenamiento[])
{
    int i, j;
    for (i = 0; i < CAPACIDADMAX - 1; i++) {
        for (j = 0; j < CAPACIDADMAX - i - 1; j++) {
            if (almacenamiento[j].idProducto > almacenamiento[j + 1].idProducto) {
                intercambiar(&almacenamiento[j], &almacenamiento[j + 1]);
            }
        }
    }
}


//**********************************************************************************************************************

/*FUNCIONES PARA LA OPCION 1 (Registrar Transacciones nuevas)*/

//Proceso de Bodega-Proveedor

/* Agrega un nuevo producto al almacenamiento, busca una posición vacía en el arreglo y copia los datos del producto proporcionado.
 * Devuelve 1 si se añade con éxito o 0 si no hay espacio disponible.*/
int anadirProductoNuevo(struct Producto almacenamiento[CAPACIDADMAX], struct Producto *ProductoAnadir)
{
    int i;
    for (i=0; i < CAPACIDADMAX; i++)
    {
        if (almacenamiento[i].idProducto == -1)
        {
            almacenamiento[i].idProducto= ProductoAnadir->idProducto;
            almacenamiento[i].stock = ProductoAnadir->stock;

            almacenamiento[i].nombre = (char *)malloc((strlen(ProductoAnadir->nombre) + 1) * sizeof(char));
            strcpy(almacenamiento[i].nombre, ProductoAnadir ->nombre);

            almacenamiento[i].precio = ProductoAnadir->precio;

            return 1;
        }
    }
    return 0;
}

/*Añade un producto a una bodega específica en un árbol de búsqueda binaria.
 * Utiliza la función anadirProductoNuevo y devuelve 1 si se agrega con éxito.*/
int anadirProductoNuevoABodega(struct NodoBodega *abbBodegas, struct Producto *ProductoAnadir)
{
    if(abbBodegas)
    {
        if (abbBodegas->datosBodega != NULL)
        {
            if(anadirProductoNuevo(abbBodegas->datosBodega->almacenamiento,ProductoAnadir) == 1)
            {
                return 1;
            }
        }
        if (anadirProductoNuevoABodega(abbBodegas->izq,ProductoAnadir) == 1) return 1;
        if (anadirProductoNuevoABodega(abbBodegas->der,ProductoAnadir) == 1) return 1;
    }
    return 0;
}

/*Aumenta la cantidad de un producto en una bodega.
 * Busca el producto en el almacenamiento de la bodega y actualiza su cantidad y precio si se encuentra.*/
void sumarProducto(struct Bodega *BodegaProducto , struct CompraVenta *proveedor)
{
    int i;
    for (i = 0; i < CAPACIDADMAX; i++)
    {
        if (BodegaProducto->almacenamiento[i].idProducto == proveedor->productoComprado->idProducto)
        {
            BodegaProducto->almacenamiento[i].stock += proveedor->cantidad;
            BodegaProducto->almacenamiento[i].precio = proveedor->productoComprado->precio;
            return;
        }
    }
}

/*Busca una bodega específica en un árbol de búsqueda binaria y llama a sumarProducto si la encuentra.
 * Devuelve 1 si la operación se realiza con éxito.*/
int buscarBodegaSumar(struct NodoBodega *abbBodegas , struct Bodega *Bodega, struct CompraVenta *proveedor)
{
    if (abbBodegas->datosBodega != NULL)
    {
        if(abbBodegas->datosBodega == Bodega)
        {
            sumarProducto(abbBodegas->datosBodega, proveedor);
            return 1;
        }
    }
    return 0;
}

/* Busca una bodega específica en un árbol de búsqueda binaria y, si la encuentra, llama a buscarBodegaSumar.
 * Retorna 1 si se realiza con éxito.*/

int encontrarBodegaSumar(struct NodoBodega *abbBodegas , struct CompraVenta *proveedor, struct Bodega *Bodega)
{
    if(abbBodegas)
    {
        if (encontrarBodegaSumar(abbBodegas->izq, proveedor,  Bodega) == 1) return 1;
        if (encontrarBodegaSumar(abbBodegas->der, proveedor, Bodega) == 1) return 1;
        if (buscarBodegaSumar(abbBodegas, Bodega, proveedor) == 1)
            return 1;
    }
    return 0;
}

/*Busca un producto en el almacenamiento de una bodega. Si encuentra el producto, devuelve 1, indicando que el producto existe en la bodega.*/

int buscarProdAlmacenProvee(struct Producto almacenamiento[CAPACIDADMAX] ,struct Producto *ProductoAnadido)
{
    int i;
    for (i=0; i < CAPACIDADMAX; i++)
    {
        if(almacenamiento[i].idProducto == ProductoAnadido->idProducto)
        {
            return 1; /*Por lo tanto el producto existe*/
        }
    }
    return 0; /*Por lo tanto el producto no existe y hay que aÃ±adirlo al almacenamiento de alguna bodega.*/
}

/*"recBodegasProvee" busca una bodega que almacene un producto específico en la cantidad necesaria en el árbol de bodegas de MercaditoLibre, utilizando un recorrido en profundidad.
 * Si encuentra una bodega adecuada, devuelve un puntero a ella; de lo contrario, devuelve NULL si el producto no está en ninguna bodega del sistema.*/

struct Bodega *recBodegasProvee(struct NodoBodega *abbBogedas , struct Producto *ProductoAnadido, int cantidad)
{
    if (abbBogedas)
    {
        if (recBodegasProvee(abbBogedas->izq, ProductoAnadido, cantidad) != NULL) return abbBogedas->izq->datosBodega;
        if (recBodegasProvee(abbBogedas->der, ProductoAnadido, cantidad) != NULL) return abbBogedas->der->datosBodega;
        if (abbBogedas->datosBodega != NULL)
        {
            if (buscarProdAlmacenProvee(abbBogedas->datosBodega->almacenamiento, ProductoAnadido) == 1)
            {
                return abbBogedas->datosBodega;
            }
        }
    }
    return NULL;
}

/* La función "comprarAProveedor" en el sistema de MercaditoLibre desempeña un papel crucial al facilitar la adquisición de productos de proveedores. Esta función verifica la validez del sistema, la transacción del proveedor y la disponibilidad de productos, y luego llama a la función "recBodegasProvee" para buscar una bodega adecuada que contenga el producto deseado en la cantidad necesaria. Si la bodega es encontrada, llama a la función "encontrarBodegaSumar" para aumentar la cantidad de productos en la bodega y registra la transacción en su historial. Si el producto no se encuentra en ninguna bodega, se utiliza la función "anadirProductoNuevoABodega" para añadirlo y registrar la transacción.
 * En resumen, "comprarAProveedor" coordina la compra de productos a proveedores, llamando a varias funciones auxiliares, y devuelve 1 si la compra es exitosa, o 0 si no puede ser completada.*/

int comprarAProvedoor(struct MercaditoLibre *ML, struct CompraVenta *proveedor)
{
    if (ML == NULL || ML->abbBodegas == NULL || proveedor == NULL) return 0;
    if(proveedor->productoComprado != NULL && strcmp(proveedor->tipo,"ProveedorBodega")==0 && proveedor->cantidad > 0)
    {
        struct Bodega *BodegaProd;
        BodegaProd = recBodegasProvee(ML->abbBodegas , proveedor->productoComprado, proveedor->cantidad);

        if(BodegaProd != NULL)
        {
            if(encontrarBodegaSumar(ML->abbBodegas, proveedor, BodegaProd) == 1)
            {
                addTransacciones(&(ML->transacciones), proveedor);
                return 1;
            }
            else
                return 0;
        }
        else
        {
            struct Producto *NuevoProducto = proveedor->productoComprado;

            if (anadirProductoNuevoABodega(ML->abbBodegas,NuevoProducto) == 1)
            {
                addTransacciones(&(ML->transacciones), proveedor);
                return 1;
            }
        }
    }
    return 0;
}

//Proceso bodega y Cliente

/*Esta funcion genera todos los prints de la opcion 1.*/
void printOpcion1(int accion ,int idProducto,int stock)
{
    if (accion == 1)
    {
        printf("ALERTA: El producto con ID: %d tiene un stock menor a 10 unidades. Por Favor reponer productos lo antes posible. (Stock Disponible: %d)", idProducto,stock);
    }
    else if (accion == 2)
    {
        printf("ALERTA: El producto con ID: %d no tiene suficiente stock. Por Favor reponer productos lo antes posible. (Stock Disponible: %d)", idProducto, stock);
    }
}

/*Esta funcion busca en el almacenamiento de una bodega un producto específico.
 * Utiliza un bucle "for" para recorrer los elementos, verificando el "idProducto" y la cantidad de stock. Si encuentra el producto y satisface la cantidad requerida, devuelve 1.
 * Si el producto está presente pero con stock insuficiente, emite una alerta y retorna 0. Si no encuentra el producto, devuelve 0 para indicar su ausencia.*/

int buscarProdAlmacenClien(struct Producto almacenamiento[CAPACIDADMAX], struct Producto *ProductoAQuitar , int cantidadProducto)
{
    int i;
    for (i=0; i < CAPACIDADMAX; i++)
    {
        if(almacenamiento[i].idProducto == ProductoAQuitar->idProducto && almacenamiento[i].stock >= cantidadProducto)
        {
            almacenamiento[i].stock-= cantidadProducto;
            if (almacenamiento[i].stock < 10)
            {
                printOpcion1(1 , almacenamiento[i].idProducto,almacenamiento[i].stock);
            }
            return 1;
        }
        else if (almacenamiento[i].idProducto == ProductoAQuitar->idProducto && almacenamiento[i].stock < cantidadProducto)
        {
            printOpcion1(2 , almacenamiento[i].idProducto,almacenamiento[i].stock);
            return 0;
        }
    }
    return 0;
}

/*Esta funcion busca una bodega en el árbol de bodegas que tenga el producto necesario para el cliente, llamando a "buscarProdAlmacenClien".
 Si encuentra una bodega adecuada, devuelve un puntero a esa bodega para completar la venta; de lo contrario, retorna NULL si el producto no está disponible en ninguna bodega del sistema, lo que impide la venta.*/
struct Bodega *recBodegasClien(struct NodoBodega *abbBodegas , struct Producto *ProductoAQuitar, int cantidad)
{
    if (abbBodegas)
    {
        recBodegasClien(abbBodegas->izq, ProductoAQuitar, cantidad);
        recBodegasClien(abbBodegas->der, ProductoAQuitar, cantidad);
        if (abbBodegas->datosBodega != NULL)
        {
            if (buscarProdAlmacenClien(abbBodegas->datosBodega->almacenamiento, ProductoAQuitar ,cantidad) == 1)
            {
                return abbBodegas->datosBodega;
            }
        }
    }
    return NULL;
}

/*La función "venderProdACliente" verifica la validez del sistema y la transacción del cliente.
 * Luego, busca una bodega con el producto necesario y la cantidad requerida. Si encuentra una bodega adecuada, registra la transacción en su historial y devuelve 1 para indicar una venta exitosa.
 * Si no encuentra una bodega con los productos necesarios, devuelve 0, indicando que la venta no puede completarse debido a la falta de existencias.*/
int venderProdACliente(struct MercaditoLibre *ML , struct CompraVenta *cliente)
{
    if (ML->abbBodegas == NULL || cliente == NULL) return 0;

    if(cliente ->productoComprado != NULL && strcmp(cliente->tipo,"ClienteBodega") == 0 && cliente->cantidad > 0)
    {
        struct Bodega *bodegaUsada;
        bodegaUsada = recBodegasClien(ML->abbBodegas,cliente->productoComprado, cliente->cantidad);

        if(bodegaUsada == NULL)
            return 0;
        else
        {
            addTransacciones(&(ML->transacciones), cliente);
            return 1;
        }
    }
    return 0;
}

/**********************************************************************************************************************/

/*FUNCIONES PARA LA OPCION 2 (Revisar Stock de todos los productos en una Bodega)*/

/* Esta función imprime la información de los productos almacenados en un arreglo de productos. Recorre el
arreglo y muestra el ID del producto, el nombre, el stock disponible y el precio de costo del producto. Solo se imprimen
los productos cuyo ID no sea -1.*/
void printProductos(struct Producto almacenamiento[]){
    int i;

    bubbleSort(almacenamiento);

    for (i = 0; i < CAPACIDADMAX; i++){
        if (almacenamiento[i].idProducto != -1){
            printf("ID Producto: %d\n", almacenamiento[i].idProducto);
            printf("Nombre Producto: %s\n", almacenamiento[i].nombre);
            printf("Stock Disponible: %d\n", almacenamiento[i].stock);
            printf("Precio Costo Producto: %d\n\n", almacenamiento[i].precio);
        }
    }
}

/*Esta función realiza un recorrido inorden en un árbol binario de nodos de bodega. Muestra la ubicación de
cada bodega y luego imprime la información de los productos almacenados en cada bodega,
utilizando la función 'printProductos'.*/
void recBodegasInOrden(struct NodoBodega *abbBodegas){
    if (abbBodegas == NULL) return;

    recBodegasInOrden(abbBodegas->izq);
    printf ("Ubicacion de la Bodega con ID %d : %s\n",abbBodegas->datosBodega->idBodega, abbBodegas->datosBodega->ubicacion);
    printf("Productos en Bodega con ID %d:\n\n",abbBodegas->datosBodega->idBodega);
    printProductos(abbBodegas->datosBodega->almacenamiento);

    recBodegasInOrden(abbBodegas->der);
}

/*Esta función permite imprimir el stock de productos en todas las bodegas de un sistema MercaditoLibre. Llama a
'recBodegasInOrden' para recorrer todas las bodegas y mostrar su información. Si el sistema de MercaditoLibre es nulo,
la función no realiza ninguna acción.*/
void printStockProductos(struct MercaditoLibre *ML){
    if (ML == NULL) return;
    recBodegasInOrden(ML->abbBodegas);
}

/**********************************************************************************************************************/

/*FUNCIONES PARA LA OPCION 3 ("Productos Mayor/Menor Rotacion")*/

/*Esta función busca un producto por su ID en el arreglo de rotaciones y devuelve 1 si se encuentra, de lo
contrario, devuelve 0.*/
int buscarProdRotaciones(int idProducto, struct Rotacion **rotaciones, int tallaMaxRotaciones)
{
    int i;

    for (i = 0; i < tallaMaxRotaciones && rotaciones[i] != NULL; i++)
    {
        if (rotaciones[i]->producto != NULL && rotaciones[i]->producto->idProducto == idProducto)
        {
            return 1;
        }
    }
    return 0;
}

/*Esta funcion incrementa el contador de rotaciones de un producto específico en el arreglo de rotaciones.*/
void sumarARotaciones(struct Producto *producto, struct Rotacion **rotaciones, int tallaMaxRotaciones)
{
    int i;

    for (i = 0; i < tallaMaxRotaciones && rotaciones[i] != NULL; i++)
    {
        if (rotaciones[i]->producto != NULL && rotaciones[i]->producto->idProducto == producto->idProducto)
        {
            rotaciones[i]->cantRotacion++;
            return;
        }
    }
}

/*Esta funcion agrega un nuevo producto al arreglo de rotaciones con un contador de rotación inicial de 1.*/
void agregarARotaciones(struct Producto *producto, struct Rotacion **rotaciones, int tallaMaxRotaciones)
{
    int i;

    for (i = 0; i < tallaMaxRotaciones; i++)
    {
        if (rotaciones[i] == NULL)
        {
            rotaciones[i]= (struct Rotacion *)malloc(sizeof(struct Rotacion));
            rotaciones[i]->producto = producto;
            rotaciones[i]->cantRotacion = 1;
            return;
        }
    }
}

/*Esta función recorre las transacciones y actualiza el arreglo de rotaciones. Si un producto comprado ya existe
en el arreglo, se incrementa su contador; de lo contrario, se agrega al arreglo de rotaciones.*/
struct Rotacion **recTranRotaciones(struct NodoCompraVenta *transacciones, struct Rotacion **rotaciones, int tallaMaxRotaciones)
{
    if (transacciones != NULL)
    {
        if (rotaciones == NULL)
        {
            rotaciones = (struct Rotacion **)malloc(tallaMaxRotaciones*sizeof(struct Rotacion *));
        }
        struct NodoCompraVenta *rec;
        rec = transacciones;

        while (rec != NULL)
        {
            if (rec->detalle != NULL && rec->detalle->productoComprado != NULL)
            {
                if (buscarProdRotaciones(rec->detalle->productoComprado->idProducto, rotaciones, tallaMaxRotaciones) == 1) //Si existe en arreglo
                {
                    sumarARotaciones(rec->detalle->productoComprado, rotaciones, tallaMaxRotaciones);
                }
                else //Si NO existe en arreglo
                {
                    agregarARotaciones(rec->detalle->productoComprado, rotaciones, tallaMaxRotaciones);
                }
            }
            rec= rec->sig;
        }
        return rotaciones;
    }
    return NULL;
}

/*Esta función principal inicializa un arreglo de rotaciones, lo pasa a la función recTranRotaciones para actualizarlo
según las transacciones de MercaditoLibre y luego lo devuelve. Si ML o su árbol de bodegas son nulos, devuelve NULL.*/
struct Rotacion **getRotaciones(struct MercaditoLibre *ML, int tallaMaxRotaciones)
{
    if (ML == NULL || ML->abbBodegas == NULL) return NULL;

    struct Rotacion **rotaciones = (struct Rotacion **)malloc((tallaMaxRotaciones) * sizeof(struct Rotacion *));
    for (int i = 0; i < tallaMaxRotaciones; i++)
    {
        rotaciones[i] = NULL;
    }

    rotaciones = recTranRotaciones(ML->transacciones, rotaciones, tallaMaxRotaciones);
    return rotaciones;
}

/*Esta función busca y devuelve el elemento con la mayor cantidad de rotaciones en el arreglo de rotaciones. Recorre el
arreglo y compara el valor del contador de rotación de cada elemento, devolviendo el elemento con la mayor cantidad.*/
struct Rotacion *getMayorRotacion(struct Rotacion **rotaciones, int tallaRotaciones)
{
    int i, check = 0;
    struct Rotacion *mayorRotacion = NULL, *prodActual;

    for (i = 0; i < tallaRotaciones && rotaciones[i] != NULL; i++)
    {
        if (check == 0)
        {
            mayorRotacion = rotaciones[i];
            check++;
        }
        else
        {
            prodActual = rotaciones[i];
            if (prodActual->cantRotacion > mayorRotacion->cantRotacion)
            {
                mayorRotacion = prodActual;
            }
        }
    }
    return mayorRotacion;
}

/*Esta función busca y devuelve el elemento con la menor cantidad de rotaciones en el arreglo de rotaciones. Recorre el
arreglo y compara el valor del contador de rotación de cada elemento, devolviendo el elemento con la menor cantidad.*/
struct Rotacion *getMenorRotacion(struct Rotacion **rotaciones, int tallaRotaciones)
{
    int i, check = 0;
    struct Rotacion *menorRotacion = NULL, *prodActual;

    for (i = 0; i < tallaRotaciones && rotaciones[i] != NULL; i++)
    {
        if (check == 0)
        {
            menorRotacion = rotaciones[i];
            check++;
        }
        else
        {
            prodActual = rotaciones[i];
            if (prodActual->cantRotacion < menorRotacion->cantRotacion)
            {
                menorRotacion = prodActual;
            }
        }
    }
    return menorRotacion;
}

/**********************************************************************************************************************/

/*FUNCIONES PARA LA OPCION 4 ("Buscar Producto Especifico")*/

/*Esta funcion imprime la informacion obtenida en las funciones de la Opcion 4*/
void printOpcion4(int idBodega)
{
    printf("El producto se encuentra en la bodega de ID %d\n", idBodega);
}

/*Esta funcion ocupa el algoritmo de Busqueda Binaria para encontrar un producto en el almacenamiento de la bodega
 Para esto, se ordena el arreglo previo a la busqueda mediante el ordenamiento burbuja*/
struct Producto *buscarProdEnAlmacen(struct Producto almacenamiento[CAPACIDADMAX] , int idProd)
{
    bubbleSort(almacenamiento);

    int pivIzq=0, pivDer = CAPACIDADMAX - 1, valorCentral, check = 0;
    while ((pivIzq <= pivDer)){
        valorCentral = (pivIzq + pivDer)/2;
        if (almacenamiento[valorCentral].idProducto == idProd){
            struct Producto *ProductoEncontrado = (struct Producto *) malloc(sizeof(struct Producto));
            ProductoEncontrado->idProducto = almacenamiento[valorCentral].idProducto;
            ProductoEncontrado->stock = almacenamiento[valorCentral].stock;
            ProductoEncontrado->nombre = (char *) malloc((strlen(almacenamiento[valorCentral].nombre) + 1) * sizeof(char));
            strcpy(ProductoEncontrado->nombre, almacenamiento[valorCentral].nombre);
            ProductoEncontrado->precio = almacenamiento[valorCentral].precio;
            return ProductoEncontrado;
        }
        else if (almacenamiento[valorCentral].idProducto > idProd) {
            pivDer = valorCentral - 1;
        }
        else{
            pivIzq = valorCentral + 1;
        }
    }
    return NULL;
}


/*"buscarProductoEnBodegas" busca de manera recursiva un producto en un árbol de bodegas, explorando cada bodega y su almacenamiento.
 * Si encuentra el producto, establece "encontrado" en 1, imprime la ubicación de la bodega y devuelve el producto. Si no lo encuentra, retorna NULL.*/
struct Producto *buscarProductoEnBodegas(struct NodoBodega *abbBodegas, int idProd, int *encontrado)
{
    if (abbBodegas)
    {
        if (buscarProductoEnBodegas(abbBodegas->izq, idProd, encontrado) != NULL)
        {
            return buscarProdEnAlmacen(abbBodegas->izq->datosBodega->almacenamiento, idProd);
        }
        if (buscarProductoEnBodegas(abbBodegas->der, idProd, encontrado) != NULL)
        {
            return buscarProdEnAlmacen(abbBodegas->der->datosBodega->almacenamiento, idProd);
        }
        if (*encontrado == 0 && buscarProdEnAlmacen(abbBodegas->datosBodega->almacenamiento, idProd) != NULL)
        {
            printOpcion4(abbBodegas->datosBodega->idBodega);
            *encontrado = 1;
            return buscarProdEnAlmacen(abbBodegas->datosBodega->almacenamiento, idProd);
        }
    }
    return NULL;
}

/*Esta funcion verifica la validez del sistema y busca un producto en el árbol de bodegas utilizando "buscarProductoEnBodegas".
 * Si encuentra el producto, devuelve un puntero a este; de lo contrario, retorna NULL.*/
struct Producto *buscarProducto(struct MercaditoLibre *ML, int idProd)
{
    if (ML == NULL || ML->abbBodegas == NULL)
        return NULL;

    int encontrado = 0;  // Variable para controlar si el producto se ha encontrado
    struct Producto *productoEncontrado = buscarProductoEnBodegas(ML->abbBodegas, idProd, &encontrado);

    if (encontrado == 1)
    {
        return productoEncontrado;
    }
    else
    {
        return NULL;
    }
}

/**********************************************************************************************************************/

/*FUNCIONES PARA LA OPCION 5 ("Eliminar producto de la Bodega")*/

/*Esta función busca y elimina un producto del arreglo de almacenamiento en función de su ID. Si encuentra
un producto con el ID especificado, lo marca como eliminado configurando su ID en -1 y restableciendo el stock,
el nombre y el precio. Devuelve 1 si el producto se encontró y eliminó con éxito, de lo contrario, devuelve 0.*/
int elimProdEnAlmacen(struct Producto almacenamiento[], int idProducto)
{
    int i;

    for (i=0; i < CAPACIDADMAX; i++)
    {
        if(almacenamiento[i].idProducto == idProducto)
        {
            almacenamiento[i].idProducto = -1;
            almacenamiento[i].stock = 0;
            almacenamiento[i].nombre = "SIN NOMBRE";
            almacenamiento[i].precio = 0;
            return 1;
        }
    }
    return 0;
}

/*Esta función realiza una búsqueda para eliminar un producto con un ID específico en todas las bodegas del árbol
de bodegas. Recorre el árbol de bodegas y llama a elimProdEnAlmacen para intentar eliminar el producto del almacenamiento de la bodega en que se encuentra.
Devuelve 1 si el producto se encuentra y se elimina, de lo contrario, devuelve 0.*/
int recBodegasElimProd(struct NodoBodega *abbBodegas, int idProducto)
{
    if (abbBodegas != NULL)
    {
        if (recBodegasElimProd(abbBodegas->izq,idProducto) == 1) return 1;
        if (recBodegasElimProd(abbBodegas->der, idProducto) == 1) return 1;
        if (elimProdEnAlmacen(abbBodegas->datosBodega->almacenamiento,idProducto) == 1) return 1;
    }
    return 0;
}

/*Esta funcion recibe el sistema completo y realiza las validaciones correspondientes*/
int EliminarTotalmenteProductoDelAlmacen(struct MercaditoLibre *ML, int idProducto)
{
    if (ML == NULL) return 0;
    if (recBodegasElimProd(ML->abbBodegas, idProducto) == 1) return 1;
    return 0;
}

/**********************************************************************************************************************/

/*FUNCIONES PARA LA OPCION 6 ("Eliminar transacciones del sistema)*/

/*Esta funcion recibe la lista NodoCompraVenta (transacciones) y un ID de transaccion especifica
  Su objetivo es eliminar los nodos que contengan el id especificado*/
void eliminarNodoCompraVenta(struct NodoCompraVenta **transacciones, int idTransaccion, int *existe){
    if ((*transacciones) == NULL) return;

    if ((*transacciones)->detalle->idTransaccion == idTransaccion){
        (*existe) = 1;
        (*transacciones) = (*transacciones)->sig;

        if ((*transacciones) != NULL){
            (*transacciones)->ant = NULL;
        }
    }

    struct NodoCompraVenta *rec = (*transacciones)->sig;
    while (rec != NULL){
        if (rec->detalle->idTransaccion == idTransaccion){
            (*existe) = 1;
            rec->ant->sig = rec->sig;
            if (rec->sig != NULL) {
                rec->sig->ant = rec->ant;
            }
        }
        rec = rec->sig;
    }
}

/*Esta funcion recibe el sistema completo y realiza las validaciones correspondientes*/
void eliminarTransaccion(struct MercaditoLibre *ML, int idTransaccion, int *existe){
    if (ML == NULL) return;
    eliminarNodoCompraVenta(&(ML->transacciones), idTransaccion, existe);
}

/**********************************************************************************************************************/

/*FUNCIONES PARA LA OPCION 7 (Mostrar transacciones)*/

/*Esta funcion imprime el historial completo de transacciones (Lista NodoCompraVenta)*/
void printLista(struct NodoCompraVenta *head)
{
    if (head == NULL)
    {
        printf("Las Transacciones no tiene datos\n");
        return;
    }

    printf("Transacciones:\n\n");
    while(head != NULL)
    {
        printf("ID de la transaccion: %d\n", head->detalle->idTransaccion);
        printf("Tipo de transaccion: %s\n", head->detalle->tipo);
        printf("Nombre de producto: %s\n", head->detalle->productoComprado->nombre);

        if (strcmp(head->detalle->tipo, "ProveedorBodega") == 0)
        {
            printf("Precio costo unitario de producto: %d\n", head->detalle->productoComprado->precio);
        }
        else
        {
            printf("Precio venta unitario de producto: %d\n", head->detalle->productoComprado->precio);
        }

        printf("Cantidad Comprada: %d\n\n", head->detalle->cantidad);
        head= head->sig;
    }
}

/*Esta funcion recibe el sistema completo y realiza las validaciones correspondientes*/
void printlistaTransacciones(struct MercaditoLibre *ML){
    if (ML == NULL ) return;
    printLista(ML->transacciones);
}

/**********************************************************************************************************************/

/*FUNCIONES PARA LA OPCION 8 (Buscar Transaccion por ID)*/

/*Esta funcion imprime la informacion obtenida en las funciones de la Opcion 8*/
void printOpcion8(int accion, int idTransaccion, char *nombre, char *tipo, int cantidad){
    if (accion == 1){
        printf ("La transaccion de ID %d si se encuentra en el historial de transacciones, detalles de transaccion:\n", idTransaccion);
        printf ("Nombre producto: %s\n", nombre);
        printf ("Tipo de transaccion %s\n", tipo);
        printf ("Cantidad Vendida/Comprada: %d\n", cantidad);
    }
}

/*Esta funcion busca en la lista Nodo CompraVenta una transaccion especifica en base a un ID recibido en esta
  Se imprime los datos de dicha transaccion*/
void buscarIDTransaccion (struct NodoCompraVenta *transacciones , int idTransaccion, int *validacion)
{
    if (transacciones == NULL)
        return;

    while (transacciones != NULL)
    {
        if (transacciones->detalle != NULL && transacciones->detalle->productoComprado != NULL)
        {
            if (transacciones->detalle->idTransaccion == idTransaccion)
            {
                printOpcion8(1, idTransaccion, transacciones->detalle->productoComprado->nombre, transacciones->detalle->tipo, transacciones->detalle->cantidad);
                (*validacion) =1;
                return;
            }
        }
        transacciones = transacciones->sig;
    }

}

/**********************************************************************************************************************/

/*FUNCIONES PARA LA OPCION 9 (Eliminar Bodega)*/

/*Esta funcion hace un reemplazo de datos del Nodo a eliminar mediante el uso de una variable auxiliar*/
void swap(struct NodoBodega **abbBodegas, struct NodoBodega **aux){
    if ((*abbBodegas)->der == NULL){
        (*aux)->datosBodega = (*abbBodegas)->datosBodega;
        (*aux) = (*abbBodegas);
        (*abbBodegas) = (*abbBodegas)->izq;
    }
    else{
        swap(&((*abbBodegas)->der), &(*aux));
    }
}

/*Esta funcion elimina estructuras de tipo NodoBodega segun 3 casos:
  1) El Nodo a eliminar NO tiene hojas
  2) El Nodo tiene una hoja, ya sea a la izquierda o derecha
  3) El Nodo posee 2 hojas*/
void eliminarNodoBodega(struct NodoBodega **abbBodegas, int idEliminar, int *existe) {
    struct NodoBodega *aux;
    if ((*abbBodegas) == NULL) return;

    if ((*abbBodegas)->datosBodega->idBodega < idEliminar) {
        eliminarNodoBodega(&((*abbBodegas)->der), idEliminar, existe);
    } else {
        if ((*abbBodegas)->datosBodega->idBodega > idEliminar) {
            eliminarNodoBodega(&((*abbBodegas)->izq), idEliminar, existe);
        } else {
            if ((*abbBodegas)->datosBodega->idBodega == idEliminar) {
                (*existe) = 1;
                aux = (*abbBodegas);
                if ((*abbBodegas)->izq == NULL) {
                    (*abbBodegas) = (*abbBodegas)->der;
                } else {
                    if ((*abbBodegas)->der == NULL) {
                        (*abbBodegas) = (*abbBodegas)->izq;
                    } else {
                        swap(&(*abbBodegas)->izq, &aux);
                    }
                }
            }
        }
    }
}

/*Esta funcion recibe la estructura del sistema completo para realizar las verificaciones correspondientes*/
void eliminarBodega(struct MercaditoLibre *ML, int idEliminar, int *existe){
    if (ML == NULL || ML->abbBodegas == NULL) return;
    eliminarNodoBodega(&(ML->abbBodegas), idEliminar, existe);
}

/**********************************************************************************************************************/

/*FUNCION PARA LA OPCION 10 (Conseguir monto total de una transaccion) EXTRA 1*/

/*Esta funcion imprime la informacion obtenida en las funciones de la Opcion 10*/
void printOpcion10(int accion, int montoTotal, int idTransaccion){
    if (accion == 1){
        printf ("\nSe gano un total de %d en la transaccion tipo Venta de ID: %d", montoTotal, idTransaccion);
    }
    else if (accion == 2){
        printf ("\nSe gasto un total de %d en la transaccion de tipo Proveedor de ID: %d ", montoTotal, idTransaccion);
    }
}

/*Esta funcion imprime el monto total figurado en una transaccion especifica.
  Dependiendo de la naturaleza de la transaccion (variable "tipo)" se imprimira un mensaje diferente que diferencie entre ganancia y perdida*/
void detalleMontoTransaccion(struct NodoCompraVenta *transacciones , int idTransaccion, int *existe)
{
    int montoTotal = 0, detalle, tipo = 0;
    while(transacciones != NULL)
    {
        if (transacciones->detalle->idTransaccion == idTransaccion)
        {
            if (strcmp(transacciones->detalle->tipo,"ProveedorBodega")==0)
            {
                tipo = 1;
            }
            else if (strcmp(transacciones->detalle->tipo,"ClienteBodega")==0){
                tipo = 0;
            }
            (*existe) = 1;
            detalle = transacciones->detalle->productoComprado->precio * transacciones->detalle->cantidad;
            montoTotal += detalle;
        }
        transacciones = transacciones->sig;
    }

    if (tipo == 0)
    {
        printOpcion10(1, montoTotal, idTransaccion);
    }
    else if(tipo == 1)
    {
        printOpcion10(2, montoTotal, idTransaccion);

    }
}

/**********************************************************************************************************************/

/*FUNCION PARA LA OPCION 11 (Reporte de Perdidas/Ganancias) EXTRA 2*/

/*Esta funcion imprime la informacion obtenida en las funciones de la Opcion 11*/
void printOpcion11(int accion, int resultado){
    if (accion == 1){
        printf("No se han realizado transacciones");
    }
    else if (accion == 2){
        printf("Hubo una perdida de %d\n", abs(resultado));
    }
    else if (accion == 3){
        printf("Hubo una ganancia de %d\n", resultado);
    }
    else if (accion == 4){
        printf("No se presentan perdidas ni ganancias\n");
    }
}

/*Esta funcion imprime si el sistema reporta ganancias o perdidas con un analisis total de compras/ventas (transacciones) del sistema*/
void reporteMonetario(struct NodoCompraVenta *head)
{
    if (head == NULL)
    {
        printOpcion11(1, 0);
        return;
    }

    struct NodoCompraVenta *rec = head;
    int acumProv = 0, acumClien = 0, resultado;

    while (rec != NULL)
    {
        if (rec->detalle != NULL)
        {
            if (strcmp(rec->detalle->tipo, "ProveedorBodega") == 0)
            {
                if (rec->detalle->productoComprado != NULL)
                {
                    acumProv+= (rec->detalle->productoComprado->precio) * (rec->detalle->cantidad);
                }
            }
            else if (strcmp(rec->detalle->tipo, "ClienteBodega") == 0)
            {
                acumClien+= (rec->detalle->productoComprado->precio) * (rec->detalle->cantidad);
            }
        }
        rec= rec->sig;
    }
    resultado = (acumClien - acumProv);
    if (resultado < 0)
    {
        printOpcion11(2, resultado);
    }
    else if (resultado > 0)
    {
        printOpcion11(3, resultado);
    }
    else
    {
        printOpcion11(4, 0);
    }
}

/**********************************************************************************************************************/

/*FUNCIONES PARA INGRESAR DATOS AL SISTEMA*/

/*Esta funcion inicializa las variables del arreglo "almacenamiento" con valores que indiquen cualidad de "vacios"*/
void inicializarAlmacenamiento(struct Producto almacenamiento[])
{
    int i;
    for (i = 0; i < CAPACIDADMAX; i++) //
    {
        almacenamiento[i].idProducto = -1;
        almacenamiento[i].stock = 0;
        almacenamiento[i].nombre = "SIN NOMBRE";
        almacenamiento[i].precio = 0;
    }
}

/*Esta funcion se encarga de crear la estructura Bodega presente en cada estructura NodoBodega que genere*/
void crearDatos(struct Bodega **nuevaBodega, int idBodega){
    struct Bodega *datos = (struct Bodega *)malloc(sizeof(struct Bodega));
    datos->idBodega = idBodega;
    char buffer[100];
    printf("Ingrese el nombre de la Ubicacion de la bodega: \n");
    scanf(" %[^\n]s", buffer);
    datos->ubicacion = (char *)malloc((strlen(buffer) + 1) * sizeof(char));
    strcpy(datos->ubicacion, buffer);

    inicializarAlmacenamiento(datos->almacenamiento);

    (*nuevaBodega) = datos;
}

/*Esta funcion se encarga de agregar un nuevo NodoBodega al arbol de busqueda binaria "abbBodegas"*/
void agregarNodoBodegaAbb(struct NodoBodega **abbBodegas, int idBodega){
    if (*abbBodegas == NULL){
        struct NodoBodega *nuevoNodo = (struct NodoBodega *)malloc(sizeof(struct NodoBodega));
        crearDatos(&(nuevoNodo->datosBodega), idBodega);
        nuevoNodo->izq = nuevoNodo->der = NULL;
        *abbBodegas = nuevoNodo;
        printf("Bodega creada con exito\n\n");
    }
    else{
        if (idBodega < (*abbBodegas)->datosBodega->idBodega){
            agregarNodoBodegaAbb(&((*abbBodegas)->izq), idBodega);
        }
        else if (idBodega > (*abbBodegas)->datosBodega->idBodega){
            agregarNodoBodegaAbb(&((*abbBodegas)->der), idBodega);
        }
        else{
            printf("Bodega ya se encuentra registrada\n\n");
        }
    }
}

/*Esta funcion itera la funcion "agregarNodoBodegaAbb" de acuerdo al valor de la variable "cantidadBodegas"*/
void ingresarDatosBodega(struct NodoBodega **abbBodegas, int cantidadBodegas){
    int i, id;
    for (i = 0; i < cantidadBodegas; i++){
        do {
            printf("Ingrese el ID de la bodega: \n");
            scanf("%d", &id);
            if (id <= 0)
            {
                printf ("Ingrese un ID mayor que 0\n");
            }
        }while (id <= 0);
        agregarNodoBodegaAbb(abbBodegas, id);
    }
}

/*Esta funcion recibe el sistema completo y realiza las validaciones correspondientes
  Ademas pregunta por la cantidad de Bodegas que se desean agregar al sistema*/
void ingresarDatos(struct MercaditoLibre *ML){
    struct NodoBodega *bodegas = NULL;

    printf("Ingrese la cantidad de bodegas que desea registrar: ");
    scanf("%d", &ML->cantidadBodegas);

    ingresarDatosBodega(&bodegas, ML->cantidadBodegas);
    ML->abbBodegas = bodegas;
    ML->transacciones = NULL;
}

/**********************************************************************************************************************/

/*PROGRAMA PRINCIPAL*/

int main()
{
    /*Definicion del sistema*/
    struct MercaditoLibre ML;
    ingresarDatos(&ML);
    int opcion = 0;
    char nombre[50];

    /*Menu del sistema*/
    while (opcion != 13)
    {
        printf("\n=======================================\n\n");
        if (ML.abbBodegas == NULL)
        {
            printf("No hay bodegas existentes para trabajar\n");
            printf("Que tenga buen dia\n");
            return 0;
        }
        printf("Sistema de Gestion de Inventario\n\n");
        printf("Ingrese un numero para realizar una accion\n");
        printf("1. Registrar Compras/Ventas nuevas\n");
        printf("2. Revisar Stock de todos los productos en una Bodega\n");
        printf("3. Productos Mayor/Menor Rotacion\n");
        printf("4. Buscar Producto Especifico\n");
        printf("5. Eliminar Producto de la bodega\n");
        printf("6. Eliminar Transaccion\n");
        printf("7. Mostrar Transacciones\n");
        printf("8. Buscar Transaccion\n");
        printf("9. Eliminar Bodega del sistema\n");
        printf("10. Determinar monto total transaccion\n");
        printf("11. Reporte de Perdidas/Ganancias\n");
        printf("12. Agregar Bodega al sistema\n");
        printf("13. Salir\n");
        scanf("%d", &opcion);

        printf("\n=======================================\n\n");

        /*Condiciones segun opcion ingresada*/
        if (opcion == 1)
        {
            struct CompraVenta *usuario;
            usuario = ingresarDatosUsuario();
            if (usuario != NULL)
            {
                if (strcmp(usuario->tipo, "ProveedorBodega") == 0)
                {
                    if (comprarAProvedoor(&ML, usuario) == 1)
                    {
                        printf("\nSe ha logrado comprar al proveedor de forma exitosa\n\n");
                    }
                    else
                    {
                        printf("\nNo se ha podido realizar la compra al proveedor\n\n");
                    }
                }
                else
                {
                    if (venderProdACliente(&ML, usuario) == 1) {
                        printf("\nSe ha logrado vender al cliente de forma exitosa\n\n");
                    }
                    else
                    {
                        printf("\nNo se ha podido realizar la venta al cliente\n\n");
                    }
                }
            }
            else
            {
                printf("No ha logrado ingresar al usuario\n\n");
            }
        }
        else if (opcion == 2)
        {
            printStockProductos(&ML);
        }
        else if (opcion == 3)
        {
            struct Rotacion **rotaciones = NULL;
            struct Rotacion *prodMayorRotacion, *prodMenorRotacion;

            int tallaMaxRotaciones = CAPACIDADMAX * (ML.cantidadBodegas);
            rotaciones = getRotaciones(&ML, tallaMaxRotaciones);

            if (rotaciones != NULL)
            {
                prodMayorRotacion = getMayorRotacion(rotaciones, tallaMaxRotaciones);
                prodMenorRotacion = getMenorRotacion(rotaciones, tallaMaxRotaciones);

                if (prodMayorRotacion != NULL)
                {
                    printf("Producto con mas rotacion\n");
                    printf("ID Producto: %d\n", prodMayorRotacion->producto->idProducto);
                    printf("Nombre Producto: %s\n", prodMayorRotacion->producto->nombre);
                    printf("Cantidad de Compras/Ventas: %d\n", prodMayorRotacion->cantRotacion);
                }
                else
                {
                    printf("\nNo se logro encontrar el producto con más rotacion\n");
                }

                if (prodMenorRotacion != NULL)
                {
                    printf("\nProducto con menor rotacion\n");
                    printf("ID Producto: %d\n", prodMenorRotacion->producto->idProducto);
                    printf("Nombre Producto: %s\n", prodMenorRotacion->producto->nombre);
                    printf("Cantidad de Compras/Ventas: %d\n", prodMenorRotacion->cantRotacion);
                }
                else
                {
                    printf("\nNo se logro encontrar el producto con menor rotacion\n");
                }
            }
            else
            {
                printf("No se ha comprado ni vendido ningun producto en el sistema de bodegas\n");
            }
        }
        else if (opcion == 4)
        {
            int id;
            do{
                printf("Ingrese el ID del producto:\n ");
                scanf("%d", &id);
                if (id <= 0)
                {
                    printf ("Ingrese un ID de producto mayor que 0!\n");
                }

            }while(id <= 0);
            struct Producto *Producto = buscarProducto(&ML, id);
            if (Producto == NULL)
            {
                printf("El producto %d no se encuentra en ninguno de nuestras bodegas.\n\n", id);
            }
            else
            {
                printf("\nEl producto %s se encuentra disponible en nuestras bodegas. STOCK DISPONIBLE DEL PRODUCTO: %d\n\n",
                       Producto->nombre, Producto->stock);
            }
        }
        else if (opcion == 5)
        {
            int id;
            printf("Ingrese el ID del producto: ");
            scanf("%d", &id);

            if (EliminarTotalmenteProductoDelAlmacen(&ML, id) == 1)
            {
                printf("\nEl producto %d se ha eliminado con exito\n", id);
            }
            else
            {
                printf("No se ha podido eliminar el producto %d\n", id);
            }
        }
        else if (opcion == 6)
        {
            int id, existe = 0;
            printf("Ingrese el ID de la transaccion que desea eliminar: \n");
            scanf("%d", &id);
            eliminarTransaccion(&ML, id, &existe);
            if (existe == 1)
            {
                printf("Se ha eliminado la transaccion con exito\n");
            }
            else
            {
                printf("No se pudo eliminar la transaccion especificada\n");
                printf("El ID de transaccion ingresada no existe en nuestro sistema o el ID es erroneo\n");
            }
        }
        else if (opcion == 7)
        {
            printlistaTransacciones(&ML);
        }
        else if (opcion == 8)
        {
            int id, validacion;
            do {
                printf("Ingrese el id de la transaccion que desea buscar:\n");
                scanf("%d", &id);
                if(id <= 0)
                    printf ("Ingrese un id de transaccion mayor que 0\n");
            }while(id <= 0);

            buscarIDTransaccion(ML.transacciones, id, &validacion);
            if (validacion == 1)
                printf ("Se ha proporcionado la informacion de la transaccion buscada por su ID de forma exitosa.\n");
            else
            {
                printf ("El ID de transaccion: %d no se encuentra en nuestro historial de transacciones o no es valido (recuerde usar ID numericos mayores que 0)\n", id);
            }
        }
        else if (opcion == 9)
        {
            int id, existe = 0;
            printf("Ingrese el ID de la Bodega a eliminar: \n");
            scanf("%d", &id);
            eliminarBodega(&ML, id, &existe);
            if (existe == 1)
            {
                printf("Se ha eliminado la bodega con exito\n");
            }
            else{
                printf("No se pudo eliminar la Bodega especificada\n");
                printf("El ID de la Bodega ingresada no existe en nuestro sistema o el ID es erroneo\n");
            }
        }
        else if (opcion == 10)
        {
            int idTransaccion , existe = 0;
            printf("Ingrese el ID de transaccion del cual desea saber el detalle monetario de perdida o ganancia: \n");
            scanf("%d",&idTransaccion);
            detalleMontoTransaccion(ML.transacciones,idTransaccion,&existe);
            if (existe ==1)
            {
                printf("\nSe ha proporcionado el detalle monetario de la transaccion con exito\n");
            }
            else
            {
                printf("No se ha podido proporcionar detalles de transaccion con el ID ingresado\n");
                printf("El ID de la transaccion ingresada no existe en el sistema o es erroneo\n");
            }

        }
        else if (opcion == 11)
        {
            reporteMonetario(ML.transacciones);
        }
        else if (opcion == 12)
        {
            ingresarDatosBodega(&(ML.abbBodegas), 1);
        }
    }
    printf("Que tenga buen dia.");
    return 0;
}
