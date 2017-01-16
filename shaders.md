Default input: `vec4 gl_FragCoord`, holds the screen coordinates of the pixel or screen fragment that the active thread is working on.

Default output: `vec4 gl_FragColor`


#### Functions

**`step()`** (linear interpolation) -  The first argument is the limit or threshold, while the second one is the value we want to check or pass. Any value under the limit will return 0.0 while everything above the limit will return 1.0.

**`smoothStep()`** - Given a range of two numbers and a value, this function will interpolate the value between the defined range. The two first parameters are for the beginning and end of the transition, while the third is for the value to interpolate.

**`fract()`** - Extracts the decimal part, f.ex: `fract(1.5) = 0.5;`  
**`ceil(x)`** - nearest integer that is greater than or equal to x  
**`floor(x)`** - nearest integer less than or equal to x  
**`sign(x)`** - extract the sign of x  
**`abs(x)`** - return the absolute value of x  
**`clamp(x,0.0,1.0)`** - constrain x to lie between 0.0 and 1.0  
**`min(0.0,x)`** - return the lesser of x and 0.0  
**`max(0.0,x)`** - return the greater of x and 0.0  

A great feature of vector types in GLSL is that the properties can be combined in any order you want, which makes it easy to cast and mix values. This ability is called **swizzle**.

    vec3 yellow, magenta, green;
    
    // Making Yellow 
    yellow.rg = vec2(1.0);  // Assigning 1. to red and green channels
    yellow[2] = 0.0;        // Assigning 0. to blue channel
    
    // Making Magenta
    magenta = yellow.rbg;   // Assign the channels with green and blue swapped
    
    // Making Green
    green.rgb = yellow.bgb; // Assign the blue channel of Yellow (0) to red and blue channels 

#### Easing Functions:

    float linear(float t) {
      return t;
    }
    
    float exponentialIn(float t) {
      return t == 0.0 ? t : pow(2.0, 10.0 * (t - 1.0));
    }
    
    float exponentialOut(float t) {
      return t == 1.0 ? t : 1.0 - pow(2.0, -10.0 * t);
    }
    
    float exponentialInOut(float t) {
      return t == 0.0 || t == 1.0
        ? t
        : t < 0.5
          ? +0.5 * pow(2.0, (20.0 * t) - 10.0)
          : -0.5 * pow(2.0, 10.0 - (t * 20.0)) + 1.0;
    }
    
    float sineIn(float t) {
      return sin((t - 1.0) * HALF_PI) + 1.0;
    }
    
    float sineOut(float t) {
      return sin(t * HALF_PI);
    }
    
    float sineInOut(float t) {
      return -0.5 * (cos(PI * t) - 1.0);
    }
    
    float qinticIn(float t) {
      return pow(t, 5.0);
    }
    
    float qinticOut(float t) {
      return 1.0 - (pow(t - 1.0, 5.0));
    }
    
    float qinticInOut(float t) {
      return t < 0.5
        ? +16.0 * pow(t, 5.0)
        : -0.5 * pow(2.0 * t - 2.0, 5.0) + 1.0;
    }
    
    float quarticIn(float t) {
      return pow(t, 4.0);
    }
    
    float quarticOut(float t) {
      return pow(t - 1.0, 3.0) * (1.0 - t) + 1.0;
    }
    
    float quarticInOut(float t) {
      return t < 0.5
        ? +8.0 * pow(t, 4.0)
        : -8.0 * pow(t - 1.0, 4.0) + 1.0;
    }
    
    float quadraticInOut(float t) {
      float p = 2.0 * t * t;
      return t < 0.5 ? p : -p + (4.0 * t) - 1.0;
    }
    
    float quadraticIn(float t) {
      return t * t;
    }
    
    float quadraticOut(float t) {
      return -t * (t - 2.0);
    }
    
    float cubicIn(float t) {
      return t * t * t;
    }
    
    float cubicOut(float t) {
      float f = t - 1.0;
      return f * f * f + 1.0;
    }
    
    float cubicInOut(float t) {
      return t < 0.5
        ? 4.0 * t * t * t
        : 0.5 * pow(2.0 * t - 2.0, 3.0) + 1.0;
    }
    
    float elasticIn(float t) {
      return sin(13.0 * t * HALF_PI) * pow(2.0, 10.0 * (t - 1.0));
    }
    
    float elasticOut(float t) {
      return sin(-13.0 * (t + 1.0) * HALF_PI) * pow(2.0, -10.0 * t) + 1.0;
    }
    
    float elasticInOut(float t) {
      return t < 0.5
        ? 0.5 * sin(+13.0 * HALF_PI * 2.0 * t) * pow(2.0, 10.0 * (2.0 * t - 1.0))
        : 0.5 * sin(-13.0 * HALF_PI * ((2.0 * t - 1.0) + 1.0)) * pow(2.0, -10.0 * (2.0 * t - 1.0)) + 1.0;
    }
    
    float circularIn(float t) {
      return 1.0 - sqrt(1.0 - t * t);
    }
    
    float circularOut(float t) {
      return sqrt((2.0 - t) * t);
    }
    
    float circularInOut(float t) {
      return t < 0.5
        ? 0.5 * (1.0 - sqrt(1.0 - 4.0 * t * t))
        : 0.5 * (sqrt((3.0 - 2.0 * t) * (2.0 * t - 1.0)) + 1.0);
    }
    
    float bounceOut(float t) {
      const float a = 4.0 / 11.0;
      const float b = 8.0 / 11.0;
      const float c = 9.0 / 10.0;
    
      const float ca = 4356.0 / 361.0;
      const float cb = 35442.0 / 1805.0;
      const float cc = 16061.0 / 1805.0;
    
      float t2 = t * t;
    
      return t < a
        ? 7.5625 * t2
        : t < b
          ? 9.075 * t2 - 9.9 * t + 3.4
          : t < c
            ? ca * t2 - cb * t + cc
            : 10.8 * t * t - 20.52 * t + 10.72;
    }
    
    float bounceIn(float t) {
      return 1.0 - bounceOut(1.0 - t);
    }
    
    float bounceInOut(float t) {
      return t < 0.5
        ? 0.5 * (1.0 - bounceOut(1.0 - t * 2.0))
        : 0.5 * bounceOut(t * 2.0 - 1.0) + 0.5;
    }
    
    float backIn(float t) {
      return pow(t, 3.0) - t * sin(t * PI);
    }
    
    float backOut(float t) {
      float f = 1.0 - t;
      return 1.0 - (pow(f, 3.0) - f * sin(f * PI));
    }
    
    float backInOut(float t) {
      float f = t < 0.5
        ? 2.0 * t
        : 1.0 - (2.0 * t - 1.0);
    
      float g = pow(f, 3.0) - f * sin(f * PI);
    
      return t < 0.5
        ? 0.5 * g
        : 0.5 * (1.0 - g) + 0.5;
    }

