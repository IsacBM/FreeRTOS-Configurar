# FreeRTOS-Configurar
> Aprendizagem...

## Instalação do FreeRTOS

### Passo 1: Baixar FreeRTOS.
Link: https://github.com/FreeRTOS/FreeRTOS/releases/download/202212.01/FreeRTOSv202212.01.zip

### Passo 2: Descompactem o arquivo Zip.
### Passo 3: Mova a pasta para um diretorio de confiança,
*  Por exemplo: O Disco Local C(C:\)
```
C:\FreeRTOSv202212.01\FreeRTOS\Source
```
<img src="https://github.com/IsacBM/FreeRTOS-Configurar/blob/main/img/Captura%20de%20tela%202025-03-25%20140953.png?raw=true" alt="Disco Local C">

### Passo 4: Cópiar o Endereço.
*  Entre na pasta `\FreeRTOSv202212.01`, depois na pasta `\FreeRTOS` e logo em seguida na pasta `\Source`
<img src="https://github.com/IsacBM/FreeRTOS-Configurar/blob/main/img/Endereco.png?raw=true" alt="Cópiar Endereço">

### Passo 5: Criar Variável de Ambiente.
* Digite o seguinte comando no Powershell:

```cmd
[System.Environment]::SetEnvironmentVariable("FREERTOS_KERNEL_PATH", "C:\FreeRTOSv202212.01\FreeRTOS\Source", "Machine")
```

### Passo 6: Criar o Projeto no VsCode:

<img src="https://github.com/IsacBM/FreeRTOS-Configurar/blob/main/img/Captura%20de%20tela%202025-03-25%20143016.png?raw=true" alt="Criando o Projeto">
* Versão do SDK testada: v1.5.1

### Passo 7: Editar o `CMakeLists.txt`
* ATENÇÂO: Substitua o `NOMEDOPROJETO` pelo nome do seu projeto/arquivo.
```
# Generated Cmake Pico project file
cmake_minimum_required(VERSION 3.13)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Initialise pico_sdk from installed location
# (note this can come from environment, CMake cache etc)

# == DO NOT EDIT THE FOLLOWING LINES for the Raspberry Pi Pico VS Code Extension to work ==
if(WIN32)
    set(USERHOME $ENV{USERPROFILE})
else()
    set(USERHOME $ENV{HOME})
endif()
set(sdkVersion 2.1.1)
set(toolchainVersion 14_2_Rel1)
set(picotoolVersion 2.1.1)
set(picoVscode ${USERHOME}/.pico-sdk/cmake/pico-vscode.cmake)
if (EXISTS ${picoVscode})
    include(${picoVscode})
endif()
# ====================================================================================
set(PICO_BOARD pico_w CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

include($ENV{FREERTOS_KERNEL_PATH}/portable/ThirdParty/GCC/RP2040/FreeRTOS_Kernel_import.cmake)

project(NOMEDOPROJETO C CXX ASM)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

# Add executable. Default name is the project name, version 0.1

add_executable(NOMEDOPROJETO NOMEDOPROJETO.c)

pico_set_program_name(NOMEDOPROJETO "NOMEDOPROJETO")
pico_set_program_version(NOMEDOPROJETO "0.1")

# Modify the below lines to enable/disable output over UART/USB
pico_enable_stdio_uart(NOMEDOPROJETO 0)
pico_enable_stdio_usb(NOMEDOPROJETO 1)

# Add the standard include files to the build
target_include_directories(NOMEDOPROJETO PRIVATE ${CMAKE_CURRENT_LIST_DIR})

# Add any user requested libraries
target_link_libraries(NOMEDOPROJETO pico_stdlib hardware_gpio pico_multicore FreeRTOS-Kernel FreeRTOS-Kernel-Heap4)

pico_add_extra_outputs(NOMEDOPROJETO)

```

### Passo 8: Adicionar o `FreeRTOSConfig.h`.
*  Nome do Arquivo: `FreeRTOSConfig.h`
*  Conteúdo:

```
/*
 * FreeRTOS V202111.00
 * Copyright (C) 2020 Amazon.com, Inc. or its affiliates.  All Rights Reserved.
 *
 * Permission is hereby granted, free of charge, to any person obtaining a copy of
 * this software and associated documentation files (the "Software"), to deal in
 * the Software without restriction, including without limitation the rights to
 * use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
 * the Software, and to permit persons to whom the Software is furnished to do so,
 * subject to the following conditions:
 *
 * The above copyright notice and this permission notice shall be included in all
 * copies or substantial portions of the Software.
 *
 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
 * FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
 * COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
 * IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
 * CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
 *
 * http://www.FreeRTOS.org
 * http://aws.amazon.com/freertos
 *
 * 1 tab == 4 spaces!
 */

 #ifndef FREERTOS_CONFIG_H
 #define FREERTOS_CONFIG_H
 
 // #include "rp2040_config.h"
 
 /*-----------------------------------------------------------
  * Application specific definitions.
  *
  * These definitions should be adjusted for your particular hardware and
  * application requirements.
  *
  * THESE PARAMETERS ARE DESCRIBED WITHIN THE 'CONFIGURATION' SECTION OF THE
  * FreeRTOS API DOCUMENTATION AVAILABLE ON THE FreeRTOS.org WEB SITE.
  *
  * See http://www.freertos.org/a00110.html
  *----------------------------------------------------------*/
 
 /* Scheduler Related */
 #define configUSE_PREEMPTION                    1
 #define configUSE_TICKLESS_IDLE                 0
 #define configUSE_IDLE_HOOK                     0
 #define configUSE_TICK_HOOK                     0
 #define configTICK_RATE_HZ                      ( ( TickType_t ) 1000 )
 #define configMAX_PRIORITIES                    32
 #define configMINIMAL_STACK_SIZE                ( configSTACK_DEPTH_TYPE ) 256
 #define configUSE_16_BIT_TICKS                  0
 
 #define configIDLE_SHOULD_YIELD                 1
 
 /* Synchronization Related */
 #define configUSE_MUTEXES                       1
 #define configUSE_RECURSIVE_MUTEXES             1
 #define configUSE_APPLICATION_TASK_TAG          0
 #define configUSE_COUNTING_SEMAPHORES           1
 #define configQUEUE_REGISTRY_SIZE               8
 #define configUSE_QUEUE_SETS                    1
 #define configUSE_TIME_SLICING                  1
 #define configUSE_NEWLIB_REENTRANT              0
 // todo need this for lwip FreeRTOS sys_arch to compile
 #define configENABLE_BACKWARD_COMPATIBILITY     1
 #define configNUM_THREAD_LOCAL_STORAGE_POINTERS 5
 
 /* System */
 #define configSTACK_DEPTH_TYPE                  uint32_t
 #define configMESSAGE_BUFFER_LENGTH_TYPE        size_t
 
 /* Memory allocation related definitions. */
 #define configSUPPORT_STATIC_ALLOCATION         0
 #define configSUPPORT_DYNAMIC_ALLOCATION        1
 #define configTOTAL_HEAP_SIZE                   (128*1024)
 #define configAPPLICATION_ALLOCATED_HEAP        0
 
 /* Hook function related definitions. */
 #define configCHECK_FOR_STACK_OVERFLOW          0
 #define configUSE_MALLOC_FAILED_HOOK            0
 #define configUSE_DAEMON_TASK_STARTUP_HOOK      0
 
 /* Run time and task stats gathering related definitions. */
 #define configGENERATE_RUN_TIME_STATS           0
 #define configUSE_TRACE_FACILITY                1
 #define configUSE_STATS_FORMATTING_FUNCTIONS    0
 
 /* Co-routine related definitions. */
 #define configUSE_CO_ROUTINES                   0
 #define configMAX_CO_ROUTINE_PRIORITIES         1
 
 /* Software timer related definitions. */
 #define configUSE_TIMERS                        1
 #define configTIMER_TASK_PRIORITY               ( configMAX_PRIORITIES - 1 )
 #define configTIMER_QUEUE_LENGTH                10
 #define configTIMER_TASK_STACK_DEPTH            1024
 
 /* Interrupt nesting behaviour configuration. */
 /*
 #define configKERNEL_INTERRUPT_PRIORITY         [dependent of processor]
 #define configMAX_SYSCALL_INTERRUPT_PRIORITY    [dependent on processor and application]
 #define configMAX_API_CALL_INTERRUPT_PRIORITY   [dependent on processor and application]
 */
 
 #if FREE_RTOS_KERNEL_SMP // set by the RP2040 SMP port of FreeRTOS
 /* SMP port only */
 #define configNUM_CORES                         2
 #define configTICK_CORE                         0
 #define configRUN_MULTIPLE_PRIORITIES           1
 #define configUSE_CORE_AFFINITY                 0
 #endif
 
 /* RP2040 specific */
 #define configSUPPORT_PICO_SYNC_INTEROP         1
 #define configSUPPORT_PICO_TIME_INTEROP         1
 
 #include <assert.h>
 /* Define to trap errors during development. */
 #define configASSERT(x)                         assert(x)
 
 /* Set the following definitions to 1 to include the API function, or zero
 to exclude the API function. */
 #define INCLUDE_vTaskPrioritySet                1
 #define INCLUDE_uxTaskPriorityGet               1
 #define INCLUDE_vTaskDelete                     1
 #define INCLUDE_vTaskSuspend                    1
 #define INCLUDE_vTaskDelayUntil                 1
 #define INCLUDE_vTaskDelay                      1
 #define INCLUDE_xTaskGetSchedulerState          1
 #define INCLUDE_xTaskGetCurrentTaskHandle       1
 #define INCLUDE_uxTaskGetStackHighWaterMark     1
 #define INCLUDE_xTaskGetIdleTaskHandle          1
 #define INCLUDE_eTaskGetState                   1
 #define INCLUDE_xTimerPendFunctionCall          1
 #define INCLUDE_xTaskAbortDelay                 1
 #define INCLUDE_xTaskGetHandle                  1
 #define INCLUDE_xTaskResumeFromISR              1
 #define INCLUDE_xQueueGetMutexHolder            1
 
 /* A header file that defines trace macro can be included here. */
 
 #endif /* FREERTOS_CONFIG_H */
 
```

### Passo 9: Testar o Projeto.

* Selecionar arquivos da Pasta build:
<img src="https://github.com/IsacBM/FreeRTOS-Configurar/blob/main/img/Captura%20de%20tela%202025-03-25%20142347.png?raw=true" alt="">
* Apagar Arquivos do BUILD:
<img src="https://github.com/IsacBM/FreeRTOS-Configurar/blob/main/img/Captura%20de%20tela%202025-03-25%20142400.png?raw=true" alt="">
* Vá até a extensão da Raspberry:
<img src="https://github.com/IsacBM/FreeRTOS-Configurar/blob/main/img/Captura%20de%20tela%202025-03-25%20142303.png?raw=true" alt="">
* Selecione a opção: `Configure CMake`
<img src="https://github.com/IsacBM/FreeRTOS-Configurar/blob/main/img/Captura%20de%20tela%202025-03-25%20142244.png?raw=true" alt="">

* Código de Exemplo feito pelo ()[]
```c
#include "pico/stdlib.h"
#include "FreeRTOS.h"
#include "task.h"
#include <stdio.h>

const uint led_pin_red = 12;

void vBlinkTask(){
    for(;;)
    {
        gpio_put(led_pin_red, 1);
        vTaskDelay(250);
        gpio_put(led_pin_red, 0);
        vTaskDelay(250);
        printf("Blinking\n");
    }
}

int main(){
    stdio_init_all();
    gpio_init(led_pin_red);
    gpio_set_dir(led_pin_red, GPIO_OUT);
    xTaskCreate(vBlinkTask, "Blink Task", 128, NULL, 1, NULL);
    vTaskStartScheduler();
}

```
