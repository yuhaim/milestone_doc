全局接口头文件
---------------

.. code-block:: C
    :linenos:

    #ifndef INTERFACE_H__
    #define INTERFACE_H__
    //==================================================================/
    // A test case for fmi simulation tools
    // Copyright (c) 2019 马玉海
    // All rights reserved.
    //
    // Version 1.0
    //==================================================================/
    #define _CRT_SECURE_NO_WARNINGS
    #include <math.h>
    #include <stdio.h>
    #include <stdlib.h>
    #include <memory.h>
    #include <string.h>
    #include <float.h>

    #define IO_PORT_FLUSH(data_type, var_name) \
        do{\
            memset(&(p->var_name), 0, sizeof(data_type));\
        } while(0);

    #ifndef __cplusplus
    #define FMI_EXPORT 
    #define bool unsigned char
    #define true 1
    #define false 0
    #else
    #define FMI_EXPORT extern "C" 
    #endif

    #ifndef _WIN32
    #include <limits.h>
    #define _MAX_PATH PATH_MAX
    #define _MAX_FNAME NAME_MAX
    #define _MAX_EXT NAME_MAX
    #endif
    // non-standard interface definition
    #define FMI_IN
    #define FMI_OUT
    #define FMI_PRM
    typedef const char * fmi_str_ptr;

    FMI_EXPORT void *fmi_instantiate(void);
    FMI_EXPORT int fmi_initialize(void *);
    FMI_EXPORT int fmi_doStep(void *);
    FMI_EXPORT int fmi_reset(void *);
    FMI_EXPORT void fmi_freeInstance(void *);

    #pragma pack(push, 8)
    typedef struct _Stru_Data_Controller_To_Plant_ex
    {
        double y;
        double z;
    }Stru_Data_Controller_To_Plant_ex;

    typedef struct _Stru_Data_Controller_To_Plant_ex1{
        double y;
        double z;
    }Stru_Data_Controller_To_Plant_ex1;
    typedef struct _Stru_Data_Controller_To_Plant_ex2 {
        double y;
        double z;
    }Stru_Data_Controller_To_Plant_ex2;
    typedef struct _Stru_Data_Controller_To_Plant_ex3	{
        double y;
        double z;
    }Stru_Data_Controller_To_Plant_ex3;

    typedef struct _Stru_Data_Controller_To_Plant{
        double F;
        double x_0;
        double v_0;
    }Stru_Data_Controller_To_Plant;

    typedef struct _Stru_Data_Plant_To_Controller
    {
        double x;
        double v;
    }Stru_Data_Plant_To_Controller;

    #pragma pack(pop)

    #endif // INTERFACE_H__