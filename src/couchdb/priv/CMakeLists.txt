SET(COUCH_PRIV_PREFIX ${COUCH_LIB_DIR}/priv)
SET(COUCH_PRIV_LIB_PREFIX ${COUCH_PRIV_PREFIX}/lib)

CONFIGURE_FILE(${CMAKE_CURRENT_SOURCE_DIR}/stat_descriptions.cfg.in
               ${CMAKE_CURRENT_BINARY_DIR}/stat_descriptions.cfg)

INCLUDE_DIRECTORIES(BEFORE ${ICU_INCLUDE_DIR}
                           ${ERLANG_INCLUDE_PATH})

ADD_DEFINITIONS(-DXP_UNIX)

SET(COUCH_EJSON_COMPARE_SRC couch_ejson_compare/couch_ejson_compare.c
                            couch_ejson_compare/couch_raw_json_compare.c
                            couch_ejson_compare/couch_ejson_compare_utils.c)

ADD_LIBRARY(couch_ejson_compare MODULE ${COUCH_EJSON_COMPARE_SRC})
SET_TARGET_PROPERTIES(couch_ejson_compare PROPERTIES PREFIX "")
TARGET_LINK_LIBRARIES(couch_ejson_compare ${COUCHBASE_UNRESOLVED} ${ICU_LIBRARIES})

INSTALL(TARGETS couch_ejson_compare
        DESTINATION ${COUCH_PRIV_LIB_PREFIX})

INSTALL(FILES ${CMAKE_CURRENT_BINARY_DIR}/stat_descriptions.cfg
        DESTINATION ${COUCH_PRIV_PREFIX})
