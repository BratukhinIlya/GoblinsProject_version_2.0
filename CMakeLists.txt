project(sfml-game)
cmake_minimum_required(VERSION 2.8)

# Ищем статическую версию библиотек SFML и её зависимостей
set(SFML_STATIC_LIBRARIES TRUE)
find_package(Freetype REQUIRED)
find_package(JPEG REQUIRED)
find_package(SFML 2 COMPONENTS window graphics system REQUIRED)

# Функция add_sample добавляет подкаталог с примером и подключает SFML
# Имя подкаталога становится именем цели сборки (и исполняемого файла)
function(add_sample NAME)
    file(GLOB SAMPLE_SRC "${NAME}/*.cpp" "${NAME}/*.h")
    add_executable(${NAME} ${SAMPLE_SRC})
    target_include_directories(${NAME} PRIVATE ${SFML_INCLUDE_DIR} ${JPEG_INCLUDE_DIR} ${FREETYPE_INCLUDE_DIRS})
    target_compile_features(${NAME} PUBLIC cxx_std_17)
    target_compile_definitions(${NAME} PRIVATE SFML_STATIC)
    target_link_libraries(${NAME} ${SFML_LIBRARIES} ${SFML_DEPENDENCIES} ${JPEG_LIBRARIES} ${FREETYPE_LIBRARIES})
endfunction()

add_sample(main) 

# Выполняем копирование файлов в выходную директорию
file(COPY images DESTINATION ${CMAKE_BINARY_DIR})