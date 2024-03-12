project(esp-tflite-micro)

add_compile_definitions(TF_LITE_DISABLE_X86_NEON)

set(tflite_dir "${CMAKE_CURRENT_SOURCE_DIR}/tensorflow/lite")
set(signal_dir "${CMAKE_CURRENT_SOURCE_DIR}/signal")
set(tfmicro_dir "${tflite_dir}/micro")
set(tfmicro_frontend_dir "${tflite_dir}/experimental/microfrontend/lib")
set(tfmicro_kernels_dir "${tfmicro_dir}/kernels")

file(GLOB srcs_micro
          "${tfmicro_dir}/*.cc"
          "${tfmicro_dir}/*.c")

file(GLOB src_micro_frontend
          "${tfmicro_frontend_dir}/*.c"
          "${tfmicro_frontend_dir}/*.cc")

file(GLOB srcs_tflite_bridge
          "${tfmicro_dir}/tflite_bridge/*.c"
          "${tfmicro_dir}/tflite_bridge/*.cc")

file(GLOB srcs_kernels
          "${tfmicro_kernels_dir}/*.c"
          "${tfmicro_kernels_dir}/*.cc")

set(signal_srcs
          "${signal_dir}/micro/kernels/rfft.cc"
          "${signal_dir}/micro/kernels/window.cc"
          "${signal_dir}/src/kiss_fft_wrappers/kiss_fft_float.cc"
          "${signal_dir}/src/kiss_fft_wrappers/kiss_fft_int16.cc"
          "${signal_dir}/src/kiss_fft_wrappers/kiss_fft_int32.cc"
          "${signal_dir}/src/rfft_float.cc"
          "${signal_dir}/src/rfft_int16.cc"
          "${signal_dir}/src/rfft_int32.cc"
          "${signal_dir}/src/window.cc")

set(lib_srcs
          "${srcs_micro}"
          "${srcs_kernels}"
          "${srcs_tflite_bridge}"
          "${src_micro_frontend}"
          "${signal_srcs}"
          "${tflite_dir}/kernels/kernel_util.cc"
          "${tflite_dir}/micro/memory_planner/greedy_memory_planner.cc"
          "${tflite_dir}/micro/memory_planner/linear_memory_planner.cc"
          "${tflite_dir}/micro/arena_allocator/non_persistent_arena_buffer_allocator.cc"
          "${tflite_dir}/micro/arena_allocator/persistent_arena_buffer_allocator.cc"
          "${tflite_dir}/micro/arena_allocator/recording_single_arena_buffer_allocator.cc"
          "${tflite_dir}/micro/arena_allocator/single_arena_buffer_allocator.cc"
          "${tflite_dir}/core/c/common.cc"
          "${tflite_dir}/core/api/error_reporter.cc"
          "${tflite_dir}/core/api/flatbuffer_conversions.cc"
          "${tflite_dir}/core/api/tensor_utils.cc"
          "${tflite_dir}/kernels/internal/common.cc"
          "${tflite_dir}/kernels/internal/quantization_util.cc"
          "${tflite_dir}/kernels/internal/portable_tensor_utils.cc"
          "${tflite_dir}/kernels/internal/tensor_utils.cc"
          "${tflite_dir}/kernels/internal/tensor_ctypes.cc"
          "${tflite_dir}/kernels/internal/reference/portable_tensor_utils.cc"
          "${tflite_dir}/kernels/internal/reference/comparisons.cc"
          "${tflite_dir}/schema/schema_utils.cc"
          "${tflite_dir}/array.cc")

add_library(${PROJECT_NAME} ${lib_srcs})

set(lib_incl
  "${CMAKE_CURRENT_SOURCE_DIR}/."
  "${CMAKE_CURRENT_SOURCE_DIR}/third_party/gemmlowp"
  "${CMAKE_CURRENT_SOURCE_DIR}/third_party/flatbuffers/include"
  "${CMAKE_CURRENT_SOURCE_DIR}/third_party/ruy"
  "${CMAKE_CURRENT_SOURCE_DIR}/third_party/kissfft"
  "${CMAKE_CURRENT_SOURCE_DIR}/signal/micro/kernels"
  "${CMAKE_CURRENT_SOURCE_DIR}/signal/src"
  "${CMAKE_CURRENT_SOURCE_DIR}/signal/src/kiss_fft_wrappers")

target_include_directories(${PROJECT_NAME} PUBLIC ${lib_incl})
