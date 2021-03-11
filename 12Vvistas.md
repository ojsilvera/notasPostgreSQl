## Vistas

Una vista es una alternativa para mostrar datos de varias tablas. Una vista es como una tabla virtual
que almacena una consulta. Los datos accesibles a través de la vista no están almacenados en la base
de datos como un objeto.

Entonces, una vista almacena una consulta como un objeto para utilizarse posteriormente. Las tablas
consultadas en una vista se llaman tablas base. En general, se puede dar un nombre a cualquier consulta
y almacenarla como una vista.

Las vistas permiten:
Ocultar información: generando el acceso a algunos datos y manteniendo oculto el resto de la información
que no se incluye en la vista. El usuario solo puede consultar la vista.

Simplificar la administración de los permisos de usuario: se pueden dar al usuario permisos para que solamente
pueda acceder a los datos a través de vistas, en lugar de concederle permisos para acceder a ciertos campos,
así se protegen las tablas base de cambios en su estructura.

Mejorar el rendimiento: se puede evitar tipear instrucciones repetidamente almacenando en una vista el resultado
de una consulta compleja que incluya información de varias tablas.

## Tipos de Vistas

- Vista Volatil
- Vista Materializada: persistente (Ayer)

### Vista Volatil
Cambia en cada ejecución.

-- Creamos la vista

    CREATE OR REPLACE VIEW public.rango_view
    AS
        SELECT *,
            CASE
            WHEN fecha_nacimiento > '2015-01-01' THEN
                'Mayor'
            ELSE
                'Menor'
            END AS tipo
        FROM pasajero ORDER BY tipo;
    ALTER TABLE public.rango_view OWNER TO postgres;

-- Ejecutamos la vista

    SELECT * FROM public.rango_view;

### Vistas Materializada
No se cambia a menos que queramos que se cambie.

    SELECT * FROM viaje WHERE inicio > '22:00:00';

-- Creamos la vista

    CREATE MATERIALIZED VIEW public.despues_noche_mview
    AS
        SELECT * FROM viaje WHERE inicio > '22:00:00';
        WITH NO DATA;
    ALTER TABLE public.despues_noche_mview OWNER TO postgres;

-- observamos la vista

    SELECT * FROM despues_noche_mview;

-- Damos refresh

    REFRESH MATERIALIZED VIEW despues_noche_mview;

-- Borramos una tupla de viaje cuando el id = 2, para observar que no se borro

    DELETE FROM viaje WHERE id = 2;