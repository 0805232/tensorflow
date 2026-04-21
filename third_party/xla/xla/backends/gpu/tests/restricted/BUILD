load("@rules_cc//cc:cc_library.bzl", "cc_library")
load("//xla/tests:build_defs.bzl", "xla_test")
load(
    "//xla/tsl/platform:build_config_root.bzl",
    "tf_gpu_tests_tags",
)

package(
    default_applicable_licenses = ["//tensorflow:license"],
    default_visibility = [
        "//platforms/xla:__subpackages__",
        "//xla:__subpackages__",
    ],
    transitive_visibility = "//xla:restricted",
)

cc_library(
    name = "gpu_codegen_test",
    testonly = True,
    srcs = ["gpu_codegen_test.cc"],
    hdrs = ["gpu_codegen_test.h"],
    tags = tf_gpu_tests_tags(),
    deps = [
        "//xla:debug_options_flags",
        "//xla:shape_util",
        "//xla/hlo/testlib:filecheck",
        "//xla/hlo/testlib:verified_hlo_module",
        "//xla/service:executable",
        "//xla/service:gpu_plugin",
        "//xla/service:hlo_module_config",
        "//xla/service/gpu:gpu_executable",
        "//xla/stream_executor:platform_manager",
        "//xla/tests/restricted:llvm_irgen_test_base",
        "//xla/tsl/platform:statusor",
        "@com_google_absl//absl/status:statusor",
        "@com_google_absl//absl/strings",
        "@com_google_googletest//:gtest_for_library",
    ],
)

xla_test(
    name = "float_conversions_test",
    srcs = ["float_conversions_test.cc"],
    backends = ["gpu"],
    use_legacy_runtime = True,
    deps = [
        ":gpu_codegen_test",
        "//xla:error_spec",
        "@com_google_absl//absl/strings:str_format",
        "@com_google_absl//absl/strings:string_view",
        "@com_google_googletest//:gtest_main",
        "@tsl//tsl/platform:test",
    ],
)

xla_test(
    name = "gpu_alignment_test",
    srcs = ["gpu_alignment_test.cc"],
    backends = ["gpu"],
    use_legacy_runtime = True,
    deps = [
        ":gpu_codegen_test",
        "@com_google_googletest//:gtest_main",
        "@tsl//tsl/platform:test",
    ],
)

xla_test(
    name = "gpu_copy_alone_test",
    srcs = ["gpu_copy_alone_test.cc"],
    backends = ["gpu"],
    use_legacy_runtime = True,
    deps = [
        ":gpu_codegen_test",
        "//xla:error_spec",
        "//xla/hlo/testlib:verified_hlo_module",
        "@com_google_googletest//:gtest_main",
        "@tsl//tsl/platform:statusor",
    ],
)

xla_test(
    name = "gpu_dyn_shape_test",
    srcs = ["gpu_dyn_shape_test.cc"],
    backends = ["gpu"],
    use_legacy_runtime = True,
    deps = [
        ":gpu_codegen_test",
        "//xla:shape_util",
        "//xla:xla_data_proto_cc",
        "//xla/hlo/ir:hlo",
        "@com_google_googletest//:gtest_main",
    ],
)

xla_test(
    name = "gpu_ftz_test",
    srcs = ["gpu_ftz_test.cc"],
    backends = ["gpu"],
    use_legacy_runtime = True,
    deps = [
        ":gpu_codegen_test",
        "//xla:shape_util",
        "//xla:xla_data_proto_cc",
        "//xla/hlo/ir:hlo",
        "//xla/hlo/testlib:verified_hlo_module",
        "@com_google_googletest//:gtest_main",
    ],
)

xla_test(
    name = "gpu_fused_mha_test",
    srcs = ["gpu_fused_mha_test.cc"],
    backend_tags = {
        "b200": ["broken"],
        "h100": ["broken"],
    },
    backends = [
        "a100",
        "h100",
        "b200",
        "amdgpu_any",
    ],
    shard_count = 2,
    use_legacy_runtime = True,
    deps = [
        ":gpu_codegen_test",
        "//xla:array4d",
        "//xla:error_spec",
        "//xla:literal",
        "//xla:literal_util",
        "//xla:shape_util",
        "//xla:types",
        "//xla:xla_data_proto_cc",
        "//xla:xla_proto_cc",
        "//xla/hlo/ir:hlo",
        "//xla/hlo/testlib:test_helpers",
        "//xla/service/gpu:stream_executor_util",
        "//xla/stream_executor:device_description",
        "//xla/stream_executor:dnn",
        "//xla/stream_executor:semantic_version",
        "//xla/stream_executor:stream_executor_h",
        "//xla/stream_executor/cuda:cuda_compute_capability",
        "//xla/stream_executor/cuda:cuda_platform_id",
        "//xla/tsl/lib/core:status_test_util",
        "//xla/tsl/platform:statusor",
        "//xla/tsl/platform:test",
        "@com_google_absl//absl/strings",
        "@com_google_absl//absl/strings:string_view",
        "@com_google_googletest//:gtest_main",
    ],
)

xla_test(
    name = "gpu_int4_test",
    srcs = ["gpu_int4_test.cc"],
    backends = ["gpu"],
    use_legacy_runtime = True,
    deps = [
        ":gpu_codegen_test",
        "@com_google_absl//absl/strings:string_view",
        "@com_google_googletest//:gtest_main",
        "@tsl//tsl/platform:test",
    ],
)

xla_test(
    name = "gpu_ldg_test",
    srcs = ["gpu_ldg_test.cc"],
    backends = ["gpu"],
    use_legacy_runtime = True,
    deps = [
        ":gpu_codegen_test",
        "//xla:literal_util",
        "//xla:shape_util",
        "//xla:xla_data_proto_cc",
        "//xla/hlo/ir:hlo",
        "@com_google_googletest//:gtest_main",
        "@tsl//tsl/platform:test",
    ],
)

xla_test(
    name = "gpu_noalias_test",
    srcs = ["gpu_noalias_test.cc"],
    backends = ["gpu"],
    use_legacy_runtime = True,
    deps = [
        ":gpu_codegen_test",
        "//xla:shape_util",
        "//xla:xla_data_proto_cc",
        "//xla/hlo/ir:hlo",
        "@com_google_googletest//:gtest_main",
        "@tsl//tsl/platform:test",
    ],
)

xla_test(
    name = "kernel_launch_test",
    srcs = ["kernel_launch_test.cc"],
    backends = ["gpu"],
    use_legacy_runtime = True,
    deps = [
        ":gpu_codegen_test",
        "//xla:error_spec",
        "@com_google_googletest//:gtest_main",
    ],
)

xla_test(
    name = "pred_arithmetic_test",
    srcs = ["pred_arithmetic_test.cc"],
    backends = ["gpu"],
    use_legacy_runtime = True,
    deps = [
        ":gpu_codegen_test",
        "//xla:literal_util",
        "@com_google_googletest//:gtest_main",
        "@tsl//tsl/platform:statusor",
    ],
)

xla_test(
    name = "select_and_scatter_test",
    srcs = ["select_and_scatter_test.cc"],
    backends = ["gpu"],
    use_legacy_runtime = True,
    deps = [
        ":gpu_codegen_test",
        "//xla:error_spec",
        "@com_google_googletest//:gtest_main",
    ],
)

xla_test(
    name = "swap_conv_operands_test",
    srcs = ["swap_conv_operands_test.cc"],
    backends = ["gpu"],
    tags = ["cuda-only"],
    use_legacy_runtime = True,
    deps = [
        ":gpu_codegen_test",
        "//xla:error_spec",
        "//xla/stream_executor:device_description",
        "@com_google_absl//absl/status:statusor",
        "@com_google_googletest//:gtest_main",
        "@tsl//tsl/platform:statusor",
        "@tsl//tsl/platform:test",
    ],
)
