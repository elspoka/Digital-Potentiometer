#include <avr/io .h>
/*  Reset pin connected to PB0
	Clock pin connected to PB1
	Data pin connected to PB2
*/
#define BIT SET(a ,b) ((a) |= (1<<(b)))
#define BIT CLEAR(a ,b) ((a) &= ˜(1<<(b)))

void DS1267( uint8 t );
uint16 t ADC Data( void );
void ADC Initialization ( void );
void USART Initialization ( void );
void USART transmit( uint16 t number );
uint8 t USART receive( void );

int main( void )
{
	// oles oi thyres epoikinwnias eksodoi
	DDRB = (1<<PB0)|(1<<PB1)|(1<<PB2);
	// oles xamhlo dunamiko
	PORTB &= ˜((1<<PB0)|(1<<PB1)|(1<<PB2));
	
	ADC Initialization ();
	USART Initialization ();
	
	while (1)
	{
	DS1267(USART receive ());
	for ( int j =0; j <100; j++) asm(”nop ”);
	USART transmit(ADC Data());
	}
	
	return 0;
}

void ADC Initialization ( void )
{
	// epilogh tashs anaforas to AVCC kai pin to ADC0
	ADMUX = (1<<REFS0);
	//energopoihsh tou ADC kai epilogh prescaler wste to roloi
	//tou ADC na einai 125KHz metaksu tou 50 kai 200 kHz
	ADCSRA = (1<<ADEN)|(1<<ADPS0)|(1<<ADPS1);
}

uint16 t ADC Data( void )
{
	// ekkinish anagnwsis
	ADCSRA |= (1<<ADSC);
	//otan oloklhrwrthei epistreferi sto 0
	while ( ADCSRA & (1<<ADSC) );
	// epistrofh dedomenwn
	return ADC;
}

void USART Initialization ( void )
{
	// epilogh tou baud rate sta 4800bps me 1MHz roloi
	UBRR0H = 0x00 ;
	UBRR0L = 0x0C;
	//energopoihsh ths apostolhs kai lhpshs
	UCSR0B |= (1<<RXEN0)|(1<<TXEN0);
	//1 stop bit kai 8 bit dedomena
	//UCSR0C = (3<<UCSZ00)|(3<<UCSZ01);
}

void USART transmit( uint16 t number )
{
	//perimenoume na teleiwsei opoiadhpote epikoinwnia
	while ( !( UCSR0A & (1<<UDRE0)) );
	char digits []= ”0000”;// megisto 4 pshfia gia to 1023
	//an einai 0 h eisodos steile to 0 se ascii
	if (number == 0) UDR0= ’0 ’;
	else
		{
			int pshfio = 0;
			while (number>0)
			{
				digits [ pshfio ] =number%10;
				number =number/10;
				pshfio++;
				
			}
			int i ;
			for ( i=pshfio −1;i >=0;i−−)
				{
					while ( !( UCSR0A & (1<<UDRE0)) );
					UDR0 =digits [ i ]+48;
				}
		}
		while ( !( UCSR0A & (1<<UDRE0)) );
		UDR0 = ’\n ’ ;
}

uint8 t USART receive( void )
{
	while (!(UCSR0A&(1<<RXC0)));
	return UDR0;
}

void DS1267( uint8 t wiper )
{
	BIT SET(PORTB,PB0);// rst high
	BIT CLEAR(PORTB,PB2);//b0 always low
	BIT SET(PORTB,PB1);// high clock
	asm(”nop”);// wait a bit
	BIT CLEAR(PORTB,PB1);// low clock
	
	for ( int i =7;i >−1;i−−)
	{
		if ((( wiper >> i ) & 0x1) == 1) BIT SET(PORTB,PB2);
		else BIT CLEAR(PORTB,PB2);
		BIT SET(PORTB,PB1);// high clock
		asm(”nop”);// wait a bit
		BIT CLEAR(PORTB,PB1);// low clock
	}
	
	for ( int i =7;i >−1;i−−)
	{
		i f ((( wiper >> i ) & 0x1) == 1) BIT SET(PORTB,PB2);
		else BIT CLEAR(PORTB,PB2);
		BIT SET(PORTB,PB1);// high clock
		asm(”nop”);// wait a bit
		BIT CLEAR(PORTB,PB1);// low clock
	}
	
	BIT CLEAR(PORTB,PB0);// rst low
	BIT CLEAR(PORTB,PB1);
	BIT CLEAR(PORTB,PB2);
}
