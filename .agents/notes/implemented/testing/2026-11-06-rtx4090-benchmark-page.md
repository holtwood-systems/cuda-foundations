# Agent Note: RTX 4090 实测页 + tensorcraft benchmark 原始数据归档

Status: implemented

## Problem

仓库此前只有一页 3060 Laptop 实测（无原始 JSON）。tensorcraft 的
benchmark 数字在教学中被引用时缺少可复核产物。另外本机验证发现
`TC_BUILD_PYTHON=ON`（默认）在 GCC 11.4 下无法编译：pybind11 模块
用 host C++ 编译器编译含 CUDA kernel 的 `attention.hpp`
（`blockIdx`/`threadIdx` 未定义）。

## Decision

- 新增 `docs/en/benchmarks/rtx4090-2026-11-06.md`，格式沿用 3060 页，
  并把三个 GBench JSON 与 ctest 日志归档到
  `docs/en/benchmarks/data/2026-11-06-rtx4090/`。
- 本页明确记录 `TC_BUILD_PYTHON=OFF` 的构建绕过与其根因；Python
  bindings 的修复作为独立后续项（需要给 kernel 头加 `__CUDACC__`
  守卫或把 bindings 拆成 CUDA 编译单元），本批不动代码。

## Alternatives considered

- 只记录 3060 页数字并声称"4090 更快"：无可复核依据，违反纪律。
- 顺手修 Python bindings：改动涉及 kernel 头文件条件编译，属于
  架构级改动，应单独决策与验证；本批聚焦证据。

## Consequences

- 教学文档引用 tensorcraft 数字时有了 sm_89 的可复核入口。
- Python bindings 在默认配置下仍不可构建；新页面与笔记已显式记录，
  避免被误读为环境问题。

## Verification

`ctest` 36/36 通过；三个 benchmark JSON 项数 12/11/10 与页面一致；
构建命令与 arch 参数在页面 Provenance 中完整记录。
