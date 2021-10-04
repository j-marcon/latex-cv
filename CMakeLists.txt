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
    set(CMAKE_INSTALL_PREFIX "${CV_SOURCE_DIR}/bin")
endif()
message(STATUS "CMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX}")

foreach(DOC IN LISTS DOCS)

    add_custom_target(${DOC}-pass1
            COMMAND ${LUALATEX_COMPILER} -draftmode -interaction=batchmode -file-line-error -recorder --output-directory=${BUILD_DIR} ${DOC}
            WORKING_DIRECTORY "${SOURCE_DIR}"
            BYPRODUCTS "${BUILD_DIR}/${DOC}.aux" "${BUILD_DIR}/${DOC}.bcf" "${BUILD_DIR}/${DOC}.fls" "${BUILD_DIR}/${DOC}.log" "${BUILD_DIR}/${DOC}.out" "${BUILD_DIR}/${DOC}.pdf" "${BUILD_DIR}/${DOC}.run.xml"
            COMMENT "${DOC}: LuaLaTeX pass 1")

    add_custom_target(${DOC}-bib
            COMMAND ${BIBER_COMPILER} --quiet ${BUILD_DIR}/${DOC}
            WORKING_DIRECTORY "${SOURCE_DIR}"
            BYPRODUCTS "${BUILD_DIR}/${DOC}.bbl" "${BUILD_DIR}/${DOC}.blg"
            COMMENT "${DOC}: Biber")
    add_dependencies(${DOC}-bib ${DOC}-pass1)

    add_custom_target(${DOC}-pass2
            COMMAND ${LUALATEX_COMPILER} -draftmode -interaction=batchmode -file-line-error -recorder --output-directory=${BUILD_DIR} ${DOC}
            WORKING_DIRECTORY "${SOURCE_DIR}"
            COMMENT "${DOC}: LuaLaTeX pass 2")
    add_dependencies(${DOC}-pass2 ${DOC}-bib)

    add_custom_target(${DOC} ALL
            COMMAND ${LUALATEX_COMPILER} -synctex=1 -interaction=batchmode -file-line-error -recorder --output-directory=${BUILD_DIR} ${DOC}
            WORKING_DIRECTORY "${SOURCE_DIR}"
            BYPRODUCTS "${BUILD_DIR}/${DOC}.synctex.gz"
            COMMENT "${DOC}: generate the PDF")
    add_dependencies(${DOC} ${DOC}-pass2)

    install(FILES ${BUILD_DIR}/${DOC}.pdf
            DESTINATION .)

endforeach()


