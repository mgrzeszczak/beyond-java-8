# Beyond Java 8

This is a curated list of language related (syntax, methods) features that are present if later versions of Java (beyond 8). Each feature will be provided with an example in a Spock-like assertion manner.

## Java 9

1. Private interface methods - to have reusable methods to use in default interface methods

   ```
   interface TestInterface {

       private void log(message) {
           System.out.println(message);
       }

       default ... action1() {
           ...
           log(...)
           ...
       }

       default ... action2() {
           ...
           log(...)
           ...
       }

   }
   ```

2. Collection static factory methods

3. New stream methods
   - `Stream.ofNullable` - create stream from a possibly null object
   ```
   Stream.ofNullable(null) == Stream.empty()
   ```
   - `dropWhile` - skip elements as long as predicate is satisfied
   ```
   Stream.of(1,2,3)
       .dropWhile(i -> i < 2) == Stream.of(2,3)
   ```
   - `takeWhile` - take elements as long as predicate is satisfied and discard the rest
   ```
   Stream.of(1,2,3)
       .takeWhile(i -> i < 2) == Stream.of(1)
   ```
   - `iterate` - generate a stream as long as predicate is satisfied
   ```
   Stream.iterate(0, i -> i < 3; i -> i + 1)
       .collect(Collectors.toList()) == [0,1,2]
   ```
4. New collector methods
   - `Collectors.filtering` - filter while collecting
   ```
   Stream.of(1,2,3)
       .collect(Collectors.filtering(i -> i < 2), Collectors.toList()) == [1]
   ```
   - `Collectors.flatMapping` - flat map objects into collection while collecting
   ```
   class Process {
       private ProcessType type;
       private List<ProcessInstance> instances;
   }
   List<Process> processes = ...
   Map<ProcessType, List<ProcessInstance>> instancesByType = processes.stream()
       .collect(Collectors.groupingBy(Process::getType, Collectors.flatMapping(p -> p.getInstances().stream(), Collectors.toList())));
   ```
5. Optional

   - `or()` - allows chaining

   ```
   Optional.ofNullable(method1())
    .or(this::method2) // method2 is called only if optional is empty
   ```

   - `stream()` - create a stream from optional, which is either empty or has one element. Useful if we have a stream of optionals.

   ```
   Stream<Optional<?>> stream = ...
   stream
    .flatMap(Optional::stream) // now it's a stream of values
    .collect(Collectors.toList())
   ```

   - `ifPresentOrElse` - consume value or execute a runnable if not present

   ```
   Optional.of(result)
    .ifPresent(r -> System.out.println("the result is: " + r), () -> System.out.println("No result value"))
   ```

6. New process API

## Java 10

1. Optional
   - `orElseThrow()` - throws NoSuchElementException when empty
   ```
   Integer value = Optional.of(1).orElseThrow()
   ```

## License

MIT License

```
Copyright (c) 2018 Maciej Grzeszczak

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
