### CL Implementations

[SnuCL](http://snucl.snu.ac.kr/), An OpenCL Framework for Heterogeneous Clusters, made by Seoul National University


[SOCL](http://hal.archives-ouvertes.fr/docs/00/85/34/23/PDF/RR-8346.pdf), An OpenCL
Implementation with Automatic Multi-Device Adaptation Support

[POCL](http://portablecl.org), Portable Computing Language


### [cf4ocl](http://fakenmc.github.io/cf4ocl/), The C Framework for OpenCL

The C Framework for OpenCL, cf4ocl, is a cross-platform pure C99 object-oriented framework for developing and benchmarking OpenCL projects in C/C++. It aims to:

1. Promote the rapid development of OpenCL programs in C/C++.
2. Assist in the benchmarking of OpenCL events, such as kernel execution and data transfers.
3. Simplify the analysis of the OpenCL environment and of kernel requirements.

### [SimpleOpenCL](https://code.google.com/p/simple-opencl/), a library written in ANSI C and born in the needs of scientific research test development

**SimpleOpenCL code version**
```c
#include "simpleCL.h"

int main() {
   char buf[]="Hello, World!";
   size_t global_size[2], local_size[2];
   int found, worksize;
   sclHard hardware;
   sclSoft software;

   // Target buffer just so we show we got the data from OpenCL
   worksize = strlen(buf);
   char buf2[worksize];
   buf2[0]='?';
   buf2[worksize]=0;
    
   // Get the hardware
   hardware = sclGetGPUHardware( 0, &found );
   // Get the software
   software = sclGetCLSoftware( "example.cl", "example", hardware );
   // Set NDRange dimensions
   global_size[0] = strlen(buf); global_size[1] = 1;
   local_size[0] = global_size[0]; local_size[1] = 1;
    
   sclManageArgsLaunchKernel( hardware, software, global_size, local_size,
                               " %r %w ",
                              worksize, buf, worksize, buf2 );
    
   // Finally, output out happy message.
   puts(buf2);

}
```

**Now the same code but in plain OpenCL WITHOUT using SimpleOpenCL**
```c
#include <stdio.h>
#include <string.h>

#include <CL/cl.h>

int main() {
        char buf[]="Hello, World!";
        char build_c[4096];
        size_t srcsize, worksize=strlen(buf);
        
        cl_int error;
        cl_platform_id platform;
        cl_device_id device;
        cl_uint platforms, devices;
    
        /* Fetch the Platforms, we only want one. */
        error=clGetPlatformIDs(1, &platform, &platforms);
        if (error != CL_SUCCESS) {
                printf("\n Error number %d", error);
        }
        /* Fetch the Devices for this platform */
        error=clGetDeviceIDs(platform, CL_DEVICE_TYPE_ALL, 1, &device, &devices);
        if (error != CL_SUCCESS) {  
                printf("\n Error number %d", error);
        }
        /* Create a memory context for the device we want to use  */
        cl_context_properties properties[]={CL_CONTEXT_PLATFORM, (cl_context_properties)platform,0};
        /* Note that nVidia's OpenCL requires the platform property */
        cl_context context=clCreateContext(properties, 1, &device, NULL, NULL, &error);
        if (error != CL_SUCCESS) {
                printf("\n Error number %d", error);
        }
        /* Create a command queue to communicate with the device */
        cl_command_queue cq = clCreateCommandQueue(context, device, 0, &error);
        if (error != CL_SUCCESS) {
                printf("\n Error number %d", error);
        }
        
        /* Read the source kernel code in exmaple.cl as an array of char's */
        char src[8192];
        FILE *fil=fopen("example.cl","r");
        srcsize=fread(src, sizeof src, 1, fil);
        fclose(fil);
    
        const char *srcptr[]={src};
        /* Submit the source code of the kernel to OpenCL, and create a program object with it */
        cl_program prog=clCreateProgramWithSource(context,
                                              1, srcptr, &srcsize, &error);
        if (error != CL_SUCCESS) {
                printf("\n Error number %d", error);
        }

        /* Compile the kernel code (after this we could extract the compiled version) */
        error=clBuildProgram(prog, 0, NULL, "", NULL, NULL);
        if ( error != CL_SUCCESS ) {
                printf( "Error on buildProgram " );
                printf("\n Error number %d", error);
                fprintf( stdout, "\nRequestingInfo\n" );
                clGetProgramBuildInfo( prog, devices, CL_PROGRAM_BUILD_LOG, 4096, build_c, NULL );
                printf( "Build Log for %s_program:\n%s\n", "example", build_c );
        }
    
        /* Create memory buffers in the Context where the desired Device is. These will be the pointer 
        parameters on the kernel. */
        cl_mem mem1, mem2;
        mem1=clCreateBuffer(context, CL_MEM_READ_ONLY, worksize, NULL, &error);
        if (error != CL_SUCCESS) {
                printf("\n Error number %d", error);
        }
        mem2=clCreateBuffer(context, CL_MEM_WRITE_ONLY, worksize, NULL, &error);
        if (error != CL_SUCCESS) {
                printf("\n Error number %d", error);
        }
        /* Create a kernel object with the compiled program */
        cl_kernel k_example=clCreateKernel(prog, "example", &error);
        if (error != CL_SUCCESS) {
                printf("\n Error number %d", error);
        }

        /* Set the kernel parameters */
        error = clSetKernelArg(k_example, 0, sizeof(mem1), &mem1);
        if (error != CL_SUCCESS) {
                printf("\n Error number %d", error);
        }
        error = clSetKernelArg(k_example, 1, sizeof(mem2), &mem2);
        if (error != CL_SUCCESS) {
                printf("\n Error number %d", error);
        }
        /* Create a char array in where to store the results of the Kernel */
        char buf2[sizeof buf];
        buf2[0]='?';
        buf2[worksize]=0;
    
        /* Send input data to OpenCL (async, don't alter the buffer!) */
        error=clEnqueueWriteBuffer(cq, mem1, CL_FALSE, 0, worksize, buf, 0, NULL, NULL);
        if (error != CL_SUCCESS) {
                printf("\n Error number %d", error);
        }
        /* Tell the Device, through the command queue, to execute que Kernel */
        error=clEnqueueNDRangeKernel(cq, k_example, 1, NULL, &worksize, &worksize, 0, NULL, NULL);
        if (error != CL_SUCCESS) {
                printf("\n Error number %d", error);
        }
        /* Read the result back into buf2 */
        error=clEnqueueReadBuffer(cq, mem2, CL_FALSE, 0, worksize, buf2, 0, NULL, NULL);
        if (error != CL_SUCCESS) {
                printf("\n Error number %d", error);
        }
        /* Await completion of all the above */
        error=clFinish(cq);
        if (error != CL_SUCCESS) {
                printf("\n Error number %d", error);
        }
        /* Finally, output the result */
        puts(buf2);
}
```

### [OCL-MLA](http://tuxfan.github.io/ocl-mla/), a mid-level set of abstractions to make OpenCL development easier

OCL-MLA provides a set of compile-time configurable logical devices that are mapped to actual node-level device resources.

**Example**
```c
const size_t ELEMENTS = 32;

int main(int argc, char ** argv) {
   size_t global_size = ELEMENTS;

   // initialize OpenCL runtime
   ocl_init();

   // create a host-side array
   float h_array[ELEMENTS];

   // initialize host-side array
   for(size_t i=0; i<ELEMENTS; ++i) {
      h_array[i] = 0.0;
   } // for

   // create a device-side array
   ocl_create_buffer(OCL_PERFORMANCE_DEVICE, "array", ELEMENTS*sizeof(float),
      CL_MEM_READ_ONLY | CL_MEM_COPY_HOST_PTR, h_array);

   // create program source from static input string
   char * source = NULL;
   ocl_add_from_string(test_PPSTR, &source, 0);

   // add program
   ocl_add_program(OCL_PERFORMANCE_DEVICE, "program", source, "-DMY_DEFINE");
   free(source);

   // add kernel
   ocl_add_kernel(OCL_PERFORMANCE_DEVICE, "program", "test", "my test");

   // use hints interface to decide what work-group size to use
   ocl_kernel_hints_t hints;
   size_t work_group_indeces;
   size_t single_indeces;

   // get kernel hints
   ocl_kernel_hints(OCL_DEFAULT_DEVICE, "program", "my test", &hints);

   // heuristic for how to execute global_size work-items
   ocl_ndrange_hints(global_size, hints.max_work_group_size,
      0.5, 0.5, &local_size, &work_group_indeces, &single_indeces);

   // set kenerl argument
   ocl_set_kernel_arg_buffer("program", "my test", "array", 0);

   // initialize event for timings
   ocl_initialize_event(&event);

   // invoke kernel
   ocl_enqueue_kernel_ndrange(OCL_PERFORMANCE_DEVICE, "program",
      "my test", 1, &global_offset, &global_size, &local_size, &event);

   // block for kernel completion
   ocl_finish(OCL_PERFORMANCE_DEVICE);

   // add a timer event for the kernel invocation
   ocl_add_timer("kernel", &event);

   // read data from device
   ocl_enqueue_read_buffer(OCL_PERFORMANCE_DEVICE, "array", 1, offset,
      ELEMENTS*sizeof(float), h_array, &event);

   // print data read from device
   for(size_t i=0; i<ELEMENTS; ++i) {
      fprintf(stderr, "%f\n", h_array[i]);
   } // for
   fprintf(stderr, "\n");

   // print timer results
   ocl_report_timer("kernel");

   // finalize OpenCL runtime
   ocl_finalize();
}
```
