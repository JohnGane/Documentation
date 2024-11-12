Открытый коммуникационный протокол, который массово используется в промышленности. 
- Основан на архитектуре master-slave;
- В качестве транспортного протокола Modbus использует TCP (в некоторых случаях UDP);
- 
Проверка скрипта [firewall_litedpi_check_alfa.tar.gz] ## Bug #912

Change Log 

1)  В разделе liteDPI (МСЭ) обновлен профиль протокла IEC104 и MODBUS - изменен подход по добавлению и удалению правил

2) Для протокла modbus:

- раширен диапазон f-кодов. 
- добавлены операции больше,меньше
- для операции равно добавлена возможность нескольких значений (по ИЛИ)
- В "тип поля" добавлена опция Range - которая является сочетанием поля "Address number" и "Word count" (определяет возможный диапазон считывания/записи переменных. Адекватно может работать только с вердиктом - "Accept", из-за того, что  протокол может задавать адрес начала считывания в одной перменной , а кол-во, в другой., поэтому может прийти запрос выходящий за рамки, установленные в фильтре. Например стоит диапазон в фильтре на запрет диапазона 2000-2050, это означает, что мы смотрим начало адреса 2000 и количество переменных <= 50. Но может прийти пакет с адресом 1999 и кол-вом запрашиваемых значений 30, что также попадает в диапазон 2000-2050, но отфильтровать мы его уже не сможем. )
- Добавлен контроль соединения "state check" - смотрит порт установленный или по дефолту  для данного протокола и проверяет что в прямом и ответном направлении соединениия сохранялось это постоянство.
- Добавлен контроль протокола "sanity check" - дополнителньо смотрит соблюдение диапазонов полей протокла

Для протокла iec104:

- раширен диапазон типов полей - добавлены все возможные (все, кроме непосредственных значений. Значения отлавливать сложно, поскольку они плавают в нагрузке). 
- добавлены операции больше,меньше
- Валидация несоответствия типов полей (например типы пакета "I" не могут быть использованы в сочетании с "U")
- Добавлен контроль соединения "state check" - смотрит порт установленный или по дефолту  для данного протокола и проверяет что в прямом и ответном направлении соединениия сохранялось это постоянство.
- Добавлен контроль протокола "sanity check" - дополнителньо смотрит соблюдение диапазонов полей протокла

3) Добавлены новые вердикты для фильтра (Reject TCP и Reject ICMP *)

# Основные сведения Modbus

Протокол использует по умолчанию TCP с портом 502.


# Modbus Slave

To run a Modbus/TCP server on Ethernet run:

`diagslave -m tcp`

 **Usage**

`Usage: diagslave [OPTIONS] [SERIALPORT]`
`Arguments:`
`SERIALPORT    Serial port when using Modbus ASCII or Modbus RTU protocol`
              `COM1, COM2 ...                on Windows`
              `/dev/ttyS0, /dev/ttyS1 ...    on Linux`
`General options:`
`-m ascii      Modbus ASCII protocol`
`-m rtu        Modbus RTU protocol (default if SERIALPORT set)`
`-m tcp        MODBUS/TCP protocol (default otherwise)`
`-m udp        MODBUS UDP`
`-m enc        Encapsulated Modbus RTU over TCP`
`-o #          Master activity time-out in seconds (1.0 - 100, 3 s is default)`
`-a #          Slave address (1-255 for RTU/ASCII, 0-255 for TCP)`
`Options for MODBUS/TCP, UDP and RTU over TCP:`
`-p #          TCP port number (502 is default)`
`-c #          Connection time-out in seconds (1.0 - 3600, 60 s is default)`
`Options for Modbus ASCII and Modbus RTU:`
`-b #          Baudrate (e.g. 9600, 19200, ...) (19200 is default)`
`-d #          Databits (7 or 8 for ASCII protocol, 8 for RTU)`
`-s #          Stopbits (1 or 2, 1 is default)`
`-p none       No parity`
`-p even       Even parity (default)`
`-p odd        Odd parity`
`-4 #          RS-485 mode, RTS on while transmitting and another # ms after`
`Options for Modbus RTU:`
`-f #          Tolerance for inter-frame gap detection in ms (0-20)`


# Modbus Master

**Usage**

`Usage: modpoll [OPTIONS] SERIALPORT|HOST [WRITEVALUES...]`
`Arguments:`
`SERIALPORT    Serial port when using Modbus ASCII or Modbus RTU protocol`
                `COM1, COM2 ...                on Windows`
                `/dev/ttyS0, /dev/ttyS1 ...    on Linux`
`HOST          Host name or dotted IP address when using MDBUS/TCP protocol`
`WRITEVALUES   List of values to be written. If none specified (default) modpoll reads data.`
`General options:`
`-m ascii      Modbus ASCII protocol`
`-m rtu        Modbus RTU protocol (default if SERIALPORT contains \ or COM)`
`-m tcp        MODBUS/TCP protocol (default otherwise)`
`-m udp        MODBUS UDP`
`-m enc        Encapsulated Modbus RTU over TCP`
`-a #          Slave address (1-247 for serial, 0-255 for TCP, 1 is default)`
`-r #          Start reference (1-65536, 1 is default). Use -0 for 0-based references.`
`-c #          Number of values to read (1-125, 1 is default), optional for writing (use -c 1 to force FC5 or FC6)`
`-t 0          Discrete output (coil) data type (FC 1)`
`-t 1          Discrete input data type (FC 2)`
`-t 3          16-bit input register data type (FC 4)`
`-t 3:hex      16-bit input register data type with hex display`
`-t 3:i32      32-bit integer data type in input register table`
`-t 3:i64      64-bit integer data type in input register table`
`-t 3:mod      32-bit module 10000 data type in input register table`
`-t 3:f32      32-bit float data type in input register table`
`-t 3:f64      64-bit double data type in input register table`
`-t 4          16-bit holding register data type (FC3, default)`
`-t 4:hex      16-bit holding register data type with hex display`
`-t 4:i32      32-bit integer data type in holding register table`
`-t 4:i64      64-bit integer data type in holding register table`
`-t 4:mod      32-bit module 10000 type in holding register table`
`-t 4:f32      32-bit float data type in holding register table`
`-t 4:f64      64-bit double data type in holding register table`
`-t id         Read device identification objects (FC 43/14)`
`-t file       File record reference type 6 (FC 20/21)`
`-n #          File number for file record (default is 4)`
`-i            Slave operates on big-endian 32-bit/64-bit integers`
`-f            Slave operates on big-endian 32-bit/64-bit floats`
`-e            Use Daniel/Enron single register 32-bit mode (implies -i and -f)`
`-x             
`Options for MODBUS/TCP, UDP and RTU over TCP:`
`-p #          IP protocol port number (502 is default)`
`Options for Modbus ASCII and Modbus RTU:`
`-b #          Baudrate (e.g. 9600, 19200, ...) (19200 is default)`
`-d #          Databits (7 or 8 for ASCII protocol, 8 for RTU)`
`-s #          Stopbits (1 or 2, 1 is default)`
`-p none       No parity`
`-p even       Even parity (default)`
`-p odd        Odd parity`
`-4 #          RS-485 mode, RTS on while transmitting and another # ms after`

Примеры:

To write the value 1234 to register 1201 using Modbus/TCP and FC 16 (Write Multiple Registers):
`modpoll -r 1201 10.0.0.100 1234`

To write the value 1234 to register 1201 using Modbus/TCP and FC 6 (Write Single Register):
`modpoll -r 1201 -c 1 10.0.0.100 1234`