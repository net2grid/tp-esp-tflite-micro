project(esp-tflite-micro)

add_compile_definitions(TF_LITE_DISABLE_X86_NEON)

set(tflite_dir "${CMAKE_CURRENT_SOURCE_DIR}/tensorflow/lite")
set(signal_dir "${CMAKE_CURRENT_SOURCE_DIR}/signal")
set(compiler_mlir_dir "${CMAKE_CURRENT_SOURCE_DIR}/tensorflow/compiler/mlir/")
set(tfmicro_dir "${tflite_dir}/micro")
set(tfmicro_frontend_dir "${tflite_dir}/experimental/microfrontend/lib")
set(tfmicro_kernels_dir "${tfmicro_dir}/kernels")
set(tfmicro_compiler_dir "${tfmicro_dir}/compiler")

set(srcs_micro
          "${tfmicro_dir}/debug_log.cc"
          "${tfmicro_dir}/flatbuffer_utils.cc"
          "${tfmicro_dir}/memory_helpers.cc"
          "${tfmicro_dir}/micro_allocation_info.cc"
          "${tfmicro_dir}/micro_allocator.cc"
          "${tfmicro_dir}/micro_context.cc"
          "${tfmicro_dir}/micro_interpreter_context.cc"
          "${tfmicro_dir}/micro_interpreter_graph.cc"
          "${tfmicro_dir}/micro_interpreter.cc"
          "${tfmicro_dir}/micro_log.cc"
          "${tfmicro_dir}/micro_op_resolver.cc"
          "${tfmicro_dir}/micro_profiler.cc"
          "${tfmicro_dir}/micro_resource_variable.cc"
          "${tfmicro_dir}/micro_time.cc"
          "${tfmicro_dir}/micro_utils.cc"
          "${tfmicro_dir}/recording_micro_allocator.cc"
          "${tfmicro_dir}/system_setup.cc")

file(GLOB src_micro_frontend
          "${tfmicro_frontend_dir}/*.c"
          "${tfmicro_frontend_dir}/*.cc")

file(GLOB srcs_tflite_bridge
          "${tfmicro_dir}/tflite_bridge/*.c"
          "${tfmicro_dir}/tflite_bridge/*.cc")

file(GLOB srcs_kernels
          "${tfmicro_kernels_dir}/*.c"
          "${tfmicro_kernels_dir}/*.cc")

file(GLOB signal_micro_kernels
          "${signal_dir}/micro/kernels/*.c"
          "${signal_dir}/micro/kernels/*.cc")

file(GLOB signal_src
          "${signal_dir}/src/*.c"
          "${signal_dir}/src/*.cc")

set(signal_srcs
          "${signal_micro_kernels}"
          "${signal_src}"
          "${signal_dir}/src/kiss_fft_wrappers/kiss_fft_float.cc"
          "${signal_dir}/src/kiss_fft_wrappers/kiss_fft_int16.cc"
          "${signal_dir}/src/kiss_fft_wrappers/kiss_fft_int32.cc")

set(lib_srcs
          "${srcs_micro}"
          "${srcs_kernels}"
          "${srcs_tflite_bridge}"
          "${src_micro_frontend}"
          "${signal_srcs}"
          "${tflite_dir}/array.cc"
          "${tflite_dir}/kernels/kernel_util.cc"
          "${tflite_dir}/micro/memory_planner/greedy_memory_planner.cc"
          "${tflite_dir}/micro/memory_planner/linear_memory_planner.cc"
          "${tflite_dir}/micro/arena_allocator/non_persistent_arena_buffer_allocator.cc"
          "${tflite_dir}/micro/arena_allocator/persistent_arena_buffer_allocator.cc"
          "${tflite_dir}/micro/arena_allocator/recording_single_arena_buffer_allocator.cc"
          "${tflite_dir}/micro/arena_allocator/single_arena_buffer_allocator.cc"
          "${tflite_dir}/core/c/common.cc"
          "${tflite_dir}/core/api/flatbuffer_conversions.cc"
          "${tflite_dir}/core/api/tensor_utils.cc"
          "${tflite_dir}/kernels/internal/common.cc"
          "${tflite_dir}/kernels/internal/quantization_util.cc"
          "${tflite_dir}/kernels/internal/portable_tensor_utils.cc"
          "${tflite_dir}/kernels/internal/tensor_utils.cc"
          "${tflite_dir}/kernels/internal/tensor_ctypes.cc"
          "${tflite_dir}/kernels/internal/reference/portable_tensor_utils.cc"
          "${tflite_dir}/kernels/internal/reference/comparisons.cc"
          "${compiler_mlir_dir}/lite/core/api/error_reporter.cc"
          "${compiler_mlir_dir}/lite/schema/schema_utils.cc")

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

