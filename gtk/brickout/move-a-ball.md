---
description: >-
  https://github.com/kion-dgl/DashGL-GTK-Brickout-Tutorial/tree/master/08_Move_a_Ball
---

# Move a Ball

<figure><img src="../../.gitbook/assets/bricks_008.png" alt=""><figcaption><p>Ball moved across the screenc</p></figcaption></figure>

Now that we have a ball and we can set its position inside the window, the next step is to move the ball. After initializing the GLGTKArea, we trigger the _on\_idle_ function every twenty milliseconds to redraw the OpenGL context.

In order to call the function _gtk\_widget\_queue\_draw_, we need to pass the GTKGLArea context into the function. The on\_idle function takes a null pointer as a parameter, so we can pass in what ever we want as long as we typecast it.

```c
#include <epoxy/gl.h>
#include <epoxy/glx.h>
#include <gtk/gtk.h>
#include <math.h>
#include "DashGL/dashgl.h"

#define WIDTH 640.0f
#define HEIGHT 480.0f

static void on_realize(GtkGLArea *area);
static void on_render(GtkGLArea *area, GdkGLContext *context);
static gboolean on_idle(gpointer data);

vec3 pos = { 50.0f, 50.0f, 0.0f };
mat4 mvp;

GLuint program;
GLuint vao, vbo_circle;
GLint attribute_coord2d;
GLint uniform_mvp;

int main(int argc, char *argv[]) {

	GtkWidget *window;
	GtkWidget *glArea;

	gtk_init(&argc, &argv);

	// Initialize Window

	window = gtk_window_new(GTK_WINDOW_TOPLEVEL);
	gtk_window_set_title(GTK_WINDOW(window), "Brickout Tutorial");
	gtk_window_set_position(GTK_WINDOW(window), GTK_WIN_POS_CENTER);
	gtk_window_set_default_size(GTK_WINDOW(window), 640, 480);
	gtk_window_set_type_hint(GTK_WINDOW(window), GDK_WINDOW_TYPE_HINT_UTILITY);
	g_signal_connect(window, "destroy", G_CALLBACK(gtk_main_quit), NULL);

	// Initialize GTK GL Area

	glArea = gtk_gl_area_new();
	gtk_widget_set_vexpand(glArea, TRUE);
	gtk_widget_set_hexpand(glArea, TRUE);
	g_signal_connect(glArea, "realize", G_CALLBACK(on_realize), NULL);
	g_signal_connect(glArea, "render", G_CALLBACK(on_render), NULL);
	gtk_container_add(GTK_CONTAINER(window), glArea);

	// Show widgets

	gtk_widget_show_all(window);
	gtk_main();

	return 0;

}

static void on_realize(GtkGLArea *area) {

	// Debug Message

	g_print("on realize\n");

	gtk_gl_area_make_current(area);
	if(gtk_gl_area_get_error(area) != NULL) {
		fprintf(stderr, "Unknown error\n");
		return;
	}

	const GLubyte *renderer = glGetString(GL_RENDER);
	const GLubyte *version = glGetString(GL_VERSION);
	const GLubyte *shader = glGetString(GL_SHADING_LANGUAGE_VERSION);

	printf("Shader %s\n", shader);
	printf("Renderer: %s\n", renderer);
	printf("OpenGL version supported %s\n", version);

	glClearColor(1.0f, 1.0f, 1.0f, 1.0f);

	glGenVertexArrays(1, &vao);
	glBindVertexArray(vao);

	int i;
	float angle, nextAngle;
	int num_segments = 100;

	GLfloat circle_vertices[6 * 100];
	
	for(i = 0; i g_timeout_add(20, on_idle, (void*)area);
	
}

static void on_render(GtkGLArea *area, GdkGLContext *context) {

	glClear(GL_COLOR_BUFFER_BIT | GL_DEPTH_BUFFER_BIT);

	mat4_translate(pos, mvp);
	glUniformMatrix4fv(uniform_mvp, 1, GL_FALSE, mvp);

	glBindVertexArray(vao);
	glEnableVertexAttribArray(attribute_coord2d);

	glBindBuffer(GL_ARRAY_BUFFER, vbo_circle);
	glVertexAttribPointer(
		attribute_coord2d,
		2,
		GL_FLOAT,
		GL_FALSE,
		0,
		0
	);

	glDrawArrays(GL_TRIANGLES, 0, 3 * 100);
	glDisableVertexAttribArray(attribute_coord2d);

}

static gboolean on_idle(gpointer data) {

	pos[0] += 1.0f;
	gtk_widget_queue_draw(GTK_WIDGET(data));
	return TRUE;

}
```

Compile with:

```bash
$ gcc -c -o DashGL/dashgl.o DashGL/dashgl.c -lepoxy -lpng -lm
$ gcc `pkg-config --cflags gtk+-3.0` main.c DashGL/dashgl.o `pkg-config --libs gtk+-3.0` \
-lepoxy -lm -lpng
```

Run with:

```
$ ./a.out
```

**Version Changes:**

* 2017.Nov.20 Changed explanation from "ten milliseconds" to "twenty milliseconds" to reflect the source code. Fix offered by u/Yell\_owish on reddit.
