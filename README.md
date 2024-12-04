# Bitrise toolchains

### How to use
Add this to .bazelrc:
```
build:remote_bitrise --action_env=BAZEL_DO_NOT_DETECT_CPP_TOOLCHAIN=1
build:remote_bitrise --extra_execution_platforms=@bitrise_toolchains//darwin_arm64
build:remote_bitrise --extra_toolchains=@bitrise_toolchains//darwin_arm64:cc-toolchain
build:remote_bitrise --crosstool_top=@bitrise_toolchains//darwin_arm64:toolchain
build:remote_bitrise --host_crosstool_top=@bitrise_toolchains//darwin_arm64:toolchain
```

Check [releases](https://github.com/bitrise-io/bitrise-toolchains/releases) page for the instructions on how to modify WORKSPACE file.