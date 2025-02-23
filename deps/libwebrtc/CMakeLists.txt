set(SOURCE_FILES
	api/test/create_frame_generator.cc
	media/base/fake_frame_source.cc
	pc/test/fake_audio_capture_module.cc
	rtc_base/task_queue_for_test.cc
	test/frame_generator.cc
	test/frame_generator_capturer.cc
	test/frame_utils.cc
	test/test_video_capturer.cc
	test/testsupport/ivf_video_frame_generator.cc
	test/testsupport/file_utils.cc
	test/testsupport/file_utils_override.cc
)

if(APPLE)
	set(SOURCE_FILES ${SOURCE_FILES}
		test/testsupport/ios_file_utils.mm
		test/testsupport/mac_file_utils.mm
	)
endif(APPLE)

# Create target.
add_library(webrtc_broadcaster STATIC ${SOURCE_FILES})

target_link_libraries(webrtc_broadcaster libwebrtc)

# Private (implementation) header files.
# target_include_directories(webrtc_broadcaster PRIVATE ${PROJECT_SOURCE_DIR}/deps/libwebrtc)

# Public (interface) headers from dependencies.
# target_include_directories(webrtc_broadcaster PUBLIC
# 	"${LIBWEBRTC_INCLUDE_PATH}"
# 	"${LIBWEBRTC_INCLUDE_PATH}/third_party/abseil-cpp"
# )

# Compile definitions for libwebrtc.
target_compile_definitions(webrtc_broadcaster PUBLIC
	$<$<NOT:$<PLATFORM_ID:Windows>>:WEBRTC_POSIX>
	$<$<PLATFORM_ID:Windows>:WEBRTC_WIN>
	$<$<PLATFORM_ID:Darwin>:WEBRTC_MAC>
)

