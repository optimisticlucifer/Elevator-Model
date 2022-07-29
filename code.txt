#define m1 2
#define m2 3
char a='0';
int b1=0,b2=0;
const int trigPin=7;
const int echoPin=4;
const float soundSpeed = 0.034; //cm per microsec
int time;
int distance;
void setup()
{
  pinMode(m1, OUTPUT);
  pinMode(m2, OUTPUT);
  pinMode(13, OUTPUT);
  pinMode(12, OUTPUT);
  pinMode(8, OUTPUT);
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  Serial.begin(9600);
}

void loop()
{
  //ultrasonic part started
	//ensure trigPin is low
  digitalWrite(trigPin, LOW);
  delayMicroseconds(10);
  
  //send trigger signal
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  
  //track time taken
  time = pulseIn(echoPin, HIGH);
  
  //compute distance 
  distance=(time*soundSpeed)/2;
  //Serial.print("Distance=");
  //Serial.println(distance);
  
  if((b1%2)==0)
  {   
    if(distance<=50)
      {
        digitalWrite(8, HIGH); 
      }
      else
      {
        digitalWrite(8, LOW);
      }
  }
  
  if(Serial.available())
  {
      char i;
      i=Serial.read();
      if(i=='1' && a=='0')
      {
      	forward1(2);
        
        Serial.println("F1");
        b1++;
        light1(b1);
              //ultrasonic part started
          //ensure trigPin is low
        digitalWrite(trigPin, LOW);
        delayMicroseconds(10);

        //send trigger signal
        digitalWrite(trigPin, HIGH);
        delayMicroseconds(10);
        digitalWrite(trigPin, LOW);

        //track time taken
        time = pulseIn(echoPin, HIGH);

        //compute distance 
        distance=(time*soundSpeed)/2;
        //Serial.print("Distance=");
        //Serial.println(distance);
        if((b1%2)==0)
        {   
          if(distance<=50)
            {
              digitalWrite(8, HIGH); 
            }
            else
            {
              digitalWrite(8, LOW);
            }
        }
        stop(3);
      }
      else if(i=='2' && a=='1')
      {
        forward2(2);
        
        Serial.println("F2");
        b2++;
        light2(b2);
        stop(3);
      }
      else if(i=='2' && a=='2')
      {
        stop(3);
        b2++;
        light1(b2);
        Serial.println("You are on floor 2 only");
        
      }
      else if(i=='0' && a=='2')
      {
      	backward0(4);
        light2(b2);
        Serial.println("F0");
       
        
        stop(3);
      }
      else if(i=='2' && a=='0')
      {
      	forward2(4);
        b2++;
        light2(b2);
        Serial.println("F2");
       
        
        stop(5);
      }
      else if(i=='1' && a=='2')
      {
      	backward1(2);
        b1++;
        light1(b1);
        
        
        
        Serial.println("F1");
        
        
        stop(3);
      }
      else if(i=='0' && a=='1')
      {
      	backward0(2);
        light1(b1);
        Serial.println("F0");
        
        
        stop(3);
      }
      else if(i=='1' && a=='1')
      {
        stop(3);
        b1++;
        light1(b1);
        Serial.println("You are on floor 1 only");
        
      }
      else if(i=='0' && a=='0')
      {
        stop(3);
        Serial.println("You are on ground only");
         
      }
    
    if(i=='3')
    {
    	lighton1();
    }
    if(i=='4')
    {
    	lightoff1();
    }
    if(i=='5')
    {
    	lighton2();
    }
    if(i=='6')
    {
    	lightoff2();
    }
    if(i=='7')
    {
    	digitalWrite(8, LOW);
    }
  }
}
void forward1(int t)
{
  //forward
  digitalWrite(m1, HIGH);
  digitalWrite(m2, LOW);
  delay(1000*t);
  //Serial.println("forward");
  a='1';
}
void forward2(int t)
{
  //forward
  digitalWrite(m1, HIGH);
  digitalWrite(m2, LOW);
  delay(1000*t);
  //Serial.println("forward");
  a='2';
}

void backward1(int t)
{
  //backward
  digitalWrite(m1, LOW);
  digitalWrite(m2, HIGH);
  delay(1000*t);
  //Serial.println("backward");
  a='1';
  
}
void backward0(int t)
{
  //backward
  digitalWrite(m1, LOW);
  digitalWrite(m2, HIGH);
  delay(1000*t);
  //Serial.println("backward");
  a='0';
}

void stop(int t)
{
  //STOP
  digitalWrite(m1, LOW);
  digitalWrite(m2, LOW);
  delay(1000*t);
  //Serial.println("stop");
}

void light1(int t)
{
	if((t%2)!=0)
    {
    	digitalWrite(12, HIGH);
    }
  	if((t%2)==0)
    {
    	digitalWrite(12, LOW);
    } 	
}
void light2(int t)
{
	if((t%2)!=0)
    {
    	digitalWrite(13, HIGH);
    }
  	if((t%2)==0)
    {
    	digitalWrite(13, LOW);
    } 	
}
void lighton1()
{
	digitalWrite(12, HIGH);
}
void lightoff1()
{
	digitalWrite(12, LOW);
}
void lighton2()
{
	digitalWrite(13, HIGH);
}
void lightoff2()
{
	digitalWrite(13, LOW);
}



