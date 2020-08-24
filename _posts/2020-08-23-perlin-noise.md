---
layout: post
title: Randomness in GLSL
description: Notes taken from the shaderbook
tags: [Computer graphics, Shadertoy]
categories: Technical
---

## Random
In c++, An iterator is any object that, pointing to some element in a range of elements (such as an array or a container), has the ability to iterate through the elements of that range using a set of operators (with at least the increment (++) and dereference (*) operators).  

For example,
```cpp
float rand(float i){
	return fract(sin(i)*43758.5453123);
}
float rand2d(vec2 uv){
    return fract(sin(dot(uv.xy,vec2(12.123452,78.48254)))*43758.5453123);
}
void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    // Normalized pixel coordinates (from 0 to 1)
    vec2 uv = fragCoord/iResolution.xy;
    if (uv.y>0.5){
        uv*=50.0;
        uv.x+=5.0*iTime;
   
    }else{
        uv*=75.0;
        uv.x-=7.5*iTime;
    }
    vec2 ipos = floor(uv);  // get the integer coords
    float random=rand(ipos.x);
    if (random<0.4)
        random=0.0;
    else
        random=1.0;
    // Time varying pixel color
    vec3 col = vec3(random);
    // Output to screen
    fragColor = vec4(col,1.0);
}

```

## Demo

{% include shadertoy.html id="wdjBzd" %} 

### Notification

{: .box-note}
**Note:** This is a notification box.

ome inline Latex: $$a^2 + b^2 = c^2$$  
$$\int e^{-kx} \, dx = -\frac{1}{k} e^{-kx}$$
### Warning

{: .box-warning}
**Warning:** This is a warning box.

### Error

{: .box-error}
**Error:** This is an error box.
