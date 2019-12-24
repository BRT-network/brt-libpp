
cmake_minimum_required(VERSION 3.9)
project(brt-libppdemo)

#[===========================================[
  this pulls-in the brtd source code via
  git submodule. This demo exe depends on the
  xrpl_core library which is defined by the
  brtd project.
#]===========================================]
if (MSVC)
  #[=======================================[
    HACK to patch a one-line windows-only
    bug in brtd CMakeLists.txt. This can
    be removed once brtd 1.2 is available
    and we update the submodule
  #]=======================================]
  execute_process( COMMAND powershell -Command
     "(Get-Content extras/brtd/CMakeLists.txt) \
     -replace '^exclude_if_included\\s+\\(rocksdb\\)', '##' \
     | Set-Content extras/brtd/CMakeLists.txt"
     WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
  )
endif ()
add_subdirectory(extras/brtd)

add_executable(brtlibppdemo src/test/brt-libpp_demo.cpp)
target_link_libraries (brtlibppdemo Ripple::xrpl_core)
