# Zig Build System & Memory Safety

| Area               | Direction                                                 |
| :----------------- | :-------------------------------------------------------- |
| **Memory**         | Explicit allocation only. No hidden heap usage.           |
| **Error Handling** | Error unions and `try`/`catch`. No exceptions.            |
| **Build Tool**     | `build.zig` is the source of truth for compilation logic. |
| **Comptime**       | Use for generics and metaprogramming instead of macros.   |

## Memory Management Strategy

- **Allocator Passing**: Functions that need memory must accept a `std.mem.Allocator`.
- **Arena Allocators**: Use for short-lived tasks to simplify cleanup.
- **Defer**: Use `defer` immediately after allocation to ensure `free()`.

## Build CLI

| Goal               | Command                                  |
| :----------------- | :--------------------------------------- |
| **Standard Build** | `zig build`                              |
| **Run Project**    | `zig build run`                          |
| **Testing**        | `zig build test`                         |
| **Optimization**   | `-Doptimize=ReleaseSafe` / `ReleaseFast` |
