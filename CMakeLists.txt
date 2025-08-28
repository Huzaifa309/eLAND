project(eLAND VERSION 1.0.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(PkgConfig REQUIRED)
pkg_check_modules(PQXX REQUIRED libpqxx)

# Find the wrapper package (installed to /usr/local or a custom prefix)
find_package(aeronWrapper CONFIG REQUIRED)

file(GLOB SOURCE_FILES "src/*.cpp")
add_executable(${PROJECT_NAME} ${SOURCE_FILES})

target_include_directories(${PROJECT_NAME} PRIVATE
    ${CMAKE_SOURCE_DIR}/include
    ${PQXX_INCLUDE_DIRS}
)

target_link_libraries(${PROJECT_NAME} PRIVATE
    aeronWrapper::aeronWrapper
    /usr/local/lib/libquillLogger.a
    /usr/local/lib/libDbFactory.a
    ${PQXX_LIBRARIES}
)
# Include Aeron headers and project includes
target_include_directories(${PROJECT_NAME} PRIVATE 
    ${CMAKE_SOURCE_DIR}/include
    /usr/local/include
)

target_link_libraries(${PROJECT_NAME} PRIVATE
    /usr/local/lib/libaeronWrapper.a
    /usr/local/lib/libquillLogger.a
    /usr/local/lib/libDbFactory.a
    aeron_client
    aeron_driver
    ${PQXX_LIBRARIES}
)
