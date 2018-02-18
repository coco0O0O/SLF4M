SLF4M is a simple but flexible logging framework for Matlab, built on top of SLF4J and Apache log4j. You can
use it to do runtime-configurable logging from your Matlab scripts and programs. This can be more informative and more manageable than commenting in and out `fprintf()` statements.

The API is simple enough that you can get up and running with it quickly, or even use it casually in scripts, but it's flexible and powerful enough to be useful for larger systems.

Usage
-------------------------


The logging functions are in the `+logm` package. Call them from within your Matlab code.

```
function helloWorld(x)

if nargin < 1 || isempty(x)
    x = 123.456;
    % These debug() calls will only show up if you set log level to DEBUG
    logm.debug('Got empty x input; defaulted to %f', x);
end
z = x + 42;

logm.info('Answer z=%f', z);
if z > intmax('int32')
    logm.warn('Large value z=%f will overflow int32', z);
end

try
    some_bad_operation(x);
catch err
    logm.error('Something went wrong in some_bad_operation(%f): %s', ...
        x, err.message);
end

end
```

The output looks like this:

```
>> helloWorld
08:57:47.178 INFO  helloWorld() - Answer z=165.456000
08:57:47.179 ERROR helloWorld() - Something went wrong in some_bad_operation(123.456000): Undefined function 'some_bad_operation' for input arguments of type 'double'.
```

Thanks to `dispstr()`, you can also pass Matlab objects to the `%s` conversions.

```
>> m = containers.Map;
>> m('foo') = 42; m('bar') = struct;
>> logm.info('Hello, world! %s', m)
09:52:29.809 INFO  base - Hello, world! 2-by-1 containers.Map
```

For more details, see the User's Guide included in the distribution.

Author
-----------------------------------

SLF4M's home page is the repo on GitHub: https://github.com/apjanke/SLF4M.
