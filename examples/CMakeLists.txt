if(NOT VIENNACORE_USE_OPENMP)
  message(SEND_ERROR "ViennaPS examples are only available with OpenMP support.")
  return()
endif()

add_custom_target(ViennaPS_Examples ALL)

if(VIENNAPS_USE_GPU)
  set(GENERATED_OPTIXIR_FILES)
  generate_pipeline(pipeDisk GENERATED_OPTIXIR_FILES ${VIENNARAY_PIPELINE_DIR}/GeneralPipelineDisk.cu)
  generate_pipeline(pipeTriangle GENERATED_OPTIXIR_FILES ${VIENNARAY_PIPELINE_DIR}/GeneralPipelineTriangle.cu)
  generate_pipeline(pipeLine GENERATED_OPTIXIR_FILES ${VIENNARAY_PIPELINE_DIR}/GeneralPipelineLine.cu)
  generate_pipeline(pipeWrapper GENERATED_OPTIXIR_FILES ${VIENNAPS_PIPELINE_DIR}/CallableWrapper.cu)

  add_custom_target(Pipelines DEPENDS ${GENERATED_OPTIXIR_FILES})
  add_dependencies(ViennaPS_Examples Pipelines)
endif()

set(VIENNAPS_ARTIFACTS_DIRECTORY $<1:${PROJECT_BINARY_DIR}/artifacts>)

if(WIN32)
  viennacore_setup_embree_env(ViennaPS_Examples ${VIENNAPS_ARTIFACTS_DIRECTORY})
  viennacore_setup_vtk_env(ViennaPS_Examples ${VIENNAPS_ARTIFACTS_DIRECTORY})
  viennacore_setup_tbb_env(ViennaPS_Examples ${VIENNAPS_ARTIFACTS_DIRECTORY})
endif()

viennacore_add_subdirs(${CMAKE_CURRENT_LIST_DIR})
# add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/holeEtching)
# add_subdirectory(${CMAKE_CURRENT_LIST_DIR}/trenchDeposition)