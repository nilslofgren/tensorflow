# Implementation of a prototype TF distributed computation library.

package(
    default_visibility = [
        "//tensorflow:internal",
    ],
)

licenses(["notice"])  # Apache 2.0

exports_files(["LICENSE"])

load("//tensorflow:tensorflow.bzl", "py_test")
load("//tensorflow:tensorflow.bzl", "cuda_py_test")

# TODO(priyag): Figure out testonly issues that are preventing us from
# including our tests in pip for now.

py_library(
    name = "values",
    srcs = ["values.py"],
    visibility = ["//tensorflow:internal"],
    deps = [
        ":input_ops",
        ":prefetching_ops_v2",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:control_flow_ops",
        "//tensorflow/python:device_util",
        "//tensorflow/python:distribute",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:training",
        "//tensorflow/python:util",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/training/checkpointable:base",
        "@six_archive//:six",
    ],
)

cuda_py_test(
    name = "values_test",
    srcs = ["values_test.py"],
    additional_deps = [
        ":mirrored_strategy",
        ":multi_worker_test_base",
        ":values",
        "//tensorflow/core:protos_all_py",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python:errors",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:constant_op",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:framework_test_lib",
        "//tensorflow/python:training",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python/eager:context",
        "//tensorflow/python:device_util",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/estimator:estimator_py",
    ],
    tags = [
        "no_pip",
    ],
)

py_library(
    name = "mirrored_strategy",
    srcs = ["mirrored_strategy.py"],
    visibility = ["//tensorflow:internal"],
    deps = [
        ":cross_tower_ops",
        ":shared_variable_creator",
        ":values",
        "//tensorflow/core:protos_all_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:constant_op",
        "//tensorflow/python:control_flow_ops",
        "//tensorflow/python:device",
        "//tensorflow/python:device_util",
        "//tensorflow/python:distribute",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:pywrap_tensorflow",
        "//tensorflow/python:training",
        "//tensorflow/python:util",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python:variables",
        "//tensorflow/python/distribute:multi_worker_util",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:tape",
    ],
)

py_library(
    name = "parameter_server_strategy",
    srcs = ["parameter_server_strategy.py"],
    visibility = ["//tensorflow:internal"],
    deps = [
        ":cross_tower_ops",
        ":mirrored_strategy",
        ":values",
        "//tensorflow/core:protos_all_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:resource_variable_ops",
        "//tensorflow/python:training",
        "//tensorflow/python:util",
        "//tensorflow/python/distribute:multi_worker_util",
        "//tensorflow/python/eager:context",
    ],
)

cuda_py_test(
    name = "parameter_server_strategy_test",
    srcs = ["parameter_server_strategy_test.py"],
    additional_deps = [
        ":combinations",
        ":multi_worker_test_base",
        ":parameter_server_strategy",
        ":values",
        "@absl_py//absl/testing:parameterized",
        "//tensorflow/core:protos_all_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:constant_op",
        "//tensorflow/python:control_flow_ops",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:gradients",
        "//tensorflow/python:layers",
        "//tensorflow/python:session",
        "//tensorflow/python:training",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python:variables",
        "//tensorflow/python/distribute:multi_worker_util",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/estimator:estimator_py",
    ],
    tags = [
        "multi_and_single_gpu",
        "no_pip",
    ],
)

py_library(
    name = "one_device_strategy",
    srcs = ["one_device_strategy.py"],
    visibility = ["//tensorflow:internal"],
    deps = [
        ":values",
        "//tensorflow/contrib/eager/python:datasets",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:distribute",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:math_ops",
        "//tensorflow/python/eager:context",
        "@six_archive//:six",
    ],
)

py_library(
    name = "collective_all_reduce_strategy",
    srcs = ["collective_all_reduce_strategy.py"],
    visibility = ["//tensorflow:internal"],
    deps = [
        ":cross_tower_ops",
        ":cross_tower_utils",
        ":mirrored_strategy",
        ":values",
        "//tensorflow/core:protos_all_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:collective_ops",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:training",
        "//tensorflow/python/distribute:multi_worker_util",
        "//tensorflow/python/eager:context",
    ],
)

py_library(
    name = "strategy_test_lib",
    testonly = 1,
    srcs = ["strategy_test_lib.py"],
    srcs_version = "PY2AND3",
    tags = [
        "no_pip",
    ],
    deps = [
        "//tensorflow/core:protos_all_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:constant_op",
        "//tensorflow/python:distribute",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:layers",
        "//tensorflow/python:training",
        "//tensorflow/python:variables",
        "//tensorflow/python/eager:backprop",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:test",
    ],
)

py_library(
    name = "combinations",
    testonly = 1,
    srcs = ["combinations.py"],
    srcs_version = "PY2AND3",
    tags = [
        "no_pip",
    ],
    deps = [
        ":mirrored_strategy",
        ":one_device_strategy",
        ":tpu_strategy",
        "//tensorflow/contrib/cluster_resolver:cluster_resolver_pip",
        "//tensorflow/contrib/optimizer_v2:training",
        "//tensorflow/python:distribute",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:training",
        "//tensorflow/python:util",
        "//tensorflow/python/eager:context",
        "@absl_py//absl/testing:parameterized",
    ],
)

py_test(
    name = "combinations_test",
    srcs = ["combinations_test.py"],
    tags = [
        "no_pip",
    ],
    deps = [
        ":combinations",
        "//tensorflow/python/eager:test",
    ],
)

py_test(
    name = "mirrored_strategy_test",
    srcs = ["mirrored_strategy_test.py"],
    srcs_version = "PY2AND3",
    tags = [
        "no_pip",
    ],
    deps = [
        ":mirrored_strategy",
        ":multi_worker_test_base",
        ":strategy_test_lib",
        "//tensorflow/python:constant_op",
        "//tensorflow/python:distribute",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:framework_test_lib",
        "//tensorflow/python:training",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:test",
    ],
)

py_test(
    name = "one_device_strategy_test",
    srcs = ["one_device_strategy_test.py"],
    srcs_version = "PY2AND3",
    tags = [
        "no_pip",
    ],
    deps = [
        ":one_device_strategy",
        ":strategy_test_lib",
        "//tensorflow/python:framework_test_lib",
        "//tensorflow/python/eager:test",
    ],
)

cuda_py_test(
    name = "mirrored_strategy_multigpu_test",
    srcs = ["mirrored_strategy_multigpu_test.py"],
    additional_deps = [
        ":mirrored_strategy",
        ":multi_worker_test_base",
        ":values",
        ":strategy_test_lib",
        "//tensorflow/python:distribute",
        "//tensorflow/core:protos_all_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:constant_op",
        "//tensorflow/python:layers",
        "//tensorflow/python:state_ops",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python:framework_test_lib",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:test",
    ],
    tags = [
        "guitar",
        "no_pip",
        "multi_and_single_gpu",
        # Do not perform the extra analysis on this test, because it is already
        # performed for the `:mirrored_strategy_test` target.
        "no_oss",
        "noasan",
        "notap",
        "notsan",
    ],
)

py_library(
    name = "multi_worker_test_base",
    testonly = 1,
    srcs = ["multi_worker_test_base.py"],
    srcs_version = "PY2AND3",
    tags = [
        "no_pip",
    ],
    deps = [
        "//tensorflow/core:protos_all_py",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:distributed_framework_test_lib",
        "//tensorflow/python:session",
        "//tensorflow/python/estimator:estimator_py",
        "//third_party/py/numpy",
    ],
)

py_library(
    name = "step_fn",
    srcs = ["step_fn.py"],
    visibility = ["//tensorflow:internal"],
    deps = [
        "//tensorflow/python:training",
        "//tensorflow/python/eager:backprop",
    ],
)

py_library(
    name = "tpu_strategy",
    srcs = ["tpu_strategy.py"],
    visibility = ["//tensorflow:internal"],
    deps = [
        ":one_device_strategy",
        ":values",
        "//tensorflow/contrib/tpu:tpu_lib",
        "//tensorflow/python:constant_op",
        "//tensorflow/python:control_flow_ops",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:util",
    ],
)

cuda_py_test(
    name = "collective_all_reduce_strategy_test",
    srcs = ["collective_all_reduce_strategy_test.py"],
    additional_deps = [
        ":collective_all_reduce_strategy",
        ":combinations",
        ":cross_tower_utils",
        ":multi_worker_test_base",
        ":strategy_test_lib",
        "@absl_py//absl/testing:parameterized",
        "//third_party/py/numpy",
        "//tensorflow/core:protos_all_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:constant_op",
        "//tensorflow/python:dtypes",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:gradients",
        "//tensorflow/python:init_ops",
        "//tensorflow/python:layers",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python:variables",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/estimator:estimator_py",
    ],
    tags = [
        "multi_and_single_gpu",
        "no_pip",
    ],
)

py_library(
    name = "minimize_loss_test_lib",
    testonly = 1,
    srcs = ["minimize_loss_test.py"],
    deps = [
        ":combinations",
        ":mirrored_strategy",
        ":single_loss_example",
        "//tensorflow/contrib/tpu:tpu_lib",
        "//tensorflow/python:control_flow_ops",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:math_ops",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python:variables",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/ops/losses",
        "//third_party/py/numpy",
        "@absl_py//absl/testing:parameterized",
    ],
)

cuda_py_test(
    name = "minimize_loss_test",
    srcs = ["minimize_loss_test.py"],
    additional_deps = [
        ":minimize_loss_test_lib",
    ],
    tags = [
        "multi_and_single_gpu",
        "no_pip",
    ],
)

cuda_py_test(
    name = "optimizer_v2_test",
    srcs = ["optimizer_v2_test.py"],
    additional_deps = [
        ":combinations",
        ":single_loss_example",
        "@absl_py//absl/testing:parameterized",
        "//third_party/py/numpy",
        "//tensorflow/python:control_flow_ops",
        "//tensorflow/python:variables",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:test",
    ],
    tags = [
        "multi_and_single_gpu",
        "no_pip",
    ],
)

cuda_py_test(
    name = "estimator_integration_test",
    srcs = ["estimator_integration_test.py"],
    additional_deps = [
        ":combinations",
        "@absl_py//absl/testing:parameterized",
        "//third_party/py/numpy",
        "//tensorflow/contrib/optimizer_v2:training",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/estimator:estimator_py",
        "//tensorflow/python/feature_column",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:platform",
        "//tensorflow/python:summary",
    ],
    tags = [
        "multi_and_single_gpu",
        "no_pip",
    ],
)

cuda_py_test(
    name = "estimator_training_test",
    size = "large",
    srcs = ["estimator_training_test.py"],
    additional_deps = [
        ":combinations",
        ":mirrored_strategy",
        ":multi_worker_test_base",
        ":parameter_server_strategy",
        "//third_party/py/numpy",
        "//tensorflow/contrib/optimizer_v2:training",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/distribute",
        "//tensorflow/python/eager:test",
        "//tensorflow/python/estimator:estimator_py",
        "//tensorflow/python/feature_column",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:platform",
        "//tensorflow/python:summary",
    ],
    tags = [
        "manual",
        "multi_and_single_gpu",
        "no_pip",
        "nogpu",
        "notap",
    ],
)

py_library(
    name = "single_loss_example",
    srcs = ["single_loss_example.py"],
    deps = [
        ":step_fn",
        "//tensorflow/contrib/data/python/ops:batching",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:constant_op",
        "//tensorflow/python:layers",
        "//tensorflow/python:math_ops",
        "//tensorflow/python/data/ops:dataset_ops",
    ],
)

py_library(
    name = "step_fn_test_lib",
    testonly = 1,
    srcs = ["step_fn_test.py"],
    deps = [
        ":combinations",
        ":single_loss_example",
        "//tensorflow/contrib/tpu:tpu_lib",
        "//tensorflow/python:variables",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:test",
        "//third_party/py/numpy",
        "@absl_py//absl/testing:parameterized",
    ],
)

cuda_py_test(
    name = "step_fn_test",
    srcs = ["step_fn_test.py"],
    additional_deps = [
        ":step_fn_test_lib",
    ],
    tags = [
        "multi_and_single_gpu",
        "no_pip",
    ],
)

py_library(
    name = "monitor",
    srcs = ["monitor.py"],
    visibility = ["//tensorflow:internal"],
    deps = [
        "//tensorflow/python:variables",
        "//tensorflow/python/eager:context",
    ],
)

cuda_py_test(
    name = "monitor_test",
    srcs = ["monitor_test.py"],
    additional_deps = [
        ":combinations",
        ":monitor",
        ":one_device_strategy",
        ":single_loss_example",
        "@absl_py//absl/testing:parameterized",
        "//third_party/py/numpy",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:test",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:training",
    ],
    tags = [
        "multi_and_single_gpu",
        "no_pip",
    ],
)

py_library(
    name = "shared_variable_creator",
    srcs = ["shared_variable_creator.py"],
    visibility = ["//tensorflow:internal"],
)

py_test(
    name = "shared_variable_creator_test",
    srcs = ["shared_variable_creator_test.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":shared_variable_creator",
        "//tensorflow/python:framework_test_lib",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python/eager:test",
    ],
)

py_library(
    name = "cross_tower_utils",
    srcs = ["cross_tower_utils.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":values",
        "//tensorflow/contrib/all_reduce:all_reduce_py",
        "//tensorflow/contrib/nccl:nccl_py",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:collective_ops",
        "//tensorflow/python:device",
        "//tensorflow/python:dtypes",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:gradients",
        "//tensorflow/python:math_ops",
    ],
)

cuda_py_test(
    name = "cross_tower_utils_test",
    srcs = ["cross_tower_utils_test.py"],
    additional_deps = [
        ":combinations",
        ":cross_tower_utils",
        "@absl_py//absl/testing:parameterized",
        "//tensorflow/python:constant_op",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:math_ops",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:test",
    ],
    tags = [
        "no_pip",
    ],
)

py_library(
    name = "cross_tower_ops",
    srcs = ["cross_tower_ops.py"],
    srcs_version = "PY2AND3",
    deps = [
        ":cross_tower_utils",
        ":values",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:device_lib",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:math_ops",
        "//tensorflow/python:platform",
        "//tensorflow/python:resource_variable_ops",
        "//tensorflow/python:training",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python/eager:context",
        "@six_archive//:six",
    ],
)

cuda_py_test(
    name = "cross_tower_ops_test",
    size = "large",
    srcs = ["cross_tower_ops_test.py"],
    additional_deps = [
        ":combinations",
        ":cross_tower_ops",
        ":multi_worker_test_base",
        ":mirrored_strategy",
        ":values",
        "@absl_py//absl/testing:parameterized",
        "//tensorflow/python:array_ops",
        "//tensorflow/python:constant_op",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:math_ops",
        "//tensorflow/python/eager:context",
        "//tensorflow/python/eager:test",
    ],
    tags = [
        "multi_and_single_gpu",
        "no_pip",
    ],
)

py_library(
    name = "prefetching_ops_v2",
    srcs = ["prefetching_ops_v2.py"],
    deps = [
        "//tensorflow/contrib/data/python/ops:contrib_op_loader",
        "//tensorflow/contrib/data/python/ops:prefetching_ops",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/data/util:nest",
        "//tensorflow/python/data/util:sparse",
    ],
)

cuda_py_test(
    name = "prefetching_ops_v2_test",
    srcs = ["prefetching_ops_v2_test.py"],
    additional_deps = [
        ":prefetching_ops_v2",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:framework_test_lib",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/data/ops:iterator_ops",
    ],
)

py_library(
    name = "input_ops",
    srcs = ["input_ops.py"],
    visibility = ["//tensorflow:internal"],
    deps = [
        "//tensorflow/python:framework_ops",
        "//tensorflow/python/data/util:nest",
    ],
)

cuda_py_test(
    name = "input_ops_test",
    srcs = ["input_ops_test.py"],
    additional_deps = [
        ":input_ops",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/contrib/data/python/ops:batching",
        "//tensorflow/contrib/data/python/ops:interleave_ops",
        "//tensorflow/python:errors",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:framework_test_lib",
        "//tensorflow/python:io_ops",
        "//tensorflow/python/data/ops:readers",
        "//tensorflow/python:util",
    ],
    tags = [
        "no_pip",
    ],
)

py_library(
    name = "keras_test_lib",
    testonly = 1,
    srcs = ["keras_test.py"],
    deps = [
        ":combinations",
        "//tensorflow/contrib/distribute/python:mirrored_strategy",
        "//tensorflow/contrib/distribute/python:tpu_strategy",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:training",
        "//tensorflow/python/estimator:estimator_py",
        "//tensorflow/python/keras",
        "//third_party/py/numpy",
        "@absl_py//absl/testing:parameterized",
    ],
)

cuda_py_test(
    name = "keras_test",
    srcs = ["keras_test.py"],
    additional_deps = [
        ":keras_test_lib",
    ],
    tags = [
        "multi_and_single_gpu",
        "no_pip",
        "no_windows_gpu",
        "notsan",
    ],
)

cuda_py_test(
    name = "metrics_v1_test",
    srcs = ["metrics_v1_test.py"],
    additional_deps = [
        ":combinations",
        "@absl_py//absl/testing:parameterized",
        "//tensorflow/contrib/data/python/ops:batching",
        "//tensorflow/python:math_ops",
        "//tensorflow/python:metrics",
        "//tensorflow/python:variables",
        "//tensorflow/python/data/ops:dataset_ops",
        "//tensorflow/python/eager:test",
    ],
    tags = [
        "multi_and_single_gpu",
        "no_pip",
    ],
)

cuda_py_test(
    name = "warm_starting_util_test",
    size = "medium",
    srcs = ["warm_starting_util_test.py"],
    additional_deps = [
        ":combinations",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:training",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python:variables",
    ],
    tags = [
        "multi_and_single_gpu",
        "no_pip",
    ],
)

cuda_py_test(
    name = "checkpoint_utils_test",
    size = "medium",
    srcs = ["checkpoint_utils_test.py"],
    additional_deps = [
        ":combinations",
        "//tensorflow/python:client_testlib",
        "//tensorflow/python:checkpoint_utils_test",
        "//tensorflow/python:framework_ops",
        "//tensorflow/python:training",
        "//tensorflow/python:variable_scope",
        "//tensorflow/python:variables",
    ],
    tags = [
        "multi_and_single_gpu",
        "no_pip",
    ],
)
