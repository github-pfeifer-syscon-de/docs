# Jogl

Some time you like more control over the grphics hardware you come to Jogl.
Here a example of using the geomtry shader (Needs the support classes used in the Jogl tutorial):

```
package de.pfeifer_syscon.visual.animated;
import com.jogamp.nativewindow.util.Dimension;
import com.jogamp.newt.Display;
import com.jogamp.newt.NewtFactory;
import com.jogamp.newt.Screen;
import com.jogamp.opengl.GL;
import com.jogamp.opengl.GL2ES2;
import com.jogamp.opengl.GL3;
import com.jogamp.opengl.GL3ES3;
import com.jogamp.opengl.GLAutoDrawable;
import com.jogamp.opengl.GLCapabilities;
import com.jogamp.opengl.GLEventListener;
import com.jogamp.opengl.GLProfile;
import com.jogamp.opengl.awt.GLCanvas;
import com.jogamp.opengl.math.FloatUtil;
import com.jogamp.opengl.util.Animator;
import com.jogamp.opengl.util.GLBuffers;
import com.jogamp.opengl.util.glsl.ShaderCode;
import com.jogamp.opengl.util.glsl.ShaderProgram;
import de.pfeifer_syscon.visual.jogl.framework.BufferUtils;
import de.pfeifer_syscon.visual.jogl.framework.Semantic;
import de.pfeifer_syscon.visual.jogl.framework.ShaderUtil;
import java.awt.BorderLayout;
import java.awt.event.WindowAdapter;
import java.awt.event.WindowEvent;
import java.nio.FloatBuffer;
import java.util.Random;
import javax.swing.JFrame;
import javax.swing.SwingUtilities;
/**
* Base on example:
* https://open.gl/geometry
*
*/
public class HelloGeometryAnim extends JFrame implements GLEventListener {
   private static int screenIdx = 0;
   private static Dimension windowSize = new Dimension(1024, 768);
   private static String title = "Hello Animated";
   public GLCanvas glCanvas;
   public Animator animator;
   private int[] objects = new int[Semantic.Object.SIZE];
   // Position interleaved with color, sides, radius and speed
   private final int NUM = 1000;
   private final int NUM_COORD = 3;
   private final int NUM_COLOR = 3;
   private final int NUM_SIDES = 1;
   private final int NUM_RADIUS = 1;
   private final int NUM_SPEED = 1;
   private final int colorAttrib = 1;      // As query back indexes failes with speed declare them here
   private final int sidesAttrib = 2;
   private final int radiusAttrib = 3;
   private final int speedAttrib = 4;
   private final float[] vertexData = new float[NUM*getStrideCount()]; // {
   //  x       y     z     r     g     b     sides radius speed
   //-0.45f,  0.45f, 0.0f, 1.0f, 0.0f, 0.0f,    6,  0.1f, 0.1f,
   // 0.45f,  0.45f, 0.0f, 0.0f, 1.0f, 0.0f,    8,  0.2f, 0.2f,
   // 0.45f, -0.45f, 0.0f, 0.0f, 0.0f, 1.0f,   10,  0.3f, 0.3f,
   //-0.45f, -0.45f, 0.0f, 1.0f, 1.0f, 0.0f,   12,  0.4f, 0.4f };
   private int program, modelToClipMatrixUL, timeUL;
   /**
    * Use pools, you don't want to create and let them cleaned by the garbage
    * collector continuously in the display() method.
    */
   private float[] viewMatrix = new float[16];
   private long start, call;
   public HelloGeometryAnim() {
       super(title);
       Display display = NewtFactory.createDisplay(null);
       Screen screen = NewtFactory.createScreen(display, screenIdx);
       GLProfile glProfile = GLProfile.get(GLProfile.GL3);
       GLCapabilities glCapabilities = new GLCapabilities(glProfile);
       glCanvas = new GLCanvas(glCapabilities);
       glCanvas.setSize(windowSize.getWidth(), windowSize.getHeight());
       add(glCanvas, BorderLayout.CENTER);
       glCanvas.addGLEventListener(this);
       //glWindow.addKeyListener(this);
       addWindowListener(new WindowAdapter() {
           @Override
           public void windowClosing(WindowEvent e) {
               glCanvas.destroy();
           }
       });
       animator = new Animator(glCanvas);
       animator.start();
       pack();
   }
   @Override
   public void init(GLAutoDrawable drawable) {
       System.out.println("init");
       GL3 gl3 = drawable.getGL().getGL3();
       initVbo(gl3);   // Vertex & texture coord buffer
       initProgram(gl3);
       initVao(gl3);   // activate buffers
       gl3.glEnable(GL3.GL_DEPTH_TEST);
       start = System.currentTimeMillis();
   }
   private void createVertex() {
       Random rnd = new Random();
       for (int i = 0; i < vertexData.length; i += getStrideCount()) {
           vertexData[i+0] = 1.0f - rnd.nextFloat() * 2.0f; // x
           vertexData[i+1] = 1.0f - rnd.nextFloat() * 2.0f;  // y
           vertexData[i+2] = - 0.6f - rnd.nextFloat();  // z
           if (NUM_COLOR > 0) {
               vertexData[i+NUM_COORD+0] = 0.5f+rnd.nextFloat()/2.0f;  // r
               vertexData[i+NUM_COORD+1] = 0.5f+rnd.nextFloat()/2.0f;  // g
               vertexData[i+NUM_COORD+2] = 0.5f+rnd.nextFloat()/2.0f;  // b
           }
           if (NUM_SIDES > 0) {
               // sides
               vertexData[i+NUM_COORD+NUM_COLOR+0] = 6+rnd.nextInt(5)*2;
           }
           if (NUM_RADIUS > 0) {
               // radius
               vertexData[i+NUM_COORD+NUM_COLOR+NUM_SIDES] = (0.3f + rnd.nextFloat()) / 13.0f;
           }
           if (NUM_SPEED > 0) {
               // as we dont want to start all stars starting again at the same time
               vertexData[i+NUM_COORD+NUM_COLOR+NUM_SIDES+NUM_RADIUS] = (0.2f + rnd.nextFloat()) / 20.0f;
           }
       }
   }
   private void initVbo(GL3 gl3) {
       createVertex();
       // Vertex
       gl3.glGenBuffers(1, objects, Semantic.Object.VBO);
       gl3.glBindBuffer(GL3.GL_ARRAY_BUFFER, objects[Semantic.Object.VBO]);
       {
           FloatBuffer vertexBuffer = GLBuffers.newDirectFloatBuffer(vertexData);
           int size = vertexData.length * Float.BYTES;
           gl3.glBufferData(GL3.GL_ARRAY_BUFFER, size, vertexBuffer, GL3.GL_STATIC_DRAW);
           /**
            * Since vertexBuffer is a direct buffer, this means it is outside
            * the Garbage Collector job and it is up to us to remove it.
            */
           BufferUtils.destroyDirectBuffer(vertexBuffer);
       }
       gl3.glBindBuffer(GL3.GL_ARRAY_BUFFER, 0);
       checkError(gl3, "initVbo");
   }
   private int getStrideCount() {
       return (NUM_COORD + NUM_COLOR + NUM_SIDES + NUM_RADIUS + NUM_SPEED);
   }
   private void initVao(GL3 gl3) {
       /**
        * Let's create the VAO and save in it all the attributes properties.
        */
       gl3.glGenVertexArrays(1, objects, Semantic.Object.VAO);
       gl3.glBindVertexArray(objects[Semantic.Object.VAO]);
       {
           /**
            * Ibo is part of the VAO, so we need to bind it and leave it bound.
            */
           gl3.glBindBuffer(GL3.GL_ELEMENT_ARRAY_BUFFER, objects[Semantic.Object.IBO]);
           {
               /**
                * VBO is not part of VAO, we need it to bind it only when we
                * call glEnableVertexAttribArray and glVertexAttribPointer, so
                * that VAO knows which VBO the attributes refer to, then we can
                * unbind it.
                */
               gl3.glBindBuffer(GL3.GL_ARRAY_BUFFER, objects[Semantic.Object.VBO]);
               {
                   int stride = getStrideCount() * Float.BYTES;
                   gl3.glEnableVertexAttribArray(Semantic.Attr.POSITION);
                   gl3.glVertexAttribPointer(Semantic.Attr.POSITION, NUM_COORD, GL3.GL_FLOAT,
                           false, stride, 0 * Float.BYTES);
                   if (NUM_COLOR > 0) {
                       //int colorAttrib = gl3.glGetAttribLocation(program, "color"); // sides
                       gl3.glEnableVertexAttribArray(colorAttrib);
                       gl3.glVertexAttribPointer(colorAttrib, NUM_COLOR, GL3.GL_FLOAT,
                               false, stride, (NUM_COORD) * Float.BYTES);
                   }
                   if (NUM_SIDES > 0) {
                       //int sidesAttrib = gl3.glGetAttribLocation(program, "sides");
                       gl3.glEnableVertexAttribArray(sidesAttrib);
                       gl3.glVertexAttribPointer(sidesAttrib, NUM_SIDES, GL3.GL_FLOAT,
                               false, stride, (NUM_COORD + NUM_COLOR) * Float.BYTES);
                   }
                   if (NUM_RADIUS > 0) {
                       //int radiusAttrib = gl3.glGetAttribLocation(program, "radius");
                       gl3.glEnableVertexAttribArray(radiusAttrib);
                       gl3.glVertexAttribPointer(radiusAttrib, NUM_RADIUS, GL3.GL_FLOAT,
                               false, stride, (NUM_COORD + NUM_COLOR + NUM_SIDES) * Float.BYTES);
                   }
                   if (NUM_SPEED > 0) {
                       //int speedAttrib = gl3.glGetAttribLocation(program, "speed");
                       System.out.format("Speed Attrib %d\n", speedAttrib);
                       if (speedAttrib >= 0) { // Here we get -1 unsure why ???
                           gl3.glEnableVertexAttribArray(speedAttrib);
                           gl3.glVertexAttribPointer(speedAttrib, NUM_SPEED, GL3.GL_FLOAT,
                               false, stride, (NUM_COORD + NUM_COLOR + NUM_SIDES + NUM_RADIUS) * Float.BYTES);
                       }
                   }
               }
               gl3.glBindBuffer(GL3.GL_ARRAY_BUFFER, 0);
           }
       }
       gl3.glBindVertexArray(0);
       checkError(gl3, "initVao");
   }
   private void initProgram(GL3 gl3) {
       String[][] vs = ShaderUtil.loadShaderCode(getClass(), "shaders/vs.glsl");
       String[][] gs = ShaderUtil.loadShaderCode(getClass(), "shaders/gs.glsl");
       String[][] fs = ShaderUtil.loadShaderCode(getClass(), "shaders/fs.glsl");
       ShaderCode vertShader = new ShaderCode(GL2ES2.GL_VERTEX_SHADER, 1, vs);
       ShaderCode geomShader = new ShaderCode(GL3ES3.GL_GEOMETRY_SHADER, 1, gs);
       ShaderCode fragShader = new ShaderCode(GL2ES2.GL_FRAGMENT_SHADER, 1, fs);
       ShaderProgram shaderProgram = new ShaderProgram();
       shaderProgram.add(vertShader);
       shaderProgram.add(geomShader);
       shaderProgram.add(fragShader);
       shaderProgram.init(gl3);
       program = shaderProgram.program();
       /**
        * These links don't go into effect until you link the program. If you
        * want to change index, you need to link the program again.
        */
       gl3.glBindAttribLocation(program, Semantic.Attr.POSITION, "pos");
       gl3.glBindAttribLocation(program, colorAttrib, "color");
       gl3.glBindAttribLocation(program, sidesAttrib, "sides");
       gl3.glBindAttribLocation(program, radiusAttrib, "radius");
       gl3.glBindAttribLocation(program, speedAttrib, "speed");
       shaderProgram.link(gl3, System.out);
       /**
        * Take in account that JOGL offers a GLUniformData class, here we don't
        * use it, but take a look to it since it may be interesting for you.
        */
       modelToClipMatrixUL = gl3.glGetUniformLocation(program, "modelToClipMatrix");
       timeUL = gl3.glGetUniformLocation(program, "uTime");
       vertShader.destroy(gl3);
       fragShader.destroy(gl3);
       geomShader.destroy(gl3);
       checkError(gl3, "initProgram");
   }
   @Override
   public void dispose(GLAutoDrawable drawable) {
       System.out.println("dispose");
       long end = System.currentTimeMillis();
       float t = (float)(end-start)/1000.0f;
       System.out.format("calls %d %.1fs %.2fCalls/s\n", call, t, (float)call/t);
       GL3 gl3 = drawable.getGL().getGL3();
       gl3.glDeleteProgram(program);
       /**
        * Clean VAO first in order to minimize problems. If you delete IBO
        * first, VAO will still have the IBO id, this may lead to crashes.
        */
       gl3.glDeleteVertexArrays(1, objects, objects[Semantic.Object.VAO]);
       gl3.glDeleteBuffers(1, objects, Semantic.Object.VBO);
       System.exit(0);
   }
   @Override
   public void display(GLAutoDrawable drawable) {
       GL3 gl3 = drawable.getGL().getGL3();
       ++call;
       long time = System.currentTimeMillis() - start;
       float vTime = time / 1000.0f;
       /**
        * We set the clear color and depth (althought depth is not necessary
        * since it is 1 by default).
        */
       gl3.glClearColor(0.05f, 0.05f, 0.2f, 1f);
       gl3.glClearDepthf(1.0f);
       gl3.glClear(GL3.GL_COLOR_BUFFER_BIT | GL3.GL_DEPTH_BUFFER_BIT);
       gl3.glUseProgram(program);
       {
           gl3.glBindVertexArray(objects[Semantic.Object.VAO]);
           {
               gl3.glUniformMatrix4fv(modelToClipMatrixUL, 1, false, viewMatrix, 0);
               gl3.glUniform1f(timeUL, vTime);
               gl3.glDrawArrays(GL3.GL_POINTS, 0, vertexData.length/getStrideCount());
           }
           /**
            * In this sample we bind VAO to the default values, this is not a
            * cheapier binding, it costs always as a binding, so here we have
            * for example 2 vao bindings. Every binding means additional
            * validation and overhead, this may affect your performances. So if
            * you are looking for high performances skip these calls, but
            * remember that OpenGL is a state machine, so what you left bound
            * remains bound!
            */
           gl3.glBindVertexArray(0);
       }
       gl3.glUseProgram(0);
       /**
        * Check always any GL error, but keep in mind this is an implicit
        * synchronization between CPU and GPU, so you should use it only for
        * debug purposes.
        */
       checkError(gl3, "display");
   }
   protected boolean checkError(GL gl, String title) {
       int error = gl.glGetError();
       if (error != GL.GL_NO_ERROR) {
           String errorString;
           switch (error) {
               case GL.GL_INVALID_ENUM:
                   errorString = "GL_INVALID_ENUM";
                   break;
               case GL.GL_INVALID_VALUE:
                   errorString = "GL_INVALID_VALUE";
                   break;
               case GL.GL_INVALID_OPERATION:
                   errorString = "GL_INVALID_OPERATION";
                   break;
               case GL.GL_INVALID_FRAMEBUFFER_OPERATION:
                   errorString = "GL_INVALID_FRAMEBUFFER_OPERATION";
                   break;
               case GL.GL_OUT_OF_MEMORY:
                   errorString = "GL_OUT_OF_MEMORY";
                   break;
               default:
                   errorString = "UNKNOWN";
                   break;
           }
           System.out.println("OpenGL Error(" + errorString + "): " + title);
           throw new Error();
       }
       return error == GL.GL_NO_ERROR;
   }
   @Override
   public void reshape(GLAutoDrawable drawable, int x, int y, int width, int height) {
       System.out.println("reshape");
       GL3 gl3 = drawable.getGL().getGL3();
       gl3.glViewport(x, y, width, height);
       float aspectX = ((float)width / (float)height);
       System.out.format("Aspect: %.3f\n", aspectX);
       viewMatrix = FloatUtil.makePerspective(viewMatrix, 0, true,
               (float)Math.toRadians(45.0), aspectX, 0.1f, 5.0f);
       debugMat(viewMatrix);
   }
   private void debugMat(float[] viewMatrix) {
       for (int i = 0; i < viewMatrix.length; i += 4) {
           System.out.format("mat[%d] %.3f %.3f %.3f %.3f\n",
                   i, viewMatrix[i+0], viewMatrix[i+1], viewMatrix[i+2], viewMatrix[i+3]);
       }
   }
   public static void main(String[] args) {
       SwingUtilities.invokeLater(new Runnable() {
           @Override
           public void run() {
               HelloGeometryAnim helloGeometry = new HelloGeometryAnim();
               helloGeometry.setVisible(true);
           }
       });
   }
}
```

Unter shaders the following definition are required.
vs.glsl:

```
#version 330
in vec3 pos;
in vec3 color;
in float sides;
in float radius;
// Uniform matrix from Model Space to Clip Space.
uniform mat4 modelToClipMatrix;
uniform vec3 offset;
out vec3 vColor;
out float vSides;
out float vRadius;
void main()
{
   gl_Position = (modelToClipMatrix * vec4(pos, 1.0)) + vec4(offset, 1.0);
   vColor = color;
   vSides = sides;
   vRadius = radius;
}
```

gs.glsl:

```
#version 330
layout(points) in;
layout(line_strip, max_vertices = 64) out;
in vec3 vColor[];
in float vSides[];
in float vRadius[];
out vec3 fColor;
const float PI = 3.1415926;
void render(int i) {
   float ang = PI * 2.0 / vSides[0] * i;
   vec4 offset = vec4(cos(ang) * vRadius[0], -sin(ang) * vRadius[0], 0.0, 0.0);
   gl_Position = gl_in[0].gl_Position + offset;
   EmitVertex();
}
void main()
{
   fColor = vColor[0];
   for (int i = 0; i <= vSides[0]; i += 2) {
       render(i);
   }
   EndPrimitive();
   for (int i = 1; i <= vSides[0]+1; i += 2) {
       render(i);
   }
   EndPrimitive();
}
```

fs.glsl:

```
#version 330
in vec3 fColor;
out vec4 outColor;
void main()
{
   outColor = vec4(fColor, 1.0);
}
