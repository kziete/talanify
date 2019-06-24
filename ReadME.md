Talanify Components

Son componentes que se construyen pensando que se van a repetir 
constantement en toda la plataforma, más allá de un módulo o vista 
específicos, tienden a ser pequeños e independientes.

Para crear un componente Talanify en general tomamos a priori las siguientes reglas:

1. Los componentes que usan props deben tener estados por default. Esto
ayuda a que cualquier desarrollador pueda conocer en primera instancia
la estructura de datos que un componente utiliza e incluso utilizarlo
para maquetaciones sencillas antes de empezar a mostrar/recibir data.

2. Los componentes que usan slots, también pueden utilizar estados por default.

3. Cada uno de los componentes debe tener un empty state, así facilitamos la creación de estos en toda la plataforma.
