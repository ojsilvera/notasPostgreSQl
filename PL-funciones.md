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

## Declarando variables

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

**Ref:**
<https://www.postgresql.org/docs/10/plpgsql.html>