## Serializacja ##

### Dzieci ###
![Red-warning-sign.jpg](https://bitbucket.org/repo/b84LLq/images/332056504-Red-warning-sign.jpg)

Dzieci **nie są** serializowane, mimo że ich dodawanie jest jeszcze wspierane na poziomie modelu.

Wynika to z decyzji projektowej, że mamy płaską strukturę timeline'ów na poziomie sceny.

### Mechanizm szukania timelineów przy deserializacji ###

Zakładamy, że struktura w TimelineManager odpowiada naszej (czyli: global timeline / scene timeline / timeliney sceny)

1. Po nazwie zserializowanej w pliku spośród dzieci scene timeline'a
2. Jak nie ma to default
3. Jak nie ma to scene timeline
4. Jak nie ma to trzeba stworzyć timeline o nazwie "fallback timeline"