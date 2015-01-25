# Productivity

## Redmine

[UML and Graphs in Redmine Wiki](http://www.jaqb.gda.pl/eng/Spolecznosc/Blog/UML-and-Graphs-in-Redmine-Wiki)

[Wiki External Filter plugin released](http://www.redmine.org/boards/3/topics/10649?page=4)

plantuml example input:

```
{{plantuml(
    Alice -> Bob: Authentication Request
    alt successful case
      Bob -> Alice: Authentication Accepted
    else some kind of failure
      Bob -> Alice: Authentication Failure
      opt
        loop 1000 times
          Alice -> Bob: DNS Attack
        end
      end
    else Another type of failure
      Bob -> Alice: Please repeat
    end
    )}}
```

and, output:

![](http://www.redmine.org/attachments/download/3013/wiki_plantuml_sample.png)

graphviz example input:

```
{{graphviz(
    digraph finite_state_machine {
        rankdir=LR;
        size="8,5" 
        node [shape = doublecircle]; LR_0 LR_3 LR_4 LR_8;
        node [shape = circle];
        LR_0 -> LR_2 [ label = "SS(B)" ];
        LR_0 -> LR_1 [ label = "SS(S)" ];
        LR_1 -> LR_3 [ label = "S($end)" ];
        LR_2 -> LR_6 [ label = "SS(b)" ];
        LR_2 -> LR_5 [ label = "SS(a)" ];
        LR_2 -> LR_4 [ label = "S(A)" ];
        LR_5 -> LR_7 [ label = "S(b)" ];
        LR_5 -> LR_5 [ label = "S(a)" ];
        LR_6 -> LR_6 [ label = "S(b)" ];
        LR_6 -> LR_5 [ label = "S(a)" ];
        LR_7 -> LR_8 [ label = "S(b)" ];
        LR_7 -> LR_5 [ label = "S(a)" ];
        LR_8 -> LR_6 [ label = "S(b)" ];
        LR_8 -> LR_5 [ label = "S(a)" ];
    }
)}}
```