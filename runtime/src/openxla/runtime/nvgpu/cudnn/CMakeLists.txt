# Copyright 2023 The OpenXLA Authors
#
# Licensed under the Apache License v2.0 with LLVM Exceptions.
# See https://llvm.org/LICENSE.txt for license information.
# SPDX-License-Identifier: Apache-2.0 WITH LLVM-exception

iree_add_all_subdirs()

iree_cc_library(
  NAME
    defs
  INCLUDES
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/../../../..>"
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/../../../..>"
  PUBLIC
)

iree_cc_library(
  NAME
    cudnn_module
  HDRS
    "cudnn_module.h"
  SRCS
    "cudnn_module.cpp"
  DEPS
    ::defs
    ::dynamic_symbols
    ::cudnn_api
  PUBLIC
)

iree_cc_library(
  NAME
    cudnn_module_dylib
  HDRS
    "cudnn_module.h"
  SRCS
    "cudnn_module.cpp"
  DEPS
    ::defs
    ::dynamic_symbols
    ::cudnn_api
  PUBLIC
  SHARED
)

iree_cc_library(
  NAME
    cudnn_api
  HDRS
    "cudnn_api.h"
  SRCS
    "cudnn_api.cpp"
  DEPS
    ::defs
    ::dynamic_symbols
    ::cudnn_stub
    cudnn_frontend
    iree::hal::drivers::cuda
    iree::modules::hal
    iree::vm
  PUBLIC
)

iree_cc_library(
  NAME
    dynamic_symbols
  HDRS
    "dynamic_symbols.h"
    "status_util.h"
  TEXTUAL_HDRS
    "dynamic_symbol_tables.h"
  SRCS
    "cudnn_headers.h"
    "dynamic_symbols.c"
    "status_util.c"
  DEPS
    ::defs
    iree::base
    iree::base::core_headers
    iree::base::internal::dynamic_library
    iree::base::tracing
    # TODO: We rely on the fact that cuDNN headers are located in the same
    # directory as CUDA headers. Fix this.
    iree_cuda::headers
  PUBLIC
)

iree_cc_library(
  NAME
    cudnn_stub
  HDRS
    "cudnn_stub.h"
  TEXTUAL_HDRS
    "cudnn_stub.h.inc"
  SRCS
    "cudnn_stub.cpp"
  DEPS
    ::defs
    ::dynamic_symbols
  PUBLIC
)
