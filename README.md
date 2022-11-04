背景：   
   日常vim开发过程中，编写代码后，希望能够立即跑相关UT，这时候经常需要退出vim找到对应涉及到的UT executable target 然后进行一长串的编译/执行。结合AsyncRun和LeaderF能力，支持搜索Cmake中 XXX.cpp 与 executable target的映射关系，指定编译/执行相关executable target。  
    
- [x] CMake 支持生成 XXX.cpp -> executable target 映射配置文件

```
if (${CMAKE_BUILD_TYPE} STREQUAL "Debug")
  macro(add_executable _target)
    _add_executable(${_target} ${ARGN})
    foreach(X ${ARGN})
      set_property(GLOBAL APPEND PROPERTY GlobalTargetList "${X} -> ${_target}\n")
    endforeach()
  endmacro()

#macro(add_library _target)
#    _add_library(${_target} ${ARGN})
#    set_property(GLOBAL APPEND PROPERTY GlobalTargetList ${_target})
#endmacro()
#macro(add_custom_target _target)
#    _add_custom_target(${_target} ${ARGN})
#    set_property(GLOBAL APPEND PROPERTY GlobalTargetList ${_target})
#endmacro()
endif()

#add_subdirectory(src)

if (${CMAKE_BUILD_TYPE} STREQUAL "Debug")
  get_property(_allTargets GLOBAL PROPERTY GlobalTargetList)
  file(WRITE test.txt ${_allTargets})
endif()

```
仅在Debug模式下生效，且尽可能的缩短范围，避免避免包含依赖库中的内容。  
- [ ] LeaderF 的扩展调研  
  涉及到文件等接口，初步以python扩展为基础，需要看下扩展接口。参考[LeaderF-marks](https://github.com/Yggdroot/LeaderF-marks)
