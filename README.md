# HTTP for Arduino

HTTP Request library for Arduino and the Ethernet shield.

# Install

Clone (or download and unzip) the repository to `~/Documents/Arduino/libraries`
where `~/Documents/Arduino` is your sketchbook directory.

    > cd ~/Documents/Arduino
    > mkdir libraries
    > cd libraries
    > git clone https://github.com/csquared/arduino-http.git HTTP

# Usage

I test every way of calling the library against a public heroku app.

This means you can cut and paste the following into Arduino IDE and you can cofirm you are up and running
with the library.

```c++

#include <Ethernet.h>
#include <SPI.h>
#include "HTTP.h"

int test_delay = 2000; //so we don't spam the API

HTTP heroku = HTTP("arduino-http-lib-test.herokuapp.com");

//Setup
void setup() {
  Serial.begin(9600);

  // Connect via DHCP
  Serial.println("dhcp");
  byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };
  if (heroku.begin(mac) == 0) {
     Serial.println("Failed to configure Ethernet using DHCP");
  }

  Serial.println("Setup!");
}

void test_connect(int error){
   delay(test_delay);
   if(error == 1){
    Serial.println("TEST RESULT: fail");
   }else{
    Serial.println("TEST RESULT: ok");
   }
}

String response;
void test_response(){
  if(response == "OK"){
   Serial.println("TEST RESULT: ok");
  }else{
   Serial.println("TEST RESULT: fail");
  }
  response = "";
}


char* headers[] = {"X-Test-Header: true"};

void loop(){

  Serial.println("Test GET with path");
  test_connect(heroku.get("/get"));

  Serial.println("Test GET with path and respnose");
  test_connect(heroku.get("/get", &response));
  test_response();

  Serial.println("Test GET with path and headers");
  test_connect(heroku.get("/get-header", headers, 1));

  Serial.println("Test GET with path and headers and response");
  test_connect(heroku.get("/get-header", headers, 1, &response));
  test_response();

  Serial.println("Test POST with path and body");
  test_connect(heroku.post("/post", post_body));

  Serial.println("Test POST with path and body and response");
  test_connect(heroku.post("/post", post_body, &response));
  test_response();

  Serial.println("Test POST with path and body and headers");
  test_connect(heroku.post("/post", post_body, headers, 1));

  Serial.println("Test POST with path and body and headers and response");
  test_connect(heroku.post("/post", post_body, headers, 1, &response));
  test_response();

}

```

# Thanks

[ricardochimal](https://github.com/ricardochimal) For all his c++ help.  Couldn't have done this without you!

[theycallmeswift](https://github.com/theycallmeswift) Helping incept and debug v1.0
