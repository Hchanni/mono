workspace(
  name="hchanni_monorepo"
)

load(
    "@bazel_tools//tools/build_defs/repo:http.bzl",
    "http_archive",
    "http_file",
)

load(
    "@bazel_tools//tools/build_defs/repo:git.bzl",
    "git_repository",
)

## Useful Bazel rules Via Skylib


http_archive(
    name = "bazel_skylib",
    sha256 = "bc283cdfcd526a52c3201279cda4bc298652efa898b10b4db0837dc51652756f",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/bazel-skylib/releases/download/1.7.1/bazel-skylib-1.7.1.tar.gz",
        "https://github.com/bazelbuild/bazel-skylib/releases/download/1.7.1/bazel-skylib-1.7.1.tar.gz",
    ],
)

load("@bazel_skylib//:workspace.bzl", "bazel_skylib_workspace")

bazel_skylib_workspace()


## Setting up Python

load("@bazel_tools//tools/build_defs/repo:http.bzl", "http_archive")

http_archive(
    name = "rules_python",
    sha256 = "ca2671529884e3ecb5b79d6a5608c7373a82078c3553b1fa53206e6b9dddab34",
    strip_prefix = "rules_python-0.38.0",
    url = "https://github.com/bazelbuild/rules_python/releases/download/0.38.0/rules_python-0.38.0.tar.gz",
)

load("@rules_python//python:repositories.bzl", "py_repositories")

py_repositories()


load("@rules_python//python:repositories.bzl", "python_register_toolchains")

# Pin our version and make sure
# Bazel doesn't pick up random other system lv python versions 
# Laying around. 
# Keep things hermetic and consistent
python_register_toolchains(
    name = "python_3_11",
    python_version = "3.11",
)

# Still needs host level python 
# Since this all happens at eval time a python
# toolchain hasn't been resolved yet bruh

load("@rules_python//python:pip.bzl", "pip_parse")

# Add this load statement
load("@python_3_11//:defs.bzl", "interpreter")

pip_parse(
    name = "pip",
    python_interpreter_target = interpreter,
    requirements_lock = "//utils/python:requirements.txt",
)

# Load the starlark macro which will define your dependencies.
load("@pip//:requirements.bzl","install_deps")

# Call it to define repos for your requirements.
install_deps()


## Seeting up Golang 

http_archive(
    name = "io_bazel_rules_go",
    integrity = "sha256-M6zErg9wUC20uJPJ/B3Xqb+ZjCPn/yxFF3QdQEmpdvg=",
    urls = [
        "https://mirror.bazel.build/github.com/bazelbuild/rules_go/releases/download/v0.48.0/rules_go-v0.48.0.zip",
        "https://github.com/bazelbuild/rules_go/releases/download/v0.48.0/rules_go-v0.48.0.zip",
    ],
)

load("@io_bazel_rules_go//go:deps.bzl", "go_register_toolchains", "go_rules_dependencies")

go_rules_dependencies()

go_register_toolchains(version = "1.22")
