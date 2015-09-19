### libcoro

This C-library implements coroutines (cooperative multitasking) in a portable fashion.

GPL v2 license.

- http://software.schmorp.de/pkg/libcoro.html
- https://github.com/clibs/coro

As long as your system implements the ucontext (Unix) or the older sigaltstack interfaces it should work out of the box, with minimal configuration (it consists of only a single .h and a single .c file). For the broken systems, it also supports a slow pthreads-based system and (optional) assembly backends for higher speed on some systems. It is known to run on a wide variety of unix systems (SunOS, IRIX, GNU/Linux, HP-UX, FreeBSD, NetBSD, OpenBSD) and also on Windows, does not require any assembly language and is architecture-independent.


### Coco

Coco is a small extension to get True C Coroutine semantics for Lua 5.1. Coco is available as a patch set against the standard Lua 5.1.5 source distribution.

- http://coco.luajit.org/index.html

```c
#elif defined(__arm__) || defined(__ARM__)

#if __GLIBC__ == 2 || defined(__UCLIBC__)  /* arm-linux-glibc2 */
#ifndef __JMP_BUF_SP
#define __JMP_BUF_SP ((sizeof(__jmp_buf)/sizeof(int))-2)
#endif
#define COCO_PATCHCTX(coco, buf, func, stack, a0) \
  buf->__jmpbuf[__JMP_BUF_SP+1] = (int)(func); /* pc */ \
  buf->__jmpbuf[__JMP_BUF_SP] = (int)(stack); /* sp */ \
  buf->__jmpbuf[__JMP_BUF_SP-1] = 0; /* fp */ \
  stack[0] = (size_t)(a0);
#define COCO_STACKADJUST 2
#define COCO_MAIN_PARAM    int _a, int _b, int _c, int _d, lua_State *L
#elif defined(__APPLE__) /* arm-ios */
#define __JMP_BUF_SP  7   /* r4 r5 r6 r7 r8 r10 fp sp lr sig ... */
#define COCO_PATCHCTX(coco, buf, func, stack, a0) \
  buf[__JMP_BUF_SP+1] = (int)(func); /* lr */ \
  buf[__JMP_BUF_SP] = (int)(stack); /* sp */ \
  buf[__JMP_BUF_SP-1] = 0; /* fp */ \
  stack[0] = (size_t)(a0);
#define COCO_STACKADJUST 2
#define COCO_MAIN_PARAM int _a, int _b, int _c, int _d, lua_State *L
#endif

#endif /* arch check */
```



### Implementing a Thread Library on Linux

http://www.evanjones.ca/software/threading.html

Using setjmp, longjmp, sigaltstack, sigemptyset, sigaction to implement threads. Not applicable for bare-metal environment.

```c
#include <malloc.h>
#include <setjmp.h>
#include <signal.h>
#include <stdio.h>

// 64kB stack
#define FIBER_STACK 1024*64

jmp_buf child, parent;

// The child thread will execute this function
void threadFunction()
{
         printf( "Child fiber yielding to parent\n" );
         if ( setjmp( child ) )
         {
                 printf( "Child thread exiting\n" );
                 longjmp( parent, 1 );
         }

         longjmp( parent, 1 );
}

void signalHandler( int arg )
{
         if ( setjmp( child ) )
         {
                 threadFunction();
         }
        
         return;
}

int main()
{
         stack_t stack;
         struct sigaction sa;
        
         // Create the new stack
         stack.ss_flags = 0;
         stack.ss_size = FIBER_STACK;
         stack.ss_sp = malloc( FIBER_STACK );
         if ( stack.ss_sp == 0 )
         {
                 perror( "malloc: Could not allocate stack." );
                 exit( 1 );
         }
         sigaltstack( &stack, 0 );
        
         // Set up the custom signal handler
         sa.sa_handler = &signalHandler;
         sa.sa_flags = SA_ONSTACK;
         sigemptyset( &sa.sa_mask );
         sigaction( SIGUSR1, &sa, 0 );
        
         // Send the signal to call the function on the new stack
         printf( "Creating child fiber\n" );
         raise( SIGUSR1 );
        
         // Execute the child context
         printf( "Switching to child fiber\n" );
         if ( setjmp( parent ) )
         {
                 printf( "Switching to child fiber again\n" );
                 if ( setjmp( parent ) == 0 ) longjmp( child, 1 );
         }
         else longjmp( child, 1 );
        
         // Free the stack
         free( stack.ss_sp );
         printf( "Child fiber returned and stack freed\n" );
         return 0;
}
```

### Portable fibers/coroutines in c++

http://www.subatomicglue.com/secret/coro/readme.html

using [setcontext](https://en.wikipedia.org/wiki/Setcontext) to implement threads, not portable for bare-metal environment.


### libconcurrency

https://code.google.com/p/libconcurrency/
https://code.google.com/p/libconcurrency/source/browse/trunk/libconcurrency/ctxt.h

GPL License




### A Minimal User-Level Thread Package

http://homepage.cs.uiowa.edu/~jones/opsys/threads/

by Douglas W. Jones 

THE UNIVERSITY OF IOWA Department of Computer Science

```text
   Permission is hereby granted to make and modify
   personal copies of this code for noncommercial or
   educational use.
```