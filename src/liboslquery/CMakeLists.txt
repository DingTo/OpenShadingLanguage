SET ( liboslquery_srcs oslquery.cpp ../liboslexec/osoreader.cpp
      ../liboslexec/typespec.cpp )
FILE ( GLOB compiler_headers "../liboslexec/*.h" )
INCLUDE_DIRECTORIES ( ../liboslexec )

FLEX_BISON ( ../liboslexec/osolex.l ../liboslexec/osogram.y oso liboslquery_srcs compiler_headers )

if (BUILDSTATIC)
	ADD_LIBRARY ( oslquery STATIC ${liboslquery_srcs} )
	TARGET_LINK_LIBRARIES ( oslquery ${OPENIMAGEIO_LIBRARY} ${Boost_LIBRARIES} )
	LINK_ILMBASE ( oslquery )

	INSTALL ( TARGETS oslquery LIBRARY DESTINATION lib ARCHIVE DESTINATION lib)
else ()
	ADD_LIBRARY ( oslquery SHARED ${liboslquery_srcs} )
	TARGET_LINK_LIBRARIES ( oslquery ${OPENIMAGEIO_LIBRARY} ${Boost_LIBRARIES} )
	LINK_ILMBASE ( oslquery )
	SET_TARGET_PROPERTIES(oslquery PROPERTIES INSTALL_NAME_DIR @loader_path/../lib)

	INSTALL ( TARGETS oslquery LIBRARY DESTINATION lib )
endif ()
