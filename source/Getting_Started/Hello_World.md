---
seo:
  title: Vlang Hello World
title: Hello World
weight: 100
layout: page
navigation:
  show: true
---
To compile the program, we use the rdmd build utility installed with DMD compiler.

{% codeblock lang:d %}
import esdl;
class Hello: Entity {
  void sayHello() {
    import std.stdio: writeln;
    writeln("Hello World from: ", Process.self.getFullName());
  }
  Task!sayHello greet[2];
}

class Top: Entity {
  Inst!Hello hello[2];
}

void main() {
  simulate!(Top)("root", 4);
}
{% endcodeblock %}

The program prints "Hello World" greetings four times, each from the pair of tasks instantiated in the couple of entity objects. The greetings strings also indicate the hierarchy the greetings originates from.

```
$ rdmd -I${VLANGDIR}/src helloWorld.d
>>>>>>>>>> Starting Phase: ELABORATE
>>>>>>>>>> Starting Phase: CONFIGURE
********** No default timePrecision specified; setting timePrecision to 1.psec
********** No default timeUnit specified; setting timeUnit to 1.nsec
>>>>>>>>>> Starting Phase: BIND
>>>>>>>>>> Calling "endElab" for all module instances
>>>>>>>>>> Start of Simulation
Hello World from: root.TopInstance.hello[0].greet[0]
Hello World from: root.TopInstance.hello[0].greet[1]
Hello World from: root.TopInstance.hello[1].greet[0]
Hello World from: root.TopInstance.hello[1].greet[1]
 > Shutting down all the active tasks ....
 > Shutting down all the Routine Threads ....
 > Simulation Complete....
```

