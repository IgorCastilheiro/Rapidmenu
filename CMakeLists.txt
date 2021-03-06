cmake_minimum_required(VERSION 3.0)

if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
  if(DEFINED ENV{DOLCESDK})
    set(CMAKE_TOOLCHAIN_FILE "$ENV{DOLCESDK}/share/dolce.toolchain.cmake" CACHE PATH "toolchain file")
  else()
    message(FATAL_ERROR "Please define DOLCESDK to point to your SDK path!")
  endif()
endif()

project(rapidmenu LANGUAGES C)
include("$ENV{DOLCESDK}/share/dolce.cmake" REQUIRED)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -nostdlib -Wall -Wextra -O3 -std=c99")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -nostdlib")

set(ELF "${PROJECT_NAME}.elf")
set(SELF "${PROJECT_NAME}.skprx")

add_executable("${ELF}"
  main.c
)

target_link_libraries("${ELF}"
  ScePowerForDriver_stub
)

dolce_create_self("${SELF}"
  "${ELF}"
  CONFIG exports.yml
  UNSAFE
)
