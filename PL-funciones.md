# Procedimientos almacenados o PL (PL/pgSQL - SQL Procedural Language)

SINTAXIS BASICA DE UNA FUCION EN SQL

## no Declarando variables

    DO
      $$
         BEGIN

          'CODIGO';

         END
      $$

### ejemplo:

    DO
      $$
         BEGIN

            RAISE NOTICE 'mensaje aquí';

         END
      $$

**RAISE NOTICE: mostrará un mensaje en el area de messages(bitacora) del
manejador**

## Con declaración de variables

    DO
      $$
         DECLARE

          (varible) (tipoDato) := valor(puede estar o no presente);

         BEGIN

          'CODIGO';

         END
      $$

### ejemplo

    DO
      $$
        DECLARE

          rec record;
          contador integer := 0;

        BEGIN

          FOR rec IN SELECT * from pasajeros

            LOOP

              contador := contador + 1;
              RAISE NOTICE 'El pasajero % se llama %', contador, rec.nombre;

            END LOOP;

          RETURN CONTADOR;

        END
      $$

## Declarando una función

    CREATE FUNCTION testPL()
      RETURNS void // No regresa nada.
    AS
      $$
        DECLARE
          rec record;
          counter integer := 0;
        BEGIN
          FOR rec IN SELECT * FROM passenger
          LOOP
            RAISE NOTICE 'Un pasajero se llama %', rec.name;
            counter := counter + 1;
          END LOOP;
          RAISE NOTICE 'La cantidad de pasajeros es: %', counter;
        END
      $$
      LANGUAGE PLPGSQL;

### Verificando el llamado a la funcion

    SELECT testPL();


## Retornando un valor

  **Borramos la función anterior, para poder actualizar el tipo de dato que retorna**

      DROP FUNCTION testPL(); //

  **Actualizamos la función para retornar un valor**

      CREATE OR REPLACE FUNCTION testPL()

        RETURNS integer

      AS
        $$

          DECLARE

            rec record;
            counter integer := 0;

          BEGIN

            FOR rec IN SELECT * FROM passenger

            LOOP
              RAISE NOTICE 'Un pasajero se llama %', rec.name;
              counter := counter + 1;
            END LOOP;

            RAISE NOTICE 'La cantidad de pasajeros es: %', counter;

            RETURN counter;

          END

        $$
        LANGUAGE PLPGSQL;

  **Nos muestra en una celda la cantidad de pasajeros que hay en nuestra tabla, adicionalmente,**

      SELECT testPL();

  En la pestaña mensajes muestra las consultas muestra el resultado de la condulta y el retorno
  del valor esperado.

**Ref:**
<https://www.postgresql.org/docs/10/plpgsql.html>


## Creando PL en PGADMIN

- En el menú desplegable de la izquierda vamos a functions, click derecho, CREATE

- le damos el nombre

- en Definition se le tiene que asignar un tipo de retorno

- en Code pegamos el bloque de código que queremos insertar, en este caso es:

      DECLARE
        rec record;
        counter integer := 0;
      BEGIN
        FOR rec IN SELECT * FROM passenger LOOP
          RAISE NOTICE 'Un pasajero se llama %', rec.name;
          counter := counter + 1;
        END LOOP;
        RAISE NOTICE 'La cantidad de pasajeros es: %', counter;
        RETURN counter;
      END

- Las configuraciones por defecto nos funcionan bien cómo están, así que no moveremos nada por ahora,    depende de los requerimientos de la base de datos y de los proyectos en los que trabajemos requeriremos estas funciones, pero por ahora, la configuración por defecto es óptima.

- En la pestaña SQL nos muestra el código que se va a ejecutar, podemos ver que postgres coloca sus propios signos pesos, razón por la cual no es necesario que peguemos en la pestaña código dichos signos.

- Guardamos y vemos que la función ha sido creada.