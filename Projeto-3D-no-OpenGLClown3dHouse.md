
/* Arthur Menezes, Katherine Lacerda, Thais Mesquita, Rafael Jatobá, Thiago Fonseca, Gabriel Ferraz Duque */

#include <windows.h> 
#include <GL/glut.h>  

/* Global variables */
char title[] = "CLOWN HOUSE 3D";
GLfloat anglePyramid = 0.0f;  
GLfloat angleCube = 0.0f;     
int refreshMills = 15;
static int slices = 32;
static int stacks = 32;
float size=1.0;
GLfloat pyramidX=-1.0;
GLfloat pyramidY=-1.0;
GLfloat pyramidZ= 0.0;
GLfloat pyramidXSpeed = 0.0f;      
GLfloat pyramidYSpeed = 0.0f;
GLfloat pyramidZSpeed = 0.0f;      

GLfloat cubeX=1.0;
GLfloat cubeY=1.0;
GLfloat cubeZ=1.0;
GLfloat cubeXSpeed = 0.0f;      
GLfloat cubeYSpeed = 0.0f;
GLfloat cubeZSpeed = 0.0f;    
float Ty = 0.0;
float Tx = 0.0;
float Tr = 0.0;
float J = 0.0;
float Cr = 0.4f; 
float Cg = 0.4f;
float Cb = 0.4f;
float Px = 0.0;

/* Initialize OpenGL Graphics */
void initGL() {
   glClearColor(0.2f, 0.5f, 0.0f, 1.0f); // cor d fundo
   glClearDepth(1.0f);                   // Set background depth to farthest
   glEnable(GL_DEPTH_TEST);   // Enable depth testing for z-culling
   glDepthFunc(GL_LEQUAL);    // Set the type of depth-test
   glShadeModel(GL_SMOOTH);   // Enable smooth shading
   glHint(GL_PERSPECTIVE_CORRECTION_HINT, GL_NICEST);  // Nice perspective corrections
}

/* Handler for window-repaint event. Called back when the window first appears and
   whenever the window needs to be re-painted. */
   
void cubo(){
	// Render a color-cube consisting of 6 quads with different colors
  	glLoadIdentity();                 // Reset the model-view matrix
   	glTranslatef(-4.5f, 3.0f, -6.0f);  // Move right and into the screen
   	glRotatef(angleCube, cubeX, cubeY, cubeZ);  // Rotate about (1,1,1)-axis [NEW]
	glBegin(GL_QUADS);                // Begin drawing the color cube with 6 quads
      // Top face (y = 1.0f)
      // Define vertices in counter-clockwise (CCW) order with normal pointing out
      glColor3f(0.6f, 0.1f, 0.3f); //vinho   
      glVertex3f( 1.0f, 1.0f, -1.0f);
      glVertex3f(-1.0f, 1.0f, -1.0f);
      glVertex3f(-1.0f, 1.0f,  1.0f);
      glVertex3f( 1.0f, 1.0f,  1.0f);

      // Bottom face (y = -1.0f)
      glColor3f(1.0f, 0.2f, 0.4f);     
      glVertex3f( 1.0f, -1.0f,  1.0f);
      glVertex3f(-1.0f, -1.0f,  1.0f);
      glVertex3f(-1.0f, -1.0f, -1.0f);
      glVertex3f( 1.0f, -1.0f, -1.0f);

      // Front face  (z = 1.0f)
      glColor3f(1.0f, 1.0f, 0.0f);
      glVertex3f( 1.0f,  1.0f, 1.0f);
      glVertex3f(-1.0f,  1.0f, 1.0f);
      glVertex3f(-1.0f, -1.0f, 1.0f);
      glVertex3f( 1.0f, -1.0f, 1.0f);

      // Back face (z = -1.0f)
      glColor3f(0.3f, 0.0f, 0.1f);   
      glVertex3f( 1.0f, -1.0f, -1.0f);
      glVertex3f(-1.0f, -1.0f, -1.0f);
      glVertex3f(-1.0f,  1.0f, -1.0f);
      glVertex3f( 1.0f,  1.0f, -1.0f);

      // Left face (x = -1.0f)
      glColor3f(0.4f, 0.4f, 1.0f);     // Blue
      glVertex3f(-1.0f,  1.0f,  1.0f);
      glVertex3f(-1.0f,  1.0f, -1.0f);
      glVertex3f(-1.0f, -1.0f, -1.0f);
      glVertex3f(-1.0f, -1.0f,  1.0f);

      // Right face (x = 1.0f)
      glColor3f(1.0f, 0.4f, 0.4f);     // Magenta
      glVertex3f(1.0f,  1.0f, -1.0f);
      glVertex3f(1.0f,  1.0f,  1.0f);
      glVertex3f(1.0f, -1.0f,  1.0f);
      glVertex3f(1.0f, -1.0f, -1.0f);
   glEnd();  // End of drawing color-cube
   
   angleCube -= 0.55f;
}
void esfera(){
glLoadIdentity();                  // Reset the model-view matrix
   glTranslatef(2.5f, -2.0f, -7.0f);   // Move left and into the screen
   glRotatef(anglePyramid, 1.0f, 1.0f, 0.0f);  // Rotate about the (1,1,0)-axis [NEW]

   glPushMatrix();
        glTranslated(-0.4,4.2,-1);
        glRotated(60,1,0,0);
        glRotated(0.9,0,0,1);
        glColor3f(0.9f, 0.4f, 0.9f);
        glutWireSphere(size,slices,stacks);
    glPopMatrix();	
}

void piramide(){
//glTranslatef(0.5f, 0.0f, 0.0f);
   glTranslatef(pyramidX, pyramidY, pyramidZ);
   
   glBegin(GL_TRIANGLES);           // Begin drawing the pyramid with 4 triangles
      // Front
    glColor3f(0.4f, 0.0f, 0.1f);     // Red
    glVertex3f( 0.0f, 1.0f, 0.0f);
	glColor3f(1.0f, 0.0f, 0.0f); 
      glVertex3f(-1.0f, -1.0f, 1.0f);
      glColor3f(0.0f, 0.0f, 1.0f);     // Blue
      glVertex3f(1.0f, -1.0f, 1.0f);

      // Right
      glColor3f(0.3f, 0.6f, 0.8f);     // Red
      glVertex3f(0.0f, 1.0f, 0.0f);
      glColor3f(0.0f, 0.0f, 0.0f);     // Blue
      glVertex3f(1.0f, -1.0f, 1.0f);
      glColor3f(0.0f, 0.0f, 1.0f);     // Green
      glVertex3f(1.0f, -1.0f, -1.0f);

      // Back
      glColor3f(1.0f, 0.0f, 0.0f);     
      glVertex3f(0.0f, 1.0f, 0.0f);
      glColor3f(1.0f, 0.0f, 1.0f);     
      glVertex3f(1.0f, -1.0f, -1.0f);
      glColor3f(0.0f, 0.0f, 1.0f);     
      glVertex3f(-1.0f, -1.0f, -1.0f);

      // Left
      glColor3f(1.0f,0.0f,0.0f);       // Red
      glVertex3f( 0.0f, 1.0f, 0.0f);
      glColor3f(0.0f,0.0f,1.0f);       // Blue
      glVertex3f(-1.0f,-1.0f,-1.0f);
      glColor3f(0.0f,0.0f,0.0f);       // Green
      glVertex3f(-1.0f,-1.0f, 1.0f);
        
   glEnd();   // Done drawing the pyramid
   
	glBegin(GL_QUADS);
	  // Bottom face (y = -1.0f)
      glColor3f(1.0f,0.0f,0.0f); // Red
      glVertex3f( 1.0f, -1.0f,  1.0f);
      glColor3f(0.0f,0.0f,1.0f);       // Blue
      glVertex3f(-1.0f, -1.0f,  1.0f);
      glColor3f(0.0f,1.0f,0.0f);       // Green
      glVertex3f(-1.0f, -1.0f, -1.0f);
      glVertex3f( 1.0f, -1.0f, -1.0f);
    glEnd();
    
    //anglePyramid += 0.5f;
    
   
}

//ponte
void Ponte() {
	glLoadIdentity();
    glBegin(GL_LINE_STRIP);
    glColor3f(0.17f,0.09f,0.06f);

    glVertex2f(0.89, 1.5);
    glVertex2f(0.89, -1.5);
    glVertex2f(0.87, 1.5);
    glVertex2f(0.87, -1.5);

    glEnd();

    glBegin(GL_LINE_STRIP);
    glColor3f(0.17f,0.09f,0.06f);

    glVertex2f(0.88, 1.5);
    glVertex2f(0.88, -1.5);
    glEnd();

    glBegin(GL_LINE_STRIP);
    glColor3f(0.17f,0.09f,0.06f);

    glVertex2f(0.86, 1.5);
    glVertex2f(0.86, -1.5);
    glEnd();
}


void desenha() {
   	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT); // Clear color and depth buffers
   	
	glMatrixMode(GL_MODELVIEW);     // To operate on model-view matrix
	cubo();
   	esfera();
   	piramide();
   	
   	glutSwapBuffers(); 
   	
	pyramidX += pyramidXSpeed;
	pyramidY += pyramidYSpeed;
	pyramidZ += pyramidZSpeed;
	
	cubeX += cubeXSpeed;
	cubeY += cubeYSpeed;
	cubeZ += cubeZSpeed;
	
	
   
}


void timer(int value) {
   glutPostRedisplay();      // Post re-paint request to activate display()
   glutTimerFunc(refreshMills, timer, 0); // next timer call milliseconds later
   pyramidX -= pyramidXSpeed;
   pyramidY -= pyramidYSpeed;
   pyramidZ -= pyramidZSpeed;
   
   	cubeX -= cubeXSpeed;
	cubeY -= cubeYSpeed;
	cubeZ -= cubeZSpeed;
}

/* Handler for window re-size event. Called back when the window first appears and
   whenever the window is re-sized with its new width and height */
void reshape(GLsizei width, GLsizei height) {  // GLsizei for non-negative integer
   // Compute aspect ratio of the new window
   if (height == 0) height = 1;                // To prevent divide by 0
   GLfloat aspect = (GLfloat)width / (GLfloat)height;

   // Set the viewport to cover the new window
   glViewport(0, 0, width, height);

   // Set the aspect ratio of the clipping volume to match the viewport
   glMatrixMode(GL_PROJECTION);  // To operate on the Projection matrix
   glLoadIdentity();             // Reset
   // Enable perspective projection with fovy, aspect, zNear and zFar
   gluPerspective(90.0f, aspect, 0.1f, 100.0f);
}

void specialKeys(int key, int x, int y) {
   switch (key) {
    	case GLUT_KEY_RIGHT:    
         	pyramidXSpeed += -0.1f;

	  		break;
         
      	case GLUT_KEY_LEFT:    
         	pyramidXSpeed += 0.1f; 

	 	 	break;
		 
      	case GLUT_KEY_UP:      
         	pyramidYSpeed += -0.1f;

	  		break;
		 
      	case GLUT_KEY_DOWN:     
         	pyramidYSpeed += 0.1f;

	  	 	break;
	  	 	
		case GLUT_KEY_PAGE_DOWN: 
    		pyramidZSpeed += -0.1f;
         	break;
         	
	  	case GLUT_KEY_PAGE_UP: 
    		pyramidZSpeed += 0.1f;
         	break;  
      
   }
   
}

static void key(unsigned char key, int x, int y)
{
    switch (key)
    {
        case 'q':
            exit(0);
            break;
        case 'w': //esfera cresce
            size= size + 0.2;
            break;
        case 's': //esfera diminui
            size= size - 0.2;
            break;
        case 'x':
        	
            cubeXSpeed = cubeXSpeed + 1.0;
            angleCube -= 5.0f;
            
			cubeY=1.0;
			cubeZ=1.0;    
			cubeYSpeed = 0.0f;
			cubeZSpeed = 0.0f;
            break;
        case 'y':
        	
            cubeYSpeed = cubeYSpeed - 1.0;
            angleCube -= 5.0f;
            
            cubeX=1.0;
			cubeZ=1.0;
			cubeXSpeed = 0.0f;      
			cubeZSpeed = 0.0f;
            break;
        case 'z':
        	
            cubeZSpeed = cubeZSpeed - 1.0;
            angleCube -= 5.0f;
            
            cubeX=1.0;
			cubeY=1.0;
			cubeXSpeed = 0.0f;      
			cubeYSpeed = 0.0f;
            break;
        
    }
}

/* Main function: GLUT runs as a console application starting at main() */
int main(int argc, char** argv) {
   glutInit(&argc, argv);            // Initialize GLUT
   glutInitDisplayMode(GLUT_DOUBLE); // Enable double buffered mode
   glutInitWindowSize(720, 600);   // Set the window's initial width & height
   glutInitWindowPosition(50, 50); // Position the window's initial top-left corner
   glutCreateWindow(title);          // Create window with the given title
   glutDisplayFunc(desenha);       // Register callback handler for window re-paint event
   glutReshapeFunc(reshape);       // Register callback handler for window re-size event
   initGL();                       // Our own OpenGL initialization
   glutTimerFunc(0, timer, 0);     // First timer call immediately [NEW]
   glutSpecialFunc(specialKeys); // Register callback handler for special-key event
   glutKeyboardFunc(key);   		// Register callback handler for special-key event
   glutMainLoop();                 // Enter the infinite event-processing loop
   return 0;
}

