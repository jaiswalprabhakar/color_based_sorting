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
void off()
{
	PORTB = 0xFF;
	PORTD = 0xFF;
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
	DDRD = 0xFF;
	DDRA = 0b11110111;
	DDRB = 0xFF;
	PORTA = ( 1 << PINA4);
	//PORTA = ( 1 << PINA5);

	TCCR1A |= 1<<WGM11 | 1<<COM1A1 | 1<<COM1A0 | 1<<COM1B1 | 1<<COM1B0 ;
	TCCR1B |= 1<<WGM13 | 1<<WGM12 | 1<<CS10;
	ICR1 = 19999;
	
	//PORTB = 0xFF;
	//DDRC = 0xFF;
	
	
	

	while(1)
	{
		
		_delay_ms(2000);
		
		int r,g,b,c;

		r=MeasureR();
		g=MeasureG();
		b=MeasureB();
		c=MeasureC();

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
		/*if(c>smallest)
		{
			smallest=c;
		}*/
		
		
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
			  OCR1B = 19200;//color header
			  _delay_ms(1000);
			  OCR1A = 19100;//ball mover
			
			 PORTB = ( 1 << PINB3);
			 _delay_ms(1000);
			 OCR1A = 17800;//ball initial position
			 
			 
 			
 		}
 		else if(smallest==r)
 		{
			   OCR1B = 18700;//color header
			   _delay_ms(1000);
			  OCR1A = 19100;//ball mover
			
			 PORTB = ( 1<< PINB2);
			  _delay_ms(1000);
			 OCR1A = 17800;//ball init pos.
			
			
			
 		}
 		else if(smallest == b)
 		{
			  OCR1B = 18150;
			  _delay_ms(1000);
			  OCR1A = 19100;
			
			PORTB = ( 1 << PINB4);
			_delay_ms(1000);
			 OCR1A = 17800;
			 
 		
 		}
//  		else if(smallest == c)
// 		 {
// 			 PORTB = 0x00;
// 		 }
//  		
		
	

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
