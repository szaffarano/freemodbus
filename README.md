Freemodbus V1.5 patcheado
========================

El [proyecto freemodbus](http://www.freemodbus.org/) que era hosteado
 por [Berlios](http://www.berlios.de/) parece haber desaparecido.  Basándome en 
 el código de la versión 1.5 que descargué de [sourceforge](http://sourceforge.net/projects/freemodbus.berlios/)
 realicé modificaciones en el port de AVR para compatibilizar con la versión 4.8.X de avr-gcc y le
 agregué lógica para que soporte al MCU ATMega328 (debería funcionar con muchos más modelos
 modificando/agregando los *#ifdef* correspondientes).
 
Tener en cuenta que el port para AVR implementado utiliza el Timer1 (16 bits) del MCU y habilita
el uso de la USART utilizando interrupciones.

Ejemplo de uso:

```c

#include <modbus/include/mb.h>
#include <modbus/include/mbport.h>

int main(void) {
	// inicializa modbus identificando al slave con ID 0x03
	// comunicándose vía RS232 a 9600 baudios sin paridad
	// utilizando modo RTU (asegurarse de tener dicho modo
	// habilitado en el archivo de configuración: MB_RTU_ENABLED 
	// seteado a 1 en include/mbconfig.h).
	eMBInit(MB_RTU, 0x03, 0, 9600, MB_PAR_NONE);

	// habilita modbus
	eMBEnable();

	for (;;) {
		// recibe / envía frames
		eMBPoll();
		
		// lógica del MCU
	}
}

eMBErrorCode eMBRegInputCB(UCHAR * pucRegBuffer, USHORT usAddress,
		USHORT usNRegs) {
	// callback para funciones modbus que modifican input registers
	return MB_ENOERR;
}

eMBErrorCode eMBRegHoldingCB(UCHAR * pucRegBuffer, USHORT usAddress,
		USHORT usNRegs, eMBRegisterMode eMode) {
	// callback para funciones modbus que modifican holding registers
	return MB_ENOERR;
}

eMBErrorCode eMBRegCoilsCB(UCHAR * pucRegBuffer, USHORT usAddress,
		USHORT usNCoils, eMBRegisterMode eMode) {
	// callback para funciones modbus que modifican coils
	return MB_ENOREG;
}

eMBErrorCode eMBRegDiscreteCB(UCHAR * pucRegBuffer, USHORT usAddress,
		USHORT usNDiscrete) {
	// callback para funciones modbus que modifican registros discretos
	return MB_ENOREG;
}
```
Más información en la [api documentation](http://www.freemodbus.org/api/index.html) del proyecto.