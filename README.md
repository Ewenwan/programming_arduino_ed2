# programming_arduino_ed2

The source code for the sketches in the second edition of the book [Programming Arduino: Getting Started with Sketches](https://www.amazon.com/Programming-Arduino-Getting-Started-Sketches/dp/1259641635) by Simon Monk published by McGraw-Hill Education (TAB)

![Programming Arduino: Getting Started with Sketches](https://i2.wp.com/simonmonk.org/wp-content/uploads/2013/11/cover4.jpg?resize=333%2C499)

# IO
## 延时闪灯
```c
void setup()
{
  pinMode(13, OUTPUT);
}

void loop()
{
 digitalWrite(13, HIGH);
 delay(500);
 digitalWrite(13, LOW);
 delay(500);
}

// sketch 03-02 加入变量====
int ledPin = 13;
int delayPeriod = 500;

void setup()
{
  pinMode(ledPin, OUTPUT);
}

void loop()
{
 digitalWrite(ledPin, HIGH);
 delay(delayPeriod);
 digitalWrite(ledPin, LOW);
 delay(delayPeriod);
}

// sketch 03-03  快闪--->满闪=====
int ledPin = 13;
int delayPeriod = 100;

void setup()
{
  pinMode(ledPin, OUTPUT);
}

void loop()
{
 digitalWrite(ledPin, HIGH);
 delay(delayPeriod);
 digitalWrite(ledPin, LOW);
 delay(delayPeriod);
 delayPeriod = delayPeriod + 100;
}


// sketch 03-06    快闪--->满闪-->快闪--->满闪---循环
int ledPin = 13;
int delayPeriod = 100;

void setup()
{
  pinMode(ledPin, OUTPUT);
}

void loop()
{
  digitalWrite(ledPin, HIGH);
  delay(delayPeriod);
  digitalWrite(ledPin, LOW);
  delay(delayPeriod);
  delayPeriod = delayPeriod + 100; // 闪烁变慢
  if (delayPeriod > 1000) // 循环===回到初始状态
  {
    delayPeriod = 100; 
  }
}


// sketch 03-08   ===== for循环 闪灯
int ledPin = 13;
int delayPeriod = 250;

void setup()
{
  pinMode(ledPin, OUTPUT);
}

void loop()
{
  for (int i = 0; i < 20; i ++)
  {
   digitalWrite(ledPin, HIGH);
   delay(delayPeriod);
   digitalWrite(ledPin, LOW);
   delay(delayPeriod);
  }
 delay(3000);
}


// sketch 03-09  带有休息时段的 常数闪灯=====
int ledPin = 13;
int delayPeriod = 100;
int count = 0;

void setup()
{
  pinMode(ledPin, OUTPUT);
}

void loop()
{
 digitalWrite(ledPin, HIGH);
 delay(delayPeriod);
 digitalWrite(ledPin, LOW);
 delay(delayPeriod);
 count ++;
 if (count == 20)
 {
   count = 0;
   delay(3000);  // 闪烁20次，定时休息
 }
}

// sketch 04-04==========带有休息时段的 常数闪灯 const static变量=====
const int ledPin = 13;
const int delayPeriod = 250;

void setup()
{
  pinMode(ledPin, OUTPUT);
}

void loop()
{
  static int count = 0;
  digitalWrite(ledPin, HIGH);
  delay(delayPeriod);
  digitalWrite(ledPin, LOW);
  delay(delayPeriod);
  count ++;
  if (count == 20)
  {
    count = 0;
    delay(3000);
  }
}


// sketch 04-01   for 循环实现闪灯  闪灯程序打包成函数====
const int ledPin = 13;
const int delayPeriod = 250;

void setup()
{
  pinMode(ledPin, OUTPUT);
}

void loop()
{
  for (int i = 0; i < 20; i ++)
  {//  for 循环实现闪灯
    flash();
  }
 delay(3000);
}

// 闪灯程序打包成函数
void flash()
{
   digitalWrite(ledPin, HIGH);
   delay(delayPeriod);
   digitalWrite(ledPin, LOW);
   delay(delayPeriod);
}



// sketch 04-02=============进一步包装闪灯程序
const int ledPin = 13;
const int delayPeriod = 250;

void setup()
{
  pinMode(ledPin, OUTPUT);
}

void loop()
{
  flash(20, delayPeriod);
  delay(3000);
}

void flash(int numFlashes, int d)
{
  for (int i = 0; i < numFlashes; i ++)
  {
    digitalWrite(ledPin, HIGH);
    delay(d);
    digitalWrite(ledPin, LOW);
    delay(d);
  }
}


// sketch 05-02   闪灯延时指定 时间  数组=====================
const int ledPin = 13;
int durations[] = {200, 200, 200, 500, 500, 500, 200, 200, 200};

void setup()
{
  pinMode(ledPin, OUTPUT);
}

void loop() 
{
  for (int i = 0; i < 9; i++)
  {
    flash(durations[i]);
  }
  delay(1000);
}

void flash(int duration)
{
   digitalWrite(ledPin, HIGH);
   delay(duration);
   digitalWrite(ledPin, LOW);
   delay(duration);
}

```

##  串口
```c
// sketch 03-04
void setup()
{
  Serial.begin(9600);
  int a = 2;
  int b = 2;
  int c = a + b;
  Serial.println(c); 
  Serial.println("Hello");
}
void loop()
{}

// sketch 5-04 ===============打印字符串数组========
char message[] = "Hello";

void setup()
{
  Serial.begin(9600);
}

void loop()
{
  Serial.println(message);
  delay(1000);
}



// sketch 03-05   串口 华氏度 摄氏度 转换=====
void setup()
{
  Serial.begin(9600);
  int degC = 20;
  int degF;
  degF = degC * 9 / 5 + 32;
  Serial.println(degF); 
}
void loop()
{}


    
// sketch 05-01   串口打印数组================
int durations[] = {200, 200, 200, 500, 500, 500, 200, 200, 200};

void setup()
{
  Serial.begin(9600);  
  for (int i = 0; i < 9; i++)
  {
    Serial.println(durations[i]);
  }
}

void loop() {}




// sketch 5-05  ======= 串口和闪灯混合，串口字符控制闪灯节奏=======

const int ledPin = 13;
const int dotDelay = 200;

char* letters[] = {
  ".-", "-...", "-.-.", "-..", ".", "..-.", "--.", "....", "..",    // A-I
  ".---", "-.-", ".-..", "--", "-.", "---", ".--.", "--.-", ".-.",  // J-R
  "...", "-", "..-", "...-", ".--", "-..-", "-.--", "--.."          // S-Z
};

char* numbers[] = {
  "-----", ".----", "..---", "...--", "....-", ".....", "-....", "--...", "---..", "----."};

void setup()                 
{
  pinMode(ledPin, OUTPUT);
  Serial.begin(9600);
}

void loop()                    
{
  char ch;
  if (Serial.available() > 0)
  {
    ch = Serial.read();
    if (ch >= 'a' && ch <= 'z')
    {
      flashSequence(letters[ch - 'a']);
    }
    else if (ch >= 'A' && ch <= 'Z')
    {
      flashSequence(letters[ch - 'A']);
    }
    else if (ch >= '0' && ch <= '9')
    {
      flashSequence(numbers[ch - '0']);
    }
    else if (ch == ' ')
    {
      delay(dotDelay * 4);  // gap between words  
    }
  }
}

void flashSequence(char* sequence)
{
  int i = 0;
  while (sequence[i] != NULL)
  {
    flashDotOrDash(sequence[i]);
    i++;
  }
  delay(dotDelay * 3);    // gap between letters
}

void flashDotOrDash(char dotOrDash)
{
  digitalWrite(ledPin, HIGH);
  if (dotOrDash == '.')
  {
    delay(dotDelay);           
  }
  else // must be a dash
  {
    delay(dotDelay * 3);           
  }
  digitalWrite(ledPin, LOW);    
  delay(dotDelay); // gap between flashes
}



```


## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```


## 
```c


```



## 
```c


```


## 
```c


```


## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```


## 
```c


```



## 
```c


```


## 
```c


```


## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```


## 
```c


```



## 
```c


```



## 
```c


```


## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```


## 
```c


```



## 
```c


```



## 
```c


```


## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```


## 
```c


```



## 
```c


```



## 
```c


```


## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```


## 
```c


```



## 
```c


```



## 
```c


```


## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```


## 
```c


```



## 
```c


```



## 
```c


```


## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```



## 
```c


```


## 
```c


```



## 
```c


```
