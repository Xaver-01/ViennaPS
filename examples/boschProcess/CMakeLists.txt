project(boschProcess LANGUAGES CXX)

if(VIENNAPS_USE_GPU)
  add_gpu_executable(boschProcessSimulate target_name "boschProcessSimulate.cpp")
else()
  add_executable(boschProcessSimulate "boschProcessSimulate.cpp")
  target_link_libraries(boschProcessSimulate PRIVATE ViennaPS)
endif()

add_executable(boschProcessEmulate "boschProcessEmulate.cpp")
target_link_libraries(boschProcessEmulate PRIVATE ViennaPS)

add_dependencies(ViennaPS_Examples boschProcessEmulate)
add_dependencies(ViennaPS_Examples boschProcessSimulate)

viennacore_setup_bat(boschProcessSimulate ${VIENNAPS_ARTIFACTS_DIRECTORY})
viennacore_setup_bat(boschProcessEmulate ${VIENNAPS_ARTIFACTS_DIRECTORY})

configure_file(config.txt ${CMAKE_CURRENT_BINARY_DIR}/config.txt COPYONLY)
