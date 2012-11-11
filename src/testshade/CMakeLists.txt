# The 'testshade' executable
SET ( testshade_srcs testshade.cpp simplerend.cpp )
ADD_EXECUTABLE ( testshade ${testshade_srcs} testshademain.cpp )
LINK_ILMBASE ( testshade )
TARGET_LINK_LIBRARIES ( testshade oslexec oslcomp oslquery ${OPENIMAGEIO_LIBRARY} ${Boost_LIBRARIES} ${CMAKE_DL_LIBS})
INSTALL ( TARGETS testshade RUNTIME DESTINATION bin )

if (BUILDSTATIC)
	ADD_LIBRARY ( "libtestshade" STATIC ${testshade_srcs} )
	TARGET_LINK_LIBRARIES (libtestshade oslexec oslcomp oslquery ${OPENIMAGEIO_LIBRARY} ${Boost_LIBRARIES} ${CMAKE_DL_LIBS} ${LLVM_LIBRARY} )
	SET_TARGET_PROPERTIES (libtestshade PROPERTIES PREFIX "")

	INSTALL ( TARGETS libtestshade LIBRARY DESTINATION lib ARCHIVE DESTINATION lib)
else ()
	# The 'libtestshade' dynamic library
	ADD_LIBRARY ("libtestshade" SHARED ${testshade_srcs})
	TARGET_LINK_LIBRARIES (libtestshade oslexec oslcomp oslquery ${OPENIMAGEIO_LIBRARY} ${Boost_LIBRARIES} ${CMAKE_DL_LIBS} ${LLVM_LIBRARY} )
	SET_TARGET_PROPERTIES (libtestshade PROPERTIES PREFIX "")
	SET_TARGET_PROPERTIES(libtestshade PROPERTIES INSTALL_NAME_DIR @loader_path/../lib)

	INSTALL (TARGETS libtestshade
	#         RUNTIME DESTINATION "${BINDIR}" COMPONENT user
			 LIBRARY DESTINATION lib 
	#COMPONENT user
	#         ARCHIVE DESTINATION "${LIBDIR}" COMPONENT developer
	)
endif ()

# The 'testshade_dso' executable
ADD_EXECUTABLE ( testshade_dso testshade_dso.cpp )
LINK_ILMBASE ( testshade_dso )
TARGET_LINK_LIBRARIES ( testshade_dso ${OPENIMAGEIO_LIBRARY} ${CMAKE_DL_LIBS} ${Boost_LIBRARIES})
#oslexec oslcomp oslquery ${OPENIMAGEIO_LIBRARY} ${Boost_LIBRARIES} ${CMAKE_DL_LIBS} )
INSTALL ( TARGETS testshade_dso RUNTIME DESTINATION bin )

