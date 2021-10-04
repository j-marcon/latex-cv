cmake_minimum_required(VERSION 3.21)

project(CV VERSION 1.0
           DESCRIPTION "My curriculum vitae"
           HOMEPAGE_URL "https://github.com/j-marcon/cv"
           LANGUAGES NONE)

find_package(LATEX COMPONENTS LUALATEX BIBER)

set(SOURCE_DIR "${CV_SOURCE_DIR}/src")
set(DOCS "us" "eu" "academic")

if(NOT (LATEX_FOUND AND LATEX_LUALATEX_FOUND AND LATEX_BIBER_FOUND))
    message(FATAL_ERROR "Cannot proceed without a LaTeX distribution with LuaLaTeX and Biber")
endif()

foreach(DOC IN LISTS DOCS)
    add_custom_target(${DOC}-pass1
            COMMAND ${LUALATEX_COMPILER} -draftmode -interaction=batchmode -file-line-error -recorder ${DOC}
            WORKING_DIRECTORY "${SOURCE_DIR}"
            BYPRODUCTS "${SOURCE_DIR}/${DOC}.aux" "${SOURCE_DIR}/${DOC}.bcf" "${SOURCE_DIR}/${DOC}.fls" "${SOURCE_DIR}/${DOC}.log" "${SOURCE_DIR}/${DOC}.out" "${SOURCE_DIR}/${DOC}.pdf" "${SOURCE_DIR}/${DOC}.run.xml"
            COMMENT "${DOC}: LuaLaTeX pass 1")

    add_custom_target(${DOC}-bib
            COMMAND ${BIBER_COMPILER} --quiet ${DOC}
            WORKING_DIRECTORY "${SOURCE_DIR}"
            BYPRODUCTS "${SOURCE_DIR}/${DOC}.bbl" "${SOURCE_DIR}/${DOC}.blg"
            COMMENT "${DOC}: Biber")
    add_dependencies(${DOC}-bib ${DOC}-pass1)

    add_custom_target(${DOC}-pass2
            COMMAND ${LUALATEX_COMPILER} -draftmode -interaction=batchmode -file-line-error -recorder ${DOC}
            WORKING_DIRECTORY "${SOURCE_DIR}"
            COMMENT "${DOC}: LuaLaTeX pass 2")
    add_dependencies(${DOC}-pass2 ${DOC}-bib)

    add_custom_target(${DOC} ALL
            COMMAND ${LUALATEX_COMPILER} -synctex=1 -interaction=batchmode -file-line-error -recorder ${DOC}
            WORKING_DIRECTORY "${SOURCE_DIR}"
            BYPRODUCTS "${SOURCE_DIR}/${DOC}.synctex.gz"
            COMMENT "${DOC}: generate the PDF")
    add_dependencies(${DOC} ${DOC}-pass2)
endforeach()


