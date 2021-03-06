################################################################################
# Package: DetSensitive
################################################################################
gaudi_subdir(DetSensitive v1r0)

gaudi_depends_on_subdirs(GaudiKernel Detector/DetCommon)



include(${Geant4_USE_FILE})
include(DD4hep)

find_package(ROOT COMPONENTS MathCore GenVector Geom REQUIRED)

gaudi_add_library(DetSensitive
                  src/*.cpp
                  INCLUDE_DIRS Geant4 DD4hep ROOT DetCommon
                  LINK_LIBRARIES GaudiKernel DetCommon DD4hep ROOT
                  PUBLIC_HEADERS DetSensitive)
target_link_libraries(DetSensitive ${Geant4_LIBRARIES})

set(CMAKE_MODULE_PATH  ${CMAKE_MODULE_PATH}  ${DD4hep_ROOT}/cmake )

set(LIBRARY_OUTPUT_PATH ${CMAKE_LIBRARY_OUTPUT_DIRECTORY})
dd4hep_generate_rootmap(DetSensitive)

include(CTest)
gaudi_add_test(ExternalTrackerSD
               WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/Detector/DetSensitive/tests/
               FRAMEWORK tests/options/testSimpleTrackerSD.py)
gaudi_add_test(ExternalCalorimeterSD
               WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/Detector/DetSensitive/tests/
               FRAMEWORK tests/options/testSimpleCaloSD.py)
gaudi_add_test(FullParticleAbsorptionSD
               WORKING_DIRECTORY ${PROJECT_SOURCE_DIR}/Detector/DetSensitive/tests/
               FRAMEWORK tests/options/testDd4hepFullParticleAbsorptionSD.py)
