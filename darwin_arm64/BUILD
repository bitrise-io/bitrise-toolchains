# Copyright 2016 The Bazel Authors. All rights reserved.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# This becomes the BUILD file for @local_config_cc// under non-BSD unixes.

load(":cc_toolchain_config.bzl", "cc_toolchain_config")
load("@rules_cc//cc:defs.bzl", "cc_toolchain", "cc_toolchain_suite")

package(default_visibility = ["//visibility:public"])

licenses(["notice"])  # Apache 2.0

cc_library(name = "empty_lib")

# Label flag for extra libraries to be linked into every binary.
# TODO(bazel-team): Support passing flag multiple times to build a list.
label_flag(
    name = "link_extra_libs",
    build_setting_default = ":empty_lib",
)

# The final extra library to be linked into every binary target. This collects
# the above flag, but may also include more libraries depending on config.
cc_library(
    name = "link_extra_lib",
    deps = [
        ":link_extra_libs",
    ],
)

cc_library(
    name = "malloc",
)

filegroup(
    name = "empty",
    srcs = [],
)

filegroup(
    name = "cc_wrapper",
    srcs = ["cc_wrapper.sh"],
)

filegroup(
    name = "validate_static_library",
    srcs = ["validate_static_library.sh"],
)

filegroup(
    name = "compiler_deps",
    srcs = glob(["extra_tools/**"], allow_empty = True) + [":builtin_include_directory_paths",
    ":cc_wrapper",
    ":validate_static_library"],
)

# This is the entry point for --crosstool_top.  Toolchains are found
# by lopping off the name of --crosstool_top and searching for
# the "${CPU}" entry in the toolchains attribute.
cc_toolchain_suite(
    name = "toolchain",
    toolchains = {
        "darwin_arm64|clang": ":cc-compiler-darwin_arm64",
        "darwin_arm64": ":cc-compiler-darwin_arm64",
    },
)

cc_toolchain(
    name = "cc-compiler-darwin_arm64",
    toolchain_identifier = "local",
    toolchain_config = ":local",
    all_files = ":compiler_deps",
    ar_files = ":compiler_deps",
    as_files = ":compiler_deps",
    compiler_files = ":compiler_deps",
    dwp_files = ":empty",
    linker_files = ":compiler_deps",
    objcopy_files = ":empty",
    strip_files = ":empty",
    supports_header_parsing = 1,
    supports_param_files = 1,
    module_map = ":module.modulemap",
)

cc_toolchain_config(
    name = "local",
    cpu = "darwin_arm64",
    compiler = "clang",
    toolchain_identifier = "local",
    host_system_name = "local",
    target_system_name = "local",
    target_libc = "macosx",
    abi_version = "local",
    abi_libc_version = "local",
    cxx_builtin_include_directories = ["/Applications/Xcode-16.2.0.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/clang/16/include",
    "/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include",
    "/Applications/Xcode-16.2.0.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/include",
    "/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/System/Library/Frameworks",
    "/Applications/Xcode-16.2.0.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/lib/clang/16/share",
    "/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk/usr/include/c++/v1",
    "/Library/Developer/CommandLineTools/SDKs/MacOSX.sdk"],
    tool_paths = {"ar": "/usr/bin/libtool",
        "ld": "/usr/bin/ld",
        "llvm-cov": "None",
        "llvm-profdata": "None",
        "cpp": "/usr/bin/cpp",
        "gcc": "cc_wrapper.sh",
        "dwp": "/usr/bin/dwp",
        "gcov": "/usr/bin/gcov",
        "nm": "/usr/bin/nm",
        "objcopy": "/usr/bin/objcopy",
        "objdump": "/usr/bin/objdump",
        "strip": "/usr/bin/strip",
        "c++filt": "/usr/bin/c++filt",
        "parse_headers": "cc_wrapper.sh",
        "validate_static_library": "validate_static_library.sh"},
    compile_flags = ["-fstack-protector",
    "-Wall",
    "-Wthread-safety",
    "-Wself-assign",
    "-Wunused-but-set-parameter",
    "-Wno-free-nonheap-object",
    "-fcolor-diagnostics",
    "-fno-omit-frame-pointer"],
    opt_compile_flags = ["-g0",
    "-O2",
    "-D_FORTIFY_SOURCE=1",
    "-DNDEBUG",
    "-ffunction-sections",
    "-fdata-sections"],
    dbg_compile_flags = ["-g"],
    conly_flags = [],
    cxx_flags = ["-std=c++14"],
    link_flags = ["-headerpad_max_install_names"],
    link_libs = ["-lc++",
    "-lm"],
    opt_link_flags = ["-Wl,-dead_strip"],
    unfiltered_compile_flags = ["-no-canonical-prefixes",
    "-Wno-builtin-macro-redefined",
    "-D__DATE__=\"redacted\"",
    "-D__TIMESTAMP__=\"redacted\"",
    "-D__TIME__=\"redacted\""],
    coverage_compile_flags = ["-fprofile-instr-generate",  "-fcoverage-mapping"],
    coverage_link_flags = ["-fprofile-instr-generate"],
    supports_start_end_lib = False,
    extra_flags_per_feature = {"use_module_maps": ["-Xclang", "-fno-cxx-modules"]},
)

toolchain(
    name = "cc-toolchain",
    exec_compatible_with = [
        "@platforms//os:macos",
        "@platforms//cpu:arm64",
    ],
    target_compatible_with = [
        "@platforms//os:macos",
        "@platforms//cpu:arm64",
    ],
    toolchain = ":cc-compiler-darwin_arm64",
    toolchain_type = "@bazel_tools//tools/cpp:toolchain_type",
)

platform(
    name = "darwin_arm64",
    constraint_values = [
        "@platforms//os:macos",
        "@platforms//cpu:arm64",
    ],
    exec_properties = {
        "OSFamily": "Darwin",
        "Arch": "arm64",
    },
)
