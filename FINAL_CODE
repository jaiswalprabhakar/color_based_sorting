// #define F_CPU 16000000
#include <avr/io.h>
#include <util/delay.h>


int MeasureR();
int MeasureG();
int MeasureB();
int MeasureC();

void TCSSelectRed()
{
	PORTA &= ~(1 << PINA2); 
	PORTA &= ~(1 << PINA1);
	
}

void TCSSelectGreen()
{
	PORTA |= (1 << PINA2) | (1 << PINA1);
	//TCSS2High();
	//TCSS3High();
}

void TCSSelectBlue()
{
	PORTA &= ~(1 << PINA2);
	PORTA |= (1 << PINA1);
	//TCSS2Low();
	//TCSS3High();
}

void TCSSelectClear()
{
	PORTA |= (1 << PINA2);
	PORTA &= ~(1 << PINA1);
	//TCSS2High();
	//TCSS3Low();
}




uint32_t TCSMeasure()
{
	char RC_PIN_STATE = (bit_is_set(PINA,3)==0);
	
	//timeout zone
	unsigned long numloops = 0;
	unsigned long maxloops = 500000;
	unsigned long width = 0;
	// wait for any previous pulse to end
	while ( RC_PIN_STATE == 1)
	{
		RC_PIN_STATE = (bit_is_set(PINA,3)==0); //keep reading the pin until it changes the state
		if (numloops++ == maxloops) break;
	}
	// wait for the pulse to start
	while (RC_PIN_STATE == 0)
	{
		RC_PIN_STATE = (bit_is_set(PINA,3)==0); //keep reading the pin until it changes the state
		if (numloops++ == maxloops) break;
	}
	
	// wait for the pulse to stop @ here we are measuring the pulse width = incrementing the WIDTH value by one each cycle. atmega328 1 micro second is equal to 16 cycles.
	while (RC_PIN_STATE == 1)
	{
		RC_PIN_STATE = (bit_is_set(PINA,3)==0);
		if (numloops++ == maxloops) break;
		width++;
	}

	float RC_VALUE = width/16;
	return RC_VALUE;
}

/*int TCSMeasure()
{
	if(bit_is_set(PINA,3)==0)
	{
		while(bit_is_set(PINA,3)==0);   //Wait for rising edge
	}

	while(bit_is_set(PINA,3)==1);   //Wait for falling edge
	
	

	TCNT1=0x0000;//Reset Counter

	TCCR1B=(1<<CS10); //Prescaller = F_CPU/1 (Start Counting)

	while(bit_is_set(PINA,3)==0);   //Wait for rising edge
 int R=TCNT1;
	float f=8000000/R;
	
	//Stop Timer
	TCCR1B=0x00;

	return f;
}*/








int main(void)
{
	//DDRD = 0x00;
	DDRA = 0b11110111;
	DDRB = 0xFF;
	PORTA = ( 1 << PINA4);
	PORTA = ( 1 << PINA5);
	
	//PORTB = 0xFF;
	//DDRC = 0xFF;
	
	
	

	while(1)
	{
		
		int r,g,b;

		r=MeasureR();
		g=MeasureG();
		b=MeasureB();

		int smallest;

		if(r<b)
		{
			if(r<g)
			smallest=r;
			else
			smallest=g;
		}
		else
		{
			if(b<g)
			smallest=b;
			else
			smallest=g;
		}
	/*	if( r >= 11000)
		{
			PORTB=0b00000010;
			//smallest = r;
		}
		else if( g >= 11000)
		{
		PORTB=0b00000001;
		// smallest = g;
			
		}*/

 		if(smallest==g)
 		{
 			PORTB = ( 1 << PINB1);
 		}
 		else if(smallest==r)
 		{
			PORTB = ( 1<< PINB0);
 		}
 		else if(smallest == b)
 		{
 			PORTB = ( 1 << PINB2);
 		}
 		
 		
		
	}

}

int MeasureR()
{
	TCSSelectRed();
	int r;

	_delay_ms(1);
	r=TCSMeasure();

	_delay_ms(1);
	r+=TCSMeasure();

	_delay_ms(1);
	r+=TCSMeasure();

	return r;

}

int MeasureG()
{
	TCSSelectGreen();
	int r;

	_delay_ms(1);
	r=TCSMeasure();

	_delay_ms(1);
	r+=TCSMeasure();

	_delay_ms(1);
	r+=TCSMeasure();

	return r;

}

int MeasureB()
{
	TCSSelectBlue();
	int r;

	_delay_ms(1);
	r=TCSMeasure();

	_delay_ms(1);
	r+=TCSMeasure();

	_delay_ms(1);
	r+=TCSMeasure();

	return r;

}

int MeasureC()
{
	TCSSelectClear();
	int r;

	_delay_ms(1);
	r=TCSMeasure();

	_delay_ms(1);
	r+=TCSMeasure();

	_delay_ms(1);
	r+=TCSMeasure();

	return r;
}
