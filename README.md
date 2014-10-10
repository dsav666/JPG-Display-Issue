JPG-Display-Issue
=================

Help needed with basic DevkitPro issue for beginner programmer.

With the following code (
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <malloc.h>
#include <ogcsys.h>
#include <gccore.h>
#include <jpeg/jpgogc.h>

extern char picdata[];
extern int  piclength;

static u32 *xfb;
static GXRModeObj *rmode;

void display_jpeg(JPEGIMG jpeg, int x, int y) {
	
	unsigned int *jpegout = (unsigned int *) jpeg.outbuffer;
	
	int i,j;
	int height = jpeg.height;
	int width = jpeg.width/2;
		for(i=0;i<=width;i++)
			for(j=0;j<=height-2;j++)
				xfb[(i+x)+320*(j+16+y)]=jpegout[i+width*j];
	
	f
}


void Initialise() {
	
	VIDEO_Init();
	PAD_Init();
	
	rmode = VIDEO_GetPreferredMode(NULL);
	
	xfb = MEM_K0_TO_K1(SYS_AllocateFramebuffer(rmode));
	console_init(xfb,20,20,rmode->fbWidth,rmode->xfbHeight,rmode->fbWidth*VI_DISPLAY_PIX_SZ);
	
	VIDEO_Configure(rmode);
	VIDEO_SetNextFramebuffer(xfb);
	VIDEO_SetBlack(FALSE);
	VIDEO_Flush();
	VIDEO_WaitVSync();
	if(rmode->viTVMode&VI_NON_INTERLACE) VIDEO_WaitVSync();
}


int main() {
	
	JPEGIMG about;
	
	memset(&about, 0, sizeof(JPEGIMG));
	
	about.inbuffer = picdata;
	about.inbufferlength = piclength;
	
	JPEG_Decompress(&about);
	
	Initialise();
	
	display_jpeg(about, 60, 100);
	
	return 0;
}
I am expecting to display JPEG located in dir. C:\devkitPro\examples\gamecube\tutorial5\include\about.jpg on Gcube 0.4.0 as per tutorial5 of teknekal's programming tutorials on codemii.com, but instead when I compile the source I get the output "> "make" 
main.c
In file included from c:/devkitPro/examples/gamecube/tutorial5/source/main.c:7:0:
c:\devkitpro\devkitppc\powerpc-eabi\include\jpeg\jpgogc.h:12:21: fatal error: jpeglib.h: No such file or directory
 #include <jpeglib.h>
                     ^
compilation terminated."
 How can I get the behavior I expect?" 
