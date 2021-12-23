cmake_minimum_required(VERSION 3.21)

project(CV VERSION 1.0
           DESCRIPTION "My curriculum vitae"
           HOMEPAGE_URL "https://github.com/j-marcon/cv"
           LANGUAGES NONE)

find_package(LATEX REQUIRED COMPONENTS LUALATEX BIBER)

set(SOURCE_DIR "${CV_SOURCE_DIR}/src")
set(BUILD_DIR "${CV_BINARY_DIR}")
set(DOCS "us" "eu" "academic")

if(CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set(CMAKE_INSTALL_PREFIX "${CV_SOURCE_DIR}/bin" CACHE PATH "..." FORCE)
endif()
message(STATUS "CMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}")

foreach(DOC IN LISTS DOCS)

    add_custom_command(OUTPUT "${DOC}.bcf"
            COMMAND "${LUALATEX_COMPILER}" "-draftmode" "-interaction=batchmode" "-file-line-error" "-recorder" "--output-directory=${BUILD_DIR}" "${DOC}"
            DEPENDS "${DOC}.tex"
            BYPRODUCTS "${DOC}.aux" "${DOC}.fls" "${DOC}.log" "${DOC}.out" "${DOC}.pdf" "${DOC}.run.xml"
            WORKING_DIRECTORY "${SOURCE_DIR}"
            COMMENT "${DOC}: LuaLaTeX pass 1")

    add_custom_command(OUTPUT "${DOC}.bbl"
            COMMAND "${BIBER_COMPILER}" "--quiet" "${BUILD_DIR}/${DOC}"
            DEPENDS "${DOC}.bcf"
            BYPRODUCTS "${DOC}.blg"
            WORKING_DIRECTORY "${SOURCE_DIR}"
            COMMENT "${DOC}: Biber")

    add_custom_command(OUTPUT "${DOC}.aux"
            COMMAND "${LUALATEX_COMPILER}" "-draftmode" "-interaction=batchmode" "-file-line-error" "-recorder" "--output-directory=${BUILD_DIR}" "${DOC}"
            DEPENDS "${DOC}.bbl"
            BYPRODUCTS "${DOC}.bcf" "${DOC}.aux" "${DOC}.fls" "${DOC}.log" "${DOC}.out" "${DOC}.pdf" "${DOC}.run.xml"
            WORKING_DIRECTORY "${SOURCE_DIR}"
            COMMENT "${DOC}: LuaLaTeX pass 2")

    add_custom_command(OUTPUT "${DOC}.pdf"
            COMMAND "${LUALATEX_COMPILER}" "-synctex=1" "-interaction=batchmode" "-file-line-error" "-recorder" "--output-directory=${BUILD_DIR}" "${DOC}"
            DEPENDS "${DOC}.aux"
            BYPRODUCTS "${DOC}.bcf" "${DOC}.aux" "${DOC}.fls" "${DOC}.log" "${DOC}.out" "${DOC}.pdf" "${DOC}.run.xml" "${BUILD_DIR}/${DOC}.synctex.gz"
            WORKING_DIRECTORY "${SOURCE_DIR}"
            COMMENT "${DOC}: generate the PDF")

    add_custom_target("${DOC}" ALL
            DEPENDS "${DOC}.pdf")

    install(FILES "${BUILD_DIR}/${DOC}.pdf"
            DESTINATION ".")

endforeach()


