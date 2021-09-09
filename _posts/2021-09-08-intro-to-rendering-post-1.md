---
layout: post
title:  "Rendering with Metal."
author: kemal
categories: [ rendering, tutorial, metal, rendering_series ]
tags: [red, yellow]
image: assets/images/rendering-metal-post-1/rendering-1.png
description: "An intro to the modern 3d rendering series."
featured: false
hidden: false
rating: 4.8
---

# Rendering with Metal.
Many things have changed in the graphics world over the past thirty years.  Hardware is more powerful, does more, and developer tools have advanced.  These changes have led to fundamental changes in how APIs work and it can be incredibly daunting to know where to start.  Although APIs have become more complex there are some core mathematical concepts that will always remain valid, no matter what APIs are available.

This series of blogs will give a brief overview of where we have come from, where we are now and why, and how to get set up and going in a pragmatic way so that you’re ready for learning more advanced concepts and techniques when you need to. 

## Part 1 - A fleeting overview of how rendering used to work.
My first experience with OpenGL was around 2002 while I was studying at university. I had further exposure to this technology while working for a games developer called Zoo Interactive, and Autodesk in Sheffield, UK.

OpenGL was one of the first multi-platform, low-level graphics APIs, and was released in 1992.  There were many APIs around this time, and OpenGL was the product of an evolution from SGI‘s (*Sillcon Graphics Incorporated.*) proprietary technology.  DirectX is another example of a graphics API for Microsoft’s Windows system that was being developed around the same time.  For this overview we only need to focus on some of the general capabilities and methods that were available to developers in those early days, so we’ll just think about OpenGL.

During the early days of OpenGL the API was built around a state machine model.  As a developer you would write code that set states, drew things to the screen, and then reset states.  This style of developing was called *OpenGL Immediate Mode* and was conceptually fairly straight forward to understand.

```c++
glBegin(GL_TRIANGLES); // Start rendering triangles 
	glColor3f(1.0f, 0.0f, 0.0f);   // Set the current color to red
	glVertex2f(0.0f,v1.0f); // Add the first point of the triangle 
	glColor3f(0.0f, 1.0f, 0.0f);  // Set the color to green   
	glVertex2f(0.87f, -0.5f); // Add the second point of the triangle
	glColor3f(0.0f, 0.0f, 1.0f); // Set the current color to blue
	glVertex2f(-0.87f, -0.5f); // Add the third point of the triangle
glEnd(); // At this point the state is sent to the GPU, and a colour interpolated triangle is rendered to the screen.
```

*Fig1. An example of drawing a triangle using OpenGL immediate mode.*

A limitation of this approach was that sending commands to the graphics card was synchronous, which caused the hardware to wait around for a developer to set all the states required, and send data.  There was no stored representation of the models so data had to be sent every time, and all of the data was lost once drawing took place.  This meant that new frames required all of the shapes to be regenerated all of the time.  It also meant that the graphics hardware was often waiting for commands to be sent to it, resulting in a loss of performance.

To get around these limitations, another way of using the API was required, and this was dubbed *Retained mode*. It’s worth noting that retained mode is a collection of ideas and APIs rather than an official term.  It’s better to think of it as ‘Not Immediate Mode’.  

The general idea was to store data on the graphics card so that it wouldn’t need to be sent every time something needed to be drawn.  This was achieved by allocating VBOs (Vertex Display Objects).  The developer would create a data structure called a display list, and then send that to the graphics card which would ‘retain’ the data so that the developer could use it again at a later point.  The data in this structure could not be changed.  

Although display lists had some limitations, they allowed for the shapes to be redisplayed with different states, and to be shared across multiple contexts.  A further enhancement to this was made later on with the inclusion of *Vertex Arrays*.  These added extra capabilities by allowing data to be dynamically changed, and by providing functions to operate on the vertex arrays, which resulted in very large performance gains.

I’m not going to talk any more about how Immediate Mode works in OpenGL as it has been an obsolete technology for some time, but it’s worth noting that OpenGL no longer supports immediate mode, and every enhancement since then has been with the aim of reducing hardware idle time and inefficiency by putting more control in the hands of developers.  This is why modern rendering APIs are much more complex than OpenGL immediate mode.

## Terms to be familiar with.
|                                   |                                                              |
|-----------------------------------|--------------------------------------------------------------|
| **Vertex**, *plural **vertices*** | A point in space. i.e. *(x,y)* or *(x,y,z)*                  |
| **Edge**                          | The line between two vertices, or the line where two faces meet on a polygon. |
| **Face**                          | A flat side of a polygon.                                    |
| **Context**                       | A concept that represents the state of everything associated with an instance of OpenGL.  In an application one might have multiple contexts for each window. |
| **VBO**                           | Vertex Display Object.  A data structure to represent and persist vertex data on graphics hardware. |

# References


### Tags
#blog/metal/1 #apple #3d #graphics #metal #vulkan #opengl

