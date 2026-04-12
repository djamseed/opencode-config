# Go Concurrency & Performance Patterns

| Concept            | Best Practice                                                                  |
| :----------------- | :----------------------------------------------------------------------------- |
| **Goroutines**     | Never start a goroutine without knowing how it will stop.                      |
| **Channels**       | Use for orchestration; use Mutexes for state protection.                       |
| **Context**        | Pass `context.Context` as the first argument to all IO/Long-running functions. |
| **Error Handling** | Don't just check errors; wrap them with `fmt.Errorf("context: %w", err)`.      |

## Safety Patterns

- **Select/Done**: Always use a `done` or `ctx.Done()` channel in `select` loops.
- **WaitGroups**: Use `sync.WaitGroup` for parallel worker synchronization.
- **Atomic Operations**: Use `sync/atomic` for simple counters to avoid Mutex overhead.

## Project Structure

- `internal/`: For code that should not be exported to other projects.
- `pkg/`: For code that is safe for external consumption.
- `cmd/`: One subdirectory per binary.
