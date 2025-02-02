# Modbus

#Description

Complemento para leer y escribir en sus dispositivos ModbusTCP/IP y RTU



# Configuración del complemento

Después de descargar el complemento, primero debe activarlo, como cualquier complemento Jeedom :

![config](./images/ModbusActiv.png)

Luego, hay que iniciar la instalación de las dependencias (aunque aparezcan OK) :

![dependances](./images/ModbusDep.png)

Finalmente, inicie el demonio :

![demon](./images/ModbusDemon.png)

Rien n'est à modifier dans le champ « Port socket interne » de la section « Configuration ».

![socket](./images/ModbusConfig.png)

En esta misma pestaña, deberás elegir el valor de descanso entre actualizar tu equipo (por defecto 5 seg)




# Uso del complemento


IMPORTANTE :

Para usar el complemento, debe conocer los parámetros de sus entradas / salidas de sus periféricos modbus (formato de datos, orden de bits, etc...)

Para los comandos, hay parámetros para seleccionar :

Detalles del parámetro :
- Valor negativo : para formatos de tipo LONG/INT, debe especificar si el valor de escritura/lectura será negativo
- Compensar : esto es si la compensación se tiene en cuenta o no en los números de registro en ciertos dispositivos Modbus
- Elija el tono del control deslizante : Esto es para elegir el paso del control deslizante en el caso de un comando de tipo Acción/Deslizador si desea enviar valores no enteros.




CONTROLES DE REPRODUCCIÓN :

Para entradas de bobinas :  
  - Agrega un ModbusTCP de E/S nuevo y nombra el comando. Elija un comando de tipo de información, en tipo binario o numérico.
  - Elija la función de código apropiada : en este caso, elija Fc1 leer Bobinas
  - Entonces es necesario elegir el registro inicial así como el número de bytes a leer (el número de registros)
  Cuando guarde, el comando creado se eliminará, para crear tantos comandos Coils como el número de bytes especificado.
  Ex: Si elige un registro de inicio de 1 y un número de bytes de 4, los comandos se crearán : LeerBobina_1, LeerBobina_2, LeerBobina_3, LeerBobina_4
  - Por supuesto, puede cambiar el nombre de las ReadCoils a su gusto.



  Para registros de existencias :
  - Agrega un ModbusTCP de E/S nuevo y nombra el comando. Elige un comando de tipo de información, en Tipo numérico.
  - Elige el formato correspondiente : Flotante o Largo/Entero
  - El registro inicial así como el número de bytes : para flotantes, el número máximo de registros codificados es de 4 registros (64 bits)



ESCRIBIR COMANDOS:

 En su equipo, por defecto habrá 3 comandos de tipo Acción/mensaje creados; Escritura de registro múltiple, escritura de bit y escritura de bobina múltiple


IMPORTANTE :


 Su principio de funcionamiento:



![cmdEcritures](./images/modbusCmdsEcritures.png)




  - Escritura de registro múltiple : en la configuración del comando se debe ingresar el registro de inicio, así como el orden de los bytes y palabra.
  Por defecto, el código de función es FC16. Por favor, deje esta configuración por defecto.

  Para cambiar los valores en los registros, use esta sintaxis:
  - valorparaenviar&nbofregistro, separados por | :   Ex:  120 y 1|214.5&4 Enviamos el entero 120 a un registro, partiendo del registro inicial configurado,
  entonces 214.5 en float en 4 registros siguientes al anterior.

  Para tipos flotantes, escriba el valor como se indica arriba, con un .



    - Escritura multibobina : en la configuración del comando, debe ingresar el registro de inicio
   Por defecto, el código de función es fc15. Por favor, deje esta configuración por defecto.

   Para cambiar los valores en los registros, use esta sintaxis:
    -  ex : 01110111 Entonces esto enviará desde el registro de inicio configurado los valores True(1) o False(0) a los registros




    - Bit de escritura : en la configuración del comando se debe ingresar el registro de inicio, así como el orden de los bytes y palabra.
    Por defecto, el código de función es fc03, porque este comando le dará el valor del registro establecido en binario al comando info "infobitbinary".

    Por favor, deje esta configuración por defecto.

    En el comando info "infobitbinary", tendrá el valor binario del registro de parámetros en el comando Write Bit.
    Para cambiar el bit en el registro

    - valuetoend&PositionBit :   Ex:  1&4 Enviamos el valor 1 al bit de la posición 4 empezando por la derecha
    En el comando de información "infobitbinary", verá el valor 10000101, que corresponde al valor binario del registro de parámetros.
    Al escribir 1 y 6, ahora tendrá el valor : 10100101 en el registro configurado.








Para escribir en una bobina :

  Ejemplo para registro 1 On:
  - Agrega un ModbusTCP de E/S nuevo y nombra el comando. Elige un comando de tipo Acción, en Tipo predeterminado.
  - Elija Fc5 Escribir bobina simple
  - Registro de salida : 1
  - Número de bytes : 1
  - Poner 1 en valor para enviar

  Ejemplo de registro 1 Off:
  - Agrega un ModbusTCP de E/S nuevo y nombra el comando. Elige un comando de tipo Acción, en Tipo predeterminado.
  - Elija Fc5 Escribir bobina simple
  - Registro de salida : 1
  - Número de bytes : 1
  - Poner 0 en valor para enviar


Al actuar sobre estos comandos de acción en su tablero, enviará Verdadero o Falso a sus Coils.




Para escribir en un registro de retención :

 - Agrega un ModbusTCP de E/S nuevo y nombra el comando. Elige un comando de tipo Acción, en Tipo de control deslizante.
 - Elija Fc5 Escribir registro único
 - Elija el formato para enviar al registro (esto cambiará el tipo de control deslizante en su tablero, dependiendo de si es flotante o largo/entero))
 - Elija el paso del control deslizante (para decimales, escriba con un .   ex: 0.2)
 - Elija también un valor mínimo y máximo para este control deslizante
