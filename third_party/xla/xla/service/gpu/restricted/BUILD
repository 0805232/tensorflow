load(
    "@local_config_rocm//rocm:build_defs.bzl",
    "if_rocm_is_configured",
)
load("//xla:xla.default.bzl", "xla_cc_test")
load("//xla/tests:build_defs.bzl", "xla_test")
load(
    "//xla/tsl/platform/default:cuda_build_defs.bzl",
    "if_cuda_is_configured",
)

package(
    default_applicable_licenses = ["//tensorflow:license"],
    default_visibility = [
        "//platforms/xla:__subpackages__",
        "//xla:__subpackages__",
    ],
    transitive_visibility = "//xla:restricted",
)

xla_test(
    name = "ptx_compilation_test",
    srcs = [
        "ptx_compilation_test.cc",
    ],
    backend_tags = {
        "b200": ["broken"],
    },
    backends = [
        "gpu",
    ],
    tags = [
        "cuda-only",
        "nomsan",
        "pjrt_migration_candidate",
    ],
    use_legacy_runtime = True,
    deps = [
        "//xla:xla_proto_cc",
        "//xla/hlo/ir:hlo",
        "//xla/service:executable",
        "//xla/service:hlo_module_config",
        "//xla/service/gpu:gpu_executable",
        "//xla/service/gpu:nvptx_compiler_impl",
        "//xla/stream_executor:device_description",
        "//xla/stream_executor/cuda:cuda_compute_capability",
        "//xla/stream_executor/cuda:nvjitlink_support",
        "//xla/stream_executor/cuda:ptx_compilation_method",
        "//xla/stream_executor/cuda:ptx_compiler_support",
        "//xla/stream_executor/cuda:ptx_linking_method",
        "//xla/tests:xla_internal_test_main",
        "//xla/tests/restricted:hlo_test_base",
        "//xla/tsl/platform:env",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/container:btree",
        "@com_google_absl//absl/container:flat_hash_map",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:status_matchers",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/types:span",
        "@com_google_googletest//:gtest",
        "@llvm-project//llvm:Object",
        "@llvm-project//llvm:Support",
        "@tsl//tsl/platform:path",
    ],
)

xla_cc_test(
    name = "gpu_aot_compilation_test",
    srcs = [
        "gpu_aot_compilation_test.cc",
    ],
    tags = [
        "gpu",
        "no_oss",
        "nomsan",
        "pjrt_migration_candidate",
        "requires-gpu-nvidia",
    ],
    deps = if_cuda_is_configured([
        "//xla/service/gpu:nvptx_compiler_impl",
        "//xla/stream_executor:cuda_platform",
    ]) + if_rocm_is_configured([
        "//xla/service/gpu:amdgpu_compiler_impl",
    ]) + [
        "//xla:literal",
        "//xla:literal_util",
        "//xla/backends/gpu/codegen/triton:support",
        "//xla/hlo/ir:hlo",
        "//xla/service:compiler",
        "//xla/service:executable",
        "//xla/service:gpu_plugin",
        "//xla/service:gpu_topology",
        "//xla/service:hlo_runner_interface",
        "//xla/service:platform_util",
        "//xla/service/gpu:gpu_transfer_manager",
        "//xla/stream_executor:platform",
        "//xla/stream_executor:platform_manager",
        "//xla/stream_executor:stream_executor_h",
        "//xla/tests:literal_test_util",
        "//xla/tests:xla_internal_test_main",
        "//xla/tests/restricted:hlo_test_base",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_googletest//:gtest",
        "@llvm-project//llvm:Support",
        "@llvm-project//mlir:IR",
        "@tsl//tsl/platform:statusor",
    ],
)

xla_test(
    name = "gpu_hlo_schedule_test",
    srcs = [
        "gpu_hlo_schedule_test.cc",
    ],
    backends = ["gpu"],
    use_legacy_runtime = True,
    deps = [
        "//xla:shape_util",
        "//xla/hlo/analysis:hlo_ordering",
        "//xla/hlo/ir:hlo",
        "//xla/hlo/testlib:filecheck",
        "//xla/hlo/testlib:verified_hlo_module",
        "//xla/hlo/utils:hlo_query",
        "//xla/service:backend",
        "//xla/service:hlo_module_config",
        "//xla/service/gpu:alias_info",
        "//xla/service/gpu:gpu_compiler",
        "//xla/service/gpu:gpu_hlo_schedule",
        "//xla/stream_executor:device_description",
        "//xla/tests:test_utils",
        "//xla/tests:xla_internal_test_main",
        "//xla/tests/restricted:hlo_test_base_legacy",
        "//xla/tsl/lib/core:status_test_util",
        "//xla/tsl/platform:logging",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/algorithm:container",
        "@com_google_absl//absl/base:log_severity",
        "@com_google_absl//absl/log",
        "@com_google_absl//absl/log:scoped_mock_log",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/status:status_matchers",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:string_view",
        "@com_google_googletest//:gtest",
        "@com_google_protobuf//:protobuf",
        "@tsl//tsl/profiler/protobuf:profiled_instructions_proto_cc",
    ],
)

xla_test(
    name = "determinism_test",
    srcs = ["determinism_test.cc"],
    backends = ["gpu"],
    tags = [
        "manual",
        "notap",
    ],
    use_legacy_runtime = True,
    deps = [
        "//xla:literal",
        "//xla:xla_proto_cc",
        "//xla/backends/autotuner:backends_proto_cc",
        "//xla/backends/gpu/tests:gpu_codegen_test",
        "//xla/backends/gpu/tests/restricted:gpu_codegen_test",
        "//xla/hlo/ir:hlo",
        "//xla/hlo/testlib:filecheck",
        "//xla/service:backend",
        "//xla/service:platform_util",
        "//xla/service/gpu/autotuning:autotuner_cache",
        "//xla/stream_executor:device_description",
        "//xla/stream_executor:mock_stream_executor",
        "//xla/stream_executor:platform",
        "//xla/stream_executor:stream_executor_h",
        "//xla/tests:literal_test_util",
        "//xla/tests:test_utils",
        "//xla/tsl/lib/core:status_test_util",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/log",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_main",
    ],
)

xla_test(
    name = "intel_gpu_compiler_test",
    srcs = ["intel_gpu_compiler_test.cc"],
    backends = ["gpu"],
    tags = [
        "gpu",
        "manual",
        "nobuilder",
        "notap",
        "oneapi-only",
        "pjrt_migration_candidate",
    ],
    use_legacy_runtime = True,
    deps = [
        "//xla/service/gpu:intel_gpu_compiler",
        "//xla/stream_executor/sycl:sycl_platform_id",
        "@com_google_googletest//:gtest_main",
    ],
)
