#define FCPU 16000000
#define FOSC 16000000 // Clock Speed
#define BAUD 9600
#define MYUBRR FOSC/16/BAUD-1

#include <avr/io.h>

int volatile temp_q = 0;
int volatile time_count = 0;

void setup()
{
  
}
void loop()
{}

int main()
{ 
 DDRB |= 1 << PB5; 
 DDRB |= 1 << PB1;
 DDRB |= 1 << PB3;
    
 OCR1A = 65535/40; // 100 ms
 TCCR1B = (1 << WGM12) | (1 << CS12) | (1 << CS00);
 TIMSK1 = (1 << OCIE1A);
    
 //Disp_Init();
 //PWM_Init();
 USART_Init(MYUBRR);
 ADC_Init();
 
 sei();
 while(1)
 {
 }
}

void Disp_Init(void)
{
 // Initializare ca iesiri.
 DDRD |= 1 << PD3 | 1 << PD2 | 1 << PD4 | 1 << PD5  | 1 << PD7;
 DDRB |= 1<< PB0 | 1 << PB2;
  
 // Pornirea tuturor LED-urilor prin 0 logic.
 PORTD &= ~(1<<PD3);
 PORTD &= ~(1<<PD2);
 PORTD &= ~(1<<PD4); 
 PORTD &= ~(1<<PD5);
 PORTD &= ~(1<<PD7);
 PORTB &= ~(1<<PB0);
 PORTB &= ~(1<<PD2);
}

void Disp_Char(char c)
{
 PORTD |= 1 << PD3 | 1 << PD2 | 1 << PD4 | 1 << PD5  | 1 << PD7;
 PORTB |= 1 << PB0 | 1 << PB2; 
  
  if (c == 'P')
  {
     PORTD &= ~(1 << PD3 | 1 << PD4 | 1 << PD2 | 1 << PD7);
     PORTB &= ~(1 << PB0 | 1 << PB2);
  }
  else if (c == 'N')
  {
     PORTD &= ~(1 << PD4 | 1 << PD7);
     PORTB &= ~(1 << PB2);
  }
}


void PWM_Init(void)
{
  // 1. LED ca iesire.
  DDRD |= 1 << PD6;
  
  // 2. Alegem Timer PWM CS
  TCCR0B = (1 << CS02);
  
  // 3. Mod FAST PWM
  TCCR0A = (1 << WGM00) | (1 << WGM01);
  
  // 4. Setam OCR output mode
  TCCR0A = (1 << COM0A1);
  
  // 5. Test valori OCR
  OCR0A = 0;  
}

void ADC_Init(void)
{
  ADCSRA = 1 << ADEN | 1 << ADIE | 1 << ADSC | 1 << ADPS1 | 1 << ADPS2 | 1 << ADPS0;
  ADMUX = 1 << REFS0;
}

void PWM_Logic(void)
{
  if((time_count >= 0) && (time_count < 10))
  {
    // Fade IN
    OCR0A += 5;
  }
  else if((time_count >= 10) && (time_count < 20))
  {
    // Aprins
    OCR0A = 50;
  }
  else if((time_count >= 20) && (time_count < 30))
  {
    // Fade out
    OCR0A -= 5;
  }
  else if ((time_count >= 30) && (time_count < 40))
  {
    // Stins
    OCR0A = 0;
  }
  else
  {
    time_count = 0;
  }
}

ISR(ADC_vect)
{
  while(ADCSRA & (1 << ADSC));
  temp_q = ADC;
  
  // Logica procesare temperaturii citite. 35C
  if(temp_q > 173)
  {
    PORTB |= 1 << PB3;
  }
  else
  {
    PORTB &= ~(1 << PB3);
  }
  ADMUX = 1 << REFS0;

  ADCSRA |= 1 << ADSC;
}

ISR(USART_RX_vect)
{
   char c = USART_Receive();
   if(c == 'A')
   {
     PORTB |= 1 << PB5;
   }
   else if (c == 'S')
   {
     PORTB &= ~(1 << PB5);
   }
}

ISR(TIMER1_COMPA_vect)
{
  time_count++;
  PWM_Logic();
  if (!(time_count % 10))
  {
    PORTB ^= 1 << PB1;
    Transmitere_Temperatura();
  }
  
  if ((time_count%40) > 20)
  {
     Disp_Char('P');
  }
  else
  {
     Disp_Char('N');
  }
}

void USART_Init(unsigned int ubrr)
{
  /* Set baud rate */
  UBRR0H = (unsigned char)(ubrr>>8);
  UBRR0L = (unsigned char)ubrr;
  /* Enable receiver and transmitter */
  UCSR0B = (1<<RXEN0)|(1<<TXEN0) | (1 << RXCIE0);
  /* Set frame format: 8data, 2stop bit */
  UCSR0C = (1<<USBS0)|(3<<UCSZ00);
}

unsigned char USART_Receive(void)
{
  /* Wait for data to be received */
  while (!(UCSR0A & (1<<RXC0)))
  ;
  /* Get and return received data from buffer */
  return UDR0;
}

void USART_Transmit(unsigned char data)
{
  /* Wait for empty transmit buffer */
  while (!(UCSR0A & (1<<UDRE0)))
  ;
  /* Put data into buffer, sends the data */
  UDR0 = data;
}

void Transmitere_Temperatura()
{
  float q = 5000.0/1023;
  float temp_u = temp_q * q;
  int temp_final = (temp_u / 10);
  int temp_zecimale = 0;
  temp_zecimale = temp_u - temp_final*10;
  
  char buf[50];
  memset(buf, 0, sizeof(buf));
  sprintf(buf, "T=%d.%d\n", temp_final, temp_zecimale);
  
  for (int i=0; i < strlen(buf); i++)
  {
    USART_Transmit(buf[i]);
  }
}

// Modul 3
//  q  = 5000mV/1023 ~5mV
// -50 ... 200 Vout 10mV/*C
// -40 -> Vout= 10C * 10mV/C = 100mV;
// -30 - 200
// 35C / 850mV   nr_q = 850mV/q = 173  
