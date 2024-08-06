BSP: STMicroelectronics [STM32H743I-EVAL](https://github.com/Open-CMSIS-Pack/STM32H743I-EVAL_BSP)
------------------------------------------ 
  
 
 ## Folders and files

| Folders                    | Description
|:---------------------------|:--------------------------------------
| [MW-RefApps](https://github.com/Open-CMSIS-Pack/STM32H743I-EVAL_BSP/tree/main/.ci/MW-RefApps)                 | Includes patch files for Middleware examples
| [.github/workflows/](https://github.com/Open-CMSIS-Pack/STM32H743I-EVAL_BSP/tree/main/.github/workflows)         | Includes workflows for creating this BSP and building MDK-Middleware examples with **AC6** and **GCC**


| Files                      | Description
|:---------------------------|:--------------------------------------
| [vcpkg-configuration.json](https://github.com/Open-CMSIS-Pack/STM32H743I-EVAL_BSP/blob/main/.ci/vcpkg-configuration.json)   | Tool configuration for the build test



 ## Workflow sequence

The simplified workflow sequence is:

* Tools installation and license activation
* Download and installation of the following packs:
  * [**STM32H743I-EVAL_BSP**](https://github.com/Open-CMSIS-Pack/STM32H743I-EVAL_BSP)
  * [**STM32H7xx_DFP**](https://github.com/Open-CMSIS-Pack/STM32H7xx_DFP)
  * [**CMSIS-Driver_STM32**](https://github.com/Open-CMSIS-Pack/CMSIS-Driver_STM32)  
  * [**MDK-Middleware**](https://github.com/ARM-software/MDK-Middleware)
* Initialization of the **CMSIS** root folder.
* Copy **MDK-Middleware** examples for each component to the **./CI/MW-RefApps/Examples/** folder
* Add toolchain specific board layer in folder **<middleware_component>/Board**

      ~                         ||    ~                               ||    ~                            ||    ~                               ||    ~                              
      │                         ||    │                               ||    │                            ||    │                               ||    │                              
      ├── MDK-Middleware        ||    ├── CI                          ||    ├── CI                       ||    ├── CI                          ||    ├── CI                         
      │   ├── Examples          ||    │   ├── MW-RefApps              ||    │   ├── MW-RefApps           ||    │   ├── MW-RefApps              ||    │   ├── MW-RefApps             
      │   │   ├── FileSystem    ||    │   │   └── Examples            ||    │   │   └── Examples         ||    │   │   └── Examples            ||    │   │   └── Examples           
      │   │   ├── Network       ||    │   │       ├── FileSystem      ||    │   │       ├── Network      ||    │   │       ├── USB             ||    │   │       ├── USB            
      │   │   └── USB           ||    │   │       │   └── Board       ||    │   │       │   └── Board    ||    │   │       │   └── Device      ||    │   │       │   └── Host       
      │   │       ├── Device    ||    │   │       │                   ||    │   │       │                ||    │   │       │      └── Board    ||    │   │       │       └── Board  
      │   │       └── Host      ||    ~   ~       ~                   ||    ~   ~       ~                ||    ~   ~       ~                   ||    ~   ~       ~                  
      ~   ~                                                                                                                                                                         
                                Copy each Middleware component and then:   (1) Build projects with AC6    (2) Upload AC6 build Artifacts                                            
                                                                           (3) Build projects with GCC    (4) Upload GCC build Artifacts                                            
                                                                                                                                                                                    
* Build operations for the **AC6** and **GCC** toolchains. Upload the build Artifacts.



| Middleware component       | Source directory
|:---------------------------|:--------------------------------------
| FileSystem   | https://github.com/ARM-software/MDK-Middleware/tree/main/Examples/FileSystem
| Network      | https://github.com/ARM-software/MDK-Middleware/tree/main/Examples/Network
| USB_Device   | https://github.com/ARM-software/MDK-Middleware/tree/main/Examples/USB/Device
| USB_Host     | https://github.com/ARM-software/MDK-Middleware/tree/main/Examples/USB/Host


 ## Workflow details

The used Middleware examples are board and device agnostic. The adaptation of these examples to a specific board is done by using a board layer. 
The current implementation provides two board-layers. Each of them is dedicated to a specific compiler and has its own location in this pack.
The following matrix contains the required configurations for setting up projects component and toolchain specific.

```
       matrix:
        solution: [ 
          {name: FileSystem,   dir: FileSystem,   layer_ac6: ./STM32H743I-EVAL_BSP/Layers/IoT,  layer_gcc: ./STM32H743I-EVAL_BSP/.ci/Layers/GCC},
          {name: Network,      dir: Network,      layer_ac6: ./STM32H743I-EVAL_BSP/Layers/IoT,  layer_gcc: ./STM32H743I-EVAL_BSP/.ci/Layers/GCC},
          {name: USB_Device,   dir: USB/Device,   layer_ac6: ./STM32H743I-EVAL_BSP/Layers/IoT,  layer_gcc: ./STM32H743I-EVAL_BSP/.ci/Layers/GCC},
          {name: USB_Host,     dir: USB/Host,     layer_ac6: ./STM32H743I-EVAL_BSP/Layers/IoT,  layer_gcc: ./STM32H743I-EVAL_BSP/.ci/Layers/GCC}
        ]

```


| Board layer                | Source directory
|:---------------------------|:--------------------------------------
| layer_ac6    | https://github.com/Open-CMSIS-Pack/STM32H743I-EVAL_BSP/tree/main/Layers/IoT
| layer_gcc    | https://github.com/Open-CMSIS-Pack/STM32H743I-EVAL_BSP/tree/main/.ci/Layers/GCC


This [workflow](https://github.com/Open-CMSIS-Pack/STM32H743I-EVAL_BSP/tree/main/.github/workflows/Test-RefApps.yml) is currently executed once a week.
