{{_toc__}}

# GitBook

[GitBook](https://github.com/GitbookIO/gitbook)

## Plugins

### [gitbook-plantuml](https://github.com/romanlytkin/gitbook-plantuml)

```
@startuml
Class Stage
Class Timeout {
    +constructor:function(cfg)
    +timeout:function(ctx)
    +overdue:function(ctx)
    +stage: Stage
}
Stage <|-- Timeout
@enduml
```

![](https://github.com/romanlytkin/gitbook-plantuml/raw/master/images/uml.png)

### [gitbook-grvis](https://github.com/romanlytkin/gitbook-grvis)

```
digraph g {
	overlap=false;
	rankdir = BT;
	node [shape=record];
	subgraph Atlantis {
	    Tour;
	    Order;
	    CollectionPoint;
	    TakePointTourist;
	    TransportOwner;
	    BusItem;
	    ReturnPointTourist;

	    Tour -> Order[label="" len=4.00];
	    Order -> Tour[label="" len=4.00];
	    CollectionPoint -> TakePointTourist[label="" len=4.00];
	    TransportOwner -> BusItem[label="" len=4.00];
	    CollectionPoint -> ReturnPointTourist[label="" len=4.00];
	}
}
```
![](https://github.com/romanlytkin/gitbook-grvis/raw/master/images/dot.png)