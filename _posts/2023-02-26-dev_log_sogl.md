---
layout: post
title: "Dev Log: Extremely Basic OpenGL Renderer"
---

Original post found [here](https://dev.to/yekyam/dev-log-extremely-basic-opengl-renderer-i40) 

## Some Background

Since the beginning of my programming journey, I’ve always been drawn to the idea of creating projects from the ground up with minimal third party code.

One of those projects was to play around with graphics, and this time I actually stuck with it. 


## Starting the Project

I decided to follow along the [Learn OpenGL](https://learnopengl.com/) tutorial. Although I found it useful for code, some of its explanations weren't clicking with me.

I was confused on how vertex array objects (VAOs) and vertex buffer objects (VBOs) fit together, and given that they're the fundamental building blocks of OpenGL, I was in a bad place. Eventually, afters tons of googling and youtube, it finally clicked.

I think the reason I was struggling so much was because I didn't understand that VAOs and VBOs are fundamentally linked. See, VAOs not only hold the specified layout of VBOs, they also hold the VBO data itself. It's kind of weird (thanks state machine!), and newer versions of OpenGL separate out the formatting of the data and the actual binding of the data (see [direct state access (DSA)](https://github.com/fendevel/Guide-to-Modern-OpenGL-Functions#glvertexattribformat--glbindvertexbuffer)). 

## Building out abstractions

Once I understood that VAOs and VBOs can't be separated out (in OpenGL versions below 4.5), this made programming an abstraction around them fairly trivial. 

I started out with the most basic data, the vertex. I don't care about textures or even lighting for this first run at OpenGL, so for now my vertices contain only position information and color information.

```
struct Vertex
{
    std::array<GLfloat, 3> position;
    std::array<GLfloat, 3> color;

    // constructor and std::ostream& operator<< defined here
};
```

Great! I had a way to represent a single vertex, but you can't do much with that. 

So, I created a class to represent a group of vertices.

```
struct Mesh
{
    std::vector<Vertex> vertices;

    // constructors
};
```

There's something missing. See, in OpenGL, because everything is defined as a set of triangles, sometimes you'll get some overlap. Imagine the face of a cube:

```
1-----2
|     |
|     |
3-----4
```
The two triangles that make up the face are: 123, 324.

That's kind of a waste, right? We shouldn't have to define duplicate vertices. 

Luckily, OpenGL solves that by introducing element buffer objects (EBOs). EBOs hold indices, meaning that we don't need to duplicate our vertices in memory.

So, the new `Mesh` struct looks like this:

```
struct Mesh
{
    std::vector<Vertex> vertices;
    std::vector<GLushort> indices;

    // constructors
};
```

Great! I can now represent the vertices and indices of a mesh. 

But now I need a way to actually send these vertices and indices to OpenGL.

So, I created a `Model` class. This class represents a `Mesh` and its OpenGL data too, including VBOs, VAO, and EBO.

```
class Model
{
    Mesh m_mesh;
    GLuint m_vao;
    GLuint m_vbos[2];
    GLuint m_ebo;

    // constructors, convenience functions, destructor
};
```

Lastly, I needed a way to actually draw the `Model` to the screen and keep track of the models.

I decided to create a `Renderer` class to keep track of camera matrices and handle all of the drawing. That way, I could keep my `Model` separate from the drawing code. 

```
class Renderer
{
public:
    std::list<std::unique_ptr<Model>> models; 
    int mode; // GL_POINTS, GL_TRIANGLES, or GL_LINES
    // other members excluded for brevity, but shader is included
    
    // constructor

    // function to add model to models

    void draw_models()
    {
        // clear the screen, color buffer, and depth buffer
        // use the shader
        // setup view matrices
        // setup shader uniforms
        for (const auto& model : models)
        {
            glBindVertexArray(model->m_vao);
            glDrawElements(mode, model-m_mesh.indices.size(), GL_UNSIGNED_SHORT, 0);
            glBindVertexArray(0);
        }
    }
    
};
```

## Loading Models from Files

I had the foundation of a renderer, but it's quite boring if I was staring at an empty screen the entire time.

So, I decided to create a function to load a `.obj` file into a `Model`. 

Luckily, the file format is fairly simple, it looks something like this:

```
v 0.5. 0.5 0.5
...
vt 1.0 1.0
...
vn 1.0 1.0 1.0
...
f 1 2 3
```

`v` denotes the vertex positions (x, y, z).
`vt` denotes texture coordinates, which I ignored to keep things simple.
`vn` denotes the surface normals, which I also ignored.
`f` denotes the indices of the triangles.

So, I created a function to loop through all of the lines and either add the vertices and indices to vectors. 

However, I did run into two subtle bugs that were easy to fix:

1. The indices are 0 indexed, not 1 indexed, so when adding the indices to the the vector I had to subtract 1.
2. Some lines of the face actually define quads instead of triangles, so the line looks like:

```
f 1 2 3 4
```
After fixing the two bugs, I had a passable `.obj` loader I could use.

## Finishing Touches

Once that was done, I decided to add some command line arguments, mainly to specify the `.obj` file to load, specify the mode to render with (GL_POINTS, GL_LINES, or GL_TRIANGLES), and the ability to change the camera's distance from the model.

Here's the classic Utah teapot rendered as a wireframe:


![A wireframe render of the Utah teapot](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/9mhkunj9qxua3etdjts8.png)

[Here’s the link to the GitHub repo.](https://github.com/yekyam/SOGL-Renderer)

## Final Thoughts and Next Steps

Overall, this was a fun project. It was nice to get something visual done, and I'm glad I finally understand how OpenGL work. 

I'm not particularly happy with the code, mainly the argument parsing and the raw `glfw` calls I make in `main()`, but I'm glad it works. 

Outside of `main()`, I do think the abstractions I made over the vertices and OpenGL itself isn't too bad, and in a future renderer I'll probably work off of this foundation. 

There are two features I'd like to add to this renderer:
* The ability to specify model color in the command line args
* Basic lighting

Once (or if) I get those done, I think I'll leave the project as-is. It was a great learning experience, and it'll be interesting to see how any future renderers I make compare to this one.

In the future, I'll probably try to look into Vulkan, I'm interested in the fact that the API --although much more verbose-- looks a whole lot cleaner than OpenGL.

#### Tips for other OpenGL beginners

1. Don't attempt to learn OpenGL if you don't know memory management and how pointers work, you will have a bad time.
 
2. OpenGL 3.3 is a bad API due to its state machine. If possible, try to learn OpenGL 4.5+ (yes, I know tutorials are sparse, but it seems easier to learn)

3. VAOs and VBOs are linked due to the state machine architecture, there's no real way to separate them out.