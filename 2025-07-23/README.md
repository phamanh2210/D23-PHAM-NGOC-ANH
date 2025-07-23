# D23-PHAM-NGOC-ANH
## A Báo cáo công việc
### 1 Đèn tín hiệu giao thông
```cpp 
#include "stm32f10x.h"
#include "stm32f10x_gpio.h"
void GPIOB_Configure()
{
	/*B1: cap clock for port */
	RCC->APB2ENR |= 0x00000008;
	GPIOB->CRL   |= 0x33333333;
	GPIOB->CRH   |= 0x00033300;
}

void GPIOC_Configure()
{
	RCC->APB2ENR |= 0x00000010;
	GPIOC->CRL   |= 0x00003300;
}

int mang[10] = {0xC0,0xF9,0xA4,0xB0,0x99,0x92,0x82,0xF8,0x80,0x90};
void delay(int time){
	int i,j;
	for(int i=0;i<time;i++){
		for(int j=0;j<0x2AFF;j++);
	}
}
int main()
{
	GPIOB_Configure();
	GPIOC_Configure();
	while(1){
		for(int i=10;i>=0;i--){
			
			if(i>2){
				GPIOB->ODR &= 0x1BFF;
				GPIOB->ODR |= ~(0x17FF );
				GPIOB->ODR |= ~(0x0FFF);
			}
			if(i==2 || i==1){
				GPIOB->ODR &= 0x17FF;
				GPIOB->ODR |=~(0x1BFF );
				GPIOB->ODR |=~(0x0FFF);
			
			}
			if(i==0){
				GPIOB->ODR &= 0x0FFF;
				GPIOB->ODR |=~(0x1BFF );
				GPIOB->ODR |=~(0x17FF);
			}
			
			for(int j=0;j<30;j++){
				GPIOB->ODR = (GPIOB->ODR & 0xFFFFFF00) | mang[i/10];
				GPIOC->ODR |= 0x00000004;
				delay(1);
				GPIOC->ODR &= ~(0x00000004);
				
				GPIOB->ODR = (GPIOB->ODR & 0xFFFFFF00) | mang[i%10];
				GPIOC->ODR |= 0x00000008;
				delay(1);
				GPIOC->ODR &= ~(0x00000008);
				
			}
		}
	}
}
```

giai thich