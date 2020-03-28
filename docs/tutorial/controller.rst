控制器模型
---------------

控制器模型为PD测速反馈控制器，如 :eq:`eq_controller` 。

.. math::
    :label: eq_controller

    F=k_p\left( r_x-x \right) -k_dv

.. code-block:: C
    :linenos:

    #ifndef CONTROLLER_H__
    #define CONTROLLER_H__

    //==================================================================/
    // Milestone: A test case for fmi simulation tools
    // Copyright (c) 2019, MA Yuhai	
    // All rights reserved.	
    //
    // Version 1.0
    //==================================================================/

    #include "interface.h"

    #define FMI_MODEL_AUTHOR "MA Yuhai"
    #define FMI_MODEL_NAME "controller"
    #define FMI_MODEL_DISCRIPTION "a controller model"
    #define FMI_PORT_POSTFIX ""

    // resource file definition if any
    #define FMI_RESOURCE_ITEM 2
    #if FMI_RESOURCE_ITEM>0 && defined EN_RES_ACCESS
    const char *resource_file_list[FMI_RESOURCE_ITEM] = {
        "init_config.txt", "init_data.dat" };
    #endif

    // task definition if any in the unit of [ms]
    #define FMI_TASK_ITEM 2
    FMI_EXPORT void task_30ms_start_0ms(void);
    FMI_EXPORT void task_100ms_start_1030ms(void);

    // define interface variables by an fmi object
    // one statement per line, no extra semicolons allowed
    // do not modify internal variables
    typedef struct st_fmi_object_t {
        // internal variables
        FMI_IN double fmi_time_current;
        FMI_IN double fmi_time_step;
    #if FMI_RESOURCE_ITEM>0
        FMI_PRM fmi_str_ptr fmi_file_list[FMI_RESOURCE_ITEM];
    #endif

        // interface variables
        FMI_IN Stru_Data_Plant_To_Controller st_data_plant_to_controller;
        FMI_OUT Stru_Data_Controller_To_Plant st_data_controller_to_plant;
        FMI_OUT double task_30ms_status;
        FMI_OUT double task_100ms_status;
    }st_fmi_object;
    #endif // CONTROLLER_H__


.. code-block:: C
    :linenos:

    #include "controller.h"
    #include <iostream>
    #include <fstream>
    #include <string>
    using namespace std;

    double x;
    double v;
    double F;
    double x_0;
    double v_0;
    int task_30ms_trigger;
    int task_100ms_trigger;

    void load_initial_data(fmi_str_ptr fmi_file_list[])
    {
        ifstream init_file;

        init_file.open(fmi_file_list[0]);
        if (!init_file.is_open()) {
            cout << "open data file error: " << fmi_file_list[0] << endl;
        }
        else {
            string buff;
            getline(init_file, buff);
            cout << buff << endl;
        }

        init_file.close();

        init_file.open(fmi_file_list[1]);
        if (!init_file.is_open()) {
            cout << "open data file error: " << fmi_file_list[1] << endl;
        }
        else {
            init_file >> x_0;
            init_file >> v_0;
        }

        init_file.close();
        return;
    }

    void task_30ms_start_0ms(void)
    {
        task_30ms_trigger = task_30ms_trigger ? 0 : 1;
    }

    void task_100ms_start_1030ms(void)
    {
        task_100ms_trigger = task_100ms_trigger ? 0 : 1;
    }

    void* fmi_instantiate(void)
    {
        st_fmi_object *p =
            (st_fmi_object *)calloc(1, sizeof(st_fmi_object));
        if (!p) {
            fprintf(stderr, "fmi_instantiate failed in model controller!\n");
            exit(EXIT_FAILURE);
        }

        return p;
    }

    int fmi_initialize(void *fmi_object)
    {
        st_fmi_object *p = (st_fmi_object *)fmi_object;

        load_initial_data(p->fmi_file_list);
        p->st_data_controller_to_plant.x_0 = x_0;
        p->st_data_controller_to_plant.v_0 = v_0;

        return 0;
    }

    int fmi_doStep(void *fmi_object)
    {
        st_fmi_object *p = (st_fmi_object *)fmi_object;
        const double pi = 3.1416;
        const double r_x = 5;
        const double m = 0.1;

        const double zeta = 0.2; // let it oscillates
        const double omega_n = 2*pi*0.5;
        const double k_p = omega_n*omega_n*m;
        const double k_d = 2*zeta*omega_n*m;

        x = p->st_data_plant_to_controller.x;
        v = p->st_data_plant_to_controller.v;

        F = k_p * (r_x - x) - k_d * v;

        p->st_data_controller_to_plant.F = F;
        p->task_30ms_status = task_30ms_trigger;
        p->task_100ms_status = task_100ms_trigger;

        return 0;
    }

    int fmi_reset(void *fmi_object)
    {
        st_fmi_object *p = (st_fmi_object *)fmi_object;
        IO_PORT_FLUSH(Stru_Data_Controller_To_Plant, st_data_controller_to_plant);
        return 0;
    }

    void fmi_freeInstance(void *fmi_object)
    {
        st_fmi_object *p = (st_fmi_object *)fmi_object;

        free(p);
    }